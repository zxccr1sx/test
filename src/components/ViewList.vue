<template>
  <div>
    <ul class="list-group">
      <li class="list-group-item task-list-item" v-for="(item, index) in limitedTasks" :key="index" v-bind:class="{ complete: item.completed }">
        <div :id="'accordion' + index" class="accordion">
          <div class="accordion-item">
            <h4 class="accordion-header d-flex align-items-center justify-content-center" :id="'heading' + index">
              <button class="accordion-button untouch text-black fs-6" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapse' + index" aria-expanded="true" :aria-controls="'collapse' + index">
                <h4>{{ item.text }}</h4>
              </button>
            </h4>
            <div :id="'collapse' + index" class="accordion-collapse collapse" :class="{ 'show': openCollapseIndex === index }" :aria-labelledby="'heading' + index" data-bs-parent="#accordionContainer">
              <div class="col row">
                <div class="accordion-body">
                  <textarea class="form-control w-100" type="textarea" disabled :value="item.dscr" rows="3"></textarea>
                  <br>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend input-group-text">
                      <input class="form-check-input" type="checkbox" value="" id="flexCheckIndeterminate" v-model="item.completed">
                    </div>
                    <button class="btn btn-danger btn-lg float-end" @click="removeTask(index)">Remove</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </li>
    </ul>
    <div class="text-center mt-3">
      <button class="btn btn-outline-success me-2" v-if="showPreviousButton" @click="loadPreviousTasks">Previous</button>
      <button class="btn btn-outline-success" v-if="showNextButton" @click="loadNextTasks">Next</button>
    </div>
  </div>
</template>

<style scoped>
.complete {
  color:forestgreen
}
.task-list-item {
  margin-bottom: 10px;
}
.accordion-button {
  padding: 0.5rem 1rem;
}
.accordion-body {
  padding: 1rem;
}
.btn-lg {
  padding: 0.5rem 1.5rem;
  font-size: 1rem;
}
</style>


<script>
export default {
  props: ['tasks'],
  data() {
    return {
      openCollapseIndex: null,
      startIndex: 0,
      endIndex: 3
    }
  },
  computed: {
    limitedTasks() {
      return this.tasks.slice(this.startIndex, this.endIndex + 1);
    },
    showPreviousButton() {
      return this.startIndex > 0;
    },
    showNextButton() {
      return this.endIndex < this.tasks.length - 1;
    }
  },
  methods: {
    removeTask(index) {
      this.$emit('removeTask', index);
      if (this.openCollapseIndex === index) {
        this.openCollapseIndex = null;
      } else if (this.openCollapseIndex > index) {
        this.openCollapseIndex--;
      }
    },
    loadPreviousTasks() {
      this.startIndex -= 4;
      this.endIndex -= 4;
    },
    loadNextTasks() {
      this.startIndex += 4;
      this.endIndex += 4;
    }
  }
}
</script>