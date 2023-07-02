<template>
  <div class="container">
    <h1 class="text-center mt-5 fw-bolder text-uppercase" style="color: white;">My Vue Todo App</h1>

    <div class="d-flex mt-3">
      <input ref="el" style="background-color: #146C94; color: white; border: none;" type="text" placeholder="Enter task here..." class="form-control" v-model="task" @keyup.enter="submitTask">
      <button class="btn btn-primary rounded ms-3" @click="submitTask">Submit</button>
    </div>
    <Table :task="task" :tasks="tasks" :taskAdded="taskAdded" :editedTask="editedTask"  :error="error" />
  </div>
</template>

<script>
import Table from "./Table.vue"
export default {
  name: 'TodoApp',
  components: { Table },
  data() {
    return {
      task: "",
      error: false,
      tasks: [],
      taskAdded: false,
      editedTask: null,
    }
  },
  methods: {
    submitTask() {
        if (this.task.length === 0) {
          this.error = true
          return
        }else {this.error = false}
        if (this.editedTask === null) {
            this.tasks.push({
            name: this.task,
            status: "to-do"
            })
            this.taskAdded = true
        }else {
            this.tasks[this.editedTask].name = this.task
            this.editedTask = null
        }
        this.task = ""
    }
  },
  watch: {
    tasks: {
      handler(newval) {
        localStorage.setItem("saveTodo", JSON.stringify(newval))
      },
      deep: true
    }
  },
  created() {
    const storedArray = localStorage.getItem('saveTodo');
    if (storedArray !== null) {
      this.tasks = JSON.parse(storedArray);
    }
  },
  mounted() {
    this.$refs.el.focus()
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .form-control:focus {
    outline: none;
    box-shadow: 0 0 0 0.2rem #19A7CE;
  }
  .form-control::placeholder {color: white;}
    .btn {
    background-color: #146C94;
    border: none;
    outline: none;
  }
  .btn:hover {
  background-color: #19A7CE;
  }
  .icon {
    cursor: pointer;
    color: #146C94;
  }
  .icon:hover, .status:hover {
    opacity: 0.8;
    transition: all 0.3s ease-in-out;
  }
  .stroke {text-decoration: line-through; text-decoration-color: red;}
  .status {cursor: pointer; min-width: 120px;}
  @media (min-width: 668px) {
    * {font-size: 1.2rem;}
  }
</style>
