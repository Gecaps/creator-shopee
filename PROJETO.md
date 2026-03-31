# Creator Shopee — Documento Mestre do Projeto

> Última atualização: 2026-03-30
> Status: Em configuração (MVP)

---

## 1. VISÃO GERAL

**O que é:** Curso/desafio de 7 dias que ensina pessoas comuns a ganhar comissão como afiliado/creator na Shopee.

**Modelo de negócio:** Venda de acesso digital (R$29,90) + order bump (Kit Creator R$37) + upsell (Mentoria R$197).

**Canal de aquisição principal:** Panfletos com QR code inseridos dentro dos pacotes da Shopee (2.000 pacotes/dia).

**Funil:**
```
Panfleto (QR) → Landing Page → Checkout Yampi → Pós-compra (automações n8n)
```

---

## 2. STACK TÉCNICA

| Componente | Ferramenta | Status |
|-----------|-----------|--------|
| Landing Page | HTML estático (Tailwind) | ✅ Feito — deploy Vercel |
| Checkout/Pagamento | **Yampi** (Mercado Pago) | ✅ Configurado |
| Automações | n8n (n8n.natuu.net) | ⏳ Pendente |
| CRM/Leads | Supabase (já tem) | ⏳ Pendente |
| Plataforma de curso | A definir | ⏳ Pendente |
| Email marketing | A definir (Brevo/SMTP) | ⏳ Pendente |
| WhatsApp API | A definir | ⏳ Pendente |
| Domínio personalizado | rendashopee.com.br (a registrar) | ⏳ Pendente |

---

## 3. YAMPI — CONFIGURAÇÃO COMPLETA

### Credenciais da API

| Campo | Valor |
|-------|-------|
| Alias | `creators-brasil` |
| Merchant ID | `1501564` |
| User ID | `1541135` |
| User-Token | `DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5` |
| User-Secret-Key | `sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P` |
| Plano | Basic |
| API Base URL | `https://api.dooki.com.br/v2/creators-brasil` |

### Gateway de Pagamento

- **Mercado Pago** (OAuth) — ativo
- Cartões aceitos: Visa, Mastercard, Elo, Amex, Diners, Discover, Aura, Hiper
- Também aceita: Boleto Bancário, Pix
- Não configurados: Depósito Bancário, Pix Parcelado
- **Parcelamento**: deve ser configurado direto no painel do Mercado Pago (a Yampi não permite customizar quando o gateway é MP)

### Produto: Desafio Creator Shopee

| Campo | Valor |
|-------|-------|
| Product ID | `44579820` |
| SKU ID | `295991787` |
| SKU Code | `CREATOR-SHOPEE-001` |
| SKU Token | `MXUJIPNG1W` |
| Nome | Desafio Creator Shopee |
| Preço | R$29,90 |
| Tipo | Digital |
| Slug | `desafio-creator-shopee` |

### Links de Checkout

| Tipo | URL |
|------|-----|
| **Link de Pagamento** (ID: 45494) | https://creators-brasil.pay.yampi.com.br/pay/FCBGQ7BJRG |
| **Compra Direta (SKU)** | https://creators-brasil.pay.yampi.com.br/r/MXUJIPNG1W |
| Catálogo | https://creators-brasil.catalog.yampi.io/desafio-creator-shopee/p |

### Marca

| Campo | Valor |
|-------|-------|
| Brand ID | `45707627` |
| Nome | Creators Brasil |

### API — Exemplos de Uso

```bash
# Headers obrigatórios em toda requisição:
# User-Token: DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5
# User-Secret-Key: sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P
# Content-Type: application/json

# Listar produtos
curl -H "User-Token: ..." -H "User-Secret-Key: ..." \
  "https://api.dooki.com.br/v2/creators-brasil/catalog/products"

# Listar pedidos
curl -H "User-Token: ..." -H "User-Secret-Key: ..." \
  "https://api.dooki.com.br/v2/creators-brasil/orders"

# Listar links de pagamento
curl -H "User-Token: ..." -H "User-Secret-Key: ..." \
  "https://api.dooki.com.br/v2/creators-brasil/checkout/payment-link"

# Criar novo link de pagamento
curl -X POST -H "User-Token: ..." -H "User-Secret-Key: ..." \
  -H "Content-Type: application/json" \
  "https://api.dooki.com.br/v2/creators-brasil/checkout/payment-link" \
  -d '{"name": "Nome do link", "active": true, "skus": [{"id": 295991787, "quantity": 1}]}'
```

### Observações Importantes sobre a Yampi

- **Yampi NÃO suporta recorrência/assinatura nativa** — cada compra é avulsa
- Para recorrência real (sem comprometer limite do cartão), usar API de Assinaturas do Mercado Pago diretamente (`/preapproval_plan` + `/preapproval`)
- O checkout da Yampi é transparente via MP (cartão, Pix, boleto — sem redirecionamento)
- A Yampi exige SKU para criar link de pagamento — produto precisa ter SKU associado
- Campos obrigatórios ao criar produto: `name`, `brand_id`, `simple`
- Campos obrigatórios ao criar SKU: `sku`, `price_sale`, `blocked_sale`, dimensões
- Campos obrigatórios ao criar marca: `name`, `active`, `featured`
- Cache de 30min em GETs — usar `?skipCache=true` para bypass
- Estoque do produto ficou como 0 (out_of_stock) — **PRECISA AJUSTAR** para liberar vendas

---

## 4. VERCEL — DEPLOY

| Campo | Valor |
|-------|-------|
| Project ID | `prj_ZnG7qwhmZiufFdMqvp6SuUsU4xAt` |
| Org/Team ID | `team_D4ZnxzaEIErRm3BwTHbTXtpp` |
| Project Name | `creator-shopee` |
| Conta | ia@gecaps.com.br |

---

## 5. INCONSISTÊNCIAS A RESOLVER

### ⚠️ Pagamento único vs Assinatura mensal

Há **conflito de copy** entre os materiais:

| Material | Diz o quê |
|----------|----------|
| Landing page (index.html) | "R$29,90/mês. Cancele quando quiser." — modelo assinatura |
| Panfleto (copy/panfleto.md) | "R$29,90. Pagamento único. Acesso vitalício." |
| Video script (copy/video-script.md) | "R$29,90. Pagamento único. Não é assinatura." |
| Email 3 (copy/emails.md) | "R$29,90 (pagamento único)" |
| Email 5 (copy/emails.md) | "Acesso vitalício" |

**Decisão necessária:** O produto é pagamento único ou assinatura mensal?
- Se **único**: atualizar landing page (remover "/mês" e "cancele quando quiser")
- Se **mensal**: atualizar panfleto, video script e emails. E implementar recorrência (Yampi não suporta — precisaria usar MP Assinaturas ou trocar gateway)
- **Situação atual na Yampi**: configurado como venda avulsa (não recorrente)

### ⚠️ Nome do produto

| Material | Nome usado |
|----------|-----------|
| Landing page | "Desafio Creator Shopee" |
| Copy (panfleto, video, emails) | "Desafio Renda Shopee" |
| Yampi (produto) | "Desafio Creator Shopee" |

**Decidir:** "Creator Shopee" ou "Renda Shopee"?

### ⚠️ Plataforma de pagamento

Os arquivos de automação (copy/automacoes.md) referenciam **Kiwify/Hotmart** como plataforma de pagamento, mas agora estamos usando **Yampi + Mercado Pago**.

**Ação:** Atualizar automacoes.md para refletir webhooks da Yampi em vez de Kiwify/Hotmart.

### ⚠️ Estoque

O SKU foi criado com estoque 0 (`stock_status: out_of_stock`). Para produto digital, precisa:
- Ou colocar estoque alto (9999)
- Ou desativar controle de estoque

---

## 6. ESTRUTURA DE ARQUIVOS

```
/CREATOR SHOPEE/
├── index.html              # Landing page (deploy Vercel)
├── yampi-config.md         # Config detalhada da Yampi (credenciais, IDs)
├── PROJETO.md              # ← ESTE ARQUIVO (visão geral do projeto)
├── .vercel/                # Config de deploy Vercel
├── .git/                   # Repositório Git
└── copy/
    ├── automacoes.md       # Fluxos n8n (captura, conversão, pós-compra)
    ├── emails.md           # Sequência de 5 emails + pós-compra + WhatsApp
    ├── panfleto.md         # Brief do panfleto (10x15cm, QR, A/B test)
    └── video-script.md     # Script do vídeo da página de obrigado (3-5 min)
```

---

## 7. PRÓXIMOS PASSOS

### Urgente (antes de vender)
- [ ] **Resolver conflito de copy** (único vs mensal) — decisão do dono
- [ ] **Ajustar estoque** do SKU na Yampi (ou desativar controle)
- [ ] **Testar o checkout** — fazer compra teste no link de pagamento
- [ ] **Configurar webhook** da Yampi no n8n para capturar pedidos aprovados

### Curto prazo
- [ ] Alinhar nome ("Creator Shopee" vs "Renda Shopee") em todos os materiais
- [ ] Atualizar automacoes.md (trocar Kiwify/Hotmart → Yampi)
- [ ] Registrar domínio rendashopee.com.br (ou creatorsshopee.com.br)
- [ ] Configurar UTMs no link do QR do panfleto
- [ ] Criar produto do Order Bump na Yampi (Kit Creator R$37)
- [ ] Criar produto do Upsell na Yampi (Mentoria R$197)

### Médio prazo
- [ ] Montar plataforma do curso (12 aulas)
- [ ] Gravar vídeo da página de obrigado
- [ ] Implementar automações n8n (captura → conversão → pós-compra)
- [ ] Configurar emails transacionais
- [ ] Imprimir primeira tiragem de panfletos (10.000 para teste)

---

## 8. YAMPI API — REFERÊNCIA RÁPIDA

### Endpoints mais usados

| Ação | Método | Endpoint |
|------|--------|----------|
| Listar produtos | GET | `/catalog/products` |
| Criar produto | POST | `/catalog/products` |
| Atualizar produto | PUT | `/catalog/products/{id}` |
| SKUs do produto | GET | `/catalog/products/{id}/skus` |
| Criar SKU | POST | `/catalog/products/{id}/skus` |
| Listar pedidos | GET | `/orders` |
| Ver pedido | GET | `/orders/{id}` |
| Links de pagamento | GET | `/checkout/payment-link` |
| Criar link pagamento | POST | `/checkout/payment-link` |
| Gateways | GET | `/checkout/gateways` |
| Marcas | GET | `/catalog/brands` |
| Categorias | GET | `/catalog/categories` |
| Webhooks | GET | `/webhooks` |
| Criar webhook | POST | `/webhooks` |

### Parâmetros úteis

| Param | Descrição |
|-------|-----------|
| `limit=N` | Resultados por página (default: 10) |
| `include=skus,images` | Carregar objetos relacionados |
| `skipCache=true` | Ignorar cache de 30min |
| `orderBy=created_at` | Ordenar por campo |
| `sortedBy=desc` | Direção da ordenação |

### Docs oficiais
- Portal: https://docs.yampi.com.br
- Referência API: https://docs.yampi.com.br/api-reference/introduction
- Índice completo (LLM-friendly): https://docs.yampi.com.br/llms.txt
