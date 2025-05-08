<script setup>
import EventList from './components/EventList.vue'
import TaskList from './components/TaskList.vue'
</script>

<template>
  <main>
    <EventList />
    <TaskList />
  </main>

</template>

<style scoped>
main {
  display: flex;
}
</style>