<template>
  <div>
    <input type="text" v-model="search" @keyup.enter="searchwiki">
    <button v-on:click="searchwiki">Search</button>
    <div class="main">
    <ul v-for="pageId in res">
      <a class="title" v-bind:href="link+pageId.title" target="_blank">
      {{pageId.title}}
      </a>
      <li>
      {{pageId.extract}}
      </li>
    </ul>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'

Vue.use(axios)

export default {
  name: 'search',
  data () {
    return {
      search: '',
      pageIds: [],
      res: '',
      link: 'http://en.wikipedia.org/wiki/'
    }
  },
  methods: {
    searchwiki: function () {
      var vm = this
      vm.res = ''
      axios.get('https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&gsrlimit=20&prop=pageimages|extracts&pilimit=max&exintro&explaintext&exsentences=1&exlimit=max&origin=*&gsrsearch=' + vm.search)
      .then(function (response) {
        vm.res = response.data.query.pages
      })
      .catch(function (error) {
        console.log(error)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@keyframes changelength {
  from {
    width: 180px;
  }
  to {
    width: 400px;
  }
}

@keyframes change {
  from {
    width: 400px;
  }
  to {
    width: 180px;
  }
}

.main {
  height: 500px;
  width: 1000px;
  border: 1px solid #eee;
  margin: 20px auto;
  padding: 20px;
  padding-top: 0px;
  overflow: scroll;
}

.title {
  font-size:25px;
}

.title:hover {
  color: #673;
}

input {
  animation: change 0.5s;
  height: 30px;
  width: 180px;
  padding-left: 10px;
}

input:focus {
  animation: changelength 0.5s;
  width: 400px;
}

button {
  background-color: #fff;
  color: #42b983;
  border: 1px solid #42b983;
  height: 34px;
  width: 90px;
}

button:hover {
  color: #fff;
  background-color: #42b983
}

a {
  margin:0px;
  color: #42b983;
  text-align: left;
  text-decoration: none;
}

ul {
  list-style-type: none;
  padding: 0;
  text-align: left;
  padding-bottom: 5px;
  border-bottom: 1px dotted #ccc;
}

li {
  text-align: left;
  list-style: none;
  width: auto;
}

</style>
