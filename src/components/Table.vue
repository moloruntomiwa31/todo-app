<template>
  <div>
    <div class="table-responsive mt-3" v-show="taskAdded">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>S/N</th>
            <th>Task</th>
            <th>Status</th>
            <!-- <th>Edit</th> -->
            <th>Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>{{ index + 1 }}</td>
            <td :class="{ stroke: task.status === 'finished' }">
              {{ task.name }}
            </td>
            <td class="text-capitalize status" @click="changeStatus(index)" style="background-color: #146C94; color: white; ">
              {{ task.status }}
            </td>
            <!-- <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen icon"></span>
              </div>
            </td> -->
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
    props: ['task', 'tasks', 'taskAdded', 'editedTask'],
    data() {
        return {
            statusArray: ["to-do", "in progress", "finished"]
        }
    },
    methods: {
        deleteTask(index) {
            this.tasks.splice(index, 1)
        },
        editTask(index) {
            this.task = this.tasks[index].name
            this.editedTask = index
        },
        changeStatus(index) {
            let newIndex = this.statusArray.indexOf(this.tasks[index].status)
            if (++newIndex > 2) newIndex = 0
            this.tasks[index].status = this.statusArray[newIndex]
        }
    }
}
</script>

<style>
</style>
