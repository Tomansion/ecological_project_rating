<template>
  <div id="app">
    <div id="top">
      <ProjectPicker
        :projects="projects"
        :selectedProjectNb="selectedProjectNb"
        v-on:newProject="newProject"
        v-on:selectProject="projectSelected"
      />
    </div>
    <div id="bottom">
      <div class="section" id="CoefPicker">
        <CoefPicker
          :criteriaList="criteriaList"
          :project="projects[selectedProjectNb]"
          v-on:updateCriteriaCoef="updateCriteriaCoef"
          v-on:updateProjectValue="updateProjectValue"
          v-on:updateProjectName="$emit('update')"
        />
      </div>
      <div class="section">
        <RadarChart :criteriaList="criteriaList" :projects="projects" />
      </div>
      <div class="section">
        <GaugeChart :criteriaList="criteriaList" :projects="projects" />
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
  components: {
    CoefPicker,
    RadarChart,
    GaugeChart,
    ProjectPicker,
  },
  data() {
    const nbCriteria = 5;
    const defaultValue = 5;
    let defaultCriteria = new Array(nbCriteria).fill(defaultValue);

    return {
      defaultValue,
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
    newProject(name) {
      this.projects.push({
        name,
        values: new Array(this.nbCriteria).fill(this.defaultValue),
      });
      this.projectSelected(this.projects.length - 1);
    },
    projectSelected(projectNb) {
      this.selectedProjectNb = projectNb;
      this.$nextTick(() => {
        this.$emit("update");
      });
    },
    updateCriteriaName({ name, criteriaNb }) {
      this.criteriaList[criteriaNb].name = name;
      this.$emit("update");
    },
    updateCriteriaCoef({ coef, criteriaNb }) {
      this.criteriaList[criteriaNb].coef = coef;
      this.$emit("update");
    },
    updateProjectValue({ value, criteriaNb }) {
      this.projects[this.selectedProjectNb].values[criteriaNb] = value;
      this.$emit("update");
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
#CoefPicker {
  flex: 0.8;
}

/* Change the sections directions when screen is on portrait mode */
@media screen and (orientation: portrait) {
  #app {
    flex-direction: column;
  }
}
</style>
