<template>
    <v-flex xs2>
      <v-combobox
        v-model="prop_selection.item"
        v-bind:items="prop_selection.items"
        v-bind:label="prop_selection.label"
        v-on:change="onChange"
        clearable
        clear-icon="clear"
      >     
      </v-combobox>
    </v-flex>
</template>

<script>
export default {
  name: "MySelection",
  props: ['prop_selection'],
  data() {
    return {
    };
  },
  methods: {
    onChange: function() {
      console.log("here");
      this.$emit("change-selection");
    }
  }
};
</script>

