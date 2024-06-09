<script setup lang="ts">
import { ref, computed } from 'vue'

const showOptions = ref<boolean>(false)
const milageRate = ref<number>(1.2)
const freeDistance = ref<number>(50)

const distance = ref<number|undefined>(undefined)
const hasAdditionalCost = ref<boolean>(false)
const additionalCost= ref<string|undefined>(undefined)

const costs = computed(() => (additionalCost.value && !isNaN(parseInt(additionalCost.value))) ? parseInt(additionalCost.value) : 0)
const paidDistance = computed(() => (distance.value && !isNaN(distance.value)) ? (distance.value - freeDistance.value) * 2 : 0)
const travelCost = computed(() => (paidDistance.value * milageRate.value + costs.value).toFixed(2))


const toggleOptions = () => showOptions.value = !showOptions.value
const copyToClipboard = () => navigator.clipboard.writeText(travelCost.value)


</script>

<template>
  <div>
    <div class="inputs">

      <label class="base-input">
        Distance to destination: 
        <input v-model="distance" placeholder="Distance to destination ..." />
      </label>
      <label class="base-input" v-if="hasAdditionalCost">
        Additional cost:
        <input v-model="additionalCost" placeholder="Additional travel cost ..." />
      </label>
      <div v-else>
        <input type="checkbox" id="hasCost" value="John" v-model="hasAdditionalCost">
        <label for="hasCost">Additional cost</label>
      </div>
      <h2
        class="result"
        @click="copyToClipboard"
      >
        Travel cost: <br /> 
        {{ travelCost }} zł
      </h2>
      <button @click="toggleOptions">Options</button>
    </div>
    <div v-if="showOptions" class="options">
      <label class="base-input">
        Cost per km:
        <input v-model="milageRate" type="text" />
      </label>
      <label class="base-input">
        Free km one way:
        <input v-model="freeDistance" type="text" />
      </label>
    </div>
  </div>
</template>

<style scoped>
.options {
  margin-top: 20px;
}

.base-input {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

.base-input input {
  padding: 5px 10px;
  min-width: 250px;
  margin-top: 3px;
}

.result {
  cursor: pointer;
}
</style>
