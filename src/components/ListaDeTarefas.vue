<script setup>
const props = defineProps(['tarefas'])
</script>

<template>
    <ul class="list-group mt-4" v-if="props.tarefas.length > 0">
        <li class="list-group-item" v-for="tarefa in props.tarefas" :key="tarefa.titulo">
            <input type="checkbox" :checked="tarefa.finalizada"
                @change="evento => tarefa.finalizada = evento.target.checked" :id="tarefa.titulo">

            <label class="ms-3" :class="{ done: tarefa.finalizada }" :for="tarefa.titulo">
                {{ tarefa.titulo }}
            </label>
        </li>
    </ul>

    <p v-else>
        😎 Você não possui tarefas pendentes 🎉
    </p>
</template>

<style scoped>
.done {
    text-decoration: line-through;
}
</style>