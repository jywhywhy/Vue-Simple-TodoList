<template>
  <div id="app">
    <div class="container">
      <div class="col-md-6 offset-md-3">
        <h1 class="text-center mb-4">Todo 어플리케이션</h1>
        <input type="text" class="form-control mb-4" v-model="userInput" @keyup.enter="addNewTodo">

        <div class="list-group mb-4">
          <template v-for="(todo, idx) in activeTodoList">
            <Todo v-bind:key="idx"
            :label="todo.label"
              @componentClick="toggleTodoState(todo)"
            />
          </template>
        </div>

        <div class="text-right">
          <button type="button" class="btn btn-sm" @click="changeCurrentState('active')">할 일</button>
          <button type="button" class="btn btn-sm" @click="changeCurrentState('done')">완료</button>
          <button type="button" class="btn btn-sm" @click="changeCurrentState('all')">전체</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Todo from './components/Todo';

export default {
  name: 'App',
  data() {
    return {
      userInput: '',
      todoList: [],
      currentState: 'active'
    }
  },
  computed: { // getter 처럼 동작
    activeTodoList() {
      return this.todoList.filter(todo => this.currentState === 'all' || todo.state === this.currentState);
      // 전체를 누르면 모두 출력, 할 일이나 완료를 누르면 해당 상태값을 가지고 있는 todo만 출력
      // 초기값을 active로 설정했기 때문에 처음엔 active 상태인 값만 보임
      // 완료를 클릭하면 상태가 done으로 바뀌기 때문에 done(완료) 상태인 것만 보임
      // 전체를 클릭하면 상태가 all로 바뀌기 대문에 all(전체)가 보임
    }
  },
  methods: {
    changeCurrentState(state) {
      this.currentState = state;
    },
    addNewTodo() {
      this.todoList.push({
        label: this.userInput,
        state: 'active'
      })
      this.userInput = '';
    },
    toggleTodoState(todo) {
      todo.state = todo.state === 'active' ? 'done' : 'active'
    },
  },
  components: {
    Todo
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
