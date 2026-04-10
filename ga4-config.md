# Google Analytics 4 — Creator Brasil

## Propriedade
- **Property ID:** 531286149
- **Account ID:** 389913570
- **Measurement ID:** G-G0J2SXC40E
- **Stream ID:** 14311931775
- **URL:** https://creatorbrasil.com.br

## Configuracao
- **Retencao de dados:** 14 meses
- **Google Signals:** Ativo
- **Trafego interno:** Filtro ativo (IPv6 prefix 2804:868:d052)
- **Search Console:** Vinculado

## Custom Dimensions
| Dimensao | Parametro | Descricao |
|----------|-----------|-----------|
| Tipo de Conteudo | content_type | Tipo de pagina (aula, modulo, landing_page) |
| Tipo de Plano | plan_type | Plano selecionado pelo usuario |
| Fonte do Lead | lead_source | De onde veio o lead |
| Nome do Formulario | form_name | Qual formulario foi preenchido |
| Nome da Feature | feature_name | Feature utilizada pelo usuario |

## Custom Metrics
| Metrica | Parametro | Unidade |
|---------|-----------|---------|
| Tempo de Leitura | reading_time | Segundos |

## Key Events
- sign_up
- generate_lead
- purchase
- begin_checkout
- add_to_cart

## Service Account (API)
- **Email:** gecaps-analytics@gecaps-workspace.iam.gserviceaccount.com
- **Key:** ~/gecaps/PROJETOS/BLOG GECAPS/secrets/ga4-service-account.json
- **Permissao:** Editor

## Data Layer (eventos recomendados)
```javascript
// Signup no curso
dataLayer.push({ event: 'sign_up', method: 'form', plan_type: 'basico' });

// Lead capturado
dataLayer.push({ event: 'generate_lead', lead_source: 'landing_page', form_name: 'checkout' });

// Compra
dataLayer.push({ event: 'purchase', currency: 'BRL', value: 97.00, items: [{ item_name: 'Curso Creator Shopee' }] });
```

*Configurado em: 2026-04-04*
