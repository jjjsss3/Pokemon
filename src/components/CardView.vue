<template>
  <div class="card-container">
    <img :src="getImageUrl(src)" alt="" />
  </div>
</template>
<script>
export default {
  props: {
    src: String,
  },
  data() {
    return {
      styles: {
        width: this.src == "icon_back" ? "50%" : "75%",
      },
    };
  },
  methods: {
    getImageUrl(imageId) {
      return require(`@/assets/images/${imageId}.png`);
    },
  },
};
</script>

<style scoped>
.card-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  /* background-color: #d4d4d4; */
  border-radius: 10px;
  /* box-shadow: rgba(121, 121, 121, 0.3) 0px 7px 7px, */
  /* rgba(63, 63, 63, 0.22) 0px 7px 7px; */
}

.container img {
  width: v-bind("styles.width");
}
</style>
