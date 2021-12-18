<template>
  <div id="GaugeChart"></div>
</template>

<script>
import Plotly from "plotly.js-dist-min";

export default {
  props: {
    criteriaValues: {
      type: Array,
      required: true,
    },
  },
  mounted() {
    this.updateGauge();
  },
  methods: {
    updateGauge() {
      let maxScore =
        this.criteriaValues.reduce((max, current) => {
          return max + current.coef;
        }, 0) * 10;

      let score = this.criteriaValues.reduce((score, current) => {
        return score + current.coef * current.value;
      }, 0);

      var data = [
        {
          domain: { x: [0, 1], y: [0, 1] },
          value: (score * 20) / maxScore,
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

      var layout = {
        margin: {
          l: 20,
          r: 40,
          b: 20,
          t: 20,
          pad: 0,
        },
      };

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