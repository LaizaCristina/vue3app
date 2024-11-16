<template>
    <div>
      <h1>Gerenciar Vocábulos</h1>
  
      <!-- Formulário de Cadastro -->
      <div>
        <h2>Cadastrar Novo Vocábulo</h2>
        <form @submit.prevent="cadastrarVocabulo">
          <div>
            <label for="termo">Termo:</label>
            <input v-model="novoVocabulo.termo" type="text" id="termo" required />
          </div>
          <div>
            <label for="significado">Significado:</label>
            <input v-model="novoVocabulo.significado" type="text" id="significado" required />
          </div>
          <div>
            <label for="versao">Versão:</label>
            <input v-model="novoVocabulo.versao" type="number" id="versao" required />
          </div>
          <div>
            <label for="dataHoraCadastro">Data de Cadastro:</label>
            <input v-model="novoVocabulo.dataHoraCadastro" type="datetime-local" />
          </div>
          <div>
            <button type="submit">Cadastrar</button>
          </div>
        </form>
      </div>
  
      <!-- Consulta de Vocábulo -->
      <div>
        <h2>Consulta de Vocábulo</h2>
        <form @submit.prevent="consultarVocabulo">
          <div>
            <label for="termoConsulta">Termo:</label>
            <input v-model="consulta.termo" type="text" id="termoConsulta" />
          </div>
          <div>
            <label for="versaoConsulta">Versão:</label>
            <input v-model="consulta.versao" type="number" id="versaoConsulta" />
          </div>
          <div>
            <button type="submit">Buscar</button>
          </div>
        </form>
      </div>
  
      <!-- Lista de Vocábulos -->
      <div>
        <h2>Vocábulos Cadastrados</h2>
        <div v-if="vocabulos.length">
          <table>
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
        vocabulos.value = [{ mensagem: 'Nenhum vocábulo encontrado para os critérios fornecidos' }]
      } else {
        vocabulos.value = response.data
      }
    } catch (error) {
      console.error('Erro ao consultar vocábulo:', error)
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
  /* Estilos para o componente, se necessário */
  </style>
  