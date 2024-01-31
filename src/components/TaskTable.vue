<template>
    <div class="table-tasks">
        <section v-for="task in tasks" :key="task.id" :class="{ 'task-container': true, 'completed': task.completed }">

            <div class="task-aling-left">

            <button @click="taskWereCompleted(task.id, task.completed)">
                <span class="material-symbols-outlined">
                    radio_button_unchecked
                </span>
            </button>

            <p>{{ task.title }}</p>
            
            </div>

            <button @click="deletedItem(task.id, task.title)">
                <span class="material-symbols-outlined">
                    delete
                </span>
            </button>

        </section>
    </div>
</template>

<script>
export default {
    props: {
        getDados: {
            type: Function
        },
        deletedItem: {
            type: Function
        },
        tasks: {
            type: Array
        },
        title: {
            type: String
        },
        completed: {
            type: String
        }
    },
    methods: {
        async taskWereCompleted(id, completed) {

            try {
                
                completed = !completed

                const dataJson = JSON.stringify({ completed: completed })

                await fetch(`http://localhost:3000/tasks/${id}`, {
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: dataJson
                })

                this.getDados()

            } catch (error) {
                console.error('Não foi possivel atualizar a task', error);
            }
        },
    },
}
</script>

<style scoped>
.table-tasks {
    display: flex;
    width: 100%;
    height: 100vh;
    flex-direction: column;
    margin-top: 30px;
}

.task-container {
    display: flex;
    width: 100%;
    border: 1px solid;
    padding: 16px;
    border-radius: 5px;
    align-items: center;
    gap: 8px;
    margin: 0 auto 30px;
    justify-content: space-between;
    transition: .2s;
    background: var(--background-white);
    box-shadow: 0 5px 10px 1px var(--background-black);
    animation: NewlyCreatedTask 1s;
}

.task-aling-left {
    display: flex;
    align-items: center;
    gap: 8px;
}

.task-container p {
    display: flex;
    position: relative;
    max-width: 658px;
    width: 100%;
    overflow: hidden;
}

.task-container.completed p::after {
  content: ' ';
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 1px;
  background: var(--background-black);
  animation-name: lineThroughAnimated;
  animation-duration: .2s;
  animation-timing-function: linear;
  animation-iteration-count: 1;
  animation-fill-mode: forwards; 
}

.task-container.completed {
    opacity: .4;
}

button {
    display: flex;
    align-items: center;
}

@keyframes lineThroughAnimated{
  0%   { width : 0; }
  100% { width: 100%; }
}

@keyframes NewlyCreatedTask {
  0%, 7% {
    transform: rotateZ(0);
  }
  15% {
    transform: rotateZ(-2deg);
  }
  20% {
    transform: rotateZ(1deg);
  }
  25% {
    transform: rotateZ(-1deg);
  }
  30% {
    transform: rotateZ(1deg);
  }
  35% {
    transform: rotateZ(-1deg);
  }
  40%, 100% {
    transform: rotateZ(0);
  }
}

</style>