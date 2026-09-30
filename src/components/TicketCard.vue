<template>
  <div class="ticket-card">
    <div class="ticket-header">
      <span class="ticket-id">#{{ ticket.id }}</span>
      <span class="status-pill" :class="ticket.status.replace(' ', '-').toLowerCase()">
        {{ ticket.status }}
      </span>
    </div>
    
    <h3 class="ticket-title">{{ ticket.titulo }}</h3>
    <p class="ticket-desc">{{ ticket.descricao }}</p>
    
    <div class="ticket-footer">
      <span class="priority" :class="ticket.prioridade.toLowerCase()">
        SLA: {{ ticket.prioridade }}
      </span>
      <div class="actions">
        <button @click="$emit('editar', ticket)" class="btn-link">Editar</button>
        <button @click="$emit('excluir', ticket.id)" class="btn-link danger">Excluir</button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  ticket: {
    type: Object,
    required: true
  }
})
defineEmits(['editar', 'excluir'])
</script>

<style scoped>
.ticket-card {
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  padding: 16px;
  margin-bottom: 12px;
  background-color: #ffffff;
  color: #333;
}
.ticket-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  font-size: 0.85em;
  color: #666;
}
.ticket-id {
  font-family: monospace;
}
.ticket-title {
  margin: 0 0 8px 0;
  font-size: 1.1em;
  font-weight: 600;
  color: #1a1a1a;
}
.ticket-desc {
  margin: 0 0 16px 0;
  font-size: 0.95em;
  color: #555;
  line-height: 1.4;
}
.ticket-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #f0f0f0;
  padding-top: 12px;
  font-size: 0.9em;
}

/* Status e Prioridade (Estilo Pílula Simples) */
.status-pill {
  padding: 2px 8px;
  border-radius: 12px;
  background-color: #f0f0f0;
  color: #555;
}
.status-pill.aberto { background-color: #fff0f0; color: #d32f2f; }
.status-pill.em-andamento { background-color: #fff8e1; color: #f57c00; }
.status-pill.resolvido { background-color: #e8f5e9; color: #388e3c; }

.priority.alta { color: #d32f2f; font-weight: bold; }
.priority.media { color: #f57c00; }
.priority.baixa { color: #388e3c; }

/* Botões simples tipo link */
.actions {
  display: flex;
  gap: 12px;
}
.btn-link {
  background: none;
  border: none;
  color: #1976d2;
  cursor: pointer;
  padding: 0;
  font-size: 0.95em;
}
.btn-link:hover { text-decoration: underline; }
.btn-link.danger { color: #d32f2f; }
</style>