<template>
  <div id="CoefPicker">
    <div id="criteriaList">
      <div class="criteria" v-for="i in nbCriteria" v-bind:key="i">
        {{ String.fromCharCode(96 + i) }}
        <div class="slider-styled" :id="'slider_' + i" />
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
    return {};
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
      slider.noUiSlider.on("update", function () {
        console.log(slider.noUiSlider.get());
      });
    }
  },
};
</script>

<style scoped>
/* Hide markers on slider handles */
.slider-styled .noUi-handle::before,
.slider-styled .noUi-handle::after {
  display: none;
}

.slider-styled .noUi-handle .noUi-touch-area {
  border: 1px solid transparent;
  position: absolute;
  top: -10px;
  left: -10px;
  right: -10px;
  bottom: -10px;
  width: auto;
  height: auto;
}

/* Show a border when hovering the area the handle responds to */
.slider-styled .noUi-handle:hover .noUi-touch-area {
  border: 1px dashed #7f7f7f;
}
</style>