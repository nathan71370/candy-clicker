<template>
  <div class="header">
    <div class="nav-item me-5"> {{Math.floor(candies)}} 🍬</div>
    <div class="nav-item"> {{money}} 💵</div>
  </div>
</template>

<script>
export default {
  props: ['candies','money']
}
</script>

<style scoped>
.header {
  display: flex;
  flex-direction: row;
  margin-bottom: 60px;
  margin-top: 10px;
  background: aqua;
  justify-content: center;
}
</style>
