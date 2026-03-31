# Fluxo de Automacoes — Desafio Renda Shopee

## ARQUITETURA DO FUNIL

```
PANFLETO (QR)
    |
    v
LANDING PAGE (captura)
    |
    +--> Webhook n8n: salvar lead (nome, email, whatsapp)
    |
    v
PAGINA OBRIGADO (video + oferta R$29,90)
    |
    +--> COMPROU? -----> Fluxo Pos-Compra
    |       |
    |       +--> Acesso plataforma
    |       +--> Email boas-vindas
    |       +--> WhatsApp boas-vindas
    |       +--> Dia 3: check-in
    |       +--> Dia 5: upsell R$197
    |
    +--> NAO COMPROU? --> Fluxo Conversao
            |
            +--> Email 1 (imediato): link do video
            +--> WhatsApp 1 (1h): "ja assistiu?"
            +--> Email 2 (dia 2): case Ana
            +--> WhatsApp 2 (dia 2): audio
            +--> Email 3 (dia 3): numeros/logica
            +--> WhatsApp 3 (dia 3): ultima msg
            +--> Email 4 (dia 4): medo/urgencia
            +--> Email 5 (dia 5): ultima chance
```

---

## AUTOMACAO 1: CAPTURA DE LEAD

**Trigger:** Webhook POST de landing page
**URL:** https://n8n.natuu.net/webhook/renda-shopee-lead

### Dados recebidos:
```json
{
  "name": "Joao Silva",
  "email": "joao@email.com",
  "whatsapp": "11999999999",
  "source": "panfleto-qr",
  "timestamp": "2026-03-30T14:30:00Z"
}
```

### Acoes:
1. Salvar no CRM/planilha (Airtable, Google Sheets, ou Supabase)
2. Adicionar tag: "lead-renda-shopee"
3. Disparar Fluxo de Conversao (se nao comprar em 2h)
4. Enviar email de boas-vindas + link do video

---

## AUTOMACAO 2: FLUXO DE CONVERSAO (quem nao comprou)

**Trigger:** Tag "lead-renda-shopee" + NAO tem tag "comprou"
**Condicao de saida:** Se tag "comprou" for adicionada → PARAR tudo

### Sequencia:

| Tempo | Canal | Conteudo |
|-------|-------|----------|
| Imediato | Email | Email 1: link do video |
| +1h | WhatsApp | "Oi [NOME], ja assistiu o video?" |
| +24h | Email | Email 2: case da Ana (R$2.800/mes) |
| +24h | WhatsApp | Audio de 40s: "queria saber se conseguiu assistir..." |
| +48h | Email | Email 3: numeros e logica |
| +48h | WhatsApp | "Vi que voce ainda nao entrou... garantia 7 dias... [LINK]" |
| +72h | Email | Email 4: medo/urgencia |
| +96h | Email | Email 5: ultima chance |

### Regras:
- Se comprou em qualquer momento → sai do fluxo automaticamente
- Maximo 3 mensagens WhatsApp (nao ser invasivo)
- Emails podem continuar ate 5

---

## AUTOMACAO 3: POS-COMPRA

**Trigger:** Webhook da plataforma de pagamento (Kiwify/Hotmart) ou tag "comprou"

### Acoes imediatas:
1. Adicionar tag: "comprou" + "aluno-desafio"
2. Remover do Fluxo de Conversao
3. Enviar email de boas-vindas com acesso
4. Enviar WhatsApp: "Parabens! Seu acesso: [LINK]"
5. Adicionar na planilha de alunos

### Sequencia pos-compra:

| Tempo | Canal | Conteudo |
|-------|-------|----------|
| Imediato | Email + WhatsApp | Acesso + primeiros passos |
| +3 dias | Email | Check-in: "como esta indo?" + intro upsell |
| +5 dias | Email | Upsell Mentoria Avancado R$197 |
| +7 dias | Email | Ultimo dia da oferta de upsell |

---

## AUTOMACAO 4: ORDER BUMP TRACKING

**Se marcou order bump (Kit Creator R$37):**
1. Tag adicional: "kit-creator"
2. Adicionar ao grupo VIP WhatsApp
3. Enviar templates e lista de produtos por email

---

## AUTOMACAO 5: METRICAS E TRACKING

### UTMs do QR:
```
?utm_source=panfleto&utm_medium=qr&utm_campaign=pacote-v1
```

### Variantes para teste A/B:
```
Variante A: utm_campaign=pacote-v1a (headline renda)
Variante B: utm_campaign=pacote-v1b (headline simplicidade)
Variante C: utm_campaign=pacote-v1c (headline voce-vendesse)
```

### Metricas a acompanhar:

| Metrica | Como medir | Meta |
|---------|-----------|------|
| Scans do QR | UTM no analytics | 2-5% dos pacotes |
| Cadastros | Webhook n8n | 50-70% dos scans |
| Assistiu video | Evento de play | 60%+ dos cadastros |
| Conversao (compra) | Webhook pagamento | 5-10% dos cadastros |
| Order bump | Tag no CRM | 30-50% dos compradores |
| Upsell | Tag no CRM | 10% dos compradores |

### Dashboard sugerido:
- Google Sheets ou Airtable com totalizadores diarios
- n8n atualiza automaticamente via webhooks
- Metricas-chave: pacotes → scans → leads → vendas → receita

---

## FERRAMENTAS NECESSARIAS

| Ferramenta | Funcao | Custo |
|-----------|--------|-------|
| Landing page | Captura + video + checkout | Vercel (gratis) |
| n8n | Automacoes e webhooks | Ja tem (n8n.natuu.net) |
| Email (Fustec/Mailchimp/Brevo) | Disparos e sequencias | R$0-100/mes |
| WhatsApp Business API | Mensagens automaticas | Depende do volume |
| Kiwify ou Hotmart | Pagamento + entrega do curso | % por venda |
| Airtable ou Supabase | CRM/banco de leads | Ja tem (Supabase) |
| Plataforma de curso | Hospedar as 12 aulas | Hotmart Club / Kiwify |
| Google Analytics | Tracking de UTMs | Gratis |

---

## IMPLEMENTACAO NO N8N

### Workflow 1: Captura de Lead
```
Webhook (POST) → Set Fields → Supabase Insert → IF (has email)
    → Send Welcome Email (Brevo/SMTP)
    → Wait 1h
    → Send WhatsApp (API)
    → Wait 24h
    → Check if tag "comprou" exists
        → YES: Stop
        → NO: Send Email 2 → Continue sequence...
```

### Workflow 2: Compra Aprovada
```
Webhook (Kiwify/Hotmart) → Parse Payment Data → Supabase Update (tag: comprou)
    → Send Access Email
    → Send WhatsApp Welcome
    → Wait 3 days
    → Send Check-in Email
    → Wait 2 days
    → Send Upsell Email
```

### Workflow 3: Metricas Diarias (cron)
```
Cron (todo dia 8h) → Supabase Query (leads do dia anterior)
    → Calculate: scans, leads, vendas, receita
    → Update Google Sheets dashboard
    → IF receita < meta → Alert WhatsApp para o dono
```
