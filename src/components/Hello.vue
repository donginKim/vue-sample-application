<template>
  <div class="hello">
    <b-card
      header="Employee List"
      style="max-width: 40rem; margin: auto; margin-top: 10vh;"
      class="mb-2"
      border-variant="info"
      align="left">

      <b-form-group id="user-input">
        <b-container fluid>
          <b-row class="user-1">
            <b-col sm="10">
              <b-form-input v-model="newUser.name" type="text" placeholder=""/>
            </b-col>
            <b-col sm="2">
              <b-button variant="outline-primary" v-on:click="createUser">추가</b-button>
            </b-col>
          </b-row>
        </b-container>
      </b-form-group>

      <b-list-group v-if="users && users.length">
        <b-list-group-item
          v-for="user of users"
          v-bind:data="user.name"
          v-bind:key="user.id"
        >
          <b-form-checkbox
            v-model="user.done">
            {{user.name}}
          </b-form-checkbox>
        </b-list-group-item>
      </b-list-group>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios'
let baseUrl = 'http://10.1.1.178:8080/api/v1'
export default {
  name: 'hello',
  data () {
    return {
      users: [],
      newUser: {}
    }
  },
  methods: {
    allUserList: function () {
      let vm = this
      axios.get(baseUrl + '/users/')
        .then(response => {
          vm.users = response.data.map(r => r.data)
        })
        .catch(e => {
          console.log('error: ', e)
        })
    },
    createUser: function (event) {
      event.preventDefault()
      let vm = this
      if (!vm.newUser.name) return
      axios.put(baseUrl + '/users/add', vm.newUser)
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created () {
    this.allUserList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #35495E;
}
</style>
