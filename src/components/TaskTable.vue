<template>
    <div class="table-tasks">

        <label for="search">

            <input 
            type="search" 
            name="search" 
            v-model="search" 
            id="search" 
            placeholder="Find your task" 
            />

        </label>

        <section v-for="task in findTasksForUser" :key="task.id" :class="{ 'task-container': true, 'completed': task.completed }">

            <div class="task-aling-left">

                <button @click="taskWereCompleted(task.id, task.completed)">

                    <span v-if="!task.completed" class="material-symbols-outlined">
                        radio_button_unchecked
                    </span>

                    <svg v-else xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="24px" height="24px" viewBox="0 0 50 50">
                    <line pathLength="100" class="stroke" x1="8.5" y1="8.5" x2="41.5" y2="41.5" />
                    <line pathLength="100" class="stroke" x1="41.5" y1="8.5" x2="8.5" y2="41.5" style="animation-delay: .5s" />
                    </svg>

                </button>

                <p>{{ task.title }}</p>

            </div>

            <button @click="deletedTaskItem(task.id, task.title)">
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
        getDadosFromTask: {
            type: Function
        },
        deletedTaskItem: {
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
    data() {
        return {
            search: '',
        }
    },
    computed: {
        findTasksForUser() {
            return this.tasks.filter((task) => task.title.match(this.search.toLowerCase()))
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

                this.getDadosFromTask()

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
    max-width: 630px;
    width: 100%;
    overflow: hidden;
}

.task-container p {
    display: flex;
    position: relative;
    text-overflow: ellipsis;
    text-transform: capitalize;
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

label {
    display: flex;
    width: 100%;
    margin-bottom: 50px;
    height: 50px;
    box-shadow: 0 5px 10px 1px var(--background-black);
}

input {
    width: 100%;
    padding: 0 16px;
    background: var(--background-gray-500);
    border: none;
    border-radius: 8px;
    color: var(--background-white);
}

#search::-webkit-search-cancel-button {
    -webkit-appearance: none;
}

.stroke {
  stroke-dasharray: 100;
  stroke-dashoffset: 100;
  stroke: var(--background-black);
  stroke-width: 3px;
  fill: none;
  animation: strokeAni .5s forwards 1;
}

@keyframes lineThroughAnimated {
    0% {
        width: 0;
    }

    100% {
        width: 100%;
    }
}

@keyframes NewlyCreatedTask {

    0%,
    7% {
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

    40%,
    100% {
        transform: rotateZ(0);
    }
    
}

@keyframes strokeAni {
  to {
    stroke-dashoffset: 0;
  }
}
</style>