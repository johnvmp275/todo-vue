<template>
  <section>
    <div>

      <strong>
        Tasks created <span>{{ allCreatedTasks }}</span>
      </strong>

    </div>
    <div>

      <strong>
        Completed <span>{{ countOfCompletedTasks }} of {{ allCreatedTasks }}</span>
      </strong>

    </div>
  </section>
</template>

<script>
export default {
  props: {
    getDadosFromTask: {
      type: Function,
    },
    tasks: {
      type: Array,
      default: ()=>[],
    },
    completed: {
      type: Array,
    },
  },
  computed: {
    allCreatedTasks() {
      return this.tasks.length;
    },
    countOfCompletedTasks() {
      return this.tasks.filter((task) => task.completed).length;
    },
  },
};
</script>

<style scoped>
section {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

strong {
  font-size: 14px;
  color: var(--background-orange);
}

span {
  background: var(--background-gray-400);
  color: var(--background-white);
  font-size: 12px;
  width: 24px;
  height: 19px;
  border-radius: 25px;
  padding: 5px 14px;
  margin-left: 10px;
  box-shadow: 0 5px 10px 1px var(--background-black);
}

</style>
