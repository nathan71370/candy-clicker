<template>
  <div class="container">

    <div class="card">
      <h1 class="ms-2">Upgrades</h1>
      <div class="card-body">
        <div v-if="money >= clickMultiplierCost" class="btn btn-primary m-1" @click="buyClickUpgrade">Multiply clicks
          value by 2
        </div>
        <div v-for="(auto, index) in automation" :key="index">
          <div v-if="money >= auto.upgradeCost && auto.amount !== 0" @click="buyUpgrade(auto.name)">
            <img height="50" width="50" :src="'@/assets/' + auto.name.toLowerCase() + '.png'" :alt="auto.name" :title="'Multiply '+ auto.name +' value by 2'">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'factory-component',
  methods: {
    buyClickUpgrade() {
      this.$emit('buyUpgrade', 'clickMultiplier')
    },
    buyUpgrade(type) {
      this.$emit('buyUpgrade', type.toLowerCase())
    }
  },
  props: ['money', 'clickMultiplierCost', 'automation'],
  computed: {
    imagePath(image) {
      return require(`@/assets/${image.toLowerCase()}.png`);
    }
  }
}

</script>
