<template>
  <div id="app">
    <div id="top">
      <ProjectPicker :projects="projects" />
    </div>
    <div id="bottom">
      <div class="section">
        <CoefPicker
          :criteriaList="criteriaList"
          :projectValues="projects[selectedProjectNb].values"
          v-on:updateCriteriaCoef="updateCriteriaCoef"
          v-on:updateProjectValue="updateProjectValue"
        />
      </div>
      <div class="section">
        <RadarChart
          :criteriaList="criteriaList"
          :projectValues="projects[selectedProjectNb].values"
        />
      </div>
      <div class="section">
        <!-- <GaugeChart
          v-if="projects[selectedProjectNb].values"
          :projectValues="projects[selectedProjectNb].values" -->
        <!-- /> -->
      </div>
    </div>
  </div>
</template>

<script>
import ProjectPicker from "./components/ProjectPicker.vue";
import CoefPicker from "./components/CoefPicker.vue";
import RadarChart from "./components/RadarChart.vue";
// import GaugeChart from "./components/GaugeChart.vue";

export default {
  name: "App",
  components: {
    CoefPicker,
    RadarChart,
    // GaugeChart,
    ProjectPicker,
  },
  data() {
    const nbCriteria = 5;
    const defaultValue = 5;
    let defaultCriteria = new Array(nbCriteria).fill(defaultValue);

    return {
      nbCriteria,
      criteriaList: [
        { name: "A", coef: 1 },
        { name: "B", coef: 1 },
        { name: "C", coef: 1 },
        { name: "D", coef: 1 },
        { name: "E", coef: 1 },
      ],
      projects: [
        {
          name: "Project 1",
          values: defaultCriteria,
        },
      ],
      selectedProjectNb: 0,
    };
  },
  methods: {
    updateCriteriaName({ name, criteriaNb }) {
      this.criteriaList[criteriaNb].name = name;
      this.$emit("update")

    },
    updateCriteriaCoef({ coef, criteriaNb }) {
      this.criteriaList[criteriaNb].coef = coef;
      this.$emit("update")
    },
    updateProjectValue({ value, criteriaNb }) {
      this.projects[this.selectedProjectNb].values[criteriaNb] = value;
      this.$emit("update")
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
  flex-direction: column;
  width: 100vw;
  background-color: white;
  height: 100vh;
}

#bottom {
  display: flex;
  justify-content: space-between;
  flex: 1;
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
