<template>
    <div>
      <input class="field-long" title="Search Courses" type="text" :value="search" @input="$emit('input', $event.target.value.toLowerCase())" placeholder="search courses" />
      <i class="fa fa-search searchIcon"></i>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SearchBox',
  		props: {
    search: {
      required: false,
      type: String,
    },
  },
};
</script>

<style scoped>

.form-style input[type="text"]
{
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  border: 1px solid #bebebe;
  padding: 7px;
  margin: 0px;
  -webkit-transition: all 0.3s ease-in-out;
  -moz-transition: all 0.3s ease-in-out;
  -ms-transition: all 0.3s ease-in-out;
  -o-transition: all 0.3s ease-in-out;
  outline: none;
}

.form-style input[type="text"]:focus,
.form-style .field-long {
  width: 90%;
  min-height: 40px !important;
  float:left;
}


.searchIcon {
  font-size: 30px;
  color: dimgray;
  margin-left: 15px;
  width: 5%;
}


</style>
