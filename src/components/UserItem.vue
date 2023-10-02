<script setup lang="ts">
defineProps<{
  user_id: number,
  user_name: string,
  user_age: number,
  user_last_login: string
}>()
</script>

<template>
  <div class="user-container">
    <div class="user__id">ID: {{ user_id }}</div>
    <div class="user__name">Name: {{ user_name }}</div>
    <div class="user__age">Age: {{ user_age }}</div>
    <div class="user__last-login">Last login: {{ user_last_login }}</div>
  </div>
</template>

<style scoped>

.user-container {
  width: fit-content;
  height: fit-content;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10px;
  padding: 10px 10px;
}

</style>
