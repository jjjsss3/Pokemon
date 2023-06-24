<template>
  <div class="copyright">
    <p>This game is owner by Vy H Hoang -</p>
    <a href="https://github.com/jjjsss3/pokemon" target="_blank">
      View source</a
    >
  </div>
</template>
<script>
export default {
  data() {
    return {
      blank: " ",
    };
  },
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Oswald:wght@500&display=swap");
.copyright {
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
  font-style: italic;
  height: 5vh;
}
.copyright a {
  /* font-weight: 800; */
  color: #deacf5;
  text-decoration: none;
  margin-left: 5px;
}
a,
p {
  font-family: "Poppins", sans-serif;
}
@media only screen and (max-width: 600px) {
  a,
  p {
    font-size: 12px;
  }
}
</style>
