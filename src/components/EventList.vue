<template>
    <div class="slide-bar">
        <div class="logo">
            <svg xmlns="http://www.w3.org/2000/svg" width="150" height="150" viewBox="0 0 24 24" fill="none"
                stroke="#ee7c11" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="icon icon-tabler icons-tabler-outline icon-tabler-clipboard-list">
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M9 5h-2a2 2 0 0 0 -2 2v12a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-12a2 2 0 0 0 -2 -2h-2" />
                <path d="M9 3m0 2a2 2 0 0 1 2 -2h2a2 2 0 0 1 2 2v0a2 2 0 0 1 -2 2h-2a2 2 0 0 1 -2 -2z" />
                <path d="M9 12l.01 0" />
                <path d="M13 12l2 0" />
                <path d="M9 16l.01 0" />
                <path d="M13 16l2 0" />
            </svg>
        </div>
        <div class="event-list">
            <button @click="addBtn" class="btn-add-event">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                    stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                    class="icon icon-tabler icons-tabler-outline icon-tabler-plus">
                    <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                    <path d="M12 5l0 14" />
                    <path d="M5 12l14 0" />
                </svg>
            </button>
            <ul>
                <!-- <li class="event-active">默认事件</li> -->
                <li v-for="(item, index) in eventList" key="item.title" :class="{ 'event-active': activeLi == index }"
                    @click="selectEvent(index)">
                    {{ item.title }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['change-index', 'add-event'])
defineProps(['eventList'])

const activeLi = ref(0)

// 添加一个事件
const addBtn = () => {
    let title = prompt("请输入事件的名称~", '')
    if (title && title.trim()) {
        emit('add-event', title.trim())
    }
}

// 高亮被点击的 Event
const selectEvent = (index) => {
    activeLi.value = index
    emit('change-index', activeLi.value)
}
</script>

<style scoped>
.slide-bar {
    flex: 1;
    border-right: 3px solid #d5d1d1;
    height: 100vh;
    padding: 1rem 2rem;
    min-width: 200px;
    overflow: auto;
}

.logo {
    display: flex;
    justify-content: center;
}

.btn-add-event {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 80%;
    height: 40px;
    background: #d5d1d1;
    border: none;
    border-radius: 10px;
    margin: 1rem auto;
}

button:hover {
    transition: 0.3s;
    box-shadow: 0 0 0 2px #ea330e;
    background: rgb(213, 209, 209, 0.5);
    cursor: pointer;
}

ul {
    list-style-type: none;
}

ul li {
    text-align: center;
    padding: 12px;
    margin: 0 auto 1rem;
    transition: 0.5s;
    width: 80%;
    height: 40px;
    border-radius: 10px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}

ul li:hover {
    background: rgb(255, 165, 0, 0.8);
    cursor: pointer;
}

.event-active {
    background: rgb(255, 165, 0);
    width: 80%;
    height: 40px;
}

@media screen and (width<660px) {
    .logo {
        display: none;
    }
}
</style>