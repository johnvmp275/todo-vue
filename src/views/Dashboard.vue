<script setup>
import TaskTable from '@/components/TaskTable.vue';
import Notification from '@/components/widgets/Notifications.vue';
import CountTasks from '@/components/widgets/CountTasks.vue';
import Loader from '@/components/widgets/LoaderWaiting.vue';
</script>

<template>

  <Loader :isloader="isloader" />

  <Notification :notifications="notificacoes" />

  <div class="table-of-tasks">

    <label for="" :class="{ 'invalid': invalidComposition }">
      <input type="text" v-model="titles" placeholder="Add a new task">
      <button @click="inviteTasks">
        <span class="material-symbols-outlined">
          library_add
        </span>
      </button>
    </label>

    <CountTasks 
    :getDados="getDados" 
    :tasks="tasks" 
    :title="titles" 
    :completed="completed" 
    />

    <TaskTable 
    :getDados="getDados" 
    :tasks="tasks" 
    :title="titles" 
    :completed="completed" 
    :deletedItem="deletedItem" 
    />

  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: {},
      titles: '',
      title: null,
      completed: {},
      check: {},
      msg: '',
      notificacoes: [],
      isloader: false,
      invalidComposition: false
    }
  },
  watch:{
    titles: 'validateTaskFields',
  },
  methods: {
    async getDados() {

      try {

        const req = await fetch('http://localhost:3000/tasks')
        const data = await req.json();

        this.tasks = data
        this.completed = data.map(task => task.completed);
        this.title = data.map(task => task.title);

        this.isloader = true

      } catch (error) {
        console.error('Não foi possivel pegar os dados', error);
      }

    },

    async inviteTasks() {

      try {

        if (this.titles !== '') {

          const data = {
            title: this.titles,
            completed: false
          }

          const dataJson = JSON.stringify(data)

          await fetch('http://localhost:3000/tasks', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: dataJson
          })

          this.notificacoes.push({
            msg: `Task ${this.titles} has just been created!`,
            icon: 'check_circle',
            color: 'green'
          })

          setTimeout(() => {
            this.notificacoes.splice(0, 1)
          }, 3000);

          this.titles = '';
          this.getDados()
        } else {
          this.invalidComposition = true

          this.notificacoes.push({
            msg: `Please fill in the field`,
            icon: 'warning',
            color: 'red'
          })

          setTimeout(() => {
            this.notificacoes.splice(0, 1)
          }, 3000);
        }
      } catch (error) {
        console.error('Não foi possivel enviar a tarefa', error)
      }
    },
    async deletedItem(id, title) {

      try {

        await fetch(`http://localhost:3000/tasks/${id}`, {
          method: 'DELETE',
        })

        this.notificacoes.push({
          msg: `Task ${title} was just removed :(`,
          icon: 'warning',
          color: 'red'
        })

        setTimeout(() => {
          this.notificacoes.splice(0, 1)
        }, 5000);

        this.getDados()
      } catch (error) {
        console.error('Não foi possivel deletar a tarefa', error)
      }
    },
    validateTaskFields(){
     if( this.titles !== ''){
      this.invalidComposition = false
     }
    }
  },
  mounted() {
    this.getDados()
  }
}
</script>


<style scoped>

label {
  display: flex;
  width: 100%;
  margin-bottom: 50px;
  height: 42px;
  box-shadow: 0 5px 10px 1px black;
  transition: .5s;
  border-radius: 8px;
}

label.invalid{
  border: 1.5px solid red;
  box-shadow: 0 0 10px 0 red;
}

input {
  width: 100%;
  padding: 0 16px;
  background: var(--background-gray-500);
  border: none;
  border-radius: 8px 0 0 8px;
  color: white;
}

button {
  width: 90px;
  border-radius: 0 8px 8px 0;
  background: var(--background-orange);
  color: white;
}
.table-of-tasks {
  width: 100%;
  height: 100%;
  max-width: 736px;
  padding: 16px;
}

::placeholder {
  color: white;
}

::-ms-input-placeholder {
  color: white;
}
</style>
