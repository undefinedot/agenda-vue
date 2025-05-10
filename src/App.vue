<template>
  <main>
    <EventList @change-index="(i) => activeEventIndex = i" @add-event="addTheEvent" :eventList="eventList" />
    <TaskList @btn-remove="removeTheEvent" :currentEvent="eventList[activeEventIndex]" @btn-add-task="addTask"
      @drag-task="modifyTaskList" @btn-del-task="delTheTask" @btn-update-task="updateTheTask" />
  </main>

</template>

<script setup>
import { onBeforeMount, ref } from 'vue';
import EventList from './components/EventList.vue'
import TaskList from './components/TaskList.vue'

const activeEventIndex = ref(0)
const eventList = ref([])
onBeforeMount(() => {
  let defaultEvent = []
  if (!localStorage.getItem('AllEvents')) {
    defaultEvent.push({
      title: '创建一个事件吧 ^^',
      todo: [],
      doing: [],
      completed: []
    })
  } else {
    defaultEvent = JSON.parse(localStorage.getItem('AllEvents'))
  }
  eventList.value = defaultEvent
})

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

  eventList.value.splice(activeEventIndex.value, 1)
  if (eventList.value.length < 1) {
    eventList.value.push({
      title: '创建一个事件吧 ^^',
      todo: [],
      doing: [],
      completed: []
    })
  }

  localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
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

const delTheTask = (i, key) => {
  eventList.value[activeEventIndex.value][key].splice(i, 1)
  localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
}

const updateTheTask = (task, key, i) => {
  if (eventList.value[activeEventIndex.value][key].some((item) => {
    return item.name.toLowerCase() === task.name.toLowerCase()
  })) {
    alert('名字已存在!')
    return
  }
  eventList.value[activeEventIndex.value][key][i] = task
  localStorage.setItem('AllEvents', JSON.stringify(eventList.value))
}
</script>

<style scoped>
main {
  display: flex;
}
</style>