<template>
  <div>
    <Header/>
    <img class="pigeon" src="./assets/pigeon.gif" v-if="!connected"/>
    <div class="pigeon1b" v-if="!connected">
      <img  class="pigeon1" src="./assets/pigeon.gif"/>
    </div>
    <div class="pigeon2b" v-if="!connected">
      <img  class="pigeon2" src="./assets/pigeon.gif"/>
    </div>
  <div class="box">
    <UserNick @submit="connectToServer" v-if="!connected"/>
    <ChatLog :chat-log="chatLog" v-if="connected" />
  </div>
  
  <iframe width="0" height="0" src="https://www.youtube.com/embed/oorXCykcDCE?rel=0&autoplay=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen loop="true"></iframe>

    <div class="onlineP" v-if="connected">
      <h3>Peeps online: </h3>
    <ul>
      <li v-for="user in connectedUsers" :key="user">{{ user }}</li>
    </ul>
  </div>

  <div class="box">
    <input v-model="text" @keyup.enter="send"  @focus="startTyping(nickname)" v-if="connected"/>
    <button @click="send" v-if="connected">Send</button>
  </div>

  </div>
</template>

<script setup>

import io from "socket.io-client";
import ChatLog from "./components/ChatLog.vue";
import { ref, computed } from 'vue';
import Header from "./components/Header.vue";

const config = useRuntimeConfig();
const chatLog = ref("");
const text = ref("");
const nickname = ref(""); 

let socket;

const connected = ref(false);
const typing = ref(false);
const typingValue = ref("")
const users = ref({});


function addToChat(m) {
  if (m.startsWith("http") || m.startsWith("www")) {
    m = `<a href="${m}" target="_blank">${m}</a>`;
  }
  chatLog.value += `<div>${m}</div>`;
}

function send() {
  socket.emit("message", text.value);
  addToChat(text.value); // optimistic UI
  text.value = "";
}

function startTyping(nickname) {
  typingValue.value = text.value
  typing.value = true;
  socket.emit("typing", nickname);
}

function connectToServer(nickname) {
  socket = io(config.public.wssUrl);
  socket.emit('join', { id: socket.id, nickname: nickname });
  connected.value = true;
  socket.on("message", (data) => {
    console.log(data, "<<<<");
    addToChat(data.message);
  });
  socket.on("updateUsers", (data) => {
    users.value = data;
  });
}

const connectedUsers = computed(() => {
  return users.value ? Object.entries(users.value).filter(([user, status]) => status).map(([user]) => user) : []
});
</script>