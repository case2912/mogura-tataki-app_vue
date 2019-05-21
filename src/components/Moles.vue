<template>
  <div class="mole-container">
    <Mole
      :key="index"
      v-for="(moleState,index) in moleData"
      :active="moleState"
      :moleId="index"
      v-on:whack="handleWhack"
      v-on:miss="handleMiss"
    />
  </div>
</template>

<script>
import Mole from "./Mole";
export default {
  name: "Moles",
  props: ["moleData"],
  components: {
    Mole
  },
  methods: {
    handleWhack: function(moleId) {
      this.$emit("whack", moleId);
    },
    handleMiss: function() {
      this.$emit("miss");
    }
  }
};
</script>
<style>
.mole-container {
  flex-direction: row;
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 1;
  min-height: 600px;
}
</style>


