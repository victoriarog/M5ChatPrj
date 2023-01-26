<template>
    <div class="nickForm">
      <input v-model="nickname" @keyup.enter="submitNickname" placeholder="Enter your nickname" />
      <button @click="submitNickname">Join Chat</button>
    </div>
  </template>
  
  <script lang="ts">
  export default {
    data() {
      return {
        nickname: ''
      }
    },
    methods: {
      submitNickname() {
        this.$emit('submit', this.nickname)
      }
    }
  }
  </script>