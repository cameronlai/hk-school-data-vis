<template>
  <v-app id="inspire">
    <v-container fluid>
      <v-layout wrap>
        <v-flex xs12>
          <v-combobox
            v-model="selected_school_type"
            :items="school_type"
            v-on:change="changeSchoolType"
            label="Select the type of school"
          ></v-combobox>
        </v-flex>
        <v-flex xs12>
          <v-data-table :headers="school_table_headers" :items="filtered_school_data" class="elevation-1">
            <template v-slot:items="schools">
              <td>{{ schools.item.D }}</td>
              <td>{{ schools.item.B }}</td>              
            </template>
          </v-data-table>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>

  <!-- <div id="app">
    <h1>Hong Kong School Information Visualization</h1>


    <button v-on:click="loadMore">Load More</button>
    <ul>
      <li v-for="msg in messages">{{ msg }}</li>
    </ul>
    <ul>
      <li v-for="item in info">{{ item.B }}</li>
    </ul>
    <TodoList />
    <Map />
  </div>-->
</template>

<script>
import axios from "axios";
import Map from "./components/Map.vue";
import TodoList from "./components/TodoList.vue";

export default {
  el: "#app",
  data() {
    return {
      school_data: [],
      filtered_school_data: [],
      messages: ["hello", "vue", "js"],

      selected_school_type: "Kindergartens",
      school_type: ["Aided Primary Schools", "Kindergartens", "Direct Subsidy Scheme Primary Schools"],

      school_table_headers: [
        { text: 'School Name', value: 'D' },
        { text: 'School Type', value: 'B' },
      ],
    };
  },
  mounted() {
    axios.get("/data3.json").then(response => {
      this.school_data = response.data;
      this.changeCriteria();
    });    
  },
  methods: {
    loadMore: function() {
      console.log("hello " + event.messages);
    },
    changeCriteria: function() {
      console.log(this.selected_school_type)
      this.filtered_school_data = this.school_data.filter(school => school.B == this.selected_school_type);
      console.log(this.filtered_school_data);
    },
    changeSchoolType: function(selectedObj) {
      console.log("here");
      console.log(this.selected_school_type);
      console.log(selectedObj);      
      this.changeCriteria();
    },
  },
  components: {
    TodoList,
    Map
  }
};
</script>
