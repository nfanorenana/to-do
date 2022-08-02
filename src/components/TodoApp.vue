<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
        <div class="card border-0 shadow rounded-3 my-5">
          <div class="card-body p-4 p-sm-5">
            <h5 class="card-title text-center mb-4 fw-light fs-5">Todo App</h5>
            <div class="form-floating mb-3">
              <input
                type="text"
                v-model="task.title"
                class="form-control"
                placeholder="Title"
                id="floatingTitle"
              />
              <label for="floatingTitle">Title</label>
            </div>
            <div class="form-floating mb-3">
              <textarea
                class="form-control"
                v-model="task.detail"
                id="floatingDetail"
                placeholder="Detail"
                rows="3"
              ></textarea>
              <label for="floatingDetail">Detail</label>
            </div>
            <div v-if="error.status" class="alert text-center" role="alert">
              {{ error.message }}
            </div>
            <div class="d-grid mt-4">
              <button
                class="btn text-uppercase fw-bold custom-btn"
                @click="submitTask"
              >
                Add task
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="tasks.length !== 0">
      <hr class="my-4" />
      <div class="d-flex justify-content-between align-items-center mb-4">
        <span class="text-muted"
          >Number of task: <b>{{ counterTask() }}</b></span
        >
        <span class="text-muted"
          >Completed task: <b>{{ counterCompletedTask() }}</b></span
        >
      </div>

      <div class="row" v-for="(task, index) in tasks" :key="index">
        <div class="col-sm-9 col-md-10 col-lg-5 mx-auto">
          <div class="card border-0 shadow rounded-5 my-2">
            <div class="card-body p-2 p-sm-4">
              <div class="d-flex justify-content-between align-items-center">
                <h3 class="card-title mb-3">{{ task.title }}</h3>
                <ToggleButton
                  :id="'task_' + index"
                  :defaultState="task.status"
                  v-on:change="updateStatus(task)"
                />
              </div>
              <p class="card-text fw-light fs-6">{{ task.detail }}</p>
            </div>
            <div class="card-footer">
              <div class="row">
                <div
                  class="col-md-6 text-center"
                  v-b-modal="'edit-modal'"
                  @click="editTask(index)"
                >
                  <span class="fa fa-pen"></span>
                </div>
                <div class="col-md-6 text-center" @click="deleteTask(index)">
                  <span class="fa fa-trash"></span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <b-modal id="edit-modal" title="Update Task" hide-footer>
      <div class="form-floating mb-3">
        <input
          type="text"
          v-model="task.title"
          class="form-control"
          placeholder="Title"
          id="floatingTitle"
        />
        <label for="floatingTitle">Title</label>
      </div>
      <div class="form-floating mb-3">
        <textarea
          class="form-control"
          v-model="task.detail"
          id="floatingDetail"
          placeholder="Detail"
          rows="3"
        ></textarea>
        <label for="floatingDetail">Detail</label>
      </div>
      <div v-if="error.status" class="alert text-center" role="alert">
        {{ error.message }}
      </div>
      <div class="d-grid mt-4">
        <button
          class="btn text-uppercase fw-bold custom-btn"
          @click="
            submitTask();
            $bvModal.hide('edit-modal');
          "
        >
          Update task
        </button>
      </div>
    </b-modal>
  </div>
</template>

<script>
import ToggleButton from "./ToggleButton.vue";

export default {
  name: "TodoApp",

  components: {
    ToggleButton,
  },

  data() {
    return {
      isEditTask: false,
      editedTask: null,
      error: {
        status: false,
        message: "Title field is required!!",
      },
      task: {
        title: "",
        detail: "",
      },
      tasks: [],
      toggleValue: false,
    };
  },

  methods: {
    submitTask() {
      if (!this.task.title) {
        this.error.status = true;
      }

      if (this.task.title.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          title: this.task.title,
          detail: this.task.detail,
          status: false,
        });
      } else {
        this.tasks[this.editedTask].title = this.task.title;
        this.tasks[this.editedTask].detail = this.task.detail;
        this.editedTask = null;
      }
      this.task.title = "";
      this.task.detail = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.isEditTask = true;
      this.task.title = this.tasks[index].title;
      this.task.detail = this.tasks[index].detail;

      this.editedTask = index;
    },

    updateStatus(task) {
      task.status ? (task.status = false) : (task.status = true);
      console.log(task);
    },

    counterTask() {
      return this.tasks.length;
    },

    counterCompletedTask() {
      let count = 0;
      this.tasks.forEach((task) => {
        if (task.status) count++;
      });
      return count;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.custom-btn {
  background-color: #d5bdaf;
  color: #fff;
}

.custom-btn:hover {
  background-color: #e3d5ca;
  color: #fff;
}

.alert {
  width: 100%;
  margin: 10px auto;
  padding: 10px;
  position: relative;
  border-radius: 5px;
  color: #f7a7a3;
}
</style>
