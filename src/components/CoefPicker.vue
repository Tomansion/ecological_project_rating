<template>
  <div id="CoefPicker">
    <div id="criteriaList">
      <div
        class="criteria"
        v-for="(criteria, i) in criteriaList"
        v-bind:key="i"
      >
        <span class="criteriaName">
          {{ fixName(criteria.name) }}
        </span>
        <div class="criteriaControl" :id="'project_' + selectedProjectNb">
          <input
            type="number"
            min="0"
            step="1"
            v-model="criteriaCoef[i]"
            @input="(v) => updateCoef(i)"
          />
          <div class=".slider-styled slider-round" :id="'slider_' + i" />
        </div>
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
    selectedProjectNb: { type: Number, requiered: true },
  },
  data() {
    return {
      criteriaCoef: Array(this.criteriaList.length).fill(1),
      criteriaEditNb: null,
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
    fixName(name) {
      return name.replace("<br>", " ");
    },
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
  min-height: 400px;
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
.criteriaName {
  font-size: 0.9em;
  padding: 10px;
}
.criteriaControl {
  display: grid;
  grid-template-columns: 1fr 10fr;
  align-items: center;
}

.criteriaControl input ::-webkit-inner-spin-button,
.criteriaControl input ::-webkit-outer-spin-button {
  opacity: 1;
}

.criteriaControl input {
  width: 35px;
  height: 22px;
  margin-right: 10px;
  padding-left: 14px;
  border: 1px solid rgb(206, 206, 206);
  border-radius: 6px;
}

.criteriaControl input:focus {
  outline: 0;
}
</style>
<style>
.noUi-handle {
  border-radius: 10px;
  cursor: pointer;
  box-shadow: inset 0 0 1px #fff;
}
/* Disable vertical white box */
.noUi-handle::before,
.noUi-handle::after {
  display: none;
}

.noUi-horizontal .noUi-tooltip {
  background-color: #21486100;
  color: white;
  border: none;
  font-size: 0.8em;
  font-weight: bold;
  bottom: 0px;
  z-index: 2;
}

#project_0 .noUi-connect {
  background: #1f77b4;
}
#project_1 .noUi-connect {
  background: #ff7f0e;
}
#project_2 .noUi-connect {
  background: #2ca02c;
}
#project_3 .noUi-connect {
  background: #d62728;
}
#project_4 .noUi-connect {
  background: #9467bd;
}
#project_0 .noUi-handle {
  background: #1f77b4;
}
#project_1 .noUi-handle {
  background: #ff7f0e;
}
#project_2 .noUi-handle {
  background: #2ca02c;
}
#project_3 .noUi-handle {
  background: #d62728;
}
#project_4 .noUi-handle {
  background: #9467bd;
}
</style>