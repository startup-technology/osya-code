<template>
  <div>
    <form id="addElement" @submit.prevent="addOsyaco">
      <label>name<input v-model="name"></label>
      <label>github url<input id="giturl" v-model="giturl"></label>
      <label>comment<input id="comment" v-model="comment"></label>
      <b-button variant="outline-primary" type="submit">そうしん</b-button>
    </form>
    <div v-if="message">{{message}}</div>
    <table>
      <thead>
        <tr>
          <th>id</th>
          <th>なまへ</th>
          <th>Github URL</th>
          <th>Comment</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr  v-for="(osyaco, index) in osyacos" :key="osyaco.id">
          <td>{{index}}</td>
          <td>{{osyaco.name}}</td>
          <td>{{osyaco.giturl}}</td>
          <td>{{osyaco.comment}}</td>
          <td v-on:click="deleteOsyaco(osyaco.id, index)"><b-button variant="outline-danger">[削除]</b-button></td>
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
      comment: '',
      message: ''
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
      if (this.name === '' || this.giturl === '' || this.comment === '') {
        this.message = '名前とGitHubURLとコメントは必須です！';
        console.log('dame');
        return;
      } else {
        this.message = '';
      }
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