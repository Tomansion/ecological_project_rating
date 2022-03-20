<template>
  <div id="RadarChart"></div>
</template>

<script>
import Plotly from "plotly.js-dist-min";

export default {
  props: {
    criteriaList: {
      type: Array,
      required: true,
    },
    projects: {
      type: Array,
      required: true,
    },
  },
  mounted() {
    this.$parent.$on("update", this.updateRadar);
    this.updateRadar();
  },
  methods: {
    updateRadar() {
      // Find the max coef
      let maxCriteriaCoef =
        this.criteriaList.reduce(
          (max, criteria) => Math.max(max, criteria.coef),
          0
        ) * 10;

      // contruct the plotly radar plot
      let data = this.projects.map((project) => {
        return {
          type: "scatterpolar",
          name: project.name,
          r: [
            ...project.values.map((v, i) => v * this.criteriaList[i].coef),
            // Add value to the end to loop the radar
            project.values[0] * this.criteriaList[0].coef,
          ],
          theta: [
            ...this.criteriaList.map((criteria) => criteria.name),
            // Add value to the end to loop the radar
            this.criteriaList[0].name,
          ],
          fill: "toself",
          line: {
            width: 3,
          },
          text: project.values.map(
            (v, i) =>
              this.criteriaList[i].name + ": " + v * this.criteriaList[i].coef
          ),
          hoverinfo: "text",
        };
      });

      let layout = {
        polar: {
          radialaxis: {
            range: [0, maxCriteriaCoef],
            linewidth: 0,
            dtick: maxCriteriaCoef / 2,
          },
          angularaxis: {
            tickfont: { size: 9},
            rotation: 90,
            direction: "clockwise",
          },
        },

        margin: {
          l: 150,
          r: 150,
          b: 100,
          t: 100,
          pad: 0,
        },
        legend: { orientation: "h" },
      };

      Plotly.react("RadarChart", data, layout, {
        displayModeBar: false,
        responsive: true,
      });
    },
  },
};
</script>

<style scoped>
#RadarChart {
  width: 100%;
  height: 100%;
  min-height: 400px;
}
</style>