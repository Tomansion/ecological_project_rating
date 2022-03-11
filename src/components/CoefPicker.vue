<template>
  <div id="CoefPicker">
    <div id="projectName">
      <input type="text" v-model="project.name"  @change="$emit('updateProjectName')" />
    </div>
    <div id="criteriaList">
      <div
        class="criteria"
        v-for="(criteria, i) in criteriaList"
        v-bind:key="i"
      >
        {{ criteria.name }}
        <input
          type="number"
          min="0"
          step="1"
          title="Coeficient"
          v-model="criteriaCoef[i]"
          @input="(v) => updateCoef(i)"
        />
        <div class=".slider-styled slider-round" :id="'slider_' + i" />
      </div>
    </div>
  </div>
</template>

<script>
import noUiSlider from "nouislider";
import "nouislider/dist/nouislider.css";

export default {
  props: {
    criteriaList: { type: Array, requiered: true },
    project: { type: Object, requiered: true },
  },
  data() {
    return {
      criteriaCoef: [1, 1, 1, 1, 1],
    };
  },
  mounted() {
    // Create the sliders
    this.criteriaList.forEach((criteria, i) => {
      let slider = document.getElementById("slider_" + i);
      noUiSlider.create(slider, {
        start: this.project.values[i],
        connect: [true, false],
        step: 1,
        range: {
          min: 0,
          max: 10,
        },
        tooltips: true,
        format: {
          from: function (value) {
            return parseInt(value);
          },
          to: function (value) {
            return parseInt(value);
          },
        },
      });

      // Bind the color changing function to the update event.
      slider.noUiSlider.on("change", () => {
        this.$emit("updateProjectValue", {
          criteriaNb: i,
          value: parseInt(slider.noUiSlider.get()),
        });
      });

      // Listen for project change
      this.$parent.$on("update", () => {
        slider.noUiSlider.set([this.project.values[i]]);
      });
    });
  },
  methods: {
    updateCoef(i) {
      // Convert the input value to a number.
      let newInput = parseFloat(this.criteriaCoef[i]);
      if (isNaN(newInput)) newInput = 0;
      if (newInput < 0) newInput = 0;
      if (newInput > 100) newInput = 100;

      this.$emit("updateCriteriaCoef", { criteriaNb: i, coef: newInput });
    },
    updateValues() {},
  },
};
</script>

<style scoped>
#CoefPicker {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}
#projectName {
  padding-bottom: 10%;
}
#criteriaList {
  height: 70%;
  width: 90%;
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: repeat(nbCriteria, minmax(min-content, max-content));
  align-items: center;
}
.criteria {
  display: grid;
  grid-template-columns: 1fr 2fr 10fr;
  align-items: center;
}

.criteria input ::-webkit-inner-spin-button,
.criteria input ::-webkit-outer-spin-button {
  opacity: 1;
}

.criteria input {
  width: 35px;
  height: 22px;
  margin-right: 10px;
  padding-left: 14px;
  border: 1px solid rgb(206, 206, 206);
  border-radius: 6px;
}

.criteria input:focus {
  outline: 0;
}
</style>
<style>
.noUi-handle {
  border-radius: 10px;
  cursor: pointer;
  box-shadow: inset 0 0 1px #fff;
  background-color: #0274be;
}
.noUi-connect {
  background: #79aed1;
}
.noUi-tooltip {
  color: #0274be;
  font-size: 0.7em;
}
</style>