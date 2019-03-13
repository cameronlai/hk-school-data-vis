<template>
  <v-app id="inspire">
    <v-container fluid>
      <v-layout wrap>
        <v-toolbar color="cyan" dark>
          <v-toolbar-side-icon></v-toolbar-side-icon>
          <v-toolbar-title>Hong Kong School Finder</v-toolbar-title>
          <v-spacer></v-spacer>
          <!--
          <v-btn icon>
            <v-icon>search</v-icon>
          </v-btn>
          -->
        </v-toolbar>
        <MySelection
          v-for="school_selection in school_selection_data"
          v-bind:prop_selection="school_selection"
        />
        <MyTable 
          v-bind:prop_school_table_headers="school_table_header"
          v-bind:prop_school_table_data="school_table_data"
        />
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";
import MyMap from "./components/MyMap.vue";
import MySelection from "./components/MySelection.vue";
import MyTable from "./components/MyTable.vue";
import TodoList from "./components/TodoList.vue";

export default {
  el: "#app",
  data() {
    return {
      school_header: [],
      school_display_column: ['D', 'Z'],
      school_selection_columns: ["B", "P", "R", "T", "V", "X", "AF"],
      school_selection_data: [],
      school_data: [],
      school_table_header: [],
      school_table_data: [
          {B: 'asdfasdfasdf', D: 'asdfsdfsadfds'},
      ],
    };
  },
  mounted() {
    //axios.get("/data3.json").then(response => {
    axios.get("/real_data.json").then(response => {
      console.log("Get data");
      this.school_header = response.data.shift();
      console.log(this.school_header);
      this.school_data = response.data;
      console.log(this.school_data[0]);

      this.getSelectionItems();
      //this.changeCriteria();
    });
  },
  methods: {
    loadMore: function() {
      console.log("hello " + event.messages);
    },
    getSelectionItems: function() {
      for (var i = 0; i< this.school_display_column.length; i++) {
        var col_name = this.school_display_column[i];
        var header = this.school_header[col_name];
        var header_obj = {};
        header_obj['text'] = header;
        header_obj['value'] = col_name;
        this.school_table_header.push(header_obj);
      }
      for (var i = 0; i < this.school_selection_columns.length; i++) {
        var col_name = this.school_selection_columns[i];
        var header = this.school_header[col_name];
        var item = {};
        item["label"] = header;
        item["items"] = Array.from(
          new Set(this.school_data.map(item => item[col_name]))
        );
        this.school_selection_data.push(item);
      }
    },
    changeCriteria: function() {
      console.log(this.selected_school_type);
      this.filtered_school_data = this.school_data.filter(
        school => school.B == this.selected_school_type
      );
      console.log(this.filtered_school_data);
    }
  },
  components: {
    TodoList,
    MyMap,
    MySelection,
    MyTable,
  }
};
</script>
