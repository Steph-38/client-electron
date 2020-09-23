<template>
  <div class="about">
    <div class="container">
      <form action="" method="post">
        <div class="form-group">
          <label for="title">Titre de la liste</label>
          <input type="text" class="form-control" id="title" v-model="title">
        </div>
        <button type="button" class="btn btn-outline-info" @click="addTaskList">Ajouter la liste</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      title: '',
      
    }
  },

  methods: {
    addTaskList: async function() {
		  await axios.post('http://localhost:3000/api/v1/list', {
        title: this.title
      })
		  .then(response => console.log(response))
			.catch(error => console.log(error));
    }
  }
}

</script>