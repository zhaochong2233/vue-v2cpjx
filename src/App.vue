<template>
  <div id="app">
    <dragArea @selected="log">
      <div class="container">
        <div class="box"></div>
        <div class="box"></div>
        <div class="box"></div>
      </div>
    </dragArea>
  </div>
</template>

<script>
import dragArea from './components/drag.vue';

export default {
  name: 'App',
  components: {
    dragArea,
  },
  methods: {
    log(val) {
      console.log(val);
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  height: 100vh;
}
.box {
  width: 90px;
  height: 90px;
  background-image: linear-gradient(120deg, #a1c4fd 0%, #c2e9fb 100%);
}
.selected {
  border: 1px solid red;
}
</style>
