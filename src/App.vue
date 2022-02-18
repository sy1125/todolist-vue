<template>
  <div id="app">
    <Modal v-show="showModal" v-on:close="shhowModal = false">
      <template v-slot:modal-text>{{modalText}}</template>
    </Modal>
    <div class="top-area">
      <div v-if="userName">
        <TodoHeader />
        <TodoTitle v-bind:propCount="checkCount" v-bind:propName="userName" />
        <TodoInput v-on:addItem="addOneItem" />
      </div>
      <div v-else>
        <TodoHello v-on:addName="addUserName" />
      </div>
    </div>
    <div class="bottom-area">
      <TodoController v-on:sortItem="sortAllItem" v-on:clearAll="clearAllItem" />
      <TodoList v-bind:propsdata="todoItems" @removeItem="removeOneItem" @toggleItem="toggleOneItem" v-bind:propEmpty="isEmpty" />
      <TodoFooter />
    </div>

  </div>
</template>
 
<script>
import TodoHeader from "./components/TodoHeader";
import TodoTitle from "./components/TodoTitle";
import TodoInput from "./components/TodoInput";
import TodoController from "./components/TodoController";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";
import getDate from "./assets/commonJS/getDate.js";
import TodoHello from "./components/TodoHello";
import Modal from "./components/common/Modal";

export default{
  data() {
    return{
      todoItems:[],
      userName:'',
      showModal:false,
      modalText:""
    };
  },
  // created() {
  //   this.userName = localStorage.getItem("userName");
  //   if (localStorage.length > 0) {
  //     for (let i = 0; i < localStorage.length; i++) {
  //       if (localStorage.key(i) !== "userName") {
  //         this.todoItems.push(
  //           JSON.parse(localStorage.getItem(localStorage.key(i))));
  //       }
  //     }
  //   }
  // },
  mounted() {
    this.sortTodoOldest();
  },
  computed: {
    isEmpty() {
      return this.todoItems.length <= 0 ? true : false;
    },
    checkCount() {
      const checkLeftItems = () => {
        let leftCount = 0;
        for(let i = 0; i < this.todoItems.length; i++) {
          if (this.todoItems[i].completed === false) {
            leftCount++;
          }
        }
        return leftCount;
      };
      const count = {
        total: this.todoItems.length,
        left: checkLeftItems()
      };
      return count;
    }
  },
  methods:{
    addUserName(userName) {
      localStorage.setItem("userName", userName);
      this.userName = userName;
    },
    removeOneItem(todoItem, index) {
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem(todoItem) {
      todoItem.completed = !todoItem.completed;
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    addOneItem(todoItem) {
      if (todoItem === "") {
        this.showModal = !this.showModal;
        this.modalText = "The form is empty. Please enter your task";
        return false;
      }
      for (let i = 0; i < this.todoItems.length; i++) {
        if (this.todoItems[i].items === todoItem) {
          this.showModal = !this.showModal;
          this.modalText = "I think you`ve already had the task.";
          return false;
        }
      }
      var value = {
        item: todoItem,
        date: `${getDate().date} ${getDate().week}`,
        time: getDate().time,
        completed: false
      };
      localStorage.setItem(todoItem, JSON.stringify(value));
      this.todoItems.push(value);
    },
    clearAllItem() {
      this.todoItems = [];
      localStorage.clear();
    },
    sortTodoLatest() {
      this.todoItems.sort(function(a, b) {
        return b.time - a.time;
      });
    },
    sortTodoOldest() {
      this.todoItems.sort(function(a, b) {
        return a.time - b.time;
      });
    },
    sortAllItem(selectedSort) {
      if(selectedSort.value === "date-desc") {
        this.sortTodoLatest();
      } else if (selectedSort.value === "date-asc") {
        this.sortTodoOldest();
      }
    }
  },

  
  components: {
    TodoHeader,
    TodoTitle,
    TodoInput,
    TodoController,
    TodoList,
    TodoFooter,
    TodoHello,
    Modal
  }
};
</script>
