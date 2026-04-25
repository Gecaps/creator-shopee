# CLAUDE.md

Site e funil de vendas do Desafio Creator Shopee (Creators Brasil).

## URLs
- **Producao:** https://creatorbrasil.com.br
- **GitHub:** Gecaps/creator-shopee
- **Vercel:** ia@gecaps.com.br

## O que e
Landing page + funil de vendas do curso "Desafio Creator Shopee". Produto: R$29,90 pagamento unico, acesso vitalicio, garantia 7 dias. Checkout via Yampi.

## Stack
- HTML/CSS estatico (sem framework)
- Deploy Vercel com vercel.json (rewrites)
- Checkout: Yampi
- Paginas de obrigado e upsell incluidas

## Arquivos principais
| Arquivo | Descricao |
|---------|-----------|
| index.html | Landing page principal |
| creatorsummit.html | Pagina Creator Summit |
| obrigado.html / obrigado-basico.html | Paginas pos-compra |
| upsell-mentoria.html | Upsell Mentoria R$197 |
| upsell-pack.html / upsell-pack-vip.html | Upsell Pack Pro R$497 |
| banner-*.html | Banners para MemberKit |
| memberkit-modulos*.html | Paginas de modulos do curso |
| copy/ | Copies de vendas e emails |
| assets/ | Imagens e logos |

## Produtos do funil
| Produto | Preco | Checkout |
|---------|-------|----------|
| Desafio Creator Shopee | R$29,90 | Yampi |
| Mentoria Creator | R$197 | Yampi |
| Pack Pro Creator | R$497 | Yampi (automacao de funil) |

## Regras
- Textos de venda revisados em `copy/` — nao alterar sem aprovacao
- GA4 configurado (ver ga4-config.md)
- Dominio creatorbrasil.com.br no Cloudflare (SSL Full, proxy OFF)

## Referencia completa
Ver `PROJETO.md` para detalhes do funil, copies e estrategia.
