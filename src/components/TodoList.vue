<template>
 <div class="todoApp">
 <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">

  <div v-for="(todo , index) in todosFiltered" :key="index" class="todo-item">
      <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed">
       <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
       class="todo-item-label" :class="{ completed : todo.completed}">{{ todo.title }}</div>
       <input v-else class="todo-item-edit" type="text" 
       v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)">
      </div>
    <div class="remove-item">
       <button @click="removeTodo(index)">Delete</button>
    </div>
  </div>

  <div class="extra-container">
      <div><label> <input type="checkbox" :checked="!anyRemaining"
      >Check All</label></div>
      <div>{{ remaining }} items left</div>
  </div>

 <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>

      <div>
        <transition name="fade">
        <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>

    </div>
 </div>
</template>

<script>
export default {
  name: 'todoApp',
  data(){
      return{
          newTodo : '',
          idForTodo : 3,
          filter: 'all',
          todos : [
              {
                  'id' : 1 ,
                  'title' : 'Finish Vue Screencast',
                  'completed' : false,
                  'editing' : false
              },
                 {
                  'id' : 2 ,
                  'title' : 'Take over world',
                  'completed' : false,
                  'editing' : false
              },
          ]
      }
  },
  methods: {
      addTodo(){
          if(this.newTodo.trim().length == 0){
              return
          }
          this.todos.push({
              id : this.idForTodo,
              title : this.newTodo,
              completed : false
          })
           this.newTodo = ''
           this.idForTodo ++
      },
      editTodo(todo){
          todo.editing = true
      },
      doneEdit(todo){
          todo.editing = false
      },
     removeTodo(index){
         this.todos.splice(index,1)
     },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  computed : {
    remaining(){
          return this.todos.filter( todo => !todo.computed).length
      },
    anyRemaining() {
      return this.remaining != 0
    },
    // checkAlltodos(){
    //     this.todos.forEach((todo) => todo.completed = 
    //     event.target.checked)
    // }
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
       showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
 @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}
.remote-item{
    cursor: pointer;
    margin-left: 14px;
}
.todo-item-left{
    display: flex;
    align-items: center;
}
.todo-item-label{
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
}
.todo-item-edit{
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
}
.completed{
    text-decoration: line-through;
    color: grey;
}
.extra-container{
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgray;
    padding-top: 14px;
    margin-bottom: 14px;
}
button{
    font-size: 14px;
    background-color: white;
    appearance: none;
}
.active {
    background: lightgreen;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

</style>
