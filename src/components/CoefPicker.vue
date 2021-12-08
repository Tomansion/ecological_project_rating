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
        start: 5,
        connect: [true, false],
        step: 0.1,
        range: {
          min: 0,
          max: 10,
        },
      });

      // Bind the color changing function to the update event.
      slider.noUiSlider.on("update", () => {
        this.criteriaValues[i - 1].value = parseFloat(slider.noUiSlider.get());
        this.$emit("updateCriteria", this.criteriaValues);
      });
    }
  },
  methods: {
    updateCoef(index) {
      // Convert the input value to a number.
      let newInput = parseFloat(this.criteriaValues[index].coef);
      if (isNaN(newInput)) newInput = 0;
      if (newInput < 0) newInput = 0;
      if (newInput > 100) newInput = 100;

      this.criteriaValues[index].coef = newInput;
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
</style>