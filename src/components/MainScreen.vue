<template>
  <div class="screen">
    <p class="title">POKEMON MEMORY</p>
    <p class="select">SELECT MODE TO PLAY</p>
    <div class="action">
      <button
        v-for="button in buttons"
        :key="button.ref"
        @click="selectMode(['match', button.ref])"
        class="btn-mode"
      >
        <span> {{ button.name }}</span>
        <span> {{ button.description }}</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttons: [
        { ref: "4", name: "4x4", description: "Easy" },
        { ref: "6", name: "6x6", description: "Medium" },
        { ref: "8", name: "8x8", description: "Hard" },
        {
          ref: "10",
          name: "10x10",
          description: "Super Hard",
        },
      ],
    };
  },
  methods: {
    selectMode(value) {
      this.$emit("selectMode", value);
    },
  },
};
</script>

<style scoped>
.screen {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  min-height: 100vh;
}
.screen .title {
  font-size: 50px;
}
.screen .select {
  margin-top: 20px;
  font-size: 20px;
}
.btn-mode {
  background-color: transparent;
  border: none;
  margin: 7px;
  width: 150px;
  height: 150px;
  border: 1px solid #fff;
  cursor: pointer;
  transition: 0.5s;
}
.btn-mode:hover {
  background: #eee;
  color: #28104e;
}
.action {
  padding: 10px;
}
.btn-mode span {
  display: flex;
  justify-content: center;
  padding: 5px;
  margin-left: 5px;
  color: #eee;
  font-size: 20px;
}
.btn-mode:hover span {
  color: #28104e;
}
</style>
