<template>
    <div class="task-bar">
        <nav>
            <span class="capital">全部任务</span>
            <button class="btn-remove" @click="emit('btn-remove')">删除事件</button>
        </nav>
        <div class="task-list">

        </div>
    </div>
</template>

<script setup>
const emit = defineEmits(['btn-remove'])
</script>

<style scoped>
.task-bar {
    flex: 3;
    margin: 3rem 2rem;
}

nav {
    display: flex;
    align-items: center;
    gap: 2rem;
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
</style>