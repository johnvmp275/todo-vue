<template>
    <section>
        <div>
            <strong>Tasks created <span>{{ allTasks }}</span></strong>
        </div>
        <div>
            <strong>Completed <span>{{ countOfCompletedTasks }} of {{ allTasks }}</span></strong>
        </div>
    </section>
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
            type: Array
        },
    },
    computed: {
        allTasks() {
            return this.tasks.length
        },
        countOfCompletedTasks() {
            return this.tasks.filter(task => task.completed).length
        }
    },
}
</script>

<style scoped>
section {
    display: flex;
    justify-content: space-between;
    width: 100%;
}

strong{
    font-size: 14px;
    color: var(--background-orange);
}

span{
    background: var(--background-gray-400);
    color: white;
    font-size: 12px;
    width: 24px;
    height: 19px;
    border-radius: 25px;
    padding: 5px 14px;
    margin-left: 10px;
    box-shadow: 0 5px 10px 1px black;
}

</style>