<template>
  <div>
  </div>
</template>

<script>
  import mixin from "@directus/extension-toolkit/mixins/interface";

  export default {
    mixins: [mixin],
    computed: {
      displayValue() {
        return this.value.toLowerCase();
      }
    }
  }
</script>

<style lang="scss" scoped>
div {
  background-color: var(--red-300);
}
</style>
