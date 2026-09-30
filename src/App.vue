<template>
  <div class="app-wrapper">
    <header class="topbar">
      <h1>Monitoramento de Chamados</h1>
    </header>

    <div class="content-layout">
      <!-- Formulário -->
      <aside class="sidebar">
        <h2>{{ editando ? 'Editar Chamado' : 'Novo Chamado' }}</h2>
        <form @submit.prevent="salvarChamado" class="simple-form">
          
          <div class="field">
            <label>Título do Incidente</label>
            <input v-model="form.titulo" type="text" />
          </div>
          
          <div class="field">
            <label>Descrição detalhada</label>
            <textarea v-model="form.descricao" rows="4"></textarea>
          </div>

          <div class="field-row">
            <div class="field">
              <label>Prioridade</label>
              <select v-model="form.prioridade">
                <option value="Baixa">Baixa</option>
                <option value="Media">Média</option>
                <option value="Alta">Alta</option>
              </select>
            </div>

            <div class="field">
              <label>Status</label>
              <select v-model="form.status">
                <option value="Aberto">Aberto</option>
                <option value="Em Andamento">Em Andamento</option>
                <option value="Resolvido">Resolvido</option>
              </select>
            </div>
          </div>

          <div v-if="erroValidacao" class="msg-error">{{ erroValidacao }}</div>

          <div class="form-actions">
            <button type="submit" class="btn-primary">
              {{ editando ? 'Guardar Alterações' : 'Criar Chamado' }}
            </button>
            <button v-if="editando" type="button" @click="cancelarEdicao" class="btn-secondary">
              Cancelar
            </button>
          </div>
        </form>
      </aside>

      <!-- Lista de Chamados -->
      <main class="ticket-list">
        <div class="list-header">
          <h2>Chamados Ativos ({{ chamados.length }})</h2>
        </div>
        
        <div v-if="chamados.length === 0" class="empty-list">
          Não há chamados na fila no momento.
        </div>
        
        <div v-else class="list-body">
          <TicketCard 
            v-for="chamado in chamados" 
            :key="chamado.id" 
            :ticket="chamado"
            @editar="prepararEdicao"
            @excluir="excluirChamado"
          />
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import TicketCard from './components/TicketCard.vue'

const chamados = ref([
  { id: 4012, titulo: 'Lentidão no Banco de Dados', descricao: 'PostgreSQL apresentando timeout nas consultas do ERP.', prioridade: 'Alta', status: 'Aberto' },
  { id: 4013, titulo: 'Acesso bloqueado', descricao: 'Colaborador não consegue autenticar na VPN corporativa.', prioridade: 'Media', status: 'Em Andamento' }
])

const estadoInicialForm = { id: null, titulo: '', descricao: '', prioridade: 'Baixa', status: 'Aberto' }
const form = reactive({ ...estadoInicialForm })
const editando = ref(false)
const erroValidacao = ref('')

const salvarChamado = () => {
  if (!form.titulo || !form.descricao) {
    erroValidacao.value = 'Preencha os campos obrigatórios (Título e Descrição).'
    return
  }
  erroValidacao.value = ''

  if (editando.value) {
    const index = chamados.value.findIndex(c => c.id === form.id)
    if (index !== -1) chamados.value[index] = { ...form }
  } else {
    chamados.value.push({
      ...form,
      id: Math.floor(Math.random() * 10000)
    })
  }
  cancelarEdicao()
}

const prepararEdicao = (ticket) => {
  Object.assign(form, ticket)
  editando.value = true
  erroValidacao.value = ''
}

const cancelarEdicao = () => {
  Object.assign(form, estadoInicialForm)
  editando.value = false
  erroValidacao.value = ''
}

const excluirChamado = (id) => {
  if (confirm('Confirma a exclusão deste chamado?')) {
    chamados.value = chamados.value.filter(c => c.id !== id)
  }
}
</script>

<style scoped>
/* Reset básico e tipografia de sistema */
.app-wrapper {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  color: #333;
  background-color: #f7f9fa;
  min-height: 100vh;
}

.topbar {
  background-color: #fff;
  border-bottom: 1px solid #e1e4e8;
  padding: 16px 24px;
}

.topbar h1 {
  margin: 0;
  font-size: 1.2em;
  font-weight: 500;
}

.content-layout {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding: 24px;
  max-width: 1000px;
  margin: 0 auto;
}

@media (min-width: 768px) {
  .content-layout {
    flex-direction: row;
    align-items: flex-start;
  }
}

/* Formulário Lateral */
.sidebar {
  flex: 1;
  background: #fff;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  padding: 20px;
}

.sidebar h2 {
  font-size: 1.1em;
  margin-top: 0;
  margin-bottom: 16px;
  border-bottom: 1px solid #eee;
  padding-bottom: 8px;
}

.simple-form .field {
  margin-bottom: 16px;
}

.field-row {
  display: flex;
  gap: 12px;
}

.field-row .field {
  flex: 1;
}

label {
  display: block;
  font-size: 0.9em;
  color: #586069;
  margin-bottom: 6px;
}

input, textarea, select {
  width: 100%;
  padding: 8px;
  border: 1px solid #d1d5da;
  border-radius: 4px;
  background-color: #fafbfc;
  font-family: inherit;
  box-sizing: border-box;
}

input:focus, textarea:focus, select:focus {
  outline: none;
  border-color: #0366d6;
  background-color: #fff;
}

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.btn-primary {
  background-color: #2ea44f;
  color: #fff;
  border: 1px solid rgba(27,31,35,0.15);
  padding: 6px 16px;
  border-radius: 4px;
  font-weight: 500;
  cursor: pointer;
}

.btn-primary:hover { background-color: #2c974b; }

.btn-secondary {
  background-color: #fafbfc;
  color: #24292e;
  border: 1px solid #d1d5da;
  padding: 6px 16px;
  border-radius: 4px;
  font-weight: 500;
  cursor: pointer;
}

.btn-secondary:hover { background-color: #f3f4f6; }

.msg-error {
  color: #cb2431;
  font-size: 0.85em;
  padding: 8px;
  background-color: #ffeef0;
  border-radius: 4px;
  margin-bottom: 16px;
}

/* Área da Lista */
.ticket-list {
  flex: 2;
}

.list-header h2 {
  font-size: 1.1em;
  margin-top: 0;
  margin-bottom: 16px;
}

.empty-list {
  background: #fff;
  border: 1px dashed #d1d5da;
  padding: 32px;
  text-align: center;
  color: #586069;
  border-radius: 6px;
}
</style>