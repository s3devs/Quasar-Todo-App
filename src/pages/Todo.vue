<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input 
      v-model="newTask"
      @keyup.enter="addTask" 
      square
      class="col"
      filled 
      bg-color="white"
      placeholder="Add Task" 
     dense>

        <template v-slot:append>
          <q-btn 
          @click="addTask"
          round 
          dense 
          flat 
          icon="add" />
        </template>
      </q-input>
    </div>
    <q-list 
    class="bg-white"
    separator
    bordered
    >
      <q-item 
      v-for="(task,index) in tasks"
      :key="task.title"
      @click="task.done = !task.done"
      clickable
      :class="{ 'done bg-blue-1':task.done}"
      v-ripple>
        <q-item-section avatar>
          <q-checkbox 
            v-model="task.done" 
            color="primary" 
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label class="q-item_lable">{{task.title}}</q-item-label>
        </q-item-section>

        <q-item-section
        v-if="task.done"
        side
        >
         <q-btn 
         @click.stop="deleteTask(index)"
         flat 
         dense
         round
        color="primary"
        icon="delete" />
        </q-item-section>
      </q-item>

    </q-list>

    <div 
    v-if="!tasks.length"
    class="no-task absolute-center">
      <q-icon
      name="check"
      size="100px"
      color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No Task
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'Get Banana',
        //   done: false
        // },
        // {
        //   title: 'Get Apple',
        //   done: false
        // },
        // {
        //   title: 'Get Pain Apple',
        //   done: false
        // },
      ]
    }
  },
  methods: {
    deleteTask(index){

      this.$q.dialog({
        title: 'Confirm',
        message: 'Would you like to delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1)
        this.$q.notify('Task Deleted')
      })

      
    },
    addTask(){
      if(this.newTask !== ''){

        this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''

      }
      
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item_lable{
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-task{
  opacity: 0.5;
}

</style>
