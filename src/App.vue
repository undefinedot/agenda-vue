<template>
  <main>
    <EventList @change-index="(i) => activeEventIndex = i" @add-event="addTheEvent" :eventList="eventTitles" />
    <TaskList @btn-remove="removeTheEvent" />
  </main>

</template>

<script setup>
import { ref } from 'vue';
import EventList from './components/EventList.vue'
import TaskList from './components/TaskList.vue'

const activeEventIndex = ref(0)

// Event: {title: '事件标题', todo: [{'taskName': 'decription'}], doing:[], completed: []}
const eventTitles = ref([{
  title: '默认事件',
  todo: [],
  doing: [],
  completed: []
}])

const addTheEvent = (t) => {
  eventTitles.value.push({
    title: t,
    todo: [],
    doing: [],
    completed: []
  })
}

const removeTheEvent = () => {
  if (!confirm(`确定删除事件【${eventTitles.value[activeEventIndex.value].title}】吗？`)) return
  eventTitles.value = eventTitles.value.filter((item, index) => {
    return index != activeEventIndex.value
  })
  if (eventTitles.value.length < 1) {
    eventTitles.value = [{
      title: '创建一个事件吧 ^^wwwwwwwwwwwwwwwwwwww wwwwwwwwwwwwww',
      todo: [],
      doing: [],
      completed: []
    }]
  }
}
</script>

<style scoped>
main {
  display: flex;
}
</style>