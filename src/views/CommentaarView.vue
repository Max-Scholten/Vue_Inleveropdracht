<template >
  <div id="app" class="cijferlijst-container bg-red-900">
    <p id="smiley">☺</p>
    <p id="onderwerpen">JavaScript<br/>Vue<br/>Laravel</p>
    <table>
      <thead>
      <tr>
        <th class="bg-sky-500">Naam</th>
        <th>Commentaar</th>
      </tr>
      </thead>
      <tbody>
      <commentaar-regel
          v-for="(c, index) in commentaren"
          @veranderId="veranderId"
          :index="index"
          :commentaar="c"
          :key="index">
      </commentaar-regel>
      </tbody>
    </table>
    <p>{{ alles }} </p>
  </div>
</template>


<script>
const api = 'http://localhost:81/api/commentaren'
import CommentaarRegel from '../components/CommentaarRegel.vue'
import axios from 'axios'
export default {
  components: {
    CommentaarRegel
  },
  data () {
    return { commentaren: [] }                 // property commentaren is een lege array
  },
  async created () {
    const response = await axios.get(api)
    const json = await response.data
    this.commentaren = json
  },
  methods: {
    async veranderId(index, id, tekst){
      const response = await axios.patch(api+id, {"tekst":tekst})
      if (response.status == 200) this.commentaren[index].tekst=tekst
    },
  },
  computed: {                                                                 // App.vue
    alles() {
      let s = ''
      this.commentaren.map(el => s += el.tekst + ' / ')
      return s
    },
  }
}
</script>

<style scoped>
.cijferlijst-container {
  max-width: 500px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background: #f9f9f9;
  color: black;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

.failed {
  background-color: #f8d7da;
}

.passed {
  background-color: #d4edda;
}

.controls {
  margin-top: 10px;
}

input[type="number"] {
  width: 60px;
}

button {
  cursor: pointer;
  padding: 5px 10px;
}
</style>