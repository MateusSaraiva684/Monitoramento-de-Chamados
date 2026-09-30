MAteus dos santos saraiva 37022010
pedro cristian lima da silva 37024861

# Dashboard ITSM - Gestão de Chamados

## Descrição do Projeto
Esta é uma Single Page Application (SPA) desenvolvida para otimizar o fluxo de um setor de suporte de TI (Help Desk/Service Desk). A aplicação resolve o problema do acompanhamento de incidentes e requisições, permitindo que a equipe técnica monitore chamados e SLAs em uma interface limpa, direta e utilitária. 

Todo o sistema funciona de forma fluida em uma única tela, permitindo a gestão dos chamados (CRUD) sem a necessidade de recarregar a página.

## Tecnologia Escolhida: Vue.js
A equipe optou pelo **Vue.js** (utilizando a Composition API) para o desenvolvimento desta solução. A escolha justifica-se pelos seguintes pontos:
- **Reatividade Integrada:** O uso de estados reativos (`ref` e `reactive`) permite que o dashboard seja atualizado instantaneamente assim que um novo chamado é aberto ou alterado pela equipe.
- **Componentização:** A interface foi estruturada em blocos lógicos. A criação do componente reutilizável `TicketCard.vue` permitiu isolar a responsabilidade de exibição e formatação de cada chamado, mantendo o arquivo principal limpo e focado nas regras de negócio.
- **Simplicidade:** O Vue proporciona uma estrutura de arquivos (SFC) muito próxima do HTML, CSS e JavaScript tradicionais, o que facilitou a criação de um layout focado na leitura rápida de dados.

## Requisitos Atendidos
- **Interface organizada e responsiva:** Design humanizado e utilitário, sem poluição visual, projetado para o uso diário de analistas de suporte.
- **Componentes reutilizáveis:** Uso de componentes dedicados para a exibição de chamados.
- **Navegação SPA:** Interação contínua e sem recarregamento da página.
- **Formulário e Validação:** Entrada de dados com validação de campos obrigatórios (Título e Descrição).
- **Apresentação e Interação:** Identificação visual imediata do Status (Aberto, Em Andamento, Resolvido) e da Prioridade de SLA.
- **Operações de Dados:** CRUD completo com inclusão, leitura, alteração e exclusão de chamados (com dados simulados em memória).

## Como Executar o Projeto

1. Certifique-se de ter o [Node.js](https://nodejs.org/) instalado no seu ambiente.
2. Clone este repositório para sua máquina local.
3. Abra o terminal na pasta raiz do projeto e instale as dependências:
   ```bash
   npm install
