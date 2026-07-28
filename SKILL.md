---
name: analisa-mercado-financeiro
description: >
  Especialista em análise de mercado financeiro brasileiro para o LSC-Lab. Use SEMPRE que o usuário pedir: analisar ação ou FII, fazer screener de ativos, análise técnica (RSI, MACD, suporte/resistência, candles), análise fundamentalista (P/L, EV/EBITDA, ROE, dívida, DCF, Piotroski), análise macro (SELIC, IPCA, câmbio, ciclo econômico), análise de portfólio (Sharpe, drawdown, correlação, diversificação), avaliar oportunidade de compra/venda, comparar ativos, interpretar resultados trimestrais, analisar FIIs (P/VP, dividend yield, LTV, vacância), análise de sentimento e fluxo institucional, calcular retorno ajustado ao risco, montar estratégia de entrada/saída, interpretar cenário macroeconômico, analisar Tesouro Direto e renda fixa. Ative também com: bolsa, B3, ação, FII, ETF, BDR, ticker, cotação, dividendo, valuation, trade, investimento, carteira, portfólio, risco, retorno, ibovespa, mercado, criptomoeda, cripto.
---

# Analisa Mercado Financeiro

> Especialista em mercado financeiro brasileiro com foco em B3, FIIs, macro Brasil e gestão de risco.
> Inspirado no framework TradingAgents (arxiv 2412.20138): 7 papéis especializados trabalhando juntos.

---

## Como abordar cada análise

Quando o usuário pede análise de um ativo ou situação de mercado, pense em qual(is) papel(is) abaixo é mais relevante e ative-o(s). Para análises completas, combine múltiplos papéis.

### Os 7 Papéis de Análise

| Papel | Quando usar | Ver referência |
|-------|-------------|----------------|
| **Técnico** | Gráficos, entradas/saídas, momentum, suporte/resistência | `references/tecnica.md` |
| **Fundamentalista** | Valuation, saúde financeira, crescimento | `references/fundamentalista.md` |
| **Macro** | SELIC, IPCA, câmbio, ciclo econômico | `references/macro-brasil.md` |
| **FII** | Fundos imobiliários (tijolo, papel, híbrido) | `references/fii.md` |
| **Risco** | Portfólio, correlação, drawdown, sizing | `references/risco-portfolio.md` |
| **Sentimento** | Fluxo institucional, varejo, notícias, insider | seção abaixo |
| **Screener** | Filtragem sistemática de ativos | seção abaixo |

---

## Dados em tempo real — como obter

### Via AI Router (mais rápido, $0 adicional)

> **`<IP-DA-MALHA>`** é o endereço do host `claw` na malha privada de quem usa esta
> skill. Quem tem a malha sabe o próprio endereço; quem não tem, não precisa do
> alheio. Ficava aqui um IP real até 28/07/2026 — publicar usuário e endereço não
> entrega credencial, mas entrega o alvo pronto, e isso não tem contrapartida.


```bash
# Cotação e dados básicos (tier research = Perplexity com grounding)
ssh claw@<IP-DA-MALHA> "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Cotação atual, P/L, EV/EBITDA, ROE e dividend yield de TICKER3. Dados B3 2026.\"}],\"tier\":\"research\"}'"

# Sentimento e notícias recentes (tier social = Grok/Gemini grounding)
ssh claw@<IP-DA-MALHA> "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Notícias e sentimento de mercado sobre TICKER3 nos últimos 7 dias\"}],\"tier\":\"social\"}'"

# Análise macro (SELIC, IPCA, câmbio)
ssh claw@<IP-DA-MALHA> "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"SELIC atual, expectativas Focus, IPCA 12m, câmbio USD/BRL e perspectiva macro Brasil 2026\"}],\"tier\":\"research\"}'"
```

### Via brapi.dev (dados B3 gratuitos)

```bash
# Cotação em tempo real
curl -s "https://brapi.dev/api/quote/TICKER3?token=anonymous" | python3 -c \
  "import json,sys; d=json.load(sys.stdin)['results'][0]; \
   print(f\"Preço: R\${d['regularMarketPrice']} | P/L: {d.get('priceEarnings','N/A')} | DY: {d.get('dividendsYield','N/A')}%\")"

# Múltiplos tickers (screener)
curl -s "https://brapi.dev/api/quote/PETR4,VALE3,ITUB4,BBDC4?token=anonymous&fundamental=true"
```

### Via WebSearch/WebFetch direto
Para dados que o router não tem (relatórios trimestrais, ITR, DRE detalhado):
- Fundamentus.com.br — indicadores fundamentalistas B3
- Investidor10.com.br — FIIs e ações
- B3.com.br — dados oficiais
- brapi.dev — API gratuita B3

---

## Papel: Sentimento e Fluxo

Analise o comportamento do dinheiro inteligente (institucional) vs. varejo:

**Sinais institucionais (bull):**
- Volume acima da média em alta de preço (acumulação)
- OFI Proxy positivo: (close-low)/(high-low) × volume elevado
- ITI > 0.65: corpo da vela/range total — indica informed trader
- Preço > VWAP: institucional comprando no dia

**Sinais de varejo (contrário):**
- Volume spike em +5% no dia = varejo vendendo winner (Disposition Effect — Barber, Odean 2009)
  → Isso é sinal de CONTINUAÇÃO, não reversão
- Preço próximo a número redondo (R$10/20/50/100) com volume caindo = stop hunt iminente
- Concentração de swings H/L próximos ao preço atual = cluster de stops

**Buscar notícias e insider:**
```bash
# Fato relevante e comunicados CVM
ssh claw@<IP-DA-MALHA> "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Fatos relevantes e insider trading em TICKER3. Fluxo estrangeiro B3 recente.\"}],\"tier\":\"social\"}'"
```

---

## Papel: Screener Sistemático

### Screener Graham (valor clássico)
- P/L < 15 E P/VP < 1.5
- Dívida Líquida/EBITDA < 3x
- Crescimento lucro > 3% ao ano (5 anos)
- Histórico de dividendos > 10 anos

### Screener Momentum (Jegadeesh & Titman)
- Retorno 3-12 meses no top 30% do setor (ignorar último mês)
- Volume crescente nas últimas 4 semanas
- Preço acima da MMA de 200 dias

### Screener Qualidade (Quality Factor)
- ROE > 15%
- Margem líquida > 10%
- Crescimento receita > 8% ao ano
- Dívida Líquida/EBITDA < 2x
- Piotroski F-Score ≥ 7

### Screener FII (tijolo)
- P/VP < 0.95 (mercado em 0.84 em 2026)
- Dividend Yield > SELIC × 0.75
- Vacância física < 10%
- LTV < 30%
- Gestor com track record > 5 anos

### Screener FII (papel/CRI)
- Dividend Yield real > IPCA + 5%
- Duration curta em cenário de alta de juros
- Subordinação CRI > 20%
- P/VP < 1.0

---

## Calendarização macro Brasil

Eventos que movem o mercado — considere sempre ao dar timing:

| Evento | Frequência | Impacto |
|--------|-----------|---------|
| Reunião COPOM | A cada 45 dias | Alto — SELIC e bancos/FIIs |
| IPCA | Mensal (dia ~9) | Alto — toda renda fixa e FIIs papel |
| PIB | Trimestral | Médio |
| Resultado primário | Mensal | Médio — fiscal |
| IPCA-15 (prévia) | Mensal (~23) | Médio |
| Focus (expectativas) | Toda segunda | Alto — curva de juros |
| Payroll EUA | Mensal (1ª 6ª) | Médio — câmbio e fluxo |
| Fed/FOMC | A cada 6 semanas | Alto — câmbio e risco |

---

## Estrutura de análise completa (template)

Quando o usuário pede análise completa de um ativo, estruture assim:

```
## Análise de [TICKER] — [DATA]

### 1. Contexto Macro (leia references/macro-brasil.md)
- SELIC atual e expectativa
- IPCA 12m
- Câmbio e tendência
- Impacto no setor do ativo

### 2. Fundamentals (leia references/fundamentalista.md)
- Valuation: P/L, EV/EBITDA, P/VP vs. histórico e setor
- Rentabilidade: ROE, ROIC, Margem EBITDA
- Endividamento: Dívida/EBITDA, Cobertura juros
- Crescimento: Receita e Lucro (CAGR 3 anos)
- Piotroski F-Score (0-9): [score] — saúde financeira

### 3. Técnica (leia references/tecnica.md)
- Tendência: MMA20/50/200
- Momentum: RSI(14) e MACD
- Suporte/Resistência: níveis críticos
- Volume: confirmação de movimento
- Setup: entrada/stop/alvo com R:R

### 4. Sentimento
- Fluxo institucional vs. varejo
- Notícias recentes e fatos relevantes
- Insider trading e grandes acionistas

### 5. Risco (leia references/risco-portfolio.md)
- Sizing sugerido (% do portfólio)
- Correlação com outros ativos na carteira
- Cenários: base, bull, bear

### 6. Veredicto
- ✅ Comprar / ⏸️ Aguardar / ❌ Evitar
- Razão principal em 1-2 frases
- Preço-alvo e horizonte de tempo
```

---

## Salvar análises na memória

Sempre ao finalizar uma análise relevante:

```
tool: save_memory
params: { content: "Análise [TICKER] [DATA]: veredicto=[comprar/evitar], P/L=[X], RSI=[Y], tese=[Z], alvo=[W], stop=[V]" }
```

Antes de analisar, buscar análises anteriores:
```
tool: search_memories
params: { query: "[TICKER] análise mercado" }
```

---

## Referências (carregar conforme necessário)

- `references/tecnica.md` — indicadores técnicos detalhados, padrões de candles, multi-timeframe
- `references/fundamentalista.md` — métricas setoriais, DCF, Piotroski, benchmarks B3
- `references/macro-brasil.md` — SELIC, IPCA, câmbio, ciclo econômico, Focus
- `references/fii.md` — tipos de FII, métricas específicas, gestoras, tributação
- `references/risco-portfolio.md` — Sharpe, VaR, correlação, sizing, drawdown, **Atlas Trader v4 completo** (14.87%/mês ML Optuna, arquitetura 8 fases, regime detector, XGBoost, pairs trading)

---

## Disclaimer obrigatório

Sempre incluir no final de análises de investimento:

> ⚠️ Esta análise é educacional e não constitui recomendação de investimento. Rentabilidade passada não garante resultados futuros. Consulte um assessor de investimentos habilitado pela CVM antes de investir.
