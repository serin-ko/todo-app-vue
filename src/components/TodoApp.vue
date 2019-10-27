<template>
  <div class="app">
      <div class="header">
          <h1 class="title">TODO APP</h1>
          <form class="form" v-on:submit.prevent="addTodo">
              <input type="text" class="form-text" placeholder="할 일을 적어주세요" v-model="text" ref="text"> 
              <button type="submit" class="form-submit">추가</button>
          </form>
      </div>
      <div class="container">
          <todo-item v-for="todo in todos" v-bind:key="todo.text" v-bind:todo="todo" v-on:delete="deleteTodo" />
          
      </div>
      <div class="footer">
          <div class="remain-info">할 일이 <span class="remain-number">{{ remainTodo.length }}</span>개 남앗어여</div>
          <button type="button" class="delete-success-all" v-on:click="dsa">완료한 할 일 모두 삭제</button>
      </div>
  </div>
</template>
<script>
import TodoItem from "./TodoItem.vue";
export default {
    data() {
        return {
          text: '',
          todos: JSON.parse(localStorage.getItem('todos')) || [],
        }
    },
    watch: {
        todos() {
            localStorage.setItem('todos', JSON.stringify(this.todos));
        }
    },
    components: {
        TodoItem,
    },
    computed: {
        remainTodo() {
            return this.todos.filter((todo) => !todo.complete);
        }
    },
    methods: {
        addTodo() {
            if (this.text != '') {
              this.todos.push({
                text: this.text,
                complete: false,
              });
              this.text = "";
              this.$refs.text.focus();    
            }
        },
        deleteTodo(targetTodo) {
            this.todos = this.todos.filter((todo) => targetTodo !== todo);
        },
        dsa() {
            this.todos = this.remainTodo;
        }
    }
}
</script>
<style>
body{
    background-color: peachpuff;
    padding: 30px;
}

.app{
    width: 550px;
    background-color: white;
    padding: 35px 15px;
    margin: 0 auto;
    border-radius: 15px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.18);
}

.title{
    font-family: Helvetica;
    font-size: 45px;
    font-weight: bold;
    text-align: center;
    color: #343434;
}

.form{
    margin-top: 12px;
    border-radius: 10px;
    border: solid 1px #efefef;
    position: relative;
}

.form-text{
    height: 60px;
    width: 100%;
    display: block;
    padding: 0 22px;
    font-size: 16px;
    color: #343434;
}

.form-submit{
    width: 100px;
    height: 36px;
    border-radius: 10px;
    background-color: #ff7f50;
    font-size: 16px;
    color: #ffffff;
    position: absolute;
    right: 14px;
    top: 50%;
    transform: translateY(-50%);
}

.container{
    margin-top: 15px;
    height: 300px;
    overflow-y: scroll;
}

.item{
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid #efefef;
    padding-left: 30px;
    position:relative;
}

.item-title{
    font-size: 16px;
    color: #343434;
}

.item-input{
    position: absolute;
    left: 6px;
    top: 50%;
    transform: translateY(-50%);
}

.item-delete{
    opacity: 0;
    background-image: url(../assets/remove.png);
    width: 25px;
    height: 25px;
    text-indent: -9999px;
    background-size: 100%;
    position: absolute;
    right: 5px;
    top: 50%;
    transform: translateY(-50%);
    transition: opacity 300ms;
}

.item:hover .item-delete{
    opacity: 1;
}

.item-input:checked + .item-title{
    color: #999999;
    text-decoration: line-through;
}

.delete-success-all, .remain-info{
    color:#999;
    font-size: 14px;
}

.remain-info{
    float: left;
}

.delete-success-all{
    float:right;
}

.remian-number{
    font-weight: bold;
}

.footer{
    margin-top:10px;
}

.footer::after{
    display:block;
    content:'';
    clear: both;
}
</style>