<template>
    <div class="task-bar">
        <nav>
            <span class="capital">全部任务</span>
            <button class="btn-remove" @click="emit('btn-remove')">删除事件</button>
        </nav>
        <div class="task-list">
            <div class="task-column">
                <h3>待办</h3>
                <button @click="addTodo" class="btn-add-task">+</button>
                <ul>
                    <draggable :list="currentEvent.todo" item-key="uuid" @change="saveLocal" group="tasks">
                        <template #item="{ element, index }">
                            <li class="task-item" @click="updateTask(element, index, 'todo')">
                                <div class="title">{{ element.name }}</div>
                                <p>{{ element.description }}</p>
                                <div class="close" @click.stop="delTask(index, 'todo')">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                                        fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                        stroke-linejoin="round"
                                        class="icon icon-tabler icons-tabler-outline icon-tabler-x">
                                        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                        <path d="M18 6l-12 12" />
                                        <path d="M6 6l12 12" />
                                    </svg>
                                </div>
                            </li>
                        </template>
                    </draggable>
                </ul>
            </div>
            <div class="task-column">
                <h3>进行中</h3>
                <button @click="addDoing" class="btn-add-task">+</button>
                <ul>
                    <draggable :list="currentEvent.doing" item-key="uuid" @change="saveLocal" group="tasks">
                        <template #item="{ element, index }">
                            <li class="task-item" @click="updateTask(element, index, 'doing')">
                                <div class="title">{{ element.name }}</div>
                                <p>{{ element.description }}</p>
                                <div class="close" @click.stop="delTask(index, 'doing')">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                                        fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                        stroke-linejoin="round"
                                        class="icon icon-tabler icons-tabler-outline icon-tabler-x">
                                        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                        <path d="M18 6l-12 12" />
                                        <path d="M6 6l12 12" />
                                    </svg>
                                </div>
                            </li>
                        </template>
                    </draggable>
                </ul>
            </div>
            <div class="task-column">
                <h3>已完成</h3>
                <button @click="addCompleted" class="btn-add-task">+</button>
                <ul>
                    <draggable :list="currentEvent.completed" item-key="uuid" @change="saveLocal" group="tasks">
                        <template #item="{ element, index }">
                            <li class="task-item" @click="updateTask(element, index, 'completed')">
                                <div class="title">{{ element.name }}</div>
                                <p>{{ element.description }}</p>
                                <div class="close" @click.stop="delTask(index, 'completed')">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                                        fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                        stroke-linejoin="round"
                                        class="icon icon-tabler icons-tabler-outline icon-tabler-x">
                                        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                        <path d="M18 6l-12 12" />
                                        <path d="M6 6l12 12" />
                                    </svg>
                                </div>
                            </li>
                        </template>
                    </draggable>
                </ul>
            </div>
        </div>
    </div>
</template>

<script setup>
import { v4 as uuid } from "uuid"
import draggable from "vuedraggable"

const emit = defineEmits(['btn-remove', 'btn-add-task', 'btn-del-task', 'drag-task', 'btn-update-task'])
defineProps(['currentEvent'])

const addTodo = () => {
    let name = prompt('添加待办事件的名称', '')
    let description = prompt('添加待办事件的详细信息', '')
    if (name || description) {
        const newTodo = {
            uuid: uuid(),
            name: name.trim(),
            description: description.trim()
        }
        emit('btn-add-task', newTodo, 'todo')
    }
}

const addDoing = () => {
    let name = prompt('添加进行中事件的名称', '')
    let description = prompt('添加进行中事件的详细信息', '')
    if (name || description) {
        const newTodo = {
            uuid: uuid(),
            name: name.trim(),
            description: description.trim()
        }
        emit('btn-add-task', newTodo, 'doing')
    }
}

const addCompleted = () => {
    let name = prompt('添加已完成事件的名称', '')
    let description = prompt('添加已完成事件的详细信息', '')
    if (name || description) {
        const newTodo = {
            uuid: uuid(),
            name: name.trim(),
            description: description.trim()
        }
        emit('btn-add-task', newTodo, 'completed')
    }
}

const saveLocal = (param) => {
    // 拖拽完成后，通知 APP 组件保存新数据到本地
    // 避免触发2次
    if (param.added || param.moved) {
        emit('drag-task')
    }
}

const delTask = (i, category) => {
    emit('btn-del-task', i, category)
}

const updateTask = (elem, i, key) => {
    const newName = prompt('请输入新的事件名', elem.name)
    const newDescription = prompt('请输入新的详细信息', elem.description)
    if (newName && newDescription) {
        const updatedTask = {
            uuid: elem.uuid,
            name: newName.trim(),
            description: newDescription.trim()
        }
        emit('btn-update-task', updatedTask, key, i)
    }
}
</script>

<style scoped>
.task-bar {
    flex: 3;
    padding: 2rem 0.8rem;
    border-left: 3px solid #d5d1d1;

}

nav {
    display: flex;
    align-items: center;
    gap: 2rem;
    margin-bottom: 2rem;
}

.capital {
    font-size: 2.5rem;
    font-weight: 600;
    letter-spacing: 2px;
}

.btn-remove {
    height: 2.5rem;
    background: #fb7299;
    padding: 0 2rem;
    border: none;
    border-radius: 10px;
    color: white;
    font-weight: bold;
}

.btn-remove:hover {
    opacity: 0.8;
    cursor: pointer;
}

.task-list {
    height: 700px;
    display: flex;
    gap: 1rem;
    align-items: flex-start;
}

.task-column {
    border-radius: 10px;
    background: rgb(219, 214, 214);
    flex: 1;
    min-width: 100px;
    max-height: 700px;
    padding: 1rem 1rem;
}

.btn-add-task {
    display: block;
    width: 80%;
    height: 40px;
    background: rgb(205, 193, 193);
    border: none;
    border-radius: 10px;
    margin: 1rem auto;
    font-size: 1.5rem;
    line-height: 1.5rem;
    padding-bottom: 3px;
}

.btn-add-task:hover {
    cursor: pointer;
    color: white;
    background: rgb(205, 193, 193, 0.8);
    transition: 0.3s;
}

.task-column ul {
    padding-top: 1rem;
    min-height: 30px;
    max-height: 600px;
    overflow: auto;
}

.task-item {
    width: 80%;
    min-height: 4rem;
    border-radius: 10px;
    background: #f0f0f0;
    margin: 0 auto 0.8rem;
    padding: 1rem 0.6rem;
    position: relative;
}

.task-item .title {
    line-height: 1.3rem;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    font-style: italic;
}

.task-item .title::before {
    content: '\25cf';
    color: #fb7299;
    margin-right: 0.5rem;
    font-style: normal;
}

.task-item p {
    color: rgba(241, 163, 18, 0.8);
    overflow-wrap: break-word;
    margin: 8px 10px 0;
}

.task-item .close {
    color: red;
    position: absolute;
    right: 5px;
    bottom: 3px;
    transition: transform 0.3s;
}

.task-item .close:hover {
    transform: rotate(90deg);
}
</style>