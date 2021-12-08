<template>
  <div id="RadarChart"></div>
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
    this.updateRadar();
  },
  methods: {
    updateRadar() {
      // Find the max coef
      let max =
        Math.max.apply(
          Math,
          this.criteriaValues.map((c) => c.coef)
        ) * 10;

      let data = [
        {
          type: "scatterpolar",
          r: this.criteriaValues.map(
            (criteria) => criteria.value * criteria.coef
          ),
          theta: this.criteriaValues.map((criteria) => criteria.name),
          fill: "toself",
        },
      ];

      // Add value to the end to loop the radar
      if (this.criteriaValues.length > 0) {
        data[0].r.push(this.criteriaValues[0].value * this.criteriaValues[0].coef);
        data[0].theta.push(this.criteriaValues[0].name);
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
            dtick: max/2 ,
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
  watch: {
    criteriaValues: {
      handler() {
        this.updateRadar();
      },
      deep: true,
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