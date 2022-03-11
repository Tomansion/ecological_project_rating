<template>
  <div id="app">
    <div id="top">
      <ProjectPicker />
    </div>
    <div id="bottom">
      <div class="section">
        <CoefPicker
          :nbCriteria="nbCriteria"
          v-on:updateCriteria="updateCriteria"
        />
      </div>
      <div class="section">
        <RadarChart
          v-if="projects[selectedProjectNb].criteriaValues"
          :criteriaValues="projects[selectedProjectNb].criteriaValues"
        />
      </div>
      <div class="section">
        <GaugeChart
          v-if="projects[selectedProjectNb].criteriaValues"
          :criteriaValues="projects[selectedProjectNb].criteriaValues"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ProjectPicker from "./components/ProjectPicker.vue";
import CoefPicker from "./components/CoefPicker.vue";
import RadarChart from "./components/RadarChart.vue";
import GaugeChart from "./components/GaugeChart.vue";

export default {
  name: "App",
  components: { CoefPicker, RadarChart, GaugeChart, ProjectPicker },
  data() {
    return {
      nbCriteria: 5,
      projects: [
        {
          name: "Project",
          criteriaValues: null,
        },
      ],
      selectedProjectNb: 0,
    };
  },
  methods: {
    updateCriteria(criteriaValues) {
      this.projects[this.selectedProjectNb].criteriaValues = criteriaValues;
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
  flex:1;
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
