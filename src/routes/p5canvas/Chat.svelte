<script lang="ts">
  import { Socket, Channel } from "phoenix";
  import { onMount } from "svelte";

  let messages: {
    time: string;
    nickname: string;
    text: string;
    self: boolean;
  }[] = [];
  let myNickname: string = "";
  let chatInput: HTMLInputElement;
  let channel: Channel;

  function connectSocket() {
    console.log("connecting to socket");
    let socket = new Socket(import.meta.env.VITE_SOCKET_URL);
    socket.connect();
    channel = socket.channel("room:lobby", {});

    channel.on("new_msg", (payload: { body: string; nickname: string }) => {
      if (!myNickname) {
        myNickname = payload.nickname;
      }
      messages = [
        ...messages,
        {
          time: new Date().toLocaleTimeString(),
          nickname: payload.nickname,
          text: payload.body,
          self: payload.nickname === myNickname,
        },
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
        <p class={message.self ? "self-message" : ""}>
          <span>{message.time}</span>
          <span class="nickname">{message.nickname}</span>
          <br />
          <span class="text">{message.text}</span>
        </p>
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
    width: 280px;
    min-height: 100%;
    max-height: 50dvh;
    height: 100%;
    display: flex;
    flex-direction: column;
    background-color: white;
  }
  .chat-pannel {
    flex-grow: 1;
    overflow-y: auto;
    padding: 16px;
  }

  .nickname {
    background-color: #eee;
  }
  .nickname,
  .text {
    font-weight: bold;
  }
  .self-message {
    color: #0000ff; 
  }
  .chat-input {
    padding: 28px 16px;
    border: none;
    border-top: 1px dashed;
  }
</style>
