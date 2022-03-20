<template>
  <div id="GaugeChart">
    <div id="plotSection">
      <div id="plot"></div>
    </div>
    <div id="projectGrades">
      <div
        :class="'grade p' + grade.projectIndex"
        v-for="grade in grades"
        v-bind:key="grade.projectIndex"
      >
        {{
          parseInt(grade.grade) - grade.grade === 0
            ? grade.grade
            : grade.grade.toFixed(2)
        }}
      </div>
    </div>
  </div>
</template>

<script>
import Plotly from "plotly.js-dist-min";

export default {
  data() {
    return {
      grades: [],
    };
  },
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
          l: 30,
          r: 30,
          b: 20,
          t: 0,
          pad: 0,
        },
        showlegend: false,
      };

      const maxScore =
        this.criteriaList.reduce((max, criteria) => max + criteria.coef, 0) *
        10;

      const colors = ["#1f77b4", "#ff7f0e", "#2ca02c", "#d62728", "#9467bd"];

      let steps = this.projects.map((project, i) => {
        const projectScore = (projectTotal(project) * 20) / maxScore;
        return {
          range: [0, projectScore],
          color: colors[i],
          projectIndex: i,
        };
      });

      const bestProjectScore = steps.reduce((max, step) => {
        return Math.max(max, step.range[1]);
      }, 0);

      const bestProjectIndexs = steps
        .filter((step) => step.range[1] === bestProjectScore)
        .map((step) => step.projectIndex);

      // Sort the steps by score
      steps.sort((a, b) => b.range[1] - a.range[1]);

      // Change the thickness if same grade
      steps = steps.map((step, i) => {
        step.thickness = 0.8;
        if (i > 0 && step.range[1] === steps[i - 1].range[1])
          step.thickness = steps[i - 1].thickness / 2;
        return step;
      });

      const data = [
        {
          type: "indicator",
          // mode: "gauge+number+delta",
          mode: "gauge+number",
          value: bestProjectScore,
          // delta: { reference: 10, increasing: { color: "RebeccaPurple" } },
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
            borderwidth: 1,
            bordercolor: "black",
            steps,
            threshold: {
              line: { color: colors[bestProjectIndexs[0]], width: 2 },
              thickness: bestProjectIndexs.length == 1 ? 0.9 : 0,
              value: bestProjectScore,
            },
          },
        },
      ];

      // Display the other best project
      this.grades = [];
      steps.forEach((step) => {
        if (
          (bestProjectIndexs.length == this.projects.length &&
            this.projects.length > 1) ||
          !bestProjectIndexs.includes(step.projectIndex)
        )
          this.grades.push({
            projectIndex: step.projectIndex,
            grade: step.range[1],
          });
      });

      layout.font = {
        color:
          bestProjectIndexs.length == 1
            ? colors[bestProjectIndexs[0]]
            : "black",
      };

      Plotly.react("plot", data, layout, {
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
  display: flex;
  flex-direction: column;
  justify-content: center;
}

#plotSection {
  flex: 1;
  width: 100%;
  max-height: 50%;
}

#projectGrades {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 70px;
  z-index: 1;
}

@media screen and (orientation: portrait) {
  #plotSection {
    min-height: 400px;
  }
  #projectGrades {
    min-height: 150px;
  }
}

.grade {
  font-size: 2em;
  font-weight: bold;
}

.grade.p0 {
  color: #1f77b4;
}
.grade.p1 {
  color: #ff7f0e;
}
.grade.p2 {
  color: #2ca02c;
}
.grade.p3 {
  color: #d62728;
}
.grade.p4 {
  color: #9467bd;
}
</style>