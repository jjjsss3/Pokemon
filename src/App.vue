<template>
  <main-screen
    class="screen"
    v-if="status === 'lobby'"
    @selectMode="selectMode"
  />
  <interact-screen
    class="screen"
    v-if="status === 'match'"
    :mode="mode"
    @selectMode="selectMode"
  />
  <result-screen
    class="screen"
    v-if="status === 'finished'"
    @selectMode="selectMode"
    :time="time"
    :step="step"
  />
  <copy-right-screen class="fixed-bottom" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  data() {
    return {
      status: "lobby",
      mode: "",
      time: "",
      step: 0,
    };
  },
  methods: {
    selectMode(value) {
      if (value[0] == "finished") {
        this.time = value[1];
        this.step = value[2];
        this.status = "finished";
      } else if (value[0] == "match") {
        this.mode = value[1];
        this.status = "match";
      } else if (value[0] == "lobby") {
        this.status = "lobby";
      }
    },
  },
};
</script>
