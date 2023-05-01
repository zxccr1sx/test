<style>
@font-face {
  font-family: "Mona Sans";
  src: local("Mona Sans"),   url(./assets/fonts/Mona-Sans.woff2) format('woff2-variations');}
</style>
<style>
  html{
     font-family: "Merienda", Helvetica, Arial;
     font-size: 20px;
  }
</style>
<template>
  
  <p class="text-white text-center align-top fw-bold fs-1" style="background-color: #198754;">Todo list</p>
  <form>
  <div class="mx-auto col-10 col-md-8 col-lg-6 ">
    <label for="title" class="form-label fw-bold fs-4 text-black-70">Task title</label>
    <input type="text" class="form-control" v-model="title" placeholder="Make some pasta...">
    <br>
    <label for="title" class="form-label fw-bold fs-4 text-black-70">Task title</label>
    <textarea type="description" class="form-control" v-model="description" placeholder="Fry some eggs and...."></textarea>
    <br>
    <button class="btn btn-success float-end" @click="add_task" type="button" data-mdb-ripple-color="dark">Add Task</button>
  </div></form><br><br><br>
  <div class="text-center" ><label class="fw-bold fs-4 text-black-70"> Your tasks: {{ tasksCount }} / {{ doneTasks }}</label></div>

  <ul class="task-list justify-content-center">
        <div class="container"><li class="task-list-item" 
        v-for="(item, index) in tasks" :key="index" v-bind:class="{complete: item.completed}">
        <div class="input-group text-center">
          <div class="input-group-text"><input class="form-check-input mt-0" type="checkbox" v-model="item.completed"/></div>
          <span class="input-group-text" v-bind:title="item.dscr">{{ ' '+item.text+' ' }}</span>
          <button v-on:click= "remove_task(index)" class="btn btn-danger">Remove</button></div>
          <br>
          
        </li></div>
      </ul>
</template>


<script>
export default{
  data(){
    return{
      title:'',
      description:'',
      tasks:[],
      tasksDscr:[],
  }},
  computed :{
    tasksCount(){
      return this.tasks.length
    },
    doneTasks(){
      return this.tasks.filter(task => task.completed).length
    }

  },
  methods : {
    add_task(){
      if (this.title !== ''){
        this.tasks.push({text: this.title, dscr: this.description, completed: false})
        this.tasksDscr.push({text: this.description})
        this.title = ''
        this.description = ''
      }
    },
    remove_task(index){
      this.tasks.splice(index,1)
      this.tasksDscr.splice(index,1)
    }
  }
}
</script>
