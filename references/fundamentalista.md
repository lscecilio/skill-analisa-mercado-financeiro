# Análise Fundamentalista — Referência B3

## Métricas de Valuation

### P/L (Preço/Lucro)
| Referência | Interpretação |
|-----------|---------------|
| < 8 | Muito barato — verificar se não é armadilha (value trap) |
| 8–15 | Razoável para empresas maduras |
| 15–25 | Justo para crescimento moderado |
| > 25 | Caro — justificado só com crescimento alto (>15%/ano) |
| Negativo | Empresa dando prejuízo — usar EV/EBITDA |

**Dica Brasil**: com SELIC alta (>12%), P/L de 8–10 é "neutro" porque o custo de oportunidade (Tesouro) está alto.

### EV/EBITDA
| Referência | Interpretação |
|-----------|---------------|
| < 5 | Barato (commodities, bancários) |
| 5–10 | Faixa normal para empresas maduras |
| 10–15 | Crescimento moderado / setor premium |
| > 15 | Crescimento alto esperado — verificar se justificado |

### P/VP (Preço/Valor Patrimonial)
- < 1.0: negociando abaixo do patrimônio → possível oportunidade (ou empresa deteriorando)
- 1.0–2.0: faixa normal para maioria das empresas
- > 3.0: premium — só justificado com ROE muito alto e crescimento forte

### Dividend Yield (DY)
- DY > SELIC × 0.70 → remuneração atrativa
- DY > 8% com SELIC em 14%: competitivo com renda fixa
- **Cuidado**: DY alto pode ser sinal de queda de preço, não generosidade

---

## Métricas de Rentabilidade

### ROE (Return on Equity)
| Setor | ROE mínimo bom |
|-------|---------------|
| Bancos | > 15% |
| Varejo | > 20% |
| Utilities | > 10% |
| Indústria | > 12% |
| Tech | > 18% |

### ROIC (Return on Invested Capital)
- ROIC > WACC: empresa cria valor
- ROIC < WACC: empresa destrói valor (evitar)
- ROIC consistente > 15% por 5+ anos: vantagem competitiva real

### Margens
```
Margem Bruta    = (Receita - CPV) / Receita        — eficiência operacional
Margem EBITDA   = EBITDA / Receita                 — geração de caixa operacional
Margem Líquida  = Lucro Líquido / Receita          — rentabilidade final

Bom por setor:
Bancos:  Margem líquida > 20%
Varejo:  Margem EBITDA > 8%
Tech:    Margem bruta > 60%
Utilities: Margem EBITDA > 30%
```

---

## Métricas de Endividamento

### Dívida Líquida / EBITDA
| Nível | Interpretação |
|-------|---------------|
| < 1.0 | Empresa quase sem dívida — sólida |
| 1.0–2.0 | Dívida confortável |
| 2.0–3.0 | Atenção — monitorar |
| > 3.0 | Risco — verificar cobertura de juros |
| > 4.0 | Alto risco — evitar em ciclo de juros altos |

### Cobertura de Juros (EBIT/Despesas Financeiras)
- > 3x: confortável
- 1.5–3x: atenção
- < 1.5x: risco de inadimplência

---

## Piotroski F-Score (0 a 9)

Mede saúde financeira em 9 critérios binários (0 ou 1 ponto cada):

**Lucratividade (4 pontos):**
1. ROA > 0 (lucro líquido positivo)
2. Operating Cash Flow > 0
3. ROA crescendo YoY
4. Cash Flow > ROA (qualidade dos lucros)

**Alavancagem/Liquidez (3 pontos):**
5. Dívida/Ativo decrescendo YoY
6. Liquidez Corrente crescendo YoY
7. Sem emissão de novas ações no ano

**Eficiência Operacional (2 pontos):**
8. Margem Bruta crescendo YoY
9. Giro do Ativo crescendo YoY

**Interpretação:**
- F-Score 0–2: empresa fraca — evitar
- F-Score 3–6: neutro — analisar contexto
- F-Score 7–9: empresa forte — candidata a compra

---

## Benchmarks por setor B3 (2026)

### Bancos (ITUB4, BBDC4, BBAS3, SANB11, BRSR6)
- P/L justo: 7–12 (com SELIC alta, múltiplo cai)
- ROE mínimo: 15%
- Índice de Basileia > 12%
- PDD (Provisão Devedores Duvidosos) crescente = sinal de alerta

### Petróleo & Gás (PETR4, PRIO3, RECV3)
- EV/EBITDA justo: 3–6
- Analisar preço do barril (Brent) e break-even da empresa
- Free Cash Flow Yield > 10% = atrativo
- PETR4: sensível a câmbio e política de paridade de preços

### Mineração (VALE3, CSNA3, GGBR4, USIM5)
- EV/EBITDA justo: 4–7
- Correlação forte com preço do minério (iron ore) e China PMI
- VALE3: sensível ao câmbio BRL/USD (exportadora)

### Utilidades (SBSP3, CMIG4, ENBR3, TAEE11)
- P/L justo: 8–15
- Dividend Yield > 6%
- Regulatório: reajustes de tarifa pelo ANEEL/ANA
- Beneficiado por queda de juros (ativo de duration longa)

### Varejo (MGLU3, LREN3, HAPV3)
- EV/EBITDA justo: 6–12
- SSS (Same Store Sales) crescimento > inflação = bom
- Endividamento crítico em ciclo de juros altos
- Sensível ao ciclo de crédito e desemprego

### Tecnologia / Growth
- P/L e EV/EBITDA altos são aceitáveis se crescimento receita > 20%/ano
- Foco em: receita recorrente (ARR), churn, LTV/CAC, unit economics

---

## DCF Simplificado (3 cenários)

```
WACC estimado Brasil 2026: 12–16% (com SELIC em 14.25%)

Cenário Bear:  crescimento = IPCA (3.8%), múltiplo saída = 8x
Cenário Base:  crescimento = 8%, múltiplo saída = 10x
Cenário Bull:  crescimento = 15%, múltiplo saída = 12x

Preço justo = média ponderada dos 3 cenários (bear 25% / base 50% / bull 25%)
Margem de segurança mínima: 30% de desconto para comprar
```

---

## Onde obter dados fundamentalistas B3

1. **brapi.dev** — API gratuita, dados básicos em JSON
2. **Fundamentus.com.br** — screener fundamentalista completo (scraping)
3. **Investidor10.com.br** — dados de FIIs e ações com histórico
4. **RI da empresa** — release de resultados, DRE, balanço (fonte primária)
5. **CVM/EDGAR** — ITR, DFP (demonstrações financeiras padronizadas)
6. **AI Router (research)** — resumo rápido de múltiplos e earnings

```bash
# Screener fundamentalista rápido via AI Router
ssh claw@100.112.103.77 "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Indicadores fundamentalistas de TICKER3: P/L, EV/EBITDA, P/VP, ROE, margem líquida, dívida/EBITDA, DY. Dados B3 2026.\"}],\"tier\":\"research\"}'"
```
