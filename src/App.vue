<template>
  <div id="app">
    <div id="top">
      <ProjectPicker
        :projects="projects"
        :selectedProjectNb="selectedProjectNb"
        v-on:newProject="newProject"
        v-on:selectProject="projectSelected"
        v-on:updateProjectName="$emit('update')"
      />
    </div>
    <div id="bottom">
      <div class="section" id="CoefPicker">
        <CoefPicker
          :criteriaList="criteriaList"
          :project="projects[selectedProjectNb]"
          :selectedProjectNb="selectedProjectNb"
          v-on:updateCriteriaCoef="updateCriteriaCoef"
          v-on:updateProjectValue="updateProjectValue"
        />
      </div>
      <div class="section" id="Radar">
        <RadarChart :criteriaList="criteriaList" :projects="projects" />
      </div>
      <div class="section" id="Gauge">
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
        { name: "Auto-organisation des écosystèmes", coef: 1 },
        { name: "Travaux exploitables par la Recherche", coef: 1 },
        { name: "Approche systémique", coef: 1 },
        { name: "Préserver les ressources non renouvelables", coef: 1 },
        { name: "Préserver et restaurer les écosystèmes", coef: 1 },
      ],
      projects: [
        {
          name: "Projet 1",
          values: defaultCriteria
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
  flex: 1;
}
.section {
  flex: 1;
}
#CoefPicker {
  flex: 0.7;
  max-width: 400px;
}
#Gauge {
  flex: 0.7;
  max-width: 600px;
}

/* Change the sections directions when screen is on portrait mode */
@media screen and (orientation: portrait) {
  #app {
    flex-direction: column;
  }
}
</style>
