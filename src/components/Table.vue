<template>
  <div>
    <transition name="fade">
      <p v-if="error" class="error">You need to add a task.</p>
    </transition>
    <div class="table-responsive mt-3" v-if="taskAdded">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>S/N</th>
            <th>Task</th>
            <th>Status</th>
            <th>Date</th>
            <th>Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>{{ index + 1 }}</td>
            <td :class="{ stroke: task.status === 'finished' }">
              {{ task.name }}
            </td>
            <td
              class="text-capitalize status"
              @click="changeStatus(index)"
              style="background-color: #146c94; color: white"
            >
              {{ task.status }}
            </td>
            <td>
              <div class="text-center">
                {{ date }}
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash icon"></span>
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
  props: ["task", "tasks", "taskAdded", "editedTask", "error"],
  data() {
    return {
      statusArray: ["to-do", "in progress", "finished"],
      date: new Date().toLocaleString()
    };
  },
  methods: {
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.statusArray.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statusArray[newIndex];
    },
  },
};
</script>

<style>
  .error {
    color: red;
    font-style: italic;
  }
  .fade-enter-active, .fade-leave-active {
      transition: opacity .9s;
  }
  .fade-enter, .fade-leave-to {
      opacity: 0;
  }
</style>
