<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        v-model="newTask"
        placeholder="Add Task"
        dense
        bg-color="white"
        @keyup.enter="addTask">

        <template v-slot:append>
          <q-btn
            round
            dense
            flat
            @click="addTask"
            icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
      separator
      bordered
      class="bg-white">
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        clickable
        @click="task.done = !task.done"
        :class="{'done bg-blue-1':task.done}"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primary"
            icon="delete"
            dense
            @click.stop="deleteTask(index)"/>
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No Tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      newTask:'',
      tasks: []
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1)
        this.$q.notify("Task Deleted")
      })
    },
    addTask() {
      this.tasks.push({
        title:this.newTask,
        done:false
      })
      this.newTask=''
      this.$q.notify("Task Added")
    }
  }
})
</script>

<style lang="scss">
  .done {
     .q-item__label {
      text-decoration: line-through;
      color: #bbb;
     }
  }
  .no-tasks {
    opacity: 0.5;
  }
</style>
