<template>
  <section class="container">
    <h2>{{ title }}</h2>
    <p>{{ message }}</p>
    <table>
      <h3>Delete</h3>
      <tr>
        <th>Target Word</th>
        <td><input v-model="del_word" /></td>
        <td><button @click="delKeyword">Delete</button></td>
      </tr>
    </table>
    <table>
      <h3>Add New Keyword</h3>
      <tr>
        <th>Word</th>
        <td><input v-model="word" /></td>
      </tr>
      <tr>
        <th>Memo</th>
        <td><input v-model="memo" /></td>
      </tr>
      <tr>
        <th></th>
        <td><button @click="addKeyword">Create</button></td>
      </tr>
    </table>
    <hr />
    <ul v-for="(data, index) in json_data" :key="index">
      <li>
        <strong>{{ index }}</strong
        ><br />{{ data }}
      </li>
    </ul>
  </section>
</template>

<script>
const axios = require('axios')

const keywordUrl = 'https://practice0-d1f6a.firebaseio.com/keywords'

export default {
  data() {
    return {
      // keywords
      word: '',
      memo: '',
      stydy_logs: '',
      del_word: '',
      // top
      title: 'Keywords',
      message: 'IT (or related) keywords you met.',
      json_data: {},
    }
  },
  created() {
    this.getKeyword()
  },
  methods: {
    addKeyword() {
      const addUrl = keywordUrl + '/' + this.word + '.json'
      const data = {
        word: this.word,
        memo: this.memo,
      }
      axios.put(addUrl, data).then((re) => {
        this.word = ''
        this.memo = ''
        this.study_logs = false
        this.getKeyword()
      })
    },
    getKeyword() {
      axios
        .get(keywordUrl + '.json')
        .then((res) => {
          this.message = 'get all data.'
          this.json_data = res.data
        })
        .catch(() => {
          this.message = 'ERROR!'
          this.json_data = {}
        })
    },
    delKeyword() {
      const delUrl = keywordUrl + '/' + this.word + '.json'
      axios.delete(delUrl).then((re) => {
        this.message = this.word + 'を削除しました。'
        this.memo = ''
        this.study_logs = false
        this.getKeyword()
      })
    },
  },
}
</script>

<style>
.container {
  padding: 5px 10px;
}
h1 {
  font-size: 60pt;
  color: #345980;
}
p {
  padding-top: 5px;
  font-size: 20pt;
}
ul {
  margin: 0px 10px;
  background-color: aliceblue;
}
li {
  padding: 10px;
  font-size: 16pt;
}
div {
  font-size: 14pt;
}
input {
  font-size: 14pt;
}
button {
  font-size: 14pt;
}
tr th {
  width: 150px;
  background-color: darkblue;
  color: white;
  font-size: 16pt;
}
tr td {
  padding: 5px 10px;
  background-color: #eef;
  font-size: 14pt;
}
pre {
  padding: 10px;
  font-size: 18pt;
  background-color: #efefef;
  white-space: pre-wrap;
}
hr {
  margin: 10px 0px;
}
</style>
