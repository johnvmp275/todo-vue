<script setup>
import TaskTable from '@/components/TaskTable.vue';
import Notification from '@/components/widgets/Notifications.vue';
import CountTasks from '@/components/widgets/CountTasks.vue';
</script>

<template>
  <Notification :notifications="notificacoes" />
  <main>
    <div>

      <label for="">
        <input type="text" v-model="titles" placeholder="Add a new task">
        <button @click="inviteTasks">
          <span class="material-symbols-outlined">
            library_add
          </span>
        </button>
      </label>

      <CountTasks :getDados="getDados" :tasks="tasks" :title="titles" :completed="completed" :count="count" />

      <TaskTable :getDados="getDados" :tasks="tasks" :title="titles" :completed="completed" :deletedItem="deletedItem" />

    </div>
  </main>
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
      count: 0
    }
  },
  methods: {
    async getDados() {

      try {

        const req = await fetch('http://localhost:3000/tasks')
        const data = await req.json();

        this.tasks = data
        this.completed = data.map(task => task.completed);
        this.title = data.map(task => task.title);

        this.completed.forEach(value => {

          console.log(value);

        });

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
            msg: `A Tarefa ${this.titles} acabou de ser criada!`,
            icon: 'check_circle',
            color: 'green'
          })

          setTimeout(() => {
            this.notificacoes.splice(0, 1)
          }, 3000);

          this.titles = '';
          this.getDados()
        } else {
          this.notificacoes.push({
            msg: `Por favor, preencha os dados`,
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
          msg: `A tarefa ${title} acabou de ser removida :(`,
          icon: 'warning',
          color: 'red'
        })

        setTimeout(() => {
          this.notificacoes.splice(0, 1)
        }, 3000);

        this.getDados()
      } catch (error) {
        console.error('Não foi possivel deletar a tarefa', error)
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
}

input {
  width: 100%;
  padding: 0 16px;
}

button {
  width: 90px;
}

div {
  width: 100%;
  max-width: 736px;
}
</style>
