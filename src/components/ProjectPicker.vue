<template>
  <div id="ProejctPicker">
      {{projects}}
  </div>
</template>

<script>
export default {
  props: {
    projects: { type: Array, required: true },
  },
};
</script>

<style>
</style>