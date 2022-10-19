<template>
  <section class="mission">
    <h1 class="title">チュートリアルToDoリスト</h1>

    <div>
      <h2 class="sub-title">ToDoリスト</h2>

      <div class="todo_list_radio" id="todo_list_radio">
        <input type="radio" value="all" v-model="radioValue" />
        <p class="todo_radio">すべて</p>

        <input type="radio" value="yet" v-model="radioValue" />
        <p class="todo_radio">未完了</p>

        <input type="radio" value="done" v-model="radioValue" />
        <p class="todo_radio">完了</p>

        <p class="number">({{ filteredTodo.length }}件)</p>
      </div>

      <div class="container">
        <p class="made-time">作成日時</p>
        <p class="memo">メモ</p>
      </div>

      <ul class="list">
        <li
          class="contents"
          id="todo.date"
          v-for="todo in filteredTodo"
          :key="todo.id"
        >
          <input type="checkbox" v-model="todo.isDone" />
          <p class="todo-date">
            <span class="year">{{ todo.Year }}.</span>{{ todo.Month }}.{{ todo.Date }}.{{ todo.Hour }}:{{
              todo.Min
            }}
          </p>
          <p class="todo-note">{{ todo.note }}</p>

          <img
            @click="doRemove(todo)"
            class="contents-img"
            src="~/assets/trash.svg"
          />
        </li>
      </ul>

      <div class="add">
        <h2>新しい作業の追加</h2>

        <div id="add_box" class="add-box">
          <label>メモ</label>
          <input
            v-model="searchText"
            type="text"
            class="box"
            placeholder="内容を入力してください"
          />
          <button
            :disabled="!searchText"
            @click="addTodo"
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
      radioValue: "all",
    };
  },

  methods: {
    addTodo() {
      const now = new Date();
      this.todos.push({
        isDone: false,
        Year: now.getFullYear(),
        Month: now.getMonth() + 1,
        Date: now.getDate(),
        Hour: now.getHours(),
        Min: now.getMinutes(),
        note: this.searchText,
      });
      this.searchText = null;
    },

    doRemove(todo) {
      const index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
  },

  computed: {
    filteredTodo() {
      if (this.radioValue === "all") {
        return this.todos;
      } else if (this.radioValue === "yet") {
        return this.todos.filter((todo) => !todo.isDone);
      } else if (this.radioValue === "done") {
        return this.todos.filter((todo) => todo.isDone);
      }
      return [];
    },
  },
};
</script>

<style>
.mission {
  margin-left: 400px;
  margin-top: 96px;
}

.title {
  padding-bottom: 48px;
}

.todo_list_radio {
  display: flex;
  align-items: center;
  margin-bottom: 24px;
}

.todo_list_radio  input{
  margin: 0;
}

.todo_radio {
  width: 50px;
  margin-left: 8px;
  margin-right: 24px;
  font-weight: 300;
}

.list {
  width: 382px;
  padding-left: 0;
}

.number {
  left: 45px;
}

.container {
  margin-left: 45px;
  display: flex;
  font-weight: 600;
}

.made-time {
  width: 96px;
  margin-right:24px;
}

.contents {
  display: flex;
  align-items: center;
}

.todo-date {
  width: 200px;
  font-size: 14px;
}

.todo-note {
  width: 382px;
  height: 21px;
  font-size: 14px;
  margin-left: 20px;
}

.contents-img {
  margin-left: 24px;
}

.add-box {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 30px;
}

.box {
  width: 200px;
  height: 20px;
}

.button {
  color: #fff;
  background-color: #808080;
  border-radius: 15px;
  border: none;
  padding: 3px 20px;
}

@media screen and (max-width: 480px) {
  .mission {
    margin-left: 16px;
  }
  
  .todo_radio {
    font-size: 14px;
  }
  .year{
    display:none;
  }
  .container {
    font-size:14px;
  }
  .todo-date{
    margin-left:23px;
    width:96px;
  }
  .todo-note {
    width:132px;
  }
  .memo{
    width:50px;
  }
  .button {
    display: block;
  }
}
</style>