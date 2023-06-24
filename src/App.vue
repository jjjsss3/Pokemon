<template>
  <main-screen v-if="status === 'lobby'" @selectMode="selectMode" />
  <Transition name="interact">
    <interact-screen
      v-if="status === 'match'"
      :mode="mode"
      @selectMode="selectMode"
    />
  </Transition>
  <result-screen v-if="status === 'finished'" :time="time" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      status: "lobby",
      mode: "",
      time: "",
    };
  },
  methods: {
    selectMode(value) {
      if (value[0] == "finished") {
        this.time = value[1];
        this.status = "finished";
      } else if (value[0] == "match") {
        this.mode = value[1];
        this.status = "match";
      }
    },
  },
};
</script>
