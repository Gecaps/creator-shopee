# Yampi - Creator Shopee

## Credenciais da API

- **Alias:** creators-brasil
- **Merchant ID:** 1501564
- **User ID:** 1541135
- **User-Token:** DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5
- **User-Secret-Key:** sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P
- **Plano:** Basic

## API Base URL

```
https://api.dooki.com.br/v2/creators-brasil
```

## Gateway de Pagamento

- **Mercado Pago** (OAuth) - ativo
- Meios aceitos: Visa, Mastercard, Elo, Amex, Diners, Discover, Aura, Hiper, Boleto, Pix

## Marca

| Campo | Valor |
|-------|-------|
| Brand ID | 45707627 |
| Nome | Creators Brasil |

---

## Escada de Valor — Produtos

### 1. Desafio Creator Shopee (Entry — R$29,90)

| Campo | Valor |
|-------|-------|
| Product ID | 44579820 |
| SKU ID | 295991787 |
| SKU Code | CREATOR-SHOPEE-001 |
| SKU Token | MXUJIPNG1W |
| Preço | R$29,90 |
| Tipo | Digital |
| Link de Pagamento (ID: 45494) | https://creators-brasil.pay.yampi.com.br/pay/FCBGQ7BJRG |
| Compra Direta (SKU) | https://creators-brasil.pay.yampi.com.br/r/MXUJIPNG1W |

### 2. Kit Creator Profissional (Order Bump — R$37)

| Campo | Valor |
|-------|-------|
| Product ID | 44581442 |
| SKU ID | 296002258 |
| SKU Code | KIT-CREATOR-001 |
| SKU Token | HPQZC73EZJ |
| Preço | R$37,00 |
| Tipo | Digital |
| Link de Pagamento (ID: 45509) | https://creators-brasil.pay.yampi.com.br/pay/GF0R8KH0MG |

### 3. Mentoria Creator Avançado (Upsell 1 — R$197)

| Campo | Valor |
|-------|-------|
| Product ID | 44581443 |
| SKU ID | 296002259 |
| SKU Code | MENTORIA-CREATOR-001 |
| SKU Token | SM9Y17TD8S |
| Preço | R$197,00 |
| Tipo | Digital |
| Link de Pagamento (ID: 45510) | https://creators-brasil.pay.yampi.com.br/pay/SC6KTZSW |

### 4. Pack Creator Pro (Upsell 2 — R$497)

| Campo | Valor |
|-------|-------|
| Product ID | 44581444 |
| SKU ID | 296002260 |
| SKU Code | PACK-CREATOR-001 |
| SKU Token | LWT43J6HSG |
| Preço | R$497,00 |
| Tipo | Digital |
| Link de Pagamento (ID: 45511) | https://creators-brasil.pay.yampi.com.br/pay/5ZGGW2AWIA |

---

## Fluxo Pós-Compra

```
Landing page (index.html) → Checkout Yampi (R$29,90)
  → upsell-mentoria.html (R$197)
    → [SIM] → Checkout Yampi Mentoria → upsell-pack.html
    → [NÃO] → upsell-pack.html (R$497)
      → [SIM] → Checkout Yampi Pack → obrigado.html
      → [NÃO] → obrigado.html
```

---

## URLs Importantes

- **Painel:** https://app.yampi.com.br/home
- **Catálogo:** https://creators-brasil.catalog.yampi.io
- **Landing Page:** https://creator-shopee.vercel.app
- **API Docs:** https://docs.yampi.com.br

## Exemplo de Requisição

```bash
curl -H "User-Token: DDxKQesuoMv9Lhyuv2paP42EhznDTPDMQkJqMRX5" \
     -H "User-Secret-Key: sk_SRoI6VVsJR1CuXVvIrggNRaLsvByIrzVSMQ7P" \
     -H "Content-Type: application/json" \
     "https://api.dooki.com.br/v2/creators-brasil/catalog/products"
```

## Observações

- Todos os produtos são pagamento único (não recorrente)
- Yampi não suporta recorrência nativa
- Checkout transparente via Mercado Pago (cartão, Pix, boleto)
- Parcelamento configurado direto no painel do Mercado Pago
- Todos os SKUs com availability 9999 e quantity_managed false
