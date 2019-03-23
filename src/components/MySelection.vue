<template>
    <v-flex xs12>
      <v-combobox
        v-model="prop_selection.item"
        :items="prop_selection.items"
        :label="prop_selection.label"
        v-on:change="$emit('change-selection')"
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
  }
};
</script>

