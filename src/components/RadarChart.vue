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
    projectValues: {
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
      let max =
        Math.max.apply(
          Math,
          this.criteriaList.map((c) => c.coef)
        ) * 10;

      // contruct the plotly radar plot
      let data = [
        {
          type: "scatterpolar",
          r: this.projectValues.map((v, i) => v * this.criteriaList[i].coef),
          theta: this.criteriaList.map((criteria) => criteria.name),
          fill: "toself",
        },
      ];

      // Add value to the end to loop the radar
      if (this.projectValues.length > 0) {
        data[0].r.push(this.projectValues[0] * this.criteriaList[0].coef);
        data[0].theta.push(this.criteriaList[0].name);
      }

      let layout = {
        polar: {
          radialaxis: {
            visible: true,
            range: [0, max],
            colcolor: "gray",
            linewidth: 0,
            tickmode: "linear",
            tick0: 0,
            dtick: max / 2,
          },
        },

        margin: {
          l: 20,
          r: 20,
          b: 20,
          t: 20,
          pad: 0,
        },

        showlegend: false,
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
}
</style>