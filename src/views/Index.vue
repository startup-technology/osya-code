<template>
  <div>
    <form id="addElement" @submit.prevent="addOsyaco">
      <label>name<input v-model="name"></label>
      <label>github url<input id="giturl" v-model="giturl"></label>
      <label>comment<input id="comment" v-model="comment"></label>
      <button type="submit">そうしん</button>
    </form>
    <table>
      <thead>
        <th>id</th>
        <th>なまへ</th>
        <th>Github URL</th>
        <th>Comment</th>
        <th></th>
      </thead>
      <tbody>
        <tr  v-for="(osyaco, index) in osyacos" :key="osyaco.id">
          <td>{{index}}</td>
          <td>{{osyaco.name}}</td>
          <td>{{osyaco.giturl}}</td>
          <td>{{osyaco.comment}}</td>
          <td v-on:click="deleteOsyaco(osyaco.id, index)">[削除]</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "Index",
  data() {
    return {
      osyacos: [],
      id: '',
      name: '',
      giturl: '',
      comment: ''
      };
  },
  mounted () {
    axios
      .get('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/todos')
      .then(response => (this.osyacos = response.data))
  },
  methods: {
    deleteOsyaco(id, index) {
      axios
        .delete('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/todos/' + id)
        .then(() => this.osyacos.splice(index, 1))
    },
    addOsyaco() {
      axios
        .post('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/todos',
          {
            "name": this.name,
            "giturl": this.giturl,
            "comment": this.comment
          }
        )
        .then(response => (this.osyacos.push(response.data)))
      }
    }
};
</script>