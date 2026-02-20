# Alta Entrega

**Sistema Operacional de Negócios | PaaS Vertical | Event-Driven Multi-Tenant Platform**

---

## 🎯 Visão Geral

Alta Entrega é um **Sistema Operacional de Negócios** (PaaS Vertical) construído para escalar para milhões de usuários. É um produto multi-tenant, event-driven, config-first, projetado para PMEs de múltiplos verticais (Beauty, Food, E-commerce, Tech, Logistics, etc.).

### Filosofia Central

- **Config-first:** Tudo governado por configuração, estado e políticas
- **State-driven UI:** Interface sempre representa o estado real do sistema
- **Event-driven:** Comunicação entre serviços via Event Bus (Redpanda)
- **Multi-tenant:** Isolamento total via tenant_id + RLS
- **Zero customização:** Nenhuma lógica customizada por cliente
- **Provider-agnostic:** Vendors escondidos atrás de capabilities

---

## 🏗️ Estrutura do Monorepo

```
alta-entrega/
├── apps/
│   ├── admin-panel/       # B2B Admin Interface
│   ├── client-app/        # B2C PWA
│   └── marketing-site/    # Institucional
├── services/
│   ├── core-api/          # Main REST API
│   ├── auth-service/      # Authentication & RBAC
│   ├── payment-service/   # Payment processing
│   ├── notification-service/
│   └── event-publisher/   # Outbox → Redpanda
├── packages/
│   ├── design-system/     # AEDS (Alta Entrega Design System)
│   ├── types/             # Shared TypeScript types
│   ├── utils/             # Shared utilities
│   └── providers/         # Capability adapters
├── docs/
│   ├── architecture/
│   ├── api-specs/
│   └── guidelines/
└── prisma/
    ├── schema.prisma
    └── migrations/
```

---

## 🚀 Stack Tecnológico

### Backend
- **Runtime:** Node.js 20 + TypeScript
- **API:** Express + tRPC
- **ORM:** Prisma
- **Database (OLTP):** PostgreSQL 16 + PgBouncer
- **Database (OLAP):** ClickHouse
- **Cache:** Redis Cluster
- **Event Bus:** Redpanda (Kafka-compatible)
- **Workflows:** Temporal
- **Auth:** Supabase (MVP) → Keycloak (Enterprise)

### Frontend
- **Framework:** React 18
- **Build:** Vite
- **Styling:** TailwindCSS v4
- **State:** Zustand + React Query
- **Routing:** React Router (Data mode)

### Infrastructure
- **Orchestration:** Kubernetes
- **Observability:** Grafana + Prometheus + Loki + Tempo
- **Feature Flags:** Unleash
- **Search:** Meilisearch
- **Storage:** MinIO (S3-compatible)
- **Ingress:** Traefik

---

## 📊 Arquitetura Event-Driven

```
Service → Outbox Table → Publisher → Redpanda → Consumers (idempotent)
```

**Garantias:**
- At-least-once delivery
- Ordem garantida por aggregate_id
- Idempotência obrigatória
- Dead Letter Queue para falhas

---

## 🎨 Design System (AEDS)

Alta Entrega possui um Design System proprietário baseado no conceito visual de **Montanha**:

- **Solidez estrutural**
- **Base forte**
- **Crescimento vertical**
- **Camadas bem definidas**
- **Previsibilidade**

**Filosofia:**
- State-driven UI (estados governam a interface)
- Progressive disclosure
- Calm technology
- Zero exceções de layout

---

## 🔐 Multi-Tenant Isolation

```sql
-- Todos os models possuem tenant_id
CREATE TABLE orders (
  id UUID PRIMARY KEY,
  tenant_id UUID NOT NULL,
  customer_id UUID NOT NULL,
  -- ...
  CONSTRAINT fk_tenant FOREIGN KEY (tenant_id) REFERENCES tenants(id)
);

-- RLS Policy (Row-Level Security)
CREATE POLICY tenant_isolation ON orders
  FOR ALL
  USING (tenant_id = current_setting('app.current_tenant_id')::UUID);
```

**Regras:**
✅ tenant_id vem do JWT (auth)
❌ tenant_id NUNCA vem do request body
✅ Todas as queries filtram por tenant_id
✅ Todos os testes cobrem isolamento multi-tenant

---

## 🚢 Getting Started

### Pré-requisitos
- Node.js 20+
- Docker + Docker Compose
- pnpm 9+

### Instalação

```bash
# Clone o repositório
git clone https://github.com/vinicsantana/alta-entrega.git
cd alta-entrega

# Instale as dependências
pnpm install

# Configure as variáveis de ambiente
cp .env.example .env

# Inicie o ambiente de desenvolvimento (30+ services)
docker-compose up -d

# Execute as migrations
pnpm prisma:migrate:dev

# Seed do banco de dados
pnpm prisma:seed

# Inicie o servidor de desenvolvimento
pnpm dev
```

### Acesso
- **Admin Panel:** http://localhost:5173/admin
- **Client App:** http://localhost:5173/app
- **API:** http://localhost:3000
- **Redpanda Console:** http://localhost:8080
- **Temporal UI:** http://localhost:8081
- **Grafana:** http://localhost:3001

---

## 📖 Documentação

- [Architecture Overview](./docs/architecture/README.md)
- [API Specification](./docs/api-specs/README.md)
- [Design Guidelines](./docs/guidelines/README.md)
- [Multi-Tenant Strategy](./docs/architecture/multi-tenant.md)
- [Event-Driven Patterns](./docs/architecture/event-driven.md)

---

## 🧪 Testing

```bash
# Unit tests
pnpm test

# Integration tests (requer Docker)
pnpm test:integration

# E2E tests
pnpm test:e2e

# Coverage
pnpm test:coverage
```

**Coverage obrigatório:**
- ✅ Isolamento multi-tenant (100%)
- ✅ State transitions (100%)
- ✅ Idempotência de consumers (100%)

---

## 🎯 Verticais Suportados

1. **Beauty** (salões, barbearias, estética)
2. **Food** (restaurantes, delivery)
3. **E-commerce** (lojas virtuais)
4. **Tech & SaaS** (software, apps)
5. **Logistics** (entregas, transportadoras)
6. **Education** (escolas, cursos)
7. **Real Estate** (imobiliárias)
8. **Healthcare** (clínicas, consultórios)
9. **Fitness** (academias, personal trainers)
10. **Professional Services** (consultorias, advocacia)

**Importante:** Verticais não mudam layout, apenas ajustam ordem de módulos e foco visual.

---

## 🤝 Contributing

Este é um repositório proprietário. Contribuições externas não são aceitas no momento.

---

## 📄 License

Proprietary - All rights reserved © Alta Entrega 2025

---

## 📞 Contact

**Maintainer:** @vinicsantana  
**Email:** dev@alta-entrega.com  
**Website:** https://alta-entrega.com

---

**Status:** 🚧 In Development | 🎯 Target: Q2 2025 Beta Launch