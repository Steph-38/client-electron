<template>
  <div class="about">
    <div class="container">
      <!-- Affichage de l'erreur si champ vide -->
      <div v-if="errorList != ''" class="alert alert-dismissible alert-danger">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>{{ errorList }}</strong>
      </div>
      <!-- Formulaire de création d'une liste -->
      <form action="" method="post">
        <div class="form-group">
          <label for="title">Titre de la liste</label>
          <input type="text" class="form-control" id="title" v-model="title">
        </div>
        <button type="submit" class="btn btn-outline-info" @click="addTasksList">Ajouter la liste</button>
      </form>
      <!-- Affichage de l'erreur si champ vide -->
      <!-- <div v-if="errorTask != ''" class="alert alert-dismissible alert-danger">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>{{ errorTask }}</strong>
      </div> -->
      <!-- Formulaire d'ajout d'un item si liste existe -->
      <!-- <form action="" method="post">
        <div class="form-group">
          <label for="title">Titre de la tache</label>
          <input type="text" class="form-control" id="title" v-model="task">
        </div>
        <button type="submit" class="btn btn-outline-info" @click="addTask(listTasks.id)">Ajouter une tache</button>
      </form> -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      title: '',
      task: '',
      listTasks: {},
      errorList: '',
      errorTask: []
    }
  },

  methods: {
    addTasksList: async function () {
      if (!this.title) {
        this.errorList = 'Le champ titre ne doit pas être vide'
      } else {
        await axios.post('http://localhost:3000/api/v1/list', {
          title: this.title
        })
          .then(response => console.log(response),
            // response => this.listTasks,
            this.errorList = '')
          .catch(error => console.log(error))
      }
    }

    // addTask: async function (id) {
    //   if (!this.task) {
    //     this.errorTask.push({ msg: 'Le champ titre ne doit pas être vide' })
    //   }
    //   if (!this.listTasks) {
    //     this.errorTask.push({ msg: 'Il n\'y a pas de liste créée' })
    //   }

    //   if (!this.errorTask) {
    //     await axios.post(`http://localhost:3000/api/v1/list/${id}/item`, {
    //       task: this.task
    //     })
    //       .then(response => console.log(response),
    //         this.errorTask = '')
    //       .catch(error => console.log(error))
    //   }
    // }

  }
}
</script>
