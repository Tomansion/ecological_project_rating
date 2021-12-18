<template>
  <div id="app">
    <div class="section">
      <CoefPicker
        :nbCriteria="nbCriteria"
        v-on:updateCriteria="updateCriteria"
      />
    </div>
    <div class="section">
      <RadarChart v-if="criteriaValues" :criteriaValues="criteriaValues" />
    </div>
    <div class="section">
      <GaugeChart v-if="criteriaValues" :criteriaValues="criteriaValues" />
    </div>
  </div>
</template>

<script>
import CoefPicker from "./components/CoefPicker.vue";
import RadarChart from "./components/RadarChart.vue";
import GaugeChart from "./components/GaugeChart.vue";

export default {
  name: "App",
  components: { CoefPicker, RadarChart, GaugeChart },
  data() {
    return {
      nbCriteria: 5,
      criteriaValues: null,
    };
  },
  methods: {
    updateCriteria(criteriaValues) {
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
  background-color: white;
}

#app {
  display: flex;
  justify-content: space-between;
  width: 100vw;
  height: 100vh;
}
.section {
  flex: 1;
}

/* Change the sections directions when screen is on portrait mode */
@media screen and (orientation: portrait) {
  #app {
    flex-direction: column;
  }
}
</style>
