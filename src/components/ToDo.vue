<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue ToDo App</h2>

    <div class="d-flex">
      <input 
        v-model="task"
        class="form-control" 
        type="text" 
        placeholder="Enter new task"
      >
      <button 
        @click="submitTask"
        class="btn btn-warning rounded-0">SUBMIT</button>
    </div>

    <table class="table table-striped mt-5">
  <thead>
    <tr>
      <th scope="col" style="width: 700px">Task</th>
      <th scope="col" style="width: 300px">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr 
      v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'Finished!'}">
          {{task.name}}
        </span>
      </td>
      <td>
        <span 
          @click="changeStatus(index)"
          :class="{
            'text-danger': task.status === 'I need to do this',
            'text-warning': task.status === 'In progress',
            'text-success': task.status === 'Finished!'
            }"
          class="pointer"
        >
          {{task.status}}
        </span>
      </td>
      <td>
        <div 
          class="text-center"
          @click="editTask(index)"
        >
          <span class="fa fa-pen pointer"></span>
        </div>
      </td>
      <td>
        <div 
          class="text-center" 
          @click="deleteTask(index)"
        >
          <span class="fa fa-trash pointer"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>

  </div>
</template>

<script>
export default {
  name: 'ToDo',
  data: () => ({
    task: '',
    editedTask: null,
    avaibleStatuses: ['I need to do this', 'In progress', 'Finished!'],
    tasks: [
      {
        name: 'Tell to russian warship to go fuck himself',
        status: 'I need to do this'
      },
    ]
  }),
  methods: {
    submitTask() {
      if(this.task.length === 0) return

      if(this.editedTask === null) {
        this.tasks.push({
        name: this.task,
        status: 'I need to do this'
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }
      this.task = ''
    },

    deleteTask(index) {
      this.tasks.splice(index, 1)
    },

    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },

    changeStatus(index) {
      let nextStatus = this.avaibleStatuses.indexOf(this.tasks[index].status)
      if(++nextStatus > 2) {
        nextStatus = 0
      }
      this.tasks[index].status = this.avaibleStatuses[nextStatus]
    }
  }
}
</script>

<style>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>