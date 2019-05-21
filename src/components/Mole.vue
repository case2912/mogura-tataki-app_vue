<template>
  <div class="mole-actions-warrper">
    <div class="mole-actions" @click="handleContainerClick">
      <div class="dirt">
        <img src="@/assets/dirt.svg">
      </div>
      <img :class="classNamesMole" src="@/assets/mole.png" @click="hundleClick">
    </div>
  </div>
</template>

<script>
export default {
  name: "Mole",
  props: ["active", "moleId"],
  computed: {
    classNamesMole: function() {
      return {
        active: this.active,
        mole: true
      };
    }
  },
  methods: {
    hundleClick: function() {
      console.log("whack");
      this.$emit("whack", this.moleId);
    },
    handleContainerClick: function(evt) {
      if (evt.target.className === "mole") {
        return;
      }
      console.log("miss");
      this.$emit("miss");
    }
  }
};
</script>
<style>
.mole-actions-warrper {
  margin: 10px;
}
.mole-actions img {
  transition: all 0.3s ease;
}
.mole-actions {
  overflow: hidden;
  display: block;
  position: relative;
  height: 300px;
}
.mole {
  position: absolute;
  margin-top: 10px;
  margin-bottom: 10px;
  margin-right: auto;
  margin-left: auto;
  width: 150px;
  top: 400px;
  right: 0;
  bottom: 0;
  left: 0;
}
.active {
  top: 0px;
}
.dirt {
  z-index: 1;
  position: relative;
  margin-top: 200px;
  width: 300px;
}
</style>

