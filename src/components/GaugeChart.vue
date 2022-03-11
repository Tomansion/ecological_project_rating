<template>
  <div id="GaugeChart"></div>
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
    this.$parent.$on("update", this.updateGauge);
    this.updateGauge();
  },
  methods: {
    updateGauge() {
      const projectTotal = (project) => {
        return project.values.reduce((score, value, i) => {
          return score + value * this.criteriaList[i].coef;
        }, 0);
      };

      const layout = {
        margin: {
          l: 20,
          r: 40,
          b: 20,
          t: 20,
          pad: 0,
        },
      };

      const maxScore =
        this.criteriaList.reduce((max, criteria) => max + criteria.coef, 0) *
        10;

      if (this.projects.length === 1) {
        // Gauge plot
        const projectScore = (projectTotal(this.projects[0]) * 20) / maxScore;

        const data = [
          {
            domain: { x: [0, 1], y: [0, 1] },
            value: projectScore,
            type: "indicator",
            mode: "gauge+number",
            gauge: {
              axis: { range: [null, 20] },
              bar: { color: "#0274be" },
              // steps: [
              //   { color: "green", range: [10, 20] },
              // ],
            },
          },
        ];

        Plotly.react("GaugeChart", data, layout, {
          displayModeBar: false,
          responsive: true,
        });
      } else {
        // Vertical bar plot

        const data = this.projects.map((project) => {
          const projectScore = (projectTotal(project) * 20) / maxScore;

          return {
            name: project.name,
            type: "bar",
            x: [project.name],
            y: [projectScore],
            text: [parseFloat(projectScore).toFixed(1)],
            tooltip: false,
            hoverinfo: "none",
          };
        });

        layout.yaxis = {
          range: [0, 20],
          showgrid: false,
          ticks: "",
          zeroline: false,
        };

        Plotly.react("GaugeChart", data, layout, {
          displayModeBar: false,
          responsive: true,
        });
      }
    },
  },
  watch: {
    criteriaValues: {
      handler() {
        this.updateGauge();
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
#GaugeChart {
  width: 100%;
  height: 100%;
}
</style>