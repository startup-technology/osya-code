<template>
  <table>
    <thead>
      <th>なまへ</th>
      <th>Github URL</th>
      <th>Comment</th>
    </thead>
    <tbody>
      <tr  v-for="osyaco in osyacos" :key="osyaco.id">
        <td>{{osyaco.name}}</td>
        <td>{{osyaco.giturl}}</td>
        <td>{{osyaco.comment}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from 'axios'

export default {
  name: "Index",
  data() {
    return {
      osyacos: [] };
  },
  mounted () {
    axios
      .get('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/todos')
      .then(response => (this.osyacos = response.data))
  }
};
</script>