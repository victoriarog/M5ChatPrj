<template>
    <div class="inputNick">
      <h3>Choose a nickname:</h3><br/>
      <input v-model="nickname" @keyup.enter="submitNickname" placeholder="Enter your nickname" />
      <button @click="submitNickname">Join</button>
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