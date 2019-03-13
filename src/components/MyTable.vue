<template>
    <v-flex xs12>
        asdf
          <v-data-table :headers="prop_school_table_headers" :items="prop_school_table_data" class="elevation-1">
            <template v-slot:items="schools">
              <td>{{ schools.item.D }}</td>
              <td>{{ schools.item.B }}</td>              
            </template>
          </v-data-table>
    </v-flex>
</template>

<script>
export default {
  name: "MyTable",
  props: ['prop_school_table_headers', 'prop_school_table_data'],
  data() {
    return { 
      school_table_headers: [
        { text: 'School Name', value: 'D' },
        { text: 'School Type', value: 'B' },
      ],
      filtered_school_data: [
          {B: 'asdfasdfasdf', D: 'asdfsdfsadfds'},
      ],
    };
  }
};
</script>

