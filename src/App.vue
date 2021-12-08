<template>
  <div id="app">
    <div class="section">
      <CoefPicker
        :nbCriteria="nbCriteria"
        v-on:updateCriteria="updateCriteria"
      />
    </div>
    <div class="section"></div>
    <div class="section"></div>
  </div>
</template>

<script>
import CoefPicker from "./components/CoefPicker.vue";

export default {
  name: "App",
  components: { CoefPicker },
  data() {
    return {
      nbCriteria: 5,
      criteriaValues: null,
    };
  },
  methods: {
    updateCriteria(criteriaValues) {
      console.log(criteriaValues);
      this.criteriaValues = criteriaValues;
    },
  },
};
</script>

<style>
html,
body {
  height: 100%;
  margin: 0px;
  font-family: sans-serif;
}

#app {
  display: flex;
  justify-content: space-between;
  width: 100vw;
  height: 100vh;
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
