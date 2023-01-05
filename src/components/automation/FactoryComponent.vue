<template>
  <div class="container">
    <div class="card">
      <h1 class="ms-2">Automation</h1>
      <div class="card-body" v-for="(auto, index) in automation" :key="index">
        {{ auto.amount }} {{auto.name}}
        <div class="btn btn-primary m-1" @click="buyAutomation(auto.name)">Buy {{ auto.name }} ({{ auto.cost }}💵)</div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'factory-component',
  methods: {
    buyAutomation(type) {
      this.$emit('buyAutomation', type.toLowerCase())
    }
  },
  props: ['automation']
}

</script>
