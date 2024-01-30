<template>
    <section>
        <div>
            Tasks created <span>{{ allTasks }}</span>
        </div>
        <div>
            Completed <span>{{ count }} of {{ allTasks }}</span>
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
        count(){
            return this.tasks.filter(task => task.completed).length
        }
    },
    mounted(){

    }
}
</script>

<style scoped>
section {
    display: flex;
    justify-content: space-between;
    width: 100%;
}
</style>