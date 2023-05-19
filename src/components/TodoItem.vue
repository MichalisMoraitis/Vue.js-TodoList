<template>
    <div class="todoItem">
        <input type="checkbox" :checked="todo.completed" @click="toggleComplete"/>
        <p :class="[{ completed: todo.completed }]" >
          {{ todo.title }}
        </p>
        <button @click="deleteItem">
          <i class="fa fa-trash"></i>
        </button>
    </div>
  </template>
  
  <script>
  export default {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    deleteItem() {
      this.$emit('delete', this.todo.id)
    },
    toggleComplete() {
      this.$emit('toggleComplete', this.todo.id)
    },
  }
  }
  </script>
  
  <style scoped>
  .todoItem{
    width: 370px;
    border-radius: 20px;
    border: 1px solid lightgrey;
    margin: 10px auto;
    display: flex;
    align-items: center;
    justify-content: space-around;
  }
  .todoItem:hover{
    box-shadow: 0px 3px 3px #aaa;
  }
  input{
    transform: scale(1.3);
    cursor: pointer;
  }
  p{
    font-size: 20px;
    margin: 10px;
    color: #2f4550;
  }
  button{
    background-color: inherit;
    border: 0;
    color: #ea3546;
    cursor: pointer;
    padding: 5px 9px;
    font-size: 20px;
    border-radius: 20px;
  }
  button:hover{
    /* outline: 1px solid #ea3546; */
    background-color: #ea3546;
    color: #fff;
  }
  .completed{
    text-decoration: line-through;
  }
  </style>
  