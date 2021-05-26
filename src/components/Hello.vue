<template>
  <div class="hello">
    <b-card
      header="What to do today 📝"
      style="max-width: 40rem; margin: auto; margin-top: 10vh"
      class="mb-2"
      border-variant="info"
      align="left"
    >
      <b-form-group id="to-do-input">
        <b-container fluid>
          <b-row class="my-2 mb-4">
            <b-col sm="10">
              <b-form-input
                v-model="newToDoItemRequest.title"
                type="text"
                placeholder="Write down new things to do."
                v-on:keyup.enter="createToDo"
              />
            </b-col>
            <b-col lg="2">
              <b-button variant="outline-info" v-on:click="createToDo"> Add </b-button>
            </b-col>
          </b-row>
        </b-container>
      </b-form-group>

      <b-list-group v-if="toDoItems && toDoItems.length">
        <b-list-group-item
          v-for="toDoItem of toDoItems"
          v-bind:data="toDoItem.title"
          v-bind:key="toDoItem.id"
        >
          <b-form-checkbox v-model="toDoItem.done" class="m-2">
            {{toDoItem.title}}
          </b-form-checkbox>
        </b-list-group-item>
      </b-list-group>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios'

let baseUrl = 'http://127.0.0.1:5000/todo/'
export default {
  name: 'hello',
  data: () => {
    return {
      toDoItems: [],
      newToDoItemRequest: {}
    }
  },
  methods: {
    initToDoList: function () {
      let vm = this
      axios.get(baseUrl)
        .then(response => {
          vm.toDoItems = response.data.map(r => r.data)
        })
        .catch(e => {
          console.log('error : ', e)
        })
    },
    createToDo: function (event) {
      event.preventDefault()
      let vm = this
      if (!vm.newToDoItemRequest.title) return
      axios.post(baseUrl, vm.newToDoItemRequest)
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created () {
    this.initToDoList()
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

