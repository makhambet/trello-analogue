<template>
  <div class="card">
    <div class="card-title">Проекты</div>
    <card-task
      v-for="item in getTasks"
      :key="item.id"
      :task="item.task"
      :index="item.id"
      @changeCardList="changing"
    ></card-task>
    {{ tasks }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        {
          id: 1,
          task: 'Task 1'
        },
        {
          id: 2,
          task: 'Task 2'
        },
        {
          id: 3,
          task: 'Task 3'
        },
        {
          id: 4,
          task: 'Task 4'
        },
        {
          id: 5,
          task: 'Task 5'
        },
      ]
    }
  },
  computed: {
    getTasks() {
      return this.tasks;
    }
  },
  methods: {
    changing(id, position) {
      let lastPosition = 0
      let index = this.tasks.findIndex(task => task.id == id)
      if ((!index && position < -20) || (index === this.tasks.length - 1 && position > 20) || (position < 20 && position > -20)) return
      if(lastPosition % 20 >= 19 || !lastPosition) {
        this.getTasks.splice((index + parseInt(lastPosition + 1) + 1), 0, this.getTasks[index])
        this.getTasks.splice(index, 1)
        lastPosition = position / 20
      }
      // else if (position % 21 < 18) {
      //   this.getTasks.splice((index + position / 20 ), 0, this.getTasks[index])
      //   this.getTasks.splice(index, 1)
      // }
      console.log(index, position, position % 21)
    }
  },
  components: {
    CardTask: () => ({
      component: import('./task')
    })
  }
}
</script>

<style scoped>
  .card {
    width: 272px;
    background: #EBECF0;
    padding: 8px;
    border-radius: 3px;
  }
  .card-title {
    margin-bottom: 8px;
  }
</style>
