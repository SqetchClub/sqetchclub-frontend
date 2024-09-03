<script lang="ts">
  import { Socket, Channel } from "phoenix";
  import { onMount } from "svelte";

  let messages: string[] = [];
  let chatInput: HTMLInputElement;
  let channel: Channel;

  function connectSocket() {
    console.log("connecting to socket");
    let socket = new Socket(import.meta.env.VITE_SOCKET_URL);
    socket.connect();
    channel = socket.channel("room:lobby", {});

    channel.on("new_msg", (payload: { body: string }) => {
      messages = [
        ...messages,
        `[${new Date().toLocaleTimeString()}] ${payload.body}`,
      ];
    });

    channel
      .join()
      .receive("ok", (resp: unknown) => {
        console.log("Joined successfully", resp);
      })
      .receive("error", (resp: unknown) => {
        console.log("Unable to join", resp);
      });
  }

  function handleKeyPress(event: KeyboardEvent) {
    if (event.key === "Enter" && chatInput.value.trim() !== "") {
      channel.push("new_msg", { body: chatInput.value });
      chatInput.value = "";
    }
  }

  onMount(() => {
    connectSocket();
  });
</script>

<div>
  <div class="container">
    <div class="chat-pannel">
      {#each messages as message}
        <p>{message}</p>
      {/each}
    </div>
    <input
      bind:this={chatInput}
      on:keypress={handleKeyPress}
      type="text"
      class="chat-input"
      placeholder="Guess the word!"
    />
  </div>
</div>

<style>
  .container {
    min-width: 400px;
    min-height: 880px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    gap: 24px;
  }
  .chat-pannel {
    flex-grow: 1;
    overflow-y: auto;
    padding: 16px;
    background-color: white;
    border: 2px solid #000;
  }
  .chat-input {
    padding: 16px;
    border: none;
    border: 2px solid #000;
  }
</style>
