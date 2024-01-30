<script setup>

</script>

<template>
    <div v-if="!tasks.length">
        <p>Ops! Você ainda não possui nenhuma tarefa :( </p>
    </div>
    <div class="table-tasks">
        <section v-for="task in tasks" :key="task.id" :class="{ 'task-container': true, 'completed': task.completed }">

            <button @click="taskWereCompleted(task.id, task.completed)">
                <span class="material-symbols-outlined">
                    check_circle
                </span>
            </button>

            <p>{{ task.title }}</p>

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
    data() {
        return {
            count: 0
        }
    },
    methods: {
        async CallDados() {
            this.getDados()
        },
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
    mounted() {
        this.CallDados()
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
    padding: 6px;
    border-radius: 5px;
    align-items: center;
    gap: 8px;
    margin: 0 auto 20px;
    transition: .2s;
}

.task-container p {
    display: flex;
    width: 100%;
    max-width: 658px;
    overflow: hidden;
}

.task-container.completed p {
    text-decoration: line-through;
}

.task-container.completed {
    opacity: .4;
}

</style>