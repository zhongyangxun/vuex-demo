<template>
  <div class="todo-list-wrapper">
    <ul class="todo-list" v-if="todoList.length">
      <li v-for="(item, index) in todoList" :key="index" class="todo-item" :class="{'done': item.done}" @click="selectItem(item)">
        <div class="checkbox-wrapper">
          <input type="checkbox" v-model="item.checked">
        </div>
        <div class="text-wrapper"><div class="text">{{item.text}}</div></div>
        <div class="menu-wrapper">
          <ul class="menu">
            <li v-if="!item.done" class="menu-item done" @click.stop="doneItem(item)">完成</li>
            <li v-if="item.done" class="menu-item reset" @click.stop="undoneItem(item)">重置</li>
          </ul>
        </div>
      </li>
    </ul>
    <div class="tip" v-else>添加第一件要做的事吧 : )</div>
    <div class="btn-group">
      <button class="btn btn-default btn-add" @click="addItem">添加</button>
      <button class="btn btn-complete btn-success" @click="doneSelected">完成</button>
      <button class="btn btn-delete btn-danger" @click="deleteSelected">删除</button>
    </div>
  </div>
</template>

<script>
  import { mapGetters, mapMutations } from 'vuex'

  export default {
    computed: {
      ...mapGetters([
        'todoList'
      ])
    },
    methods: {
      doneItem (item) {
        item.done = true
      },
      undoneItem (item) {
        item.done = false
      },
      addItem () {
        let itemText  = window.prompt('请输入要添加的事项：')
        if (itemText) {
          let list = this.todoList.slice();
          list.push({
            text: itemText,
            done: false,
            checked: false
          })
          this.setTodoList(list)
        }
      },
      selectItem (item) {
        item.checked = !item.checked
      },
      deleteSelected () {
        if (window.confirm('确定删除所选事项吗？')) {
          let newList = this.todoList.filter ((item) => !item.checked)
          this.setTodoList(newList)
        }
      },
      doneSelected () {
        this.todoList.map((item) => {
          if (item.checked && !item.done) {
            item.done = true
            item.checked = false
          }
        })
      },
      ...mapMutations({
        setTodoList: 'SET_TODO_LIST'
      })
    }
  }
</script>

<style lang="css" scoped>
  .todo-list-wrapper {
    max-width: 500px;
    margin: 0 auto;
  }

  ul {
    list-style: none;
    padding: 0;
  }

  .todo-item {
    display: flex;
    border: 1px solid #ccc;
    color: #333;
    font-size: 30px;
    padding: 10px 20px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  .todo-item.done {
    color: #aaa;
  }

  .todo-item:hover {
    background-color: #eee;
    transform: scale(1.02);
    box-shadow: 2px 1px 5px #ddd;
  }

  .todo-item + .todo-item {
    margin-top: 15px;
  }

  .todo-item.done:hover {
    transform: none;
    box-shadow: none;
  }

  .todo-item.done .text {
    text-decoration: line-through;
  }

  .text-wrapper {
    flex-grow: 1;
  }

  .checkbox-wrapper,
  .menu-wrapper {
    flex-shrink: 0;
  }

  .menu {
    list-style: none;
    display: flex;
    font-size: 16px;
    position: absolute;
    align-items: stretch;
    padding: 0;
    top: 0;
    right: 0;
    bottom: 0;
    transform: translateX(100%);
    transition: transform .3s;
  }

  .todo-item:hover .menu {
    transform: translateX(0);
  }

  .menu-item {
    display: flex;
    align-items: center;
    padding: 10px;
  }

  .menu-item.done {
    background-color: #5cb85c;
    color: #fff;
  }

  .menu-item.done:hover {
    background-color: #449d44;
  }

  .menu-item.reset {
    color: #333;
    background-color: #ccc;
  }

  .menu-item.reset:hover {
    background-color: #aaa;
  }

  .btn-group {
    margin-top: 40px;
  }

  .btn {
    border-radius: 3px;
    padding: 5px 15px;
    font-size: 16px;
    box-shadow: none;
    cursor: pointer;
    border: 1px solid;
    margin: 10px 20px;
  }

  .btn-default {
    color: #333;
    background-color: #fff;
    border-color: #ccc;
  }

  .btn-default:hover {
    background-color: #e6e6e6;
    border-color: #adadad;
  }

  .btn-success,
  .btn-danger {
    color: #fff;
  }

  .btn-success {
    background-color: #5cb85c;
    border-color:#4cae4c;
  }

  .btn-success:hover {
    background-color: #449d44;
    border-color: #255625;
  }

  .btn-danger {
    background-color: #d9534f;
    border-color: #d43f3a;
  }

  .btn-danger:hover {
    background-color: #ac2925;
    border-color: #ac2925;
  }

</style>
