<template>
  <div>
    <candy-header :candies="candies" :money="money"/>
    <div v-if="automation.cook.amount !== 0">Producing {{ parseFloat(candyRate).toFixed(1) }}🍬/sec</div>
    <upgrade-component v-if="upgrades.clickMultiplier.cost <= money || upgrades.clickMultiplier.amount > 0"
                       :automation="automation" :money="money" :clickMultiplierCost="upgrades.clickMultiplier.cost"
                       v-on:buyUpgrade="buyUpgrade"></upgrade-component>

    <factory-component v-if="money >= 10 || automation.cook.amount !== 0" v-on:buyAutomation="buyAutomation"
                       :automation="automation"></factory-component>
    <div class="buttons">
      <div class="btn btn-primary m-1" @click="getCandy">Get Candy 🍬</div>
      <div class="btn btn-primary m-1" @click="sellCandy(1)">Sell candy 💵</div>
      <div class="btn btn-primary" @click="sellCandy(10)">x10💵</div>
      <div class="btn btn-primary m-1" @click="sellCandy(100)">x100💵</div>
      <div class="btn btn-primary m-1" @click="sellCandy('max')">Max💵</div>
    </div>
  </div>
</template>

<script>

import CandyHeader from "@/components/CandyHeader";
import UpgradeComponent from "@/components/UpgradeComponent";
import FactoryComponent from "@/components/automation/FactoryComponent";

export default {
  data() {
    return {
      candies: 1000,
      candyRate: 0,
      candyValue: 1,

      clickRate: 1,

      money: 0,

      upgrades: {
        clickMultiplier: {
          cost: 100,
          multiplier: 2,
          amount: 0
        }
      },

      automationCostIncreasesPercent: 10,
      automation: {
        cook: {
          name: 'Cook',
          amount: 0,
          cost: 10,
          revenue: 0.1,
          upgradeCost: 500
        },
        factory: {
          name: 'Factory',
          amount: 0,
          cost: 100,
          revenue: 1,
          upgradeCost: 1000
        }
      }
    }
  },
  methods: {

    //Candies
    getCandy() {
      this.candies += this.clickRate
    },
    sellCandy(amount) {
      if (amount === 'max') {
        if (this.candies >= 1)
          amount = Math.floor(this.candies);
        else
          amount = 0;
      }
      if (this.candies < amount) {
        return;
      }
      this.candies -= amount;
      this.money += this.candyValue * amount;
    },

    //Upgrades
    buyUpgrade(upgrade) {
      switch (upgrade) {
        case 'clickMultiplier':
          if (this.money >= this.upgrades[upgrade].cost) {
            this.money -= this.upgrades[upgrade].cost
            this.upgrades[upgrade].amount++;
            this.upgrades[upgrade].cost *= 10;
            this.clickRate *= this.upgrades[upgrade].multiplier;
          }
          break;
        default:
          if (this.money >= this.automation[upgrade].upgradeCost) {
            this.money -= this.automation[upgrade].upgradeCost
            this.automation[upgrade].upgradeCost *= 10;
            this.automation[upgrade].revenue *= 2;
            this.recalculateCandyRate();
          }
          break;
      }
    },

    //Automation
    buyAutomation(automation) {
      if (this.money >= this.automation[automation].cost) {
        this.money -= this.automation[automation].cost
        this.candyRate += this.automation[automation].revenue
        this.automation[automation].amount++;
        this.automation[automation].cost += Math.round(this.automation[automation].cost / this.automationCostIncreasesPercent);
      }
    },

    recalculateCandyRate() {
      let rate = 0
      console.log(this.automation)
      for (let obj in this.automation) {
        rate += this.automation[obj].amount * this.automation[obj].revenue;
      }

      this.candyRate = rate;
    }
  },
  components: {
    CandyHeader,
    FactoryComponent,
    UpgradeComponent
  },
  created() {
    setInterval(() => {
      this.candies += this.candyRate;
    }, 1000)
  },
}
</script>

<style>
.buttons {
  align-items: center;
}
</style>
