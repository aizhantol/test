<template>
  <div id="app">
    <h2 class="mb20">To Do</h2>
    <Search @search="search" />
    <Switcher @change="filter" />
    <div class="row">
      <button @click="removeAll" class="mb20 red">Удалить помеченные</button>
      <button
        v-if="!showNew"
        type="inline"
        class="mb20"
        @click="showNew = !showNew"
      >
        <img src="@/assets/plus.svg" alt="" />
        Добавить
      </button>
    </div>
    <div class="row mb20" v-if="showNew">
      <div>{{ items.length + 1 }}.</div>
      <input v-model="data.todo" placeholder="Todo" class="sel" />
      <button type="inline" class="plus" @click="save">
        <img src="@/assets/plus.svg" alt="" />
      </button>
    </div>
    <ol>
      <Todo
        v-for="(item, i) in items"
        :key="item.id"
        :item="item"
        @completed="completed(i)"
        @remove="remove(i)"
        @edit="edit(item)"
      />
    </ol>
  </div>
</template>

<script>
import Todo from "./components/Todo";
import Search from "./components/Search";
import Switcher from "./components/Switcher";
import axios from "axios";

export default {
  components: { Todo, Search, Switcher },
  name: "App",
  data() {
    return {
      showNew: false,
      data: { todo: "" },
      items: []
    };
  },
  mounted() {
    console.log("d");
    axios
      .get("https://dummyjson.com/todos")
      .then(res => {
        this.items = res.data.todos.splice(0, 10);
        this.items.forEach(e => (e.checked = false));
        this.oldItems = [...this.items];
        console.log(res);
      })
      .catch(e => {
        console.log(e);
      });
  },
  methods: {
    filter(val) {
      if (val === "выполненные") {
        this.items = this.oldItems.filter(e => e.completed);
      } else if (val === "не выполненные") {
        this.items = this.oldItems.filter(e => !e.completed);
      } else {
        this.items = [...this.oldItems];
      }
    },
    search(val) {
      if (val) {
        this.items = this.oldItems.filter(e => e.todo.includes(val));
      } else {
        this.items = [...this.oldItems];
      }
    },
    remove(data) {
      this.items.splice(data, 1);
      this.oldItems.splice(data, 1);
    },
    edit(data) {
      this.data = data;
      this.showNew = true;
    },
    removeAll() {
      this.items = this.items.filter(e => !e.checked);
      this.oldItems = this.oldItems.filter(e => !e.checked);
    },
    save() {
      if (!this.data.id) {
        this.data.checked = false;
        this.data.completed = false;
        this.data.id = this.oldItems.length + 1;
        this.items.push(this.data);
        this.oldItems.push(this.data);
      }

      this.data = { todo: "" };
      this.showNew = false;
    },
    completed(data) {
      this.$set(this.items, data, {
        ...this.items[data],
        completed: !this.items[data].completed
      });

      this.$set(this.oldItems, data, {
        ...this.oldItems[data],
        completed: !this.oldItems[data].completed
      });
    }
  }
};
</script>

<style>
button {
  border: 1px solid transparent;
  outline: none;
  appearance: none;
  padding: 15px 0;
  text-align: center;
  border-radius: 30px;
  font-weight: 600;
  text-decoration: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  width: 260px;
  background: #00c48c;
  color: white;
  height: 50px;
  align-items: center;
  gap: 12px;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.mb20 {
  margin-bottom: 20px;
}
.comp {
  text-decoration: line-through;
}
.icon {
  width: 30px;
  height: 30px;
  background: #00c48c;
  padding: 7px;
  border-radius: 50%;
  box-shadow: 0px 10px 30px rgba(0, 196, 140, 0.15);
}
.plus {
  width: 30px;
  height: 30px;
  background: #00c48c;
  padding: 7px;
  border-radius: 50%;
  box-shadow: 0px 10px 30px rgba(0, 196, 140, 0.15);
}
.row {
  display: flex;
  align-items: center;
  gap: 16px;
}
input {
  height: 40px;
  outline: none;
  padding: 0 5px;
}
.sel {
  background: white;
  border-radius: 10px;
  box-shadow: 0px 10px 30px rgba(138, 149, 158, 0.2);
  border-radius: 16px;
  width: 600px;
  border: none;
}
.red {
  background: red;
}
</style>
