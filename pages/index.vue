<template>
  <section class="mission">
    <h1 class="title">チュートリアルToDoリスト</h1>

    <div>
      <h2 class="sub-title">ToDoリスト</h2>

      <div class="todo_list_radio" id="todo_list_radio">
        <input type="radio" :value="-1" v-model="radioValue" />すべて

        <input type="radio" :value="0" v-model="radioValue" />未完了

        <input type="radio" :value="1" v-model="radioValue" />完了

        <p class="number">({{ filteredTodo.length }}件)</p>
      </div>

      <div class="container">
        <h3>作成日時</h3>
        <h3>メモ</h3>
      </div>

      <ul>
        <li
          class="contents"
          id="contents"
          v-for="todo in filteredTodo"
          :key="todo.id"
        >
          <input type="checkbox" v-model="todo.isDone" />
          <p>{{ todo.date }}</p>
          <p>{{ todo.note }}</p>

          <img
            v-on:click="doRemove(todo)"
            class="contents-img"
            src="~/assets/trash.svg"
          />
        </li>
      </ul>

      <div class="add">
        <h2>新しい作業の追加</h2>

        <div id="add_box" class="add-box">
          <p>メモ</p>
          <input
            v-model="searchText"
            type="text"
            class="box"
            placeholder="内容を入力してください"
          />
          <button
            :disabled="!searchText"
            v-on:click="addTodo"
            type="button"
            class="button"
          >
            追加
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      searchText: "",
      processing: false,
      radioValue: -1,
    };
  },

  methods: {
    addTodo() {
      const now = new Date();
      this.todos.push({
        isDone: false,
        date: now.toLocaleString(),
        note: this.searchText,
      });
    },

    doRemove: function (todo) {
      var todos = this.todos;
      var index = todos.indexOf(todo);
      todos.splice(index, 1);
    },

    noText() {
      if (this.searchText == "") {
        return false;
      } else {
        return true;
      }
    },
    
  },

  computed: {
    filteredTodo: function () {
      if (this.radioValue === -1) {
        return this.todos;
      } else if (this.radioValue === 0) {
        return this.todos.filter((todo) => !todo.isDone);
      } else if (this.radioValue === 1) {
        return this.todos.filter((todo) => todo.isDone);
      }
      return []
    },
  },
};
</script>

<style>
.mission {
  padding: 20px 150px;
}

.title {
  padding-bottom: 5%;
}

.todo_list_radio {
  display: flex;
  align-items: center;
}

.todo_list_radio input {
  left: 0;
  margin-left: 50px;
}

.number {
  margin-left: 50px;
}

.container {
  margin-left: 85px;
  display: flex;
  gap: 105px;
}

.contents {
  display: flex;
  gap: 30px;
  align-items: center;
}

.contents-img {
  position: absolute;
  margin-left: 650px;
}

.add-box {
  display: flex;
  align-items: center;
  gap: 30px;
}

.box {
  width: 300px;
  height: 20px;
}

.button {
  background-color: rgb(9, 217, 245);
  border-radius: 15px;
  border: none;
  padding: 3px 20px;
}
</style>