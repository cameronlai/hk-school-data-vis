<template>
    <v-flex xs12>
      <v-combobox
        v-model="prop_selection.item"
        :items="prop_selection.items"
        :label="prop_selection.label"
        v-on:change="$emit('change-selection')"
      >     
      </v-combobox>
    </v-flex>
</template>

<script>
export default {
  name: "MySelection",
  components: {
  },
  props: ['prop_selection'],
  data() {
    return {
      //selected_item: "Kindergartens",
      //selected_items: ["Aided Primary Schools", "Kindergartens", "Direct Subsidy Scheme Primary Schools", "Aided Special Schools"]
    };
  }
};
</script>

