<template>
  <div class="container">
    <div class="pt-5 text-center">
      <h2 class="text-center">TO-DO LIST</h2>
      <!-- INPUT -->
      <div class="d-flex">
        <input
          v-model="task"
          type="text"
          placeholder="Enter task"
          class="rounded w-100"
        />

        <span @click="submitTask" class="border circle"><i style="font-size:30px;" class="bi text-success bi-plus-lg"></i></span>

      </div>

      <!-- TASK TABLE -->
      <table class="table mt-5">
        <thead>
          <tr>
            <th>#</th>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col">Edit</th>
            <th scope="col">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <th scope="row">{{ index }}</th>
            <th scope="row">
              <span :class="{ Done: task.status === 'Done' }">
                {{ task.name }}
              </span>
            </th>
            <td style="width: 100px">
              <span
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-success': task.status === 'Done',
                  'text-warning': task.status === 'Under-way',
                }"
                class="click"
                @click="changeStatus(index)"
                >{{ task.status }}</span
              >
            </td>
            <td>
              <div @click="editTask(index)">
                <i class="bi bi-pencil-square"></i>
              </div>
            </td>
            <td>
              <div @click="deleteTask(index)">
                <i class="bi bi-trash-fill"></i>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      local_storage : "task-storage",
      task: "",
      editedTask: null,
      statuses: ["To-do", "Under-way", "Done"],
      tasks: [
        {
          name: "Study",
          status:""

        },
        {
          name:"dishes",
          status:""
        },
        {
          name:"test",
          status:""
        },
                {
          name:"reading a book",
          status:""
        },

      ],
    };
  },
    created() {
    this.tasks = JSON.parse(localStorage.getItem(this.local_storage) || "[]");
    console.log(this.tasks);
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem(local_storage, JSON.stringify(this.tasks));
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
      localStorage.setItem(local_storage, JSON.stringify(this.tasks));
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
      localStorage.setItem(local_storage, JSON.stringify(this.tasks));
    },
  },
};
</script>

<style scoped>
.Done {
  text-decoration: line-through;
}
.Under-way {
  color: orange;
}

table{
  width: 50%;
}
</style>