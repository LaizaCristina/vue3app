<template>
  <div class="container">
    <h1 class="title">Gerenciar Vocábulos</h1>

    <!-- Formulário de Cadastro -->
    <div class="card">
      <h2 class="subtitle">Cadastrar Novo Vocábulo</h2>
      <form @submit.prevent="cadastrarVocabulo">
        <div class="form-group">
          <label for="termo">Termo:</label>
          <input v-model="novoVocabulo.termo" type="text" id="termo" required />
        </div>
        <div class="form-group">
          <label for="significado">Significado:</label>
          <input v-model="novoVocabulo.significado" type="text" id="significado" required />
        </div>
        <div class="form-group">
          <label for="versao">Versão:</label>
          <input v-model="novoVocabulo.versao" type="number" id="versao" required />
        </div>
        <div class="form-group">
          <label for="dataHoraCadastro">Data de Cadastro:</label>
          <input v-model="novoVocabulo.dataHoraCadastro" type="datetime-local" />
        </div>
        <div class="form-group">
          <button type="submit" class="btn">Cadastrar</button>
        </div>
      </form>
    </div>

    <!-- Consulta de Vocábulo -->
    <div class="card">
      <h2 class="subtitle">Consulta de Vocábulo</h2>
      <form @submit.prevent="consultarVocabulo">
        <div class="form-group">
          <label for="termoConsulta">Termo:</label>
          <input v-model="consulta.termo" type="text" id="termoConsulta" />
        </div>
        <div class="form-group">
          <label for="versaoConsulta">Versão:</label>
          <input v-model="consulta.versao" type="number" id="versaoConsulta" />
        </div>
        <div class="form-group">
          <button type="submit" class="btn">Buscar</button>
        </div>
      </form>
    </div>

    <!-- Lista de Vocábulos -->
    <div class="card">
      <h2 class="subtitle">Vocábulos Cadastrados</h2>
      <div v-if="vocabulos.length">
        <table class="table">
          <thead>
            <tr>
              <th>ID</th>
              <th>Termo</th>
              <th>Significado</th>
              <th>Versão</th>
              <th>Situação</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="vocabulo in vocabulos" :key="vocabulo.id">
              <td>{{ vocabulo.id }}</td>
              <td>{{ vocabulo.termo }}</td>
              <td>{{ vocabulo.significado }}</td>
              <td>{{ vocabulo.versao }}</td>
              <td>{{ vocabulo.situacao }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else>
        <p>Nenhum vocábulo encontrado.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// Definir as variáveis de estado
const novoVocabulo = ref({
  termo: '',
  significado: '',
  versao: '',
  dataHoraCadastro: '',
})
const consulta = ref({
  termo: '',
  versao: '',
})
const vocabulos = ref([])

// Função para cadastrar um novo vocábulo
const cadastrarVocabulo = async () => {
  try {
    const response = await axios.post('/vocabulo', novoVocabulo.value)
    vocabulos.value.push(response.data)
    limparFormulario()
  } catch (error) {
    console.error('Erro ao cadastrar vocábulo:', error)
  }
}

// Função para consultar vocábulos
const consultarVocabulo = async () => {
  try {
    const response = await axios.get(`/vocabulo/${consulta.value.termo}/${consulta.value.versao}`)
    if (response.data.length === 0) {
      alert('Nenhum vocábulo encontrado para os critérios fornecidos.')
      vocabulos.value = []
    } else {
      vocabulos.value = response.data
    }
  } catch (error) {
    console.error('Erro ao consultar vocábulo:', error)
    alert('Ocorreu um erro ao realizar a consulta. Tente novamente.')
  }
}

// Função para carregar todos os vocábulos ao montar o componente
const carregarVocabulos = async () => {
  try {
    const response = await axios.get('/vocabulo')
    vocabulos.value = response.data.map(vocabulo => ({
      ...vocabulo,
      situacao: vocabulo.dataHoraDesativacao ? 'Desativado' : 'Ativo'
    }))
  } catch (error) {
    console.error('Erro ao carregar vocábulos:', error)
  }
}

// Função para limpar o formulário
const limparFormulario = () => {
  novoVocabulo.value = {
    termo: '',
    significado: '',
    versao: '',
    dataHoraCadastro: '',
  }
}

// Carregar os vocábulos ao montar o componente
onMounted(carregarVocabulos)
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

.subtitle {
  color: #2c3e50;
  margin-bottom: 10px;
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

.btn {
  background-color: #4caf50;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.btn:hover {
  background-color: #388e3c;
}

.table {
  width: 100%;
  border-collapse: collapse;
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
</style>
