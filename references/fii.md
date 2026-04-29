# FIIs — Fundos de Investimento Imobiliário

## Tipos de FII

### Tijolo (imóveis físicos)
Receita vem de aluguéis de imóveis físicos.
- **Logística**: galpões, CDUs — XPLG11, BRCO11, HGLG11
- **Escritórios**: lajes corporativas — KNRI11, BRCR11, RBRP11
- **Shopping**: shoppings centers — XPML11, VISC11, HSML11
- **Residencial**: apartamentos — IRDM11, RZTR11
- **Híbrido**: mistura de segmentos — KNRI11, RECT11

**Métricas principais:**
| Métrica | Bom | Atenção |
|---------|-----|---------|
| P/VP | 0.80–1.05 | > 1.20 (caro) |
| Dividend Yield | > 8% (SELIC 14%) | < 6% |
| Vacância física | < 10% | > 20% |
| Vacância financeira | < 8% | > 15% |
| LTV (Loan-to-Value) | < 30% | > 50% |
| ABL (Área Bruta Locável) crescendo | + | — |
| Cap Rate implícito | > 8% | < 5% |

**P/VP médio do mercado (2026):** 0.84 para tijolo → segmento barato vs histórico

### Papel (CRI/LCI/LCA)
Receita vem de recebíveis imobiliários (CRIs, LCIs).
- Indexados a: CDI, IPCA+, IGPM+, prefixado
- **Papel CDI**: KNCR11, MXRF11, RBRF11 — protege em alta de juros
- **Papel IPCA**: IRDM11, KNIP11, HABT11 — protege contra inflação

**Métricas principais:**
| Métrica | Bom | Atenção |
|---------|-----|---------|
| P/VP | 0.95–1.10 | > 1.20 |
| Dividend Yield | > IPCA + 5% | < SELIC × 0.70 |
| Duration da carteira | Curta em alta juros | Longa em alta juros |
| Subordinação CRI | > 20% | < 10% |
| LTV médio da carteira | < 60% | > 75% |
| % CRI inadimplente | 0% | > 5% |

### FOF (Fundo de Fundos)
Investe em cotas de outros FIIs.
- P/VP deve ser avaliado contra o P/VP médio da carteira
- Gestão ativa tem que justificar o custo (taxa de administração)
- Bom para diversificação com pouco capital

---

## Distribuição de rendimentos (yield)

```
Dividend Yield Mensal = (Rendimento por cota / Preço da cota) × 100

DY anualizado = DY mensal × 12  (mas não é garantia — yield varia)

Comparação com CDI:
- DY FII > CDI × 0.75: FII tem prêmio de risco adequado
- Com SELIC 14.25%: DY mínimo adequado ≈ 8–9%/ano (10.7% bruto CDI × 0.75)
- FIIs têm isenção de IR sobre rendimentos para PF (cotas < 10% do fundo, > 50 cotistas)
```

**Cuidado com DY alto:**
- Pode ser fruto de venda de ativos (não recorrente)
- Pode ser distribuição de reservas (não sustentável)
- Verificar se é rendimento operacional ou amortização

---

## Tributação FIIs (PF)

| Evento | Imposto | Alíquota |
|--------|---------|----------|
| Rendimentos mensais | Isento de IR | 0% (se atender critérios) |
| Ganho de capital na venda | Imposto de Renda | 20% sobre o lucro |
| Ganho de capital < R$20k/mês | Não isento (diferente de ações) | 20% |

**Critérios de isenção dos rendimentos:**
1. Fundo com mais de 50 cotistas
2. PF não pode ter mais de 10% das cotas

---

## Como analisar um FII (passo a passo)

### 1. Tipo e segmento
- Qual tipo (tijolo/papel/FOF)?
- Qual segmento (logística/escritório/shopping)?
- É o momento certo para o segmento? (ex: escritório sofreu pós-pandemia)

### 2. Qualidade do portfólio
- **Localização**: imóveis em A+ (São Paulo, Rio, centros logísticos prime)
- **Inquilinos**: rating de crédito dos locatários (para papel: emissores dos CRIs)
- **Contratos**: típicos (3 anos, revisão IGPM) vs atípicos (10+ anos, revisão IPCA)
- **Vacância**: tendência — subindo ou caindo?

### 3. Gestão
- Track record da gestora (histórico de distribuição estável)
- Transparência nos relatórios mensais (IR disponível?)
- Taxa de administração + performance (< 1.5% do PL é razoável)

### 4. Valuation
```
Cap Rate implícito = Renda anualizada / Valor de mercado do portfólio
→ Compare com cap rate de mercado para o segmento/localização

P/VP < 0.90 com portfólio de qualidade = desconto injustificado → oportunidade
P/VP > 1.20 = premium — verifique se crescimento de DY justifica
```

### 5. Cenário macro e sensibilidade a juros
- **Tijolo**: correlação negativa forte com SELIC
  - Queda de 100bps na SELIC → P/VP sobe ~10–15%
  - **2026**: expectativa de cortes = catalisador para tijolo
- **Papel CDI**: beneficia de SELIC alta, prejudica com queda
- **Papel IPCA**: beneficia com inflação alta

---

## Dados para pesquisar FIIs

```bash
# Via AI Router — dados rápidos
ssh claw@100.112.103.77 "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Dados de FII TICKER11: P/VP, DY atual, vacância, tipo, segmento, último rendimento declarado, relatório mensal mais recente. 2026.\"}],\"tier\":\"research\"}'"

# Via brapi.dev
curl -s "https://brapi.dev/api/quote/TICKER11?token=anonymous&fundamental=true"
```

**Fontes complementares:**
- Investidor10.com.br — melhor site para FIIs
- FundsExplorer.com.br — screener de FIIs
- Relatórios mensais na CVM (ir.com.br de cada FII)
- Canal ClubeFII, Tiago Reis (YouTube) para contexto qualitativo
