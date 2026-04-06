# Creator Shopee — Documento Mestre do Projeto

> Última atualização: 2026-04-04
> Status: MemberKit configurado (5 módulos, 15 aulas, capas subidas), Yampi integrada, deploy no ar. **Falta gravar os vídeos das aulas**, teste de compra e analytics.

---

## 1. VISÃO GERAL

**O que é:** Curso/desafio de 30 dias que ensina pessoas comuns a ganhar comissão como afiliado/creator na Shopee. Garantia de 7 dias.

**Modelo de negócio:** Pagamento único R$29,90 + upsell Mentoria R$197 + upsell Pack Pro R$497.

**Canal de aquisição principal:** Panfletos com QR code inseridos dentro dos pacotes da Shopee (~2.000 pacotes/dia).

**Funil completo:**
```
Panfleto (QR → /pflto) → Landing Page (index.html) → Checkout Yampi (R$29,90)
    → Página de Obrigado (obrigado.html) → Upsell Mentoria (R$197)
        → [Aceita] Checkout Mentoria
        → [Rejeita] Upsell Pack Pro (R$497)
            → [Aceita] Checkout Pack Pro
            → [Rejeita] obrigado-basico.html (sem WhatsApp)
```

**Variação:** `upsell-pack-vip.html` é idêntico ao `upsell-pack.html`, mas ao rejeitar redireciona para `obrigado.html` (com WhatsApp) em vez de `obrigado-basico.html`.

---

## 2. STACK TÉCNICA

| Componente | Ferramenta | Status |
|-----------|-----------|--------|
| Landing Page | HTML estático (Tailwind) | ✅ Deploy Vercel |
| Checkout/Pagamento | Yampi + Mercado Pago | ✅ 4 produtos configurados |
| Plataforma de curso | **MemberKit** (creator-brasil.memberkit.com.br) | ✅ Curso criado, 5 módulos, 15 aulas |
| Integração Yampi → MemberKit | Webhook nativo | ✅ Configurado |
| Emails de acesso | MemberKit (automático) | ✅ Envia ao liberar acesso |
| WhatsApp grupo | chat.whatsapp.com/JLT1F6ZlARd6R5lS2t2aaL | ✅ Criado |
| Automações | n8n (n8n.natuu.net) | ⏳ Pendente |
| CRM/Leads | Supabase | ⏳ Pendente |
| Analytics | **Google Analytics 4** (G-G0J2SXC40E) | ✅ Em todas as páginas do funil |

---

## 3. MEMBERKIT — PLATAFORMA DE CURSO

### Acesso
| Campo | Valor |
|-------|-------|
| Painel admin | `https://creator-brasil.memberkit.com.br` |
| Login | Via `https://painel.memberkit.com.br` |
| Curso | Desafio Creator Shopee |
| URL do curso | `creator-brasil.memberkit.com.br/280225-desafio-creator-shopee` |

### Estrutura do Curso (5 módulos, 15 aulas)

**Módulo 0 — Comece por Aqui** (Boas-vindas)
1. Bem-vindo ao Desafio
2. Como usar a plataforma
3. Entre no grupo do WhatsApp

**Módulo 1 — Preparação** (Dias 1-3)
4. Como funciona o programa de afiliados da Shopee
5. Criando sua conta de Creator
6. Escolhendo seus primeiros produtos

**Módulo 2 — Primeiras Vendas** (Dias 4-10)
7. Como criar links de afiliado
8. Divulgando no WhatsApp
9. Divulgando no Instagram e TikTok

**Módulo 3 — Acelerando** (Dias 11-20)
10. Criando conteúdo que vende
11. Encontrando produtos com alta comissão
12. Estratégias para aumentar cliques

**Módulo 4 — Escalando** (Dias 21-30)
13. Analisando seus resultados
14. Escalando de R$500 para R$3.000
15. Próximos passos e plano de ação

### Integração Yampi → MemberKit (Webhook)

| Campo | Valor |
|-------|-------|
| URL de notificação | `https://memberkit.com.br/callbacks/yampi/2jiadVshas7kaZPLjUX2tX1R` |
| Webhook ID (Yampi) | `wh_kl3YJnTweqG0WW915LyMO2fRy2Ce5r8C4FwvB` |
| Chave secreta | `sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P` |
| Eventos | Pedido criado + Status de pedido atualizado |
| Produtos | Todos (sem filtro) |
| Modalidade | Cursos |

**Fluxo automático:** Compra aprovada na Yampi → webhook → MemberKit cadastra aluno → email de acesso enviado automaticamente.

### Emails
O MemberKit envia automaticamente os emails de acesso ao curso (boas-vindas, dados de login). Não precisa de Resend/Brevo para isso — o MemberKit cuida sozinho.

---

## 4. PRODUTOS E PREÇOS (YAMPI)

Todos os produtos são **pagamento único** (não recorrente). Yampi não suporta recorrência nativa.

| # | Produto | Preço | Link de Checkout | Tipo |
|---|---------|-------|-----------------|------|
| 1 | Desafio Creator Shopee | R$29,90 | https://creators-brasil.pay.yampi.com.br/pay/FCBGQ7BJRG | Entry point |
| 2 | Kit Creator Profissional | R$37,00 | https://creators-brasil.pay.yampi.com.br/pay/GF0R8KH0MG | Order bump |
| 3 | Mentoria Creator Avançado | R$197,00 | https://creators-brasil.pay.yampi.com.br/pay/SC6KTZSW | Upsell 1 |
| 4 | Pack Creator Pro | R$497,00 | https://creators-brasil.pay.yampi.com.br/pay/5ZGGW2AWIA | Upsell 2 |

### Credenciais da API Yampi

| Campo | Valor |
|-------|-------|
| Alias | `creators-brasil` |
| Merchant ID | `1501564` |
| User-Token | `DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5` |
| User-Secret-Key | `sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P` |
| API Base URL | `https://api.dooki.com.br/v2/creators-brasil` |
| Gateway | Mercado Pago (cartão, Pix, boleto) |

---

## 5. VERCEL — DEPLOY

| Campo | Valor |
|-------|-------|
| Project ID | `prj_ZnG7qwhmZiufFdMqvp6SuUsU4xAt` |
| Org/Team ID | `team_D4ZnxzaEIErRm3BwTHbTXtpp` |
| Project Name | `creator-shopee` |
| Conta | ia@gecaps.com.br |
| GitHub Repo | `Gecaps/creator-shopee` (privado) |

### Domínios ativos
- `creator-shopee-gecaps.vercel.app`
- `creatorbrasil.com.br` (DNS no Cloudflare, SSL Full)

### Redirects planejados (vercel.json)

| Redirect | Destino | Uso |
|----------|---------|-----|
| `/pflto` | `/?utm_source=panfleto&utm_medium=qr&utm_campaign=pacote` | ✅ Já configurado |
| `/checkout` | Yampi checkout R$29,90 | ⏳ Pendente |
| `/curso` | MemberKit área do aluno | ⏳ Pendente |
| `/grupo` | WhatsApp grupo de suporte | ⏳ Pendente |

### ⚠️ ALERTA — Deploys falhando

Os **últimos 10 deploys estão em ERROR** (sem build logs). O último deploy funcional é o commit `8aa298a` ("briefing pro designer").

**Causa provável:** O repo foi mudado de **público para privado** no GitHub. A integração Vercel pode ter perdido acesso.

**Ação necessária:** Verificar se o GitHub App da Vercel tem acesso ao repo privado `Gecaps/creator-shopee`.

---

## 6. ESTRUTURA DE ARQUIVOS

```
/CREATOR SHOPEE/
├── index.html                  # Landing page principal (R$29,90)
├── obrigado.html               # Pós-compra: confirmação + grupo WhatsApp + próximos passos
├── obrigado-basico.html        # Pós-compra alternativa: SEM grupo WhatsApp
├── upsell-mentoria.html        # Upsell 1: Mentoria R$197
├── upsell-pack.html            # Upsell 2: Pack Pro R$497 → rejeitar leva a obrigado-basico
├── upsell-pack-vip.html        # Upsell 2 (variação): rejeitar leva a obrigado (com WhatsApp)
├── panfleto-frente.html        # Panfleto frente (420x618px, para impressão)
├── panfleto-verso.html         # Panfleto verso (420x618px, para impressão)
├── banner-checkout.html        # Banner 1200x400 — produto base
├── banner-mentoria.html        # Banner 1200x400 — mentoria (tema roxo)
├── banner-pack.html            # Banner 1200x400 — pack pro (tema dourado)
├── product-image.html          # Card 800x800 — imagem de produto para social
├── memberkit-banner-topo.html  # Banner MemberKit topo do curso (1152x320)
├── memberkit-banner-capa.html  # Banner MemberKit capa/vitrine (430x215)
├── memberkit-modulos.html      # 5 capas de módulo para MemberKit (em exportação)
├── vercel.json                 # Redirect /pflto → LP com UTMs de panfleto
├── yampi-config.md             # Config detalhada da Yampi (credenciais, IDs, escada de valor)
├── pesquisa-dores-audiencia-renda-extra.md  # Pesquisa de audiência (dados CNC, IBGE, dores, perfil)
├── PROJETO.md                  # ← ESTE ARQUIVO
├── .vercel/                    # Config de deploy Vercel
├── .git/                       # Git (repo: Gecaps/creator-shopee, branch: main)
├── assets/
│   ├── banner-checkout.png     # PNG do banner base
│   ├── banner-mentoria.png     # PNG do banner mentoria
│   ├── banner-pack.png         # PNG do banner pack
│   ├── panfleto-frente.png     # PNG do panfleto frente
│   ├── panfleto-verso.png      # PNG do panfleto verso
│   ├── product-desafio.png     # PNG do card de produto
│   ├── qrcode-panfleto.png     # QR code (laranja)
│   ├── qrcode-panfleto-pb.png  # QR code (preto e branco)
│   ├── memberkit-banner-topo.png   # Banner topo MemberKit (1152x320)
│   └── memberkit-banner-capa.png   # Banner capa MemberKit (430x215)
└── copy/
    ├── automacoes.md            # Fluxos n8n: captura, conversão, pós-compra (⚠️ referencia Kiwify/Hotmart)
    ├── emails.md                # Sequência de 8 emails (5 conversão + 3 pós-compra) + 3 WhatsApp
    ├── panfleto.md              # Copy do panfleto (versão agressiva, pedido do Gerson)
    ├── video-script.md          # Script do vídeo 3:30-4:30min (Hook→Story→Mecanismo→Prova→Oferta)
    └── briefing-designer-panfleto.md  # Briefing para designer trocar textos no Canva
```

---

## 7. PÁGINAS — DETALHAMENTO

### index.html (Landing Page)
- Headline: "De zero a R$3.000/mês como Creator Shopee"
- Preço: R$29,90 (pagamento único, acesso vitalício)
- CTA: "QUERO MEU ACESSO POR R$29,90" → checkout Yampi
- Seções: Hero → Benefícios (4 cards) → Por que funciona → Como funciona (3 passos) → Pricing → FAQ (6 perguntas)
- Garantia: 7 dias
- Analytics: nenhum

### obrigado.html (Pós-compra principal)
- Confirmação de pagamento + 3 próximos passos
- Botão: "ENTRAR NO GRUPO DO WHATSAPP" → chat.whatsapp.com/JLT1F6ZlARd6R5lS2t2aaL
- Botão: "Acessar a plataforma do curso (em breve)" → # (inativo — atualizar para MemberKit)

### obrigado-basico.html (Pós-compra sem WhatsApp)
- Versão simplificada, SEM link do grupo WhatsApp
- Usado quando pessoa rejeita o upsell Pack Pro

### upsell-mentoria.html (R$197)
- Headline: "O desafio te ensina a começar. A mentoria te ensina a escalar."
- Inclui: 30 dias de acompanhamento, estratégias avançadas, calls semanais
- CTA: "QUERO A MENTORIA — R$197"
- Rejeitar → upsell-pack.html

### upsell-pack.html / upsell-pack-vip.html (R$497)
- Headline: "Pack Creator Pro — 30 produtos prontos e estratégia completa"
- Inclui: 30 produtos validados, 4 calls ao vivo, suporte prioritário
- CTA: "QUERO O PACK CREATOR PRO — R$497"
- Rejeitar → obrigado-basico.html (pack) ou obrigado.html (pack-vip)

---

## 8. COPY E MATERIAIS

### Sequência de Emails (copy/emails.md)
**Fluxo conversão (5 emails, padrão GLM):**
1. Imediato: GANHO — "seu acesso está aqui" + link vídeo
2. Dia 2: GANHO — Case Ana (R$2.800/mês)
3. Dia 3: LÓGICA — Números da Shopee
4. Dia 4: MEDO — FOMO
5. Dia 5: ÚLTIMA CHANCE

**Fluxo pós-compra (3 emails):**
1. Imediato: Bem-vindo + link plataforma
2. Dia 3: Check-in + introdução mentoria
3. Dia 5: Upsell completo mentoria

**WhatsApp (3 mensagens):** 1h, Dia 2, Dia 3

### Panfleto (copy/panfleto.md)
- Versão agressiva (pedido do Gerson)
- Frente: "GANHE ATÉ R$3.000 POR MÊS" + QR code
- Verso: Dicotomia + 3 passos + oferta R$29,90
- Specs: 10x15cm, couchê 150g, ~R$0,08-0,12/unidade

### Vídeo (copy/video-script.md)
- 3:30-4:30min, talking head
- Estrutura: Hook → Story → Mecanismo → Prova → Oferta

---

## 9. HISTÓRICO DE PEDIDOS (YAMPI)

| Data | Cliente | Pagamento | Status | Resultado |
|------|---------|-----------|--------|-----------|
| 31/03 21:38 | Adriano teste (teste interno) | Pix | Cancelado | Pix expirou, Mercado Pago cancelou |
| 02/04 11:11 | Laise Pereira Dias | Pix | Cancelado | Pix expirou (1h35), Mercado Pago cancelou |

Ambos via mobile, link direto (sem UTMs de panfleto), sem order bump, sem upsell. Nenhuma compra concluída até o momento.

---

## 10. O QUE FOI FEITO EM 03/04/2026

### Mapeamento completo do projeto
- [x] Análise de todos os 12 arquivos HTML (landing, upsells, obrigado, panfletos, banners)
- [x] Análise de todos os 7 arquivos de copy/docs
- [x] Verificação do estado do GitHub (21 commits)
- [x] Verificação da Vercel (deploys falhando)
- [x] Consulta de pedidos na Yampi (2 pedidos, ambos cancelados por Pix expirado)
- [x] PROJETO.md atualizado com estado real

### MemberKit — Plataforma de curso
- [x] Assinatura do MemberKit
- [x] Criação do curso "Desafio Creator Shopee" com descrição, URL de vendas
- [x] Definição da estrutura: 5 módulos, 15 aulas (com títulos e descrições completas)
- [x] Banner de topo (1152x320) e capa (430x215) gerados e uploadados
- [x] Configuração do onboarding (texto de boas-vindas no primeiro acesso)
- [x] Texto para não-matriculados configurado
- [x] Layout: poster vertical com imagens de capa

### Capas dos módulos (3 iterações)
- [x] v1: Landscape 430x215 com emojis — **descartada** (formato errado, emojis infantis)
- [x] v2: Portrait 300x420 com emojis + sacolinhas Shopee — **descartada** (emojis ainda infantis)
- [x] v3: Portrait 300x420 com ilustrações CSS profissionais — **aprovada**
  - Mod 0 (verde): Radar/pulso — ponto de partida
  - Mod 1 (laranja): Celular com tela — tudo pelo celular
  - Mod 2 (vermelho): Gráfico de barras + PIX — primeiras vendas
  - Mod 3 (roxo): Velocímetro — aceleração
  - Mod 4 (dourado): Montanha com bandeira + R$3.000/mês — escalada
- [x] Capas exportadas em 300x420 e copiadas para Downloads

### Integração Yampi → MemberKit
- [x] Integração nativa via webhook configurada no MemberKit
- [x] Webhook criado na Yampi (eventos: pedido criado + status atualizado)
- [x] Todos os produtos vinculados (sem filtro)
- [x] Fluxo automático: compra → webhook → acesso liberado → email enviado
- [x] Primeira tentativa com chave secreta errada — corrigido, URL mudou

### Drip Content (liberação programada)
- [x] Pesquisado como funciona no MemberKit (9 regras disponíveis)
- [x] Configuração definida: Mod 0+1 imediato, Mod 2 dia 4, Mod 3 dia 11, Mod 4 dia 21
- [x] Erro ao salvar na Turma A — resolvido trocando pra aba anônima (conflito de sessão)

### Emails
- [x] Pesquisado Resend (API, integração, DNS, limites)
- [x] Decidido que o MemberKit cuida dos emails de acesso automaticamente
- [x] Resend descartado por enquanto (MemberKit não suporta nativamente)

### Redirects (vercel.json)
- [x] `/pflto` → LP com UTMs (já existia)
- [x] `/checkout` → Yampi checkout R$29,90
- [x] `/curso` → MemberKit área do aluno
- [x] `/grupo` → WhatsApp grupo de suporte
- [x] Todos testados via curl — funcionando (307 redirect)

### Deploy Vercel — Diagnóstico e correção
- [x] Identificado: últimos 14 deploys em ERROR desde commit `d0c8c66`
- [x] Causa raiz: repo mudou de **público para privado** no GitHub
- [x] Deploy via CLI também falhava (mesmo erro "Unexpected error")
- [x] Painel Vercel mostrou: "Deployment blocked — no git user associated with commit"
- [x] **Solução:** repo voltou para público via `gh repo edit --visibility public`
- [x] Deploy via CLI funcionou: `dpl_4AAnU2rKtPBumxBPPfVJ8Sf5h4Fy` — READY
- [x] Site no ar: `https://creatorbrasil.com.br` com todos os redirects
- [x] Commit + push das mudanças locais ✅ 04/04

### Organização de arquivos
- [x] Downloads organizado: pasta "Creator Shopee" criada com capas + panfleto PDF
- [x] Mapeamento de todas as pastas (squad-fullstack, GPTMaker, Regulatório, etc.)

### Erros e aprendizados
1. **Playwright screenshot travava** — resolvido usando Chrome headless nativo
2. **Formato das capas errado** (430x215 → 300x420) — refiz no formato portrait
3. **Emojis ficaram infantis** — evoluiu pra ilustrações CSS (radar, celular, gráfico, velocímetro, montanha)
4. **Deploy Vercel bloqueado** — repo privado no GitHub impedia. Solução: voltar pra público
5. **MemberKit não salva drip content** — resolvido em aba anônima
6. **MemberKit API não cria cursos** — só gerencia membros/matrículas. Criação é manual no painel
7. **Resend não integra com MemberKit** — MemberKit tem seu próprio sistema de email

---

## 10b. O QUE FOI FEITO EM 04/04/2026

### Correções de inconsistências
- [x] Links "Acessar plataforma" em obrigado.html e obrigado-basico.html: `#` → `https://creator-brasil.memberkit.com.br`
- [x] Banner pack: "50 produtos" → "30 produtos" (alinhado com upsell-pack.html)
- [x] copy/automacoes.md: referências a Kiwify/Hotmart → Yampi + Mercado Pago + MemberKit

### Google Analytics 4
- [x] Property GA4 criada (conta GECAPS)
- [x] Measurement ID: `G-G0J2SXC40E`
- [x] API Secret Key (Measurement Protocol): `IwjWiXosQPWvickK3btRaQ`
- [x] gtag.js adicionado em 6 páginas do funil: index, obrigado, obrigado-basico, upsell-mentoria, upsell-pack, upsell-pack-vip
- [x] Banners/panfletos/memberkit HTMLs não incluídos (são só pra exportar imagem)

### Commits e deploys
- [x] Commit `c3c0b9a`: fix de links, banner e docs
- [x] Commit `fca2eb9`: GA4 em todas as páginas
- [x] Push pro GitHub, deploy automático na Vercel

---

## 10c. O QUE FOI FEITO EM 05/04/2026

### Landing page Creator Summit (`creatorsummit.html`)
- [x] Página criada em `creatorbrasil.com.br/creatorsummit`
- [x] Rewrite `/creatorsummit` → `/creatorsummit.html` no vercel.json
- [x] Design dark baseado no template PULSE (fornecido pelo Gerson)
- [x] TikTok SVG grande (400px) no fundo do hero — funcionando
- [x] Celular mockup com tela de notificações Shopee/TikTok (comissões, vendas, saque)
- [x] Copy baseada na pesquisa de mercado (pesquisa-dores-audiencia-renda-extra.md)
- [x] Headline: "Se você sabe mandar um zap, já pode ganhar R$3.000/mês"
- [x] Seções: hero, vantagens (bento grid), como funciona (4 passos), desafio (módulos), oferta, FAQ
- [x] FAQ com objeções reais: "É golpe?", "Já comprei curso", "Não entendo de tecnologia"
- [x] GA4 incluído
- [ ] **Pendente:** sacolinha Shopee não renderiza visível no fundo escuro — corrigir na próxima sessão

### Evolution API
- [x] DNS `evolution.gecaps.com.br` criado no Cloudflare → servidor Hetzner (178.104.45.62)
- [x] Evolution API instalada no EasyPanel pelo Gerson
- [x] API Key salva: `29683C4C977415CAAFCCE10F7D57E11`
- [x] Credenciais salvas em `N8N GECAPS/secrets/n8n_credentials.env`
- [ ] **Pendente:** criar instância e conectar WhatsApp do Gerson

### Automações n8n
- [ ] **Pendente:** montar workflow Yampi → n8n → WhatsApp (aguardando instância Evolution)

---

## 11. INCONSISTÊNCIAS CONHECIDAS

### ~~⚠️ automacoes.md desatualizado~~ ✅ Corrigido em 04/04
~~O arquivo `copy/automacoes.md` referencia Kiwify/Hotmart como plataforma.~~ Atualizado para Yampi + Mercado Pago + MemberKit.

### ~~⚠️ Banner Pack — quantidade divergente~~ ✅ Corrigido em 04/04
~~O `banner-pack.html` diz "50 produtos", mas o `upsell-pack.html` diz "30 produtos".~~ Alinhado para 30 produtos.

### ~~⚠️ Sem analytics~~ ✅ Corrigido em 04/04
Google Analytics 4 instalado em todas as 6 páginas do funil. Measurement ID: `G-G0J2SXC40E`. API Secret Key: `IwjWiXosQPWvickK3btRaQ`.

### ~~⚠️ Link "Acessar plataforma" nas páginas de obrigado~~ ✅ Corrigido em 04/04
~~Os botões apontavam para `#` (inativo).~~ Atualizados para `https://creator-brasil.memberkit.com.br`.

### Regra de acesso ao grupo WhatsApp
O grupo VIP do WhatsApp é **exclusivo para quem comprou upsell** (Mentoria R$197 ou Pack Pro R$497). Quem comprou só o Desafio (R$29,90) **NÃO tem acesso** ao grupo. O link aparece apenas em `obrigado.html` (pós-upsell). O redirect `/grupo` foi desativado — redireciona pra landing page.

### ~~⚠️ Mudanças locais não commitadas~~ ✅ Resolvido em 04/04
Todas as correções commitadas e pushadas (commits `c3c0b9a` e `fca2eb9`).

---

## 12. PRÓXIMOS PASSOS

### Imediato (próxima sessão)
- [x] **Commit + push** das mudanças locais pro GitHub ✅ 04/04 (commits c3c0b9a, fca2eb9)
- [x] **Subir capas v3** dos 5 módulos no MemberKit ✅ já feito
- [ ] **Fazer teste de compra** — validar fluxo completo: Yampi → MemberKit → email → acesso
- [x] **Atualizar links** das páginas de obrigado: `#` → `creator-brasil.memberkit.com.br` ✅ 04/04
- [ ] **Verificar drip content** — confirmar que as regras de liberação salvaram no MemberKit

### Curto prazo
- [x] Instalar analytics (GA4 G-G0J2SXC40E) em todas as páginas do funil ✅ 04/04
- [x] Atualizar `copy/automacoes.md` (trocar Kiwify/Hotmart → Yampi + MemberKit) ✅ 04/04
- [x] Alinhar "30 vs 50 produtos" no banner vs upsell ✅ 04/04
- [ ] Configurar webhook da Yampi no n8n para capturar pedidos
- [ ] Testar checkout completo (compra + upsells + obrigado)

### Médio prazo
- [ ] **Sacolinha Shopee no fundo do hero** — SVG não renderiza visível no fundo escuro. Tentar PNG com fundo transparente ou ajustar cor/opacity
- [ ] Gravar vídeo da página de obrigado (script pronto)
- [ ] Implementar automações n8n (captura → conversão → pós-compra)
- [ ] Imprimir primeira tiragem de panfletos (10.000 para teste)
- [ ] **Gravar vídeos das 15 aulas** e subir no MemberKit (estrutura e módulos já estão prontos)

---

## 13. YAMPI API — REFERÊNCIA RÁPIDA

### Endpoints mais usados

| Ação | Método | Endpoint |
|------|--------|----------|
| Listar produtos | GET | `/catalog/products` |
| Criar produto | POST | `/catalog/products` |
| Atualizar produto | PUT | `/catalog/products/{id}` |
| SKUs do produto | GET | `/catalog/products/{id}/skus` |
| Listar pedidos | GET | `/orders` |
| Ver pedido | GET | `/orders/{id}` |
| Links de pagamento | GET | `/checkout/payment-link` |
| Criar link pagamento | POST | `/checkout/payment-link` |
| Webhooks | GET | `/webhooks` |
| Criar webhook | POST | `/webhooks` |

### Headers obrigatórios
```
User-Token: DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5
User-Secret-Key: sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P
Content-Type: application/json
```

### MemberKit API — Referência

| Ação | Método | Endpoint |
|------|--------|----------|
| Listar cursos | GET | `/api/v1/courses` |
| Ver curso | GET | `/api/v1/courses/{id}` |
| Listar turmas | GET | `/api/v1/classrooms` |
| Cadastrar/matricular membro | POST | `/api/v1/users` |
| Listar membros | GET | `/api/v1/users` |
| Gerar link mágico | POST | `/api/v1/tokens` |

**Base URL:** `https://memberkit.com.br/api/v1/`
**Auth:** `?api_key=SUA_API_KEY` (encontrada em Configurações do MemberKit)
**Rate limit:** 120 req/min
