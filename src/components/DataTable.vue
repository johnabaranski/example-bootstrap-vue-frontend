<template>

<b-container class="bv-example-row">

  <b-row>
    <b-col>
          <b-input-group size="sm">
            <b-form-input
              v-model="filter"
              type="search"
              id="filterInput"
              placeholder="Type to Search"
            ></b-form-input>
            <!-- <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
            </b-input-group-append> -->
          </b-input-group>
    </b-col>
  </b-row>
    <b-row>
    <b-col>

<b-tabs pills>
  <b-tab title="All Goals" active @click="getfilteredData('All Goals')"></b-tab>
      <b-tab title="Goal 1" @click="getfilteredData('1')"></b-tab>
      <b-tab title="Goal 2" @click="getfilteredData('2')"></b-tab>
      <b-tab title="Goal 3" @click="getfilteredData('3')"></b-tab>
      <b-tab title="Goal 4" @click="getfilteredData('4')"></b-tab>
      <b-tab title="Goal 5" @click="getfilteredData('5')"></b-tab>
      <b-tab title="Goal 6" @click="getfilteredData('6')"></b-tab>

    </b-tabs>
    </b-col>
  </b-row>
     
  <b-row>
    <b-col>
    <b-table striped hover
      id="my-table"
      :items="mutatedData"
      :fields="fields"
      :per-page="perPage"
      :current-page="currentPage"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      :filter="filter"
      :filterIncludedFields="filterOn"
       @filtered="onFiltered"
    >

      <template v-slot:cell(fullCourse)="data">
        <a target="_blank" v-bind:href="createLink(data.item.subject, data.item.cat_nbr)">{{ data.item.course }}</a>
      </template>

      <template v-slot:cell(validdt)="data">
        {{dateFormater(data.item.validdt)}}
      </template>

      <template v-slot:cell(attr_desc)="data">
        <span v-html="createGoalLinks(data.item.attr_desc)"></span>
      </template>

   
    </b-table>
       </b-col>
  </b-row>
          
  <b-row>
  <b-col>
    <b-pagination
      v-model="currentPage"
      :total-rows="totalRows"
      :per-page="perPage"
      sort-icon-left
      aria-controls="my-table"
    ></b-pagination>
    </b-col>
    <b-col>
          <p class="mt-3">Current Page: {{ currentPage }}</p>
    </b-col>
  </b-row>
</b-container>
   
  
</template>

<script>
import axios from 'axios';
import SearchBox from './SearchBox';

export default {
  name: 'DataTable',
  components: {
    SearchBox
  },
  props: {
    msg: String,
  },
  data() {
    return {
      coreData: [],
      mutatedData: [],
      fields: [
          { key: 'fullCourse', label: 'Course', sortable: true },
          { key: 'title', label: 'Course Title', sortable: true },
          { key: 'attr_desc', label: 'Course Description', sortable: true },
          { key: 'validdt', label: 'Effective Term', sortable: true }
        ],
        
      errors: [],
      search: '',
      currentSort: 'name',
      currentSortDir: 'asc',
      pageSize: 25,
      currentGoal: 'All Goals',
      perPage: 25,
      currentPage: 1,
      filter: '',
      goalFilter: '',
      filterOn: [],
      sortBy: '',
      sortDesc: false,

    };
  },
  created() {
    axios
      .get('goals.json')
      .then((response) => {
        this.coreData = response.data;

        function addKeyValue(obj, key, data){
          obj[key] = data;
        }

        let addCourse = this.coreData.map(function(e) {
             return addKeyValue(e, 'course', e.subject + " " + e.cat_nbr);
        })
        
        this.mutatedData = this.coreData;
        this.totalRows = this.mutatedData.length
        this.currentPage = 1
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
  methods: {
    sort(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }
      this.currentSort = s;
    },
    dateFormater(item) {
      const newdate = new Date(item);

      if (newdate.getUTCMonth() == 0) {
        return `Spring ${newdate.getUTCFullYear()}`;
      } if (newdate.getUTCMonth() == 5) {
        return `Summer ${newdate.getUTCFullYear()}`;
      } if (newdate.getUTCMonth() == 7) {
        return `Fall ${newdate.getUTCFullYear()}`;
      }
      return 'Fall 2013';
    },
    createLink(a, b) {
      return (
        `http://myCourse?classesSearchText=${
          a
        }+${
          b}`
      );
    },
    onFiltered(filteredItems) {
        // Trigger pagination to update the number of buttons/pages due to filtering
        this.totalRows = filteredItems.length
        this.currentPage = 1
      },

    getfilteredData(e) {
      this.currentGoal = e;
      if (this.currentGoal != 'All Goals') {
        this.mutatedData =  this.coreData.filter((data) => data.attribute.charAt(2).match(this.currentGoal));
        this.totalRows = this.mutatedData.length
        this.currentPage = 1

      } else {

     
        this.mutatedData = this.coreData;
        this.totalRows = this.mutatedData.length
        this.currentPage = 1
      }
      
    },
    createGoalLinks(a) {
      const _str = a;
     
      return _str
        .replace('Goal 1', '<a href=https://mygoal/goal1 target=_blank>Goal 1</a>')
        .replace('Goal 2', '<a href=https://mygoal//goal2 target=_blank>Goal 2</a>')
        .replace('Goal 3', '<a href=https://mygoal//goal3 target=_blank>Goal 3</a>')
        .replace('Goal 4', '<a href=https://mygoal//goal4 target=_blank>Goal 4</a>')
        .replace('Goal 5', '<a href=https://mygoal//goal5 target=_blank>Goal 5</a>')
        .replace('Goal 6', '<a href=https://mygoal//goal6 target=_blank>Goal 6</a>');
    },

  },

};
</script>

<style>
/* body ul .page-item {
    list-style-type: none !important;
} */

body ul > li {
    text-indent: 5px !important;
    list-style-type: none !important;
    margin:0 !important;
}

#region-content .nav-pills .nav-link .active {
    color: white !important;
}

a.page-link {
    padding-left: 20px !important;
}

.page-item {
    text-indent: 5px !important;
    list-style-type: none !important;
    margin:0 !important;
}

#app .btn .btn-secondary {

background: rgb(95, 95, 95) !important;

}

.nav-item .active {

     color: #fff !important;
     
}

.nav-pills .nav-link.active, .nav-pills .show>.nav-link {

    background-color: #0052bc !important;
}

</style>
