<template>
  <form>
    <div class="mx-auto col-10 col-md-8 col-lg-6">

      <label for="title" class="form-label fw-bold fs-4 text-black-70">Task title</label>
      <input
        type="text"
        class="form-control"
        v-model="title"
        :class="{'is-invalid': v$?.title?.$error}"
        placeholder="Make some pasta..."
      >
      <div v-for="error in titleErrors" :key="error" class="text-danger">{{ error }}</div>
      <br>
      

      <label for="description" class="form-label fw-bold fs-4 text-black-70">Task Description</label>
      <textarea
        type="description"
        class="form-control"
        v-model="description"
        :class="{'is-invalid': v$?.description?.$error}"
        placeholder="Fry some eggs and...."
      ></textarea>
      <div v-for="error in descriptionErrors" :key="error" class="text-danger">{{ error }}</div>
      <br>
      

      <button class="btn btn-success float-end" @click="add_task" type="button" data-mdb-ripple-color="dark">Add Task</button>
    </div>
  </form>
</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import { minLength, maxLength } from '@vuelidate/validators'

export default {
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      title: '',
      description: ''
    }
  },
  validations() {
return {
  title: {
    minLength: minLength(3),
    maxLength: maxLength(100)
  },
  description: {
    minLength: minLength(0),
    maxLength: maxLength(500)
  }
}
},
  computed: {
    titleErrors() {
      const errors = []
      if (!this.v$.$dirty) return errors
      if (!this.v$.$invalid) return errors
      if (!this.v$?.title?.minLength) errors.push('Please enter at least 3 characters.')
      if (!this.v$?.title?.maxLength) errors.push('Please enter at most 100 characters.')
      return errors
    },
    descriptionErrors() {
      const errors = []
      if (!this.v$.$dirty) return errors
      if (!this.v$.$invalid) return errors
      if (!this.v$?.description?.maxLength) errors.push('Please enter at most 500 characters.')
      return errors
    }
  },
  methods: {
add_task() {
  this.v$.$touch();
  if (this.v$.$invalid) return;

  let task = {
    title: this.title,
    description: this.description
  }

  this.$emit('addTask', task)
  this.title = ''
  this.description = ''
}
}
}
</script>

<style>
  .is-invalid {
    border-color: red;
  }
</style>
