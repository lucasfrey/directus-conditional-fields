<template>
  <input @input="emitValue" :value="value" />
</template>

<script>
  import mixin from "@directus/extension-toolkit/mixins/interface";

  export default {
    mixins: [mixin],
    mounted() {
      // Fetch block options
        const { values } = this._props

      // Start block system
      // Basically we hide/show field relevant to the type of blocks

      const fieldsNode = document.querySelectorAll('[data-field]');
      const interface = document.querySelector('[data-field="block_interface"]');
      const typeField = document.querySelector('[data-field=type]');

      const options = this.getOptions();

      // hide this custom interface
      if (interface) interface.style.display = 'none'

      // hide all fields until the user choose a type
      this.hideAll(fieldsNode)

      // Show the default value if present
      this.showFields(options[values.type])

      if (typeField) {
        typeField.addEventListener('change', (e) => {
          const value = e.target.value

          // Hide all the fields on change
          this.hideAll(fieldsNode)

          // show fields from the options
          this.showFields(options[value])
        })
      }
    },
    methods: {
      emitValue(event) {
        const value = event.target.value;
        this.$emit("input", value);
      },
      showAll(fieldsNode) {
        for (let i = 0; i < fieldsNode.length; i++) {
          fieldsNode[i].style.display = 'block'
        }
      },
      hideAll(fieldsNode) {
        // Hide everything except the status bar, title and type
        for (let i = 0; i < fieldsNode.length; i++) {
          let field = fieldsNode[i].dataset.field
          if (field !== 'status' &&
          field !== 'type' &&
          field !== 'title' )
          fieldsNode[i].style.display = 'none'
        }
      },
      getOptions() {
        // Fetch block options
        const { fields } = this._props

        // build options
        let blockOpts = [];

        Object.keys(fields).forEach(( key, index ) => {
            if (key.startsWith('block_') && key !== "block_interface") {
            let splitKeys = key.split("_");

            if (!blockOpts[splitKeys[1]]) blockOpts[splitKeys[1]] = []
                blockOpts[splitKeys[1]].push(key)
            }
        });

        return blockOpts;
      },
      showFields(fieldsNode) {
        if (fieldsNode) {
          for (let i = 0; i < fieldsNode.length; i++) {
            let field = document.querySelector('[data-field=' + fieldsNode[i] + ']');

            if (field) {
              field.style.display = 'block'
            }
          }
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
input {
  border-radius: var(--border-radius);
}
</style>
