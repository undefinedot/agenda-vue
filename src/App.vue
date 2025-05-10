<template>
  <main>
    <EventList @change-index="(i) => activeEventIndex = i" @add-event="addTheEvent" :eventList="eventList" />
    <TaskList @btn-remove="removeTheEvent" :currentEvent="eventList[activeEventIndex]" @btn-add-task="addTask"
      @drag-task="modifyTaskList" />
  </main>

</template>

<script setup>
import { ref } from 'vue';
import EventList from './components/EventList.vue'
import TaskList from './components/TaskList.vue'
import tasksData from '@/../data.js'

const activeEventIndex = ref(0)

// Event: {title: '事件标题', todo: [{'taskName': 'decription'}], doing:[], completed: []}

localStorage.setItem('AllEvents', JSON.stringify(tasksData))
const eventList = ref(JSON.parse(localStorage.getItem('AllEvents')))

const addTheEvent = (title) => {
  // 去重
  const t = title.toLowerCase()
  if (eventList.value.find((item) => { return item.title.toLowerCase() == t })) {
    alert('事件已存在!')
    return
  }
  eventList.value.push({
    title: t,
    todo: [],
    doing: [],
    completed: []
  })
  localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
}

const removeTheEvent = () => {
  if (!confirm(`确定删除事件【${eventList.value[activeEventIndex.value].title}】吗？`)) return

  let eventListArray = []
  eventListArray = eventList.value.filter((item, index) => {
    return index != activeEventIndex.value
  })
  if (eventListArray.length < 1) {
    eventListArray = [{
      title: '创建一个事件吧 ^^',
      todo: [],
      doing: [],
      completed: []
    }]
  }

  localStorage.setItem('AllEvents', JSON.stringify(eventListArray))
  eventList.value = JSON.parse(localStorage.getItem('AllEvents'))
}

const addTask = (obj, key) => {
  // 简单去重
  if (eventList.value[activeEventIndex.value][key].some((item) => {
    return item.name.toLowerCase() === obj.name.toLowerCase()
  })) {
    alert('事件名已存在，请重新添加')
    return
  } else {
    eventList.value[activeEventIndex.value][key].push(obj)
    localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
  }
}

// save to localStorage after dragged
const modifyTaskList = () => {
  localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
}
</script>

<style scoped>
main {
  display: flex;
}
</style>