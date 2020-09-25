<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div class="container" v-for="listTask in listTasks" :key="listTask.id">
      <div class="card text-white bg-dark mb-3">
        <div class="card-header">{{ listTask.title }} créée le {{ listTask.created_at }} <button type="button" class="btn btn-danger" @click="deleteList(listTask.id)" @task-remove="refresh">Supprimer la liste</button></div>
        <div class="card-body">
          <div v-if="listTask.nb_item > 0">
            <a class="card-text" v-on:click="displayItems(listTask.id)" >{{ listTask.nb_item }} taches en cours</a>
            <ul v-for="item in listItems" :key="item.id">
              <li>{{ item.title }} créée le {{ item.created_at }}</li>
            </ul>
          </div>
          <p class="card-text" v-else>Il n'y a pas encore de taches associées à cette liste</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'Home',
  // components: {
  //   moment
  // },

  data () {
    return {
      listTasks: [],
      listItems: []
    }
  },

  async beforeCreate () {
    const response = await axios.get('http://localhost:3000/api/v1/list')
    this.listTasks = response.data.map(listTask => ({ ...listTask, created_at: moment(listTask.created_at).format('DD/MM/YYYY') }))
  },

  methods: {

    displayItems: async function (i) {
      const response = await axios.get(`http://localhost:3000/api/v1/list/${i}/item`)
      this.listItems = response.data.map(listItem => ({ ...listItem, created_at: moment(listItem.created_at).format('DD/MM/YYYY') }))
    },
    deleteList: async function (i) {
      await axios.delete(`http://localhost:3000/api/v1/list/${i}`)
        .then(response => this.refresh())
        .catch(error => console.log(error))
    },
    refresh: async function () {
      const response = await axios.get('http://localhost:3000/api/v1/list')
      this.listTasks = response.data.map(listTask => ({ ...listTask, created_at: moment(listTask.created_at).format('DD/MM/YYYY') }))
    }
  }
}
</script>
