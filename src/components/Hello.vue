<template>
  <div class="container">
    <div class="text-center">
      <img src="../assets/logo.png">
    </div>
    <div class="jumbotron">
      <h2 class="text-center">Todos</h2>
      <div class="panel panel-primary">
        <div class="panel-heading"><span class="glyphicon glyphicon-paperclip"></span> Demo todo app with Vue.js</div>
          <div class="panel-body">
            <div class="row container">
              <label for="txttodo">Name: </label>
              <input type="text" id="txttodo" placeholder="What you want to do ?" class="form-control" v-on:keyup.enter="addTodo()" v-model="todoText">                  </div>
            <hr>
            <div class="row">
<!--binding data-->
              <div class="col-md-6">
                <label for="">Todos list:</label>
                  <ul class="list-group">
                      <!--all todos-->
                      <li class="list-group-item btnDel col-md-12" v-for="item in todos" v-show="isAll">
                          <todo-list v-bind:item="item" v-bind:class="{ complete: item.status }"></todo-list>
                          <a title="Delete" class="pull-right" v-on:click="deleteTodo(item)">X</a>
                          <a title="Complete" class="pull-right" v-on:click="completeTodo(item)">V</a>
                      </li>
                      <!--active todos-->
                      <li class="list-group-item btnDel col-md-12" v-for="item in activeTodoList" v-show="isActive">
                          <todo-list v-bind:item="item"></todo-list>
                          <a title="Delete" class="pull-right" v-on:click="deleteTodo(item)">X</a>
                          <a title="Complete" class="pull-right" v-on:click="completeTodo(item)">V</a>
                      </li>
                      <!--complete todos-->
                      <li class="list-group-item btnDel col-md-12" v-for="item in completeTodoList" v-show="isComplete">
                          <todo-list v-bind:item="item" class="complete"></todo-list>
                          <a title="Delete" class="pull-right" v-on:click="deleteTodo(item)">X</a>
                          <a title="Complete" class="pull-right" v-on:click="completeTodo(item)">V</a>
                      </li>
                      <div class="col-md-2 del" v-show="isComplete">
                          <button type="button" class="btn btn-sm btn-danger" v-on:click="deleteAllComplete">Delete all complete <span class="badge">{{ completeTodoList.length }}</span></button>
                      </div>
                  </ul>
              </div>
              <!--something-->
              <div class="col-md-4 col-xs-offset-1">
                <div class="btn-group  btn-group-justified" role="group">
                  <div class="btn-group">
                    <button type="button" class="btn btn-info" v-on:click="allTodoList">All <span class="badge">{{ todos.length }}</span></button>
                  </div>
                  <div class="btn-group">
                    <button type="button" class="btn btn-warning" v-on:click="activeList">Active <span class="badge">{{ activeTodoList.length }}</span></button>
                  </div>
                  <div class="btn-group">
                    <button type="button" class="btn btn-success" v-on:click="completeList">Complete <span class="badge">{{ completeTodoList.length }}</span></button>
                  </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {
    'todo-list': {
      data () {
        return {
          editMode: false
        }
      },
      props: ['item'],
      template: '<font><span v-show="!editMode" @dblclick="clicked()" title="Double click to edit">Task {{ item.id }}: {{ item.name }} </span> <input class="form-control input-sm" v-on:keyup.enter="saveEdit()" v-model="item.name" v-show="editMode" /></font>',
      methods: {
        clicked: function () {
          this.editMode = true
        },
        saveEdit: function () {
          this.editMode = false
        }
      }
    }
  },
  name: 'app1',
  data () {
    return {
      todoText: '',
      addId: 1,
      todos: [
        { id: 0, name: 'This default message', status: false }
      ],
      isAll: true,
      isActive: false,
      isComplete: false
    }
  },
  computed: {
    activeTodoList: function (todos) {
      return this.todos.filter(function (todo) {
        return !todo.status
      })
    },
    completeTodoList: function (todos) {
      return this.todos.filter(function (todo) {
        return todo.status
      })
    }
  },
  methods: {
    addTodo: function () {
      let todoText = this.todoText.trim()
      if (todoText) {
        let id = this.addId++
        this.todos.push({ id: id, name: todoText, status: false })
        this.todoText = ''
      }
    },
    deleteTodo: function (item) {
      let pos = this.todos.indexOf(item)
      this.todos.splice(pos, 1)
    },
    completeTodo: function (item) {
      item.status = !item.status
    },
    allTodoList: function () {
      this.isAll = true
      this.isActive = false
      this.isComplete = false
    },
    activeList: function () {
      this.isAll = false
      this.isActive = true
      this.isComplete = false
    },
    completeList: function () {
      this.isAll = false
      this.isActive = false
      this.isComplete = true
    },
    deleteAllComplete: function () {
      this.todos.splice(0, this.completeTodoList.length)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
  color: #42b983;
}
body {
    margin-top: 50px;
}

li.btnDel a{
    display: none;
    text-decoration: none;
}
.complete{
    color: blue;
}
li.btnDel:hover a:nth-child(2n+1){
    color: green;
    font-weight: bold;
    display: inline-block;
    margin-right: 15px;
    cursor: pointer;
}
li.btnDel:hover a:nth-child(2n+2){
    color: red;
    font-weight: bold;
    display: inline-block;
    cursor: pointer;
}
li:hover {
    cursor: default;
}
.del {
    margin-top: 10px;
    margin-left: -2.8%;
}
</style>
