<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    { titulo: 'Estudar ES6', finalizada: false },
    { titulo: 'Estudar SASS', finalizada: false },
    { titulo: 'Ir para a academia', finalizada: true }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  if (!estado.tarefaTemp.trim()) return;

  estado.tarefas.push({
    titulo: estado.tarefaTemp,
    finalizada: false
  })

  estado.tarefaTemp = '';
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>

    <form @submit.prevent="cadastraTarefa">
      <div class="row g-2 align-items-center">
        <div class="col">
          <!-- MELHOR: v-model -->
          <input
            v-model="estado.tarefaTemp"
            required
            type="text"
            placeholder="Digite a descrição da tarefa"
            class="form-control"
          />
        </div>

        <div class="col-md-2">
          <button type="submit" class="btn btn-primary w-100">
            Cadastrar
          </button>
        </div>

        <div class="col-md-2">
          <select 
            @change="evento => estado.filtro = evento.target.value" 
            class="form-control"
          >
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    <!-- v-if correto -->
    <ul 
      class="list-group mt-4"
      v-if="getTarefasFiltradas().length > 0"
    >
      <li 
        class="list-group-item" 
        v-for="tarefa in getTarefasFiltradas()" 
        :key="tarefa.titulo"
      >
        <input 
          type="checkbox"
          :checked="tarefa.finalizada"
          @change="evento => tarefa.finalizada = evento.target.checked"
          :id="tarefa.titulo"
        >

        <label 
          class="ms-3"
          :class="{ done: tarefa.finalizada }"
          :for="tarefa.titulo"
        >
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>

    <!-- v-else logo após -->
    <p v-else>
      😎 Você não possui tarefas pendentes 🎉
    </p>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>