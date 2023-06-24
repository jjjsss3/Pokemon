<template>
  <Transition name="interact">
    <main-screen v-if="status === 'lobby'" @selectMode="selectMode" />
  </Transition>

  <Transition name="interact">
    <interact-screen
      v-if="status === 'match'"
      :mode="mode"
      @selectMode="selectMode"
    />
  </Transition>
  <Transition name="interact">
    <result-screen
      v-if="status === 'finished'"
      @selectMode="selectMode"
      :time="time"
      :step="step"
    />
  </Transition>
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
