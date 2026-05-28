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

---

## Próximos Passos

- [ ] **Passo 1 — Repositório GitHub:** Criar o repositório `chamados_eer` no GitHub e fazer o primeiro commit com a estrutura inicial
- [ ] **Passo 2 — Docker Compose base:** Criar `docker-compose.yml` com os serviços: `db` (PostgreSQL), `backend` (Django), `frontend` (React/Node)
- [ ] **Passo 3 — Backend Django:** Inicializar projeto Django com os apps `core`, `chamados` e `financeiro`; configurar conexão com PostgreSQL
- [ ] **Passo 4 — Autenticação:** Configurar autenticação JWT no Django (djangorestframework-simplejwt)
- [ ] **Passo 5 — Models do app `chamados`:** Definir models: `Chamado`, `Categoria`, `Status`, `Comentario`
- [ ] **Passo 6 — Models do app `financeiro`:** Definir models: `Cobranca`, `Pagamento`, `Lancamento`
- [ ] **Passo 7 — Frontend React:** Inicializar app React com roteamento e estrutura de módulos
- [ ] **Passo 8 — Integração frontend/backend:** Configurar Axios, variáveis de ambiente e fluxo de autenticação na SPA
