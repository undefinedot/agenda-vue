<template>
  <main>
    <EventList @change-index="(i) => activeEventIndex = i" @add-event="addTheEvent" :eventList="eventList" />
    <TaskList @btn-remove="removeTheEvent" :currentEvent="eventList[activeEventIndex]" @btn-add-task="addTask" />
  </main>

</template>

<script setup>
import { ref } from 'vue';
import EventList from './components/EventList.vue'
import TaskList from './components/TaskList.vue'
import tasksData from '@/../data.js'

const activeEventIndex = ref(0)

// Event: {title: '事件标题', todo: [{'taskName': 'decription'}], doing:[], completed: []}

const eventList = ref(tasksData)

const addTheEvent = (t) => {
  eventList.value.push({
    title: t,
    todo: [],
    doing: [],
    completed: []
  })
}

const removeTheEvent = () => {
  if (!confirm(`确定删除事件【${eventList.value[activeEventIndex.value].title}】吗？`)) return
  eventList.value = eventList.value.filter((item, index) => {
    return index != activeEventIndex.value
  })
  if (eventList.value.length < 1) {
    eventList.value = [{
      title: '创建一个事件吧 ^^',
      todo: [],
      doing: [],
      completed: []
    }]
  }
}

const addTask = (obj, key) => {
  eventList.value[activeEventIndex.value][key].push(obj)
}
</script>

<style scoped>
main {
  display: flex;
}
</style>