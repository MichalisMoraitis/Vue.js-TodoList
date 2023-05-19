<template>
  <div class="todolist">
    <h1>Things Todo...</h1>
    <input placeholder="Search Todo " v-model="search" class="search" v-show="todos.length != 0">
    <div class="itemlist">
      <TransitionGroup>
        <TodoItem v-for="todo in todosOptionFilter"
          @delete="handleDelete"
          @toggleComplete="toggleComplete"
          :todo="todo"
          :key="todo.id"
        />
      </TransitionGroup>
    </div>
    <div class="new-item">
      <input placeholder="Add things to do" v-model="newTodoTitle" @keydown.enter="addTodo">
      <button @click="addTodo">
        <i class="fa fa-plus"></i>
      </button>
    </div>

    <div class="option">
      <button @click="this.option = 'All'"
        :class="[{ active: this.option === 'All' }]">
        <i class="fa fa-check"></i>{{ " " }}
        <i class="fa fa-square"> All</i> 
       </button>
      <button @click="this.option = 'COMPLETED'"
        :class="[{ active: this.option === 'COMPLETED' }]">
        <i class="fa fa-check"> Completed</i>
      </button>
      <button @click="this.option = 'UNCOMPLETED'"
        :class="[{ active: this.option === 'UNCOMPLETED' }]">
        <i class="fa fa-square"> Uncompleted</i>
      </button>
    </div>

    <div class="footer">
      <p>Completed {{ completedTodosLength }} of {{ todos.length }}</p>
      <button @click="deleteCompletedTodos" v-show="completedTodosLength > 0">
        <i class="fa fa-trash"> Delete Completed</i>
        
      </button>
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  components: { TodoItem },
  data() {
    return {
      todos: [
        { title: 'todo1', completed: true, id:1 },
        { title: 'todo2', completed: false, id:2 },
        { title: 'todo3', completed: false, id:3 }
      ],
      newTodoTitle: '',
      newId:4,
      search:'',
      option: "All"
    }
  },
  methods: {
    handleDelete(id){
      this.todos = this.todos.filter(todo => todo.id !== id);  
      console.log(this.todos);
    },
    addTodo() {
      if(this.newTodoTitle != ''){
        this.todos.push({ title: this.newTodoTitle, id:this.newId, completed:false });
        this.newTodoTitle = '';
        this.newId++;
        console.log(this.todos);
      }
    },
    toggleComplete(id){
      this.todos = this.todos.map(todo =>
        todo.id === id
          ? { ...todo, completed: !todo.completed }
          : todo
      );
      console.log(this.todos);
    },
    deleteCompletedTodos() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  },
  computed: {
    todosSearchFilter() {
      if (this.search === '') {
        return this.todos;
      }

      return this.todos.filter(todo => {
        if (todo.title.includes(this.search)) {
          return true;
        }
        return false;
      });
    },
    todosOptionFilter() {
      console.log(this.option);
      if(this.option === "All"){
        return this.todosSearchFilter;
      }else if(this.option === "COMPLETED"){
        return this.todosSearchFilter.filter(todo => {
          if (todo.completed) {
            return true;
          }
          return false;
        });
      }else{
        return this.todosSearchFilter.filter(todo => {
          if (!todo.completed) {
            return true;
          }
          return false;
        });
      }
    },
    completedTodosLength(){
      return this.todos.filter(todo => todo.completed).length;
    }
  }
}
</script>

<style scoped>
.todolist{
  background-color: #e9ecef;
  border-radius: 15px;
  max-width: 400px;
  margin: auto;
}
.itemlist{
  width: 370px;
  margin: auto;
}
h1{
  padding-top: 20px;
  text-align: center;
  font-weight: bold;
  color: #2a9d8f;
}
input.search{
  display: block;
  width: 350px;
  padding: 10px;
  font-size:15px;
  border-radius: 19px;
  border: 0;
  outline: 1px solid #2a9d8f;
  margin: 10px auto;
  font-size: 18px;
}
/* add new item */
.new-item{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.new-item input{
  width: 350px;
  outline: 0;
  border-radius: 30px;
  border: 1px solid #2a9d8f;
  padding: 10px;
  font-size: 18px;
}
.new-item button{
  display: block;
  cursor: pointer;
  border: 0px solid #2a9d8f;
  background-color: inherit;
  align-self: flex-end;
  margin-right: 16px;
  position: relative;
  top: -40px;
  font-size: 27px;
  color: #2a9d8f;
  padding:  4px 9px;
  border-radius: 20px;
}
.new-item button:hover{
  background-color: #2a9d8f;
  color: #fff;
}
/* option */
.option{
  display: flex;
  justify-content: space-around;
}
.option button{
  font-size: 13px;
  font-weight: bold;
  padding: 11px;
  cursor: pointer;
  border: 0;
  border-radius: 30px;
  background-color: #586f7c;
  color: white;
  position: relative;
  top: -31px;
  width: 107px;
}
.option button:hover{
  opacity: 0.95;
  box-shadow: 0px 3px 3px #aaa;
}
.option button.active{
  background-color: #2f4550;
}
/* footer */
.footer{
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding-bottom: 40px;
}
.footer p{
  text-decoration: underline;
  color: #2f4550;
}
.footer button{
  font-weight: bold;
  padding: 10px 20px;
  cursor: pointer;
  border: 0;
  border-radius: 20px;
  color: #ea3546;
  outline: 1px solid #ea3546;
}
.footer button:hover{
  background-color: #ea3546;
  color: white;
}
/* transition */

.v-enter-from {
  opacity: 0;
  translate:0 -50px;
}
.v-enter-to {
  opacity: 1;
  translate: 0 0;
}
.v-enter-active,.v-leave-active, .v-move{
  transition: all 0.7s;
}
.v-leave-from {
  opacity: 1;
  translate: 0 0;
}
.v-leave-active { position: absolute; }
.v-leave-to {
  opacity: 0;
  translate:0 50px;
}

</style>

