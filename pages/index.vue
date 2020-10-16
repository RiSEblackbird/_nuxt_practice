<template>
  <section class="container">
    <h2>{{ title }}</h2>
    <p>{{ message }}</p>
    <table>
      <h3>Delete</h3>
      <tr>
        <th>Target Email</th>
        <td><input v-model="del_email" /></td>
        <td><button @click="delData" />Delete</td>
      </tr>
    </table>
    <table>
      <h3>Add</h3>
      <tr>
        <th>Email</th>
        <td><input v-model="email" /></td>
      </tr>
      <tr>
        <th>Name</th>
        <td><input v-model="username" /></td>
      </tr>
      <tr>
        <th>Age</th>
        <td><input type="number" v-model="age" /></td>
      </tr>
      <tr>
        <th>Tel</th>
        <td><input v-model="tel" /></td>
      </tr>
      <tr>
        <th></th>
        <td><button @click="addData" />Create</td>
      </tr>
    </table>
    <hr />
    <ul v-for="(data, key) in json_data">
      <li>
        <strong>{{ key }}</strong
        ><br />{{ data }}
      </li>
    </ul>
  </section>
</template>

<script>
const axios = require('axios')

const keywordUrl = 'https://practice0-d1f6a.firebaseio.com/keywords'
const stydyLogUrl = 'https://practice0-d1f6a.firebaseio.com/study_logs'

export default {
  data: function () {
    return {
      title: 'Axios',
      // keywords
      word: '',
      memo: '',
      stydy_logs: '',
      del_word: '',
      // study_logs
      body: '',
      keywords: '',
      del_body: '',
      message: 'axios sample.',
      json_data: {},
    }
  },
  methods: {
    addKeyword: function () {
      let add_url = keywordUrl + '/' + this.word + '.json'
      let data = {
        word: this.word,
        memo: this.memo
      }
      axios.put(add_url, data).then((re) => {
        this.word = '',
        this.memo = '',
        this.study_logs = false,
        this.getKeyword()
      })
    },
    getKeyword: function () {
      axios
        .get(keywordUrl + '.json')
        .then((res) => {
          this.message = 'get all data.'
          this.json_data = res.data
        })
        .catch((error) => {
          this.message = 'ERROR!'
          this.json_data = {}
        })
    },
    delKeyword: function () {
      let del_url = keywordUrl + '/' + this.word + '.json'
      axios.delete(del_url).then((re) => {
        this.message = this.word + 'を削除しました。',
        this.memo = '',
        this.study_logs = false,
        this.getKeyword()
      })
    },
  },
  created: function () {
    this.getKeyword()
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
