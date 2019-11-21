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
      
      const fieldsNode = document.querySelectorAll('[data-field]');
      const typeField = document.querySelector('[data-field=type]');
      const options = this.getOptions();

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
        const collectionName = this.getCollectionName(this.$parent.collection);

        // Hide all the fields that start with the collection name + the conditional_interface
        for (let i = 0; i < fieldsNode.length; i++) {
          let field = fieldsNode[i].dataset.field

          if (field.startsWith(`${collectionName}_`) || field === 'conditional_interface')
            fieldsNode[i].style.display = 'none'
        }
      },
      getOptions() {
        // Fetch options
        const { fields } = this._props
        const collectionName = this.getCollectionName(this.$parent.collection);

        // Build options
        let options = [];

        Object.keys(fields).forEach(( key, index ) => {
          if (key.startsWith(`${collectionName}_`) && key !== "conditional_interface") {
            const splitKeys = key.split("_");
            const type = splitKeys[1];

            if (!options[type]) options[type] = []

            options[type].push(key)
          }
        });

        return options;
      },
      showFields(fieldsNode) {
        if (fieldsNode) {
          for (let i = 0; i < fieldsNode.length; i++) {
            let field = document.querySelector('[data-field=' + fieldsNode[i] + ']');

            if (field) field.style.display = 'block';
          }
        }
      },
      getCollectionName(collection) {
        // If the collection name ends with an 's', we use the singular name
        return collection.endsWith('s') ? collection.substring(0, collection.length - 1) : collection
      }
    }
  }
</script>

<style lang="scss" scoped>
input {
  border-radius: var(--border-radius);
}
</style>
