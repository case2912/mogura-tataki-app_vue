<template>
  <v-card class="counter">
    <v-card-title primary-title>
      <h1>{{label}} : {{value}}</h1>
    </v-card-title>
  </v-card>
</template>

<script>
export default {
  name: "Counter",
  props: ["label", "value"]
};
</script>
<style>
.counter {
  width: 20vw;
  height: 10vh;
  min-height: 100px;
  margin: 10px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
}
</style>

