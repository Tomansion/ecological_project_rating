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
          l: 27,
          r: 50,
          b: 80,
          t: 10,
          pad: 0,
        },
        showlegend: false,
      };

      const maxScore =
        this.criteriaList.reduce((max, criteria) => max + criteria.coef, 0) *
        10;

      const colors = ["#1f77b4", "#ff7f0e", "#2ca02c", "#d62728", "#9467bd"];

      // Gauge plot
      // {
      //   domain: { x: [0, 1], y: [0, 1] },
      //   value: projectScore,
      //   type: "indicator",
      //   mode: "gauge+number",
      //   gauge: {
      //     axis: { range: [null, 20] },
      //     bar: { color: "#0274be" },
      //   },
      // },

      let steps = this.projects.map((project, i) => {
        const projectScore = (projectTotal(project) * 20) / maxScore;
        return {
          range: [0, projectScore],
          color: colors[i],
        };
      });

      const bestProjectScore = steps.reduce((max, step) => {
        return Math.max(max, step.range[1]);
      }, 0);

      console.log(bestProjectScore);

      const bestProjectIndexs = steps.filter(
        (step) => step.range[1] === bestProjectScore
      ).map((step) => steps.indexOf(step));

      console.log(bestProjectIndexs);

      // Sort the steps by score
      steps.sort((a, b) => b.range[1] - a.range[1]);
      // steps = steps.map((step, i) => {
      //   step.thickness = 1 - i * 0.15;
      //   return step;
      // });
      steps = steps.map((step, i) => {
        step.thickness = 0.8;
        if (i > 0 && step.range[1] === steps[i - 1].range[1]) {
          step.thickness = steps[i - 1].thickness / 2;
        }
        return step;
      });

      const data = [
        {
          type: "indicator",
          // mode: "gauge+number+delta",
          mode: "gauge+number",
          value: bestProjectScore,
          // title: { text: "Speed", font: { size: 24 } },
          // delta: { reference: 400, increasing: { color: "RebeccaPurple" } },
          gauge: {
            axis: {
              range: [null, 20],
              tickwidth: 1,
              // tickcolor: "darkblue",
            },
            bar: {
              color: "rgba(0, 0, 0, 0)", // Hide the bar
            },
            bgcolor: "lightgray",
            borderwidth: 2,
            bordercolor: "black",
            steps,
            threshold: {
              line: { color: colors[bestProjectIndexs[0]], width: 4 },
              thickness: bestProjectIndexs.length == 1 ? 1 : 0,
              value: bestProjectScore,
            },
          },
        },
      ];

      Plotly.react("GaugeChart", data, layout, {
        displayModeBar: false,
        responsive: true,
      });
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