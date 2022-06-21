<template>
<section>
  <div class="container">
    <div class="pt-3 text-center">
      <h2 class="text-center">TO-DO LIST</h2>
      <!-- INPUT -->
      <div class="d-flex mt-5">
        <input
          v-model="task"
          type="text"
          placeholder="Enter task"
          class=" border-bottom w-100"
        />

        <span @click="submitTask" class="POINT"><i style="font-size:30px; height:100%;" class="bi text-white submitt bi-plus-lg"></i></span>

      </div>

      <!-- TASK TABLE -->
      <table class="table mt-5">
        <thead>
          <tr class="text-white">
            <th>#</th>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <!-- <th scope="col">Edit</th> -->
            <th scope="col">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr class="text-white" v-for="(task, index) in tasks" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <th scope="row">
              <span :class="{ Done: task.status === 'Done' }">
                {{ task.name }}
              </span>
            </th>
            <td class="POINT" style="width: 100px">
              <span
                :class="{
                  'text-danger': task.status === 'to-do',
                  'bg-success': task.status === 'Done',
                  'bg-warning': task.status === 'Under-way',
                }"
                class="click"
                @click="changeStatus(index)"
                >{{ task.status }}</span
              >
            </td>
            <!-- <td>
              <div class="POINT" @click="editTask(index)">
                <i class="bi bi-pencil-square"></i>
              </div>
            </td> -->
            <td>
              <div class="POINT" @click="deleteTask(index)">
                <i class="bi bi-trash-fill"></i>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>  
</section>

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
      tasks: [],
    };
  },
    created() {
    this.tasks = JSON.parse(localStorage.getItem("tasks")  || "[]");
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
          status: "To-do",
        });
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
        this.task = "";
      }

       
    },
    deleteTask(index) {
      this.tasks.splice(index,1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },

    // EDITING 
    // editTask(index) {
    //   this.task = this.tasks[index].name;
    //   this.editedTask = index; 
    //   localStorage.setItem("tasks", JSON.stringify(this.tasks));
    // },

    // CHANGING STATUS
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
section{
  height: 100vh;
  margin-top: 0;
  background-image: url('https://images.unsplash.com/photo-1509773896068-7fd415d91e2e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80');
  background-repeat: no-repeat;
  background-size: cover !important;
  background-image: linear-gradient(
      0deg,
      rgba(0, 0, 0, 0.2),
      rgba(0, 0, 0, 0.2)
    ),
    url('https://images.unsplash.com/photo-1509773896068-7fd415d91e2e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80');
  overflow-x: hidden;
  overflow-y: hidden;
  background-position: 70%;
}
.POINT{
  cursor: pointer;
}
.submitt{
  background-color: darkgreen;
}
.submitt:hover{
  background-color: rgb(0, 255, 0) !important;
}
input{
  border: none;
  background-color: transparent;
  color: white;
  outline: none;

}
.Done {
  text-decoration: line-through;
}
.Under-way {
  color: orange;
}

table{
  width: 50%;
}
h2{
  color: rgb(72, 222, 72);
}
</style>