<template>
  <v-app id="inspire">
    <v-container fluid>
      <v-layout wrap>
        <v-toolbar color="cyan" dark>
          <v-toolbar-side-icon></v-toolbar-side-icon>
          <v-toolbar-title>Hong Kong School Finder</v-toolbar-title>
          <v-spacer></v-spacer>
        </v-toolbar>
        <MySelection
          v-for="school_selection in school_selection_data"
          v-bind:prop_selection="school_selection"
          v-on:change-selection="changeCriteria"
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
      // Actual data
      school_header: [],
      school_data: [],
      // Selection data
      school_display_column: ['D', 'Z'],
      school_selection_columns: ["B"],//, "P", "R", "T", "V", "X", "AF"],
      school_selection_data: [],
      // Table data
      school_table_header: [],
      school_table_data: [
          {B: 'asdfasdfasdf', D: 'asdfsdfsadfds'},
      ],
    };
  },
  mounted() {
    axios.get("/data3.json").then(response => {
    //axios.get("/real_data.json").then(response => {
      console.log("Get data");
      this.school_header = response.data.shift();
      console.log(this.school_header);
      this.school_data = response.data;
      console.log(this.school_data[0]);
      
      this.school_table_data = this.school_data; // Get table data
      this.getTableHeader();
      this.getSelectionItems(); // Get selection items based on table data
    });
  },
  methods: {
    loadMore: function() {
      console.log("hello " + event.messages);
    },
    getTableHeader: function() {
      console.log("Get Table Header");
      for (var i = 0; i< this.school_display_column.length; i++) {
        var col_name = this.school_display_column[i];
        var header = this.school_header[col_name];
        var header_obj = {};
        header_obj['text'] = header;
        header_obj['value'] = col_name;
        this.school_table_header.push(header_obj);
      }
    },
    getSelectionItems: function() {
      console.log("Get Selection Data");
      for (var i = 0; i < this.school_selection_columns.length; i++) {
        var col_name = this.school_selection_columns[i];
        var header = this.school_header[col_name];
        var item = {};
        console.log(header);
        item["label"] = header;
        item["items"] = Array.from(
          // Note that this is based on school table data to provide dynamic filters
          new Set(this.school_data.map(item => item[col_name]))
        );
        this.school_selection_data.push(item);
      }
    },
    filterFunc: function(item) {
      console.log("Filter");
      for (var i = 0; i < this.school_selection_columns.length; i++) {      
          var col_name = this.school_selection_columns[i];
          var selected_item = this.school_selection_data[i].item;          
          console.log(col_name);
          console.log(selected_item);
          if (selected_item == undefined) {
            continue;
          }
          if (item[col_name] == undefined || item[col_name] != selected_item) {
            return false;
          }          
        }
        return true;
    },
    changeCriteria: function() {
      console.log("Change Criteria");
      console.log(this.school_selection_data);
      this.school_table_data = this.school_data.filter(this.filterFunc);      
      //this.getSelectionItems();      
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
