<template>
  <div id="CoefPicker">
    <div id="criteriaList">
      <div class="criteria" v-for="i in nbCriteria" v-bind:key="i">
        {{ String.fromCharCode(64 + i) }}
        <input
          type="number"
          min="0"
          step="1"
          title="Coeficient"
          v-model="criteriaValues[i - 1].coef"
          @input="(v) => updateCoef(i - 1)"
        />
        <div :id="'slider_' + i" />
      </div>
    </div>
  </div>
</template>

<script>
import noUiSlider from "nouislider";
import "nouislider/dist/nouislider.css";

export default {
  props: {
    nbCriteria: { type: Number, requiered: true },
  },
  data() {
    return {
      criteriaValues: null,
    };
  },
  created() {
    this.criteriaValues = [];
    for (let i = 0; i < this.nbCriteria; i++)
      this.criteriaValues.push({
        coef: 1,
        value: 5,
        name: String.fromCharCode(65 + i),
      });
    this.$emit("updateCriteria", this.criteriaValues);
  },
  mounted() {
    for (let i = 1; i < this.nbCriteria + 1; i++) {
      let slider = document.getElementById("slider_" + i);
      noUiSlider.create(slider, {
        start: [5],
        connect: true,
        step: 0.1,
        range: {
          min: 0,
          max: 10,
        },
      });

      // Bind the color changing function to the update event.
      slider.noUiSlider.on("update", () => {
        this.criteriaValues[i - 1].value = parseInt(slider.noUiSlider.get());
        this.$emit("updateCriteria", this.criteriaValues);
      });
    }
  },
  methods: {
    updateCoef(index) {
      // Convert the input value to a number.
      this.criteriaValues[index].coef = parseInt(
        this.criteriaValues[index].coef
      );
      this.$emit("updateCriteria", this.criteriaValues);
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

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type="number"] {
  -moz-appearance: textfield;
}

.criteria input {
  width: 25px;
  height: 22px;
  margin-right: 10px;
  padding-left: 14px;
  border: 1px solid rgb(206, 206, 206);
  border-radius: 6px;
}

.criteria input:focus {
  outline: 0;
}

.criteria-nav {
  float: left;
  position: relative;
  height: 42px;
}

.criteria-button {
  position: relative;
  cursor: pointer;
  border-left: 1px solid #eee;
  width: 20px;
  text-align: center;
  color: #333;
  font-size: 13px;
  font-family: "Trebuchet MS", Helvetica, sans-serif !important;
  line-height: 1.7;
  -webkit-transform: translateX(-100%);
  transform: translateX(-100%);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.criteria-button.criteria-up {
  position: absolute;
  height: 50%;
  top: 0;
  border-bottom: 1px solid #eee;
}

.criteria-button.criteria-down {
  position: absolute;
  bottom: -1px;
  height: 50%;
}
</style>