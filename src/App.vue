<template>
  <div id="app">
    <table>
      <thead>
        <th>Id</th>
        <th>Title</th>
        <th>Year</th>
        <th>On Display</th>
      </thead>
      <tbody>
        <tr v-for="painting in paintings" :key="painting.id">
          <td>{{painting.id}}</td>
          <td>{{painting.title}}</td>
          <td>{{painting.year}}</td>
          <td>{{painting.on_display}}</td>
          <td><button @click="deletePainting(painting.id)">Delete</button></td>
        </tr>
        <tr>
          <td><input type="hidden" v-model="painting.id"></td>
          <td><input type="text" v-model="painting.title"></td>
          <td><input type="number" v-model="painting.year"></td>
          <td><input type="checkbox" v-model="painting.on_display"></td>
          <td><button @click="addPainting" :disabled="saving">Add</button></td>
        </tr>
      </tbody>
    </table>
    <button @click="loadData">Load</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      paintings: [],
      painting: {
        id: null,
        title: null,
        year: null,
        on_display: false
      },
      saving: false
    }
  },
  methods: {
    async loadData(){

      let response = await fetch("http://127.0.0.1:8000/api/paintings")
      let data = await response.json()

      this.paintings = data
      console.log(this.paintings)
    },

    async deletePainting(id){
      await fetch(`http://127.0.0.1:8000/api/paintings/${id}`, {
        method: "DELETE"
      })

      await this.loadData()
    },

    async addPainting(){
      this.saving = true
      await fetch(`http://127.0.0.1:8000/api/paintings`, {
        method: "POST",
        headers: {
          "Content-type" : "application/json",
          "Accept" : "application/json"
        },
        body: JSON.stringify(this.painting)
      })

      await this.loadData()
      this.saving = false
    },

    async updatePainting(id){
      await fetch(`http://127.0.0.1:8000/api/paintings/${id}`, {
        method: "PATCH",
        headers: {
          "Content-type" : "application/json",
          "Accept" : "application/json"
        },
        body: JSON.stringify(this.painting)
      })

      await this.loadData()
    },
  },
  mounted() {
    this.loadData()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
