<script setup>
import { reactive } from 'vue';

  const estado  =  reactive({

    filtro: 'todas',

    tarefaTemp: ' ',

    tarafas: [
      {
        titulo:'Estudar ES6',
        finalizada: false,
      },
      {
        titulo:'Estudar SASS',
        finalizada: false,
      },
      {
        titulo:'Estudar JavaScript',
        finalizada: false,
      },
      {
        titulo:'Estudar Python',
        finalizada: true,
      },
    ]
    
  });

  const cadastrarTarefa = () => {
    const novaTarefa = {
      titulo : estado.tarefaTemp,
      finalizada: false,

    }

    estado.tarafas.push(novaTarefa);
    estado.tarefaTemp = '';
  }

  const getTarfeasPendentes = () => {
    return estado.tarafas.filter(tarafa => !tarafa.finalizada)
  }

  const getTarfeasFinalizadas = () => {
    return estado.tarafas.filter(tarafa => tarafa.finalizada)
  }
  const getTarfeasFiltradas = () => {
    const {filtro} = estado;

    switch (filtro){

      case 'pendentes':
        return getTarfeasPendentes();

        case 'finalizadas':
          return getTarfeasFinalizadas();

          default:
            return estado.tarafas
    }
  }


</script>

<template>
<div class="container">
  <header class="p-5 mb-4 bg-ligth rounded-3">
    <h1>Minhas Tarefas</h1>
    <p >
      Você possui {{getTarfeasPendentes().length}} tarefas pendentes
    </p>
  </header>
  <form @submit.prevent="cadastrarTarefa">
  <div class="row">
    <div class="col">
      <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" type="text" placeholder="Digite a descrição da tarefa" class="form-control" required>
    </div>
    <div class="col-md-2">
      <button class="btn btn-primary">
        Cadastrar
      </button>
    </div>
    <div class="col-md-2">
      <select class="form-control" @change="evento => estado.filtro = evento.target.value">     
       <option value="todas">Todas as Tarefas</option>
       <option value="pendentes">Tarefas Pendentes</option>
       <option value="finalizadas">Tarefas Finalizadas</option>
      </select>
    </div>
  </div>
  {{ estado.filtro }}
</form>
<ul class=" list-group mt-4">
  <li class="list-group-item" v-for="tarafa in getTarfeasFiltradas()">
    <input type="checkbox" @change="evento => tarafa.finalizada = evento.target.checked" :checked="tarafa.finalizada" :id="tarafa.titulo"> 

    <label :class="{ done: tarafa.finalizada}" class="ms-3" :for="tarafa.titulo">
     {{ tarafa.titulo }}
    </label>
  </li>
</ul>
</div>


</template>

<style scoped>

.done{
  text-decoration: line-through;
}
</style>
