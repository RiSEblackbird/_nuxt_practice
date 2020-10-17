<template>
  <section class="container">
    <h2>{{ title }}</h2>
    <p>{{ message }}</p>
    <table>
      <h3>Delete</h3>
      <tr>
        <th>Target Body</th>
        <td><input v-model="del_body" /></td>
        <td><button @click="delStudyLog">Delete</button></td>
      </tr>
    </table>
    <table>
      <h3>Add New Study_log</h3>
      <tr>
        <th>log_body</th>
        <td><input v-model="body" /></td>
      </tr>
      <tr>
        <th></th>
        <td><button @click="addStudyLog">Create</button></td>
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

const stydyLogUrl = 'https://practice0-d1f6a.firebaseio.com/study_logs'

export default {
  data() {
    return {
      // study_logs
      body: '',
      keywords: '',
      del_body: '',
      // top
      title: 'StudyLog',
      message: 'What did you do?',
      json_data: {},
    }
  },
  created() {
    this.getStudyLog()
  },
  methods: {
    addStudyLog() {
      const addUrl = stydyLogUrl + '/' + this.body + '.json'
      const data = {
        body: this.body,
        keywords: this.keywords,
      }
      axios.put(addUrl, data).then((re) => {
        this.body = ''
        this.keywords = false
        this.getStudyLog()
      })
    },
    getStudyLog() {
      axios
        .get(stydyLogUrl + '.json')
        .then((res) => {
          this.message = 'get all data.'
          this.json_data = res.data
        })
        .catch(() => {
          this.message = 'ERROR!'
          this.json_data = {}
        })
    },
    delStudyLog() {
      const delUrl = stydyLogUrl + '/' + this.body + '.json'
      axios.delete(delUrl).then((re) => {
        this.message = this.body + 'を削除しました。'
        this.body = ''
        this.keywords = false
        this.getStudyLog()
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
