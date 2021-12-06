<template>
  <div id="todo-do" class="container">
    <!-- Header -->
    <h2 class="text-center mt-5">checkbox</h2>

    <!-- Input -->
    <div class="d-flex mb-20">
      <input v-model="task" type="text" class="form-control" placeholder="New Task">
      <button type="submit" @click="submitTask" class="btn btn-warning rounded-0">ADD</button>
    </div>

    <!-- Task list -->
    <table class="table table-hover table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th>
            <span :class="{'finished': task.status === 'Finished'}">
              {{task.name}}
            </span>
          </th>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer"
              :class="{'text-danger': task.status === 'To-do',
                'text-warning': task.status === 'In-progress',
                  'text-success': task.status === 'Finished'}">
              {{task.status}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      task:'',
      editTasks: null,
      statuses: ['To-do', 'In-progress', 'Finished'],
      tasks: [
          {
            name: 'Finish Cross-platform project',
            status: 'To-do'
          },
          {
            name: 'Update CV',
            status: 'In-progress'
          }
        ]
    }
  },

  methods: {

    submitTask(){
      if(this.task.length === 0)
        return;

      if (this.editTasks === null) {
        this.tasks.push({
          name: this.task,
          status: 'To-do'
        });
      }
      else {
        this.tasks[this.editTasks].name = this.task;
        this.editTasks = null;
      }


      this.task = '';
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editTasks = index;
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    changeStatus(index){
      let newIndex = this.statuses.indexOf(this.tasks[index].status);

      if (++newIndex > 2)
        newIndex = 0;

      this.tasks[index].status = this.statuses[newIndex];
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pointer{
    cursor: pointer;
  }
  .finished {
    text-decoration: line-through;
  }
</style>
