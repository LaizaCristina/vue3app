<template>
  <div class="container">
    <h1 class="title">Entregas</h1>

    <!-- Formulário de Inclusão/Atualização -->
    <div class="card">
      <h2 class="subtitle">Cadastro de Entrega</h2>
      <form @submit.prevent="incluir">
        <div class="form-group">
          <label for="descricao">Descrição:</label>
          <input v-model="entrega.descricao" type="text" id="descricao" required />
        </div>
        <div class="form-group">
          <label for="dataHoraLimite">Data/Hora Limite:</label>
          <input v-model="entrega.dataHoraLimite" type="datetime-local" id="dataHoraLimite" required />
        </div>
        <div class="form-group">
          <label for="peso">Peso:</label>
          <input v-model="entrega.peso" type="number" id="peso" required />
        </div>
        <div class="form-group">
          <label for="observacoes">Observações:</label>
          <input v-model="entrega.observacoes" type="text" id="observacoes" />
        </div>
        <div class="form-group">
          <button type="submit" class="btn">Incluir</button>
        </div>
      </form>
      <button @click="buscarEntregas" class="btn">Atualizar</button>
      <p v-if="erro" class="error">{{ erro }}</p>
    </div>

    <!-- Tabela de Entregas -->
    <div class="card">
      <h2 class="subtitle">Lista de Entregas</h2>
      <div v-if="entregas.length">
        <table class="table">
          <thead>
            <tr>
              <th>Id</th>
              <th>Descrição</th>
              <th>Data/Hora Limite</th>
              <th>Peso</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="entrega in entregas" :key="entrega.id">
              <td>{{ entrega.id }}</td>
              <td>{{ entrega.descricao }}</td>
              <td>{{ entrega.dataHoraLimite }}</td>
              <td>{{ entrega.peso }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else>
        <p>Nenhuma entrega encontrada.</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { onMounted, ref } from 'vue';
  import axios from 'axios';
  
  const entrega = ref({
    descricao: '',
    dataHoraLimite: Date.now(),
    peso: 1,
    observacoes: ''
  });
  const entregas = ref([]);
  const erro = ref("");

  async function incluir() {
    erro.value = "";
    try {
      await axios.post("entrega", entrega.value);
    } catch (e) {
      erro.value = (e as Error).message;
    }
    buscarEntregas();
  }

  async function buscarEntregas() {
    entregas.value = (await axios.get("entrega")).data;
  }

  onMounted(() => {
    buscarEntregas();
  });
</script>

<style scoped>
.container {
  font-family: 'Arial', sans-serif;
  padding: 20px;
  max-width: 800px;
  margin: auto;
}

.title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.card {
  background-color: #e8f5e9;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: #2c3e50;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #b2dfdb;
  border-radius: 8px;
  font-size: 14px;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #388e3c;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th, .table td {
  padding: 10px;
  text-align: left;
  border: 1px solid #b2dfdb;
}

.table th {
  background-color: #80cbc4;
  color: white;
}

.table tr:nth-child(even) {
  background-color: #e0f2f1;
}

.table tr:hover {
  background-color: #b2dfdb;
}

.error {
  color: red;
  font-size: 14px;
  margin-top: 10px;
}
</style>
