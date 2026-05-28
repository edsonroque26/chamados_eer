# chamados_eer — Registro de Desenvolvimento

> **Regra:** Qualquer alteração no projeto — estrutura, decisão arquitetural, nova funcionalidade, configuração ou correção — deve ser registrada neste arquivo antes de ser considerada concluída.

---

## Visão Geral do Sistema

**Nome:** chamados_eer  
**Objetivo:** Sistema de gestão de chamados com módulo financeiro integrado.  
**Stack:**
- Backend: Django (Python)
- Frontend: React (JavaScript/TypeScript)
- Banco de dados: PostgreSQL
- Containerização: Docker / Docker Compose
- Versionamento: GitHub

---

## Arquitetura

### Backend — Django Apps

O projeto Django será dividido em apps separados, cada um com responsabilidade bem definida:

| App | Responsabilidade |
|-----|-----------------|
| `chamados` | Abertura, acompanhamento e encerramento de chamados/tickets |
| `financeiro` | Gestão financeira: cobranças, pagamentos, relatórios |
| `core` | Configurações globais, autenticação, usuários e permissões |

### Frontend — React

- Aplicação SPA (Single Page Application)
- Consome API REST do Django
- Organizado por módulos espelhando os apps do backend

### Estrutura de Pastas (prevista)

```
chamados_eer/
├── backend/
│   ├── core/           # app Django: autenticação e usuários
│   ├── chamados/       # app Django: módulo de chamados
│   ├── financeiro/     # app Django: módulo financeiro
│   ├── manage.py
│   ├── requirements.txt
│   └── config/         # settings, urls, wsgi
├── frontend/
│   ├── src/
│   │   ├── modules/
│   │   │   ├── chamados/
│   │   │   └── financeiro/
│   │   └── ...
│   ├── package.json
│   └── ...
├── docker/
│   ├── backend.Dockerfile
│   └── frontend.Dockerfile
├── docker-compose.yml
├── .env.example
└── desenvolvimento.md
```

---

## Histórico de Decisões e Alterações

### 2026-05-28 — Início do projeto

- Criado repositório local `/home/eer/chamados_eer`
- Definida stack: Django + React + PostgreSQL + Docker
- Definida arquitetura com apps separados: `chamados`, `financeiro`, `core`
- Criado este arquivo `desenvolvimento.md` como registro central do projeto

### 2026-05-28 — Estrutura Docker

- Criados arquivos Docker com multi-stage builds (base → development → production)
- `docker-compose.yml`: configuração base para produção
- `docker-compose.dev.yml`: overrides para desenvolvimento (hot reload, portas expostas)
- `backend/Dockerfile`: Python 3.12-slim, stages dev e prod, usuário não-root em produção
- `frontend/Dockerfile`: Node 20 para dev, build com Vite, servido via nginx em produção
- `frontend/nginx.conf`: SPA routing + proxy reverso para `/api/` apontando para o backend
- `.env.example`: template de variáveis de ambiente (`.env` real nunca vai para o git)
- `.gitignore`: configurado para Python, Node, Django e variáveis de ambiente
- Decisões de boas práticas aplicadas:
  - Health check no PostgreSQL antes de subir o backend
  - Rede isolada `chamados_net` entre os serviços
  - Volume nomeado `postgres_data` para persistência do banco
  - `.dockerignore` em backend e frontend para imagens menores
  - Usuário não-root no container de produção do Django
  - `gunicorn` com workers/threads para produção

### 2026-05-28 — Configuração do Git e GitHub

- Instalado GitHub CLI (`gh`) via apt
- Autenticação realizada com `gh auth login` (usuário: edsonroque26)
- Configurado git global: `user.email = edsonroque26@gmail.com`, `user.name = edsonroque26`
- Criado repositório público no GitHub: https://github.com/edsonroque26/chamados_eer
- Primeiro commit realizado: `inicio do projeto chamados_eer`
- Branch principal: `master` vinculada ao remote `origin`

---

## Pendências — Detalhamento Necessário

> Antes de continuar o desenvolvimento, o responsável pelo projeto precisa detalhar os itens abaixo. Nenhum model, view ou tela deve ser criado sem esse alinhamento.

### App `chamados` — a detalhar
- Como funciona o fluxo de um chamado? (quem abre, quem atende, etapas)
- Quais os status possíveis? (ex: Aberto, Em andamento, Aguardando, Encerrado)
- Quem pode abrir chamados? Clientes, funcionários internos ou ambos?
- Existe prioridade? (ex: Baixa, Média, Alta, Urgente)
- Haverá SLA / prazo de atendimento?
- Chamados têm categorias? Quais?
- Como funciona a comunicação dentro do chamado? (comentários, histórico)

### App `financeiro` — a detalhar
- O que será controlado? (cobranças, pagamentos, boletos, recibos?)
- O financeiro está ligado aos chamados? (ex: chamado gera cobrança?)
- Haverá contas a pagar e a receber?
- Precisará de relatórios? Quais?
- Integrará com algum sistema de pagamento externo?

---

## Próximos Passos

- [x] **Passo 1 — Repositório GitHub:** Criar o repositório `chamados_eer` no GitHub e fazer o primeiro commit com a estrutura inicial
- [x] **Passo 2 — Docker Compose base:** Criar `docker-compose.yml` com os serviços: `db` (PostgreSQL), `backend` (Django), `frontend` (React/Node)
- [ ] **Passo 3 — Backend Django:** Inicializar projeto Django com os apps `core`, `chamados` e `financeiro`; configurar conexão com PostgreSQL
- [ ] **Passo 4 — Autenticação:** Configurar autenticação JWT no Django (djangorestframework-simplejwt)
- [ ] **Passo 5 — Models do app `chamados`:** Definir models: `Chamado`, `Categoria`, `Status`, `Comentario`
- [ ] **Passo 6 — Models do app `financeiro`:** Definir models: `Cobranca`, `Pagamento`, `Lancamento`
- [ ] **Passo 7 — Frontend React:** Inicializar app React com roteamento e estrutura de módulos
- [ ] **Passo 8 — Integração frontend/backend:** Configurar Axios, variáveis de ambiente e fluxo de autenticação na SPA
