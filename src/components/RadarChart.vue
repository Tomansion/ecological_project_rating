<template>
  <div id="RadarChart"></div>
</template>

<script>
import Plotly from 'plotly.js-dist-min'

export default {
  props: {
    criteriaValues: {
      type: Array,
      required: true,
    },
  },
  mounted() {
    let data = [
      {
        type: "scatterpolar",

        r: [39, 28, 8, 7, 28, 39],

        theta: ["A", "B", "C", "D", "E", "A"],

        fill: "toself",
      },
    ];

    let layout = {
      polar: {
        radialaxis: {
          visible: true,

          range: [0, 50],
        },
      },

      showlegend: false,
    };

    Plotly.newPlot("RadarChart", data, layout);
  },
};
</script>

<style>
</style>