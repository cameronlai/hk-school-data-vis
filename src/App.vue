<template>
  <v-app id="inspire">
    <v-container fluid>
      <v-layout row wrap>
        <v-toolbar color="cyan" dark>
          <v-toolbar-side-icon></v-toolbar-side-icon>
          <v-toolbar-title>Hong Kong School Finder</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn v-on:click="selectLang(1)">中文</v-btn>
          <v-btn v-on:click="selectLang(0)">Eng</v-btn>
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
      lang_index: 0, // 0: Eng, 1: Chi
      school_display_column: [
        ["D", "P", "R", "T", "V", "X", "AF"], 
        ["E", "Q", "S", "U", "W", "Y", "AG"]
        ],
      school_selection_columns: [
        ["P", "R", ],//"T", "V", "X", "AF"],
        ["Q", "S", ]//"U", "W", "Y", "AG"],
      ],      
      school_selection_data: [],
      // Table data
      school_table_header: [],
      school_table_data: []
    };
  },
  mounted() {
    axios.get("/data3.json").then(response => {
      //axios.get("/real_data.json").then(response => {
      this.school_header = response.data.shift();
      this.school_data = response.data;

      this.school_table_data = this.school_data; // Get table data
      this.getTableHeader();
      this.getSelectionItems(); // Get selection items based on table data
    });
  },
  methods: {
    selectLang: function(lang) {
      this.lang_index = lang;
      this.school_table_data = this.school_data; // Get table data
      this.getTableHeader();
      this.getSelectionItems(); // Get selection items based on table data
    },
    getTableHeader: function() {
      this.school_table_header = [];
      for (var i = 0; i < this.school_display_column[this.lang_index].length; i++) {
        var col_name = this.school_display_column[this.lang_index][i];
        var header = this.school_header[col_name];
        var header_obj = {};
        header_obj["text"] = header;
        header_obj["value"] = col_name;
        this.school_table_header.push(header_obj);
      }
    },
    getSelectionItems: function() {
      this.school_selection_data = [];
      for (var i = 0; i < this.school_selection_columns[this.lang_index].length; i++) {
        var col_name = this.school_selection_columns[this.lang_index][i];
        var header = this.school_header[col_name];
        var item = {};
        item["label"] = header;
        item["items"] = Array.from(
          // Note that this is based on school table data to provide dynamic filters
          new Set(this.school_table_data.map(item => item[col_name]))
        );
        this.school_selection_data.push(item);
      }
    },
    updateSelectionItems: function() {
      for (var i = 0; i < this.school_selection_columns[this.lang_index].length; i++) {
        var col_name = this.school_selection_columns[this.lang_index][i];
        this.school_selection_data[i]["items"] = Array.from(
          // Note that this is based on school table data to provide dynamic filters
          new Set(this.school_table_data.map(item => item[col_name]))
        );
      }
    },
    filterFunc: function(item) {
      for (var i = 0; i < this.school_selection_columns[this.lang_index].length; i++) {
        var col_name = this.school_selection_columns[this.lang_index][i];
        var selected_item = this.school_selection_data[i].item;
        console.log("---")
        console.log(selected_item);
        console.log(item[col_name]);
        if (selected_item == undefined || selected_item == null) {
          continue;
        }
        if (item[col_name] == undefined || item[col_name] != selected_item) {
          return false;
        }
      }
      return true;
    },
    changeCriteria: function() {
      this.school_table_data = this.school_data.filter(this.filterFunc);
      this.updateSelectionItems();
    }

  },
  components: {
    TodoList,
    MyMap,
    MySelection,
    MyTable
  }
};
</script>
