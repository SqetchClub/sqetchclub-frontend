<script lang="ts">
  import { onMount } from "svelte";
  import { writable, type Writable } from "svelte/store";
  import { Socket, Channel } from "phoenix";

  let channel: Channel;

  function connectSocket() {
    console.log("connecting to socket canvas");
    let socket = new Socket(import.meta.env.VITE_SOCKET_URL);
    socket.connect();
    channel = socket.channel("room:lobby", {});

    channel
      .join()
      .receive("ok", (resp: unknown) => {
        console.log("Joined successfully", resp);
      })
      .receive("error", (resp: unknown) => {
        console.log("Unable to join", resp);
      });
  }

  function sendLine(
    x1: number,
    y1: number,
    x2: number,
    y2: number,
    color: string
  ) {
    const lineData = {
      x1,
      y1,
      x2,
      y2,
      color,
    };

    channel.push("new_sqetch", { body: JSON.stringify(lineData) });
  }

  let canvasContainer: any = null;
  let p5: any;
  let canvas: any;

  const colors = [
    "#000", //black
    "#FFE400", //yellow
    "#6AF217", //green
    "#EF120C", //red
    "#0038FF", //blue
  ];
  const currentColor = writable("#000"); // Default color
  const bgOrEraserColor = "#fff"; // Eraser color

  const sketch = (p: any) => {
    const canvasContainerWidth = canvasContainer.clientWidth;
    const canvasContainerHeight = canvasContainer.clientWidth;
    p.setup = () => {
      canvas = p
        .createCanvas(canvasContainerWidth, canvasContainerHeight)
        .parent(canvasContainer);
      p.background(bgOrEraserColor);
      connectSocket();

      channel.on("new_sqetch", (payload: { body: string }) => {
        const lineData = JSON.parse(payload.body);
        p.stroke(lineData.color);
        p.line(lineData.x1, lineData.y1, lineData.x2, lineData.y2);
      });
    };

    p.windowResized = () => {
      const canvasContainerWidth = canvasContainer.clientWidth;
      const canvasContainerHeight = canvasContainerWidth;
      p.resizeCanvas(canvasContainerWidth, canvasContainerHeight);
      p.background(bgOrEraserColor); // Reset background on resize
    };

    p.draw = () => {};

    p.mouseDragged = () => {
      currentColor.subscribe((color) => {
        if (color === bgOrEraserColor) {
          p.strokeWeight(20); // Thicker stroke for eraser
        } else {
          p.strokeWeight(4); // Default stroke weight
        }
        p.stroke(color);
        p.line(p.pmouseX, p.pmouseY, p.mouseX, p.mouseY);
        sendLine(p.pmouseX, p.pmouseY, p.mouseX, p.mouseY, color);
      });
    };
  };

  onMount(async () => {
    const p5Module = await import("p5");
    p5 = p5Module.default;
    new p5(sketch);
  });

  function setColor(color: string) {
    currentColor.set(color);
  }
</script>

<div class="container">
  <div bind:this={canvasContainer} class="canvas-container" />
  <div class="palette">
    {#each colors as color}
      <button
        class="color-swatch"
        style="background-color: {color};"
        on:click={() => setColor(color)}
      ></button>
    {/each}
    <button
      class="eraser"
      style="background-color: #fff;"
      on:click={() => setColor(bgOrEraserColor)}
    >
      ERASER
    </button>
  </div>
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
  }

  .canvas-container {
    width: 100%;
  }

  .palette {
    width: 100%;
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .color-swatch {
    width: 100%;
    height: 100%;
    cursor: pointer;
    border: none;
  }

  .eraser {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    border: none;
    border-top: 1px dashed;
  }
</style>
