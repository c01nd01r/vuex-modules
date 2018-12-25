# vuex-modules
Independent modules for Vuex store.  
**In development**.

## Installation

```bash
yarn add vuex-modules
npm install vuex-modules
```
## Configuration

```js
import Vue from 'vue';
import Vuex from 'vuex';
import { Module } from 'vuex-modules';

Vue.use(Vuex);

const Store = new Vuex.Store({
  plugins: [Module.plugin],
});

const Counter = new Store.Module('Counter', {
  state: {
    counter: 0,
  },
  getters: {
    formated(state) {
      return `Counter: ${state.counter}`;
    },
  },
  mutations: {
    increase(state) {
      state.counter = state.counter + 1;
    },
    decrease(state) {
      state.counter = state.counter - 1;
    },
  },
  actions: {
    up({ commit }) {
      commit('increase');
    },
    down({ commit }) {
      commit('decrease');
    },
});

console.log(Counter);
// Object {
//  counter: 0,
//  formated: 'Counter: 0'
//  increase: function,
//  decrease: function,
//  up: function,
//  down: function,
// }

Counter.up()       // Promise<void>
Counter.formated   // 'Counter: 1'
Counter.decrease() // undefined
Counter.counter    // 0

const unwatchFn = Counter.$watch(
  (state, getters) => state.counter, // return state and/or getters for watching
  (newVal, oldVal) => console.log(newVal), // callback
  { deep: false, immediate: false }, // options like Vue.$watch() https://vuejs.org/v2/api/#vm-watch
);

Counter.$unregisterModule();
```

## In components

```html
<template>
  <div>
    <div>{{Counter.counter}} -- {{Counter.formated}}</div>
    <button @click="Counter.up">Call 'up' action</button>
    <button @click="Counter.decrease">Call 'decrease' mutation</button>
  </div>
</template>

<script>
import { connect } from 'vuex-modules';
import Counter from './Counter';

export default connect({ Counter })({
  created() {
    console.log(this.$props);
    // { Counter: {...} }
  }
})
</script>
```
