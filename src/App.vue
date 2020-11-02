<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-4">
        <img alt="Vue logo" src="./assets/logo.png">
        <h1>
          {{ state.target.name }}
        </h1>
        <p :class="healthColor()">
          Health: {{ state.target.health }}
        </p>
        <p>Hits: {{ state.target.hits }}</p>
        <button class="btn btn-danger btn-block" @click="attack(attackValue, attackKey)" v-for="(attackValue, attackKey) in state.target.attacks" :key="attackValue" :disabled="state.target.health <= 0">
          {{ attackKey }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, watch } from 'vue'
export default {
  name: 'App',
  setup(context) {
    const state = reactive({
      target: {
        name: 'Bob',
        health: 100,
        hits: 0,
        attacks: {
          slap: 1,
          punch: 5,
          kick: 15
        }
      }
    })

    watch(
      () => state.target.health,
      (newValue, oldValue) => {
        if (newValue < 0) {
          state.target.health = 0
        } else {
          state.target.health = newValue
        }
      }
    )

    return {
      state,
      attack(val, key) {
        // state.target.health -= val
        // NOTE or
        state.target.health -= state.target.attacks[key]
        state.target.hits++
      },
      healthColor() {
        let cssClass = 'text-success'
        if (state.target.health < 20) {
          cssClass = 'text-danger'
        } else if (state.target.health < 50) {
          cssClass = 'text-warning'
        }
        return cssClass
      }
    }
  }

}
</script>

<style scoped>

</style>
