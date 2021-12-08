<template>
  <div id="app">
    <div class="section">

    </div>
    <div class="section">

    </div>
    <div class="section">

    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
};
</script>

<style>
html, body {
  height: 100%;
  margin: 0px;
}

#app {
  display: flex;
  justify-content: space-between;
  width: 100vw;
  height: 100vh;
  background-color: cadetblue;
}
.section {
  border: solid 1px #ccc;
  flex: 1;
}

/* Change the sections directions when screen is on portrait mode */
@media screen and (orientation: portrait) {
  #app {
    flex-direction: column;
  }
}
</style>
