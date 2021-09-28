<template>
  <div class="container">
    <div class="row">
      <div class="col 12">
        <table class="table table-bordered mt5">
          <thead>
            <tr>
              <th>Id</th>
              <th>Content</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, i) in todoList" :key="i">
              <td class="table-danger">{{ item.id }}</td>
              <td>{{ item.content }}</td>
              <td class="align-middle text-center w-75">
                <div class="d-flex">
                  <button
                    class="btn btn-info btn-sm mx-1"
                    @click="handleEdit(item.id)"
                  >
                    Edit
                  </button>
                  <button
                    class="btn btn-danger btn-sm mx-1"
                    @click="handleDelete(item.id)"
                  >
                    Delete
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="row">
          <div class="col-6 align-middle text-center w-75">
            <div class="form-group">
              <label for="">{{ editMode ? "Edit" : "Add" }}</label>
              <input
                type="text"
                class="form-control"
                v-model="todoItem.content"
              />
            </div>
          </div>
          <div class="col-6 align-middle text-center w-25">
            <button class="btn btn-primary btn-sm mx-1" @click="handleToDoItem">
              {{ editMode ? "Edit" : "Add" }}
            </button>

            <button
              v-if="editMode"
              class="btn btn-danger btn-sm mx-1"
              @click="handleCancel"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
const todoUrl = "http://localhost:3500/todo";
export default {
  name: "MainComponent",
  data() {
    return {
      todoList: [],
      todoItem: {},
      editMode: false
    };
  },
  async created() {
    const res = await Axios.get(todoUrl);
    this.todoList = res.data;
  },
  methods: {
    edit() {},
    handleEdit(id) {
      this.editMode = true;
      const item = this.todoList.find(i => i.id === id);
      this.todoItem = item;
    },
    handleDelete(id) {
      const index = this.todoList.findIndex(i => i.id === id);
      this.todoList.splice(index, 1);
    },
    handleToDoItem() {
      if (this.editMode === true) {
        console.log('aqui se edita');
      } else {
        this.todoList.push({
          id: this.todoList.length + 1,
          content: this.todoItem.content
        });
        this.todoItem.content = "";
      }
    },
    handleCancel() {
      this.editMode = false
      this.todoItem = {}
    }
  }
};
</script>

<style scoped>
.table > :not(caption) > * > * {
  background-color: none;
}
</style>
