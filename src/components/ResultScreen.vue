<template>
  <div class="container">
    <p>CONGRATULATIONS</p>
    <p class="time">
      {{ time }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    time: String,
  },
};
</script>
<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
p {
  font-size: 24px;
}
</style>
