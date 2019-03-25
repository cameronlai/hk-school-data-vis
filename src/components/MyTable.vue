<template>
  <v-flex xs12>
    <v-data-table
      v-bind:headers="prop_school_table_headers"
      v-bind:items="prop_school_table_data"
      class="elevation-1"
    >
      <template v-slot:items="schools">
        <td v-for="header in prop_school_table_headers">{{ schools.item[header.value] }}</td>
      </template>
    </v-data-table>
  </v-flex>
</template>

<script>
export default {
  name: "MyTable",
  props: ["prop_school_table_headers", "prop_school_table_data"],
  data() {
    return {
      school_table_headers: [
        { text: "School Name", value: "D" },
        { text: "School Type", value: "B" }
      ],
      filtered_school_data: [{ B: "asdfasdfasdf", D: "asdfsdfsadfds" }]
    };
  }
};
</script>

