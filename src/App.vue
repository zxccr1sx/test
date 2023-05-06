<template>
  <p class="text-white text-center align-top fw-bold fs-1" style="background-color: #198754;">Todo list</p>
  <form-add @addTask="functionAddTask"/><br><br><br>
  <div class="text-center" >
    <label class="fw-bold fs-4 text-black-70"> Done: {{ doneTasks }} of {{ tasksCount }}  </label>
  </div>
  <br/>
  <view-list :tasks="tasks" :tasks-dscr="tasksDscr" @removeTask="functionRemoveTask"/>
</template>


<script>
import FormAdd from './components/FormAdd.vue'
import ViewList from './components/ViewList.vue'
import { useVuelidate } from '@vuelidate/core'
import { between } from '@vuelidate/validators'
export default {
  components: {
    FormAdd,
    ViewList
  },
  data() {
    return {
      tasks: [],
      tasksDscr: []
    }
  },
  computed: {
    tasksCount() {
      return this.tasks.length
    },
    doneTasks() {
      return this.tasks.filter(task => task.completed).length
    }
  },
  methods: {
    functionAddTask(task) {
      this.tasks.push({ text: task.title, dscr: task.description, completed: false })
      this.tasksDscr.push({ text: task.description })
    },
    functionRemoveTask(index) {
      this.tasks.splice(index, 1)
      this.tasksDscr.splice(index, 1)
    }
  }
}
</script>
