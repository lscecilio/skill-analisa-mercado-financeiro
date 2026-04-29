# Análise Técnica — Referência Completa

## Indicadores principais e interpretação

### RSI (Índice de Força Relativa) — período 14
| Zona | Valor | Interpretação |
|------|-------|---------------|
| Sobrecomprado | > 70 | Possível reversão / stop loss em tendência |
| Neutro | 40–60 | Sem sinal claro |
| Sobrevendido | < 30 | Possível reversão / entrada em tendência de alta |
| Divergência bullish | Preço faz LL, RSI faz HL | Fraqueza do movimento de baixa — reversão |
| Divergência bearish | Preço faz HH, RSI faz LH | Fraqueza do movimento de alta — reversão |

**Parâmetros otimizados (evidência Atlas Trader B3):**
- rsi_oversold: 27-39 (zona de compra mais eficiente no Ibovespa)
- rsi_overbought: 73-77 (zona de venda mais eficiente)
- **Regra**: confirmar com VWAP e volume antes de entrar

### MACD (12, 26, 9)
- **Cruzamento bullish**: linha MACD cruza acima da signal → compra quando em tendência de alta
- **Cruzamento bearish**: linha MACD cruza abaixo da signal → venda
- **Histograma crescendo**: momentum aumentando na direção da tendência
- **Divergência**: MACD aponta direção oposta ao preço → reversão iminente

### Médias Móveis — framework multi-timeframe
```
MMA20:  tendência de curto prazo (swing trade)
MMA50:  tendência de médio prazo (referência diária)
MMA200: tendência de longo prazo (bull/bear market)

Setup clássico bullish: preço > MMA20 > MMA50 > MMA200
Golden Cross: MMA50 cruza acima da MMA200 → tendência de alta de longo prazo
Death Cross: MMA50 cruza abaixo da MMA200 → tendência de baixa
```

### Bandas de Bollinger (20, 2σ)
- Preço toca banda inferior + volume acima da média → possível reversão / suporte
- Preço toca banda superior + volume acima da média → possível resistência / reversão
- Squeeze (bandas se fechando) → breakout iminente — direção indicada pelo MACD
- **Walk the band**: em tendências fortes, preço "caminha" pela banda — não vender só por isso

### Volume — confirmação de movimentos
- Alta de preço com volume acrescente → tendência sustentável
- Alta de preço com volume decrescente → fraqueza — possível reversão
- Queda de preço com volume alto → capitulação (fundo possível)
- Queda de preço com volume baixo → correção saudável em tendência de alta

### VWAP (Volume Weighted Average Price)
- Preço acima VWAP: pressão compradora institucional → LONG
- Preço abaixo VWAP: pressão vendedora → SHORT ou aguardar
- VWAP como suporte/resistência dinâmico intraday

### ATR (Average True Range) — gestão de risco
```
Stop loss = entrada - (ATR × 2.1 a 3.4)   # parâmetros B3 otimizados
Alvo      = entrada + (ATR × 5.1 a 6.2)   # R:R mínimo de 2:1
```

---

## Multi-Timeframe Analysis

Sempre analise em pelo menos 2 timeframes:

| Timeframe superior | Timeframe operacional | Timeframe de entrada |
|--------------------|-----------------------|---------------------|
| Semanal (tendência) | Diário (setup) | 1h ou 4h (timing) |
| Mensal (macro) | Semanal (tendência) | Diário (execução) |

**Regra**: só operar na direção do timeframe superior.

---

## Padrões de candle mais relevantes

### Reversão de baixa para alta
- **Martelo (Hammer)**: corpo pequeno no topo, sombra longa inferior (>2× corpo) — fundo
- **Engolfo de alta (Bullish Engulfing)**: vela verde engole vela vermelha — reversão
- **Morning Star**: 3 velas (vermelha grande → doji/indecisão → verde grande) — fundo
- **Pin Bar bullish**: sombra inferior muito longa, fecha próximo da máxima

### Reversão de alta para baixa
- **Shooting Star / Martelo Invertido**: sombra superior longa no topo — topo
- **Engolfo de baixa (Bearish Engulfing)**: vela vermelha engole vela verde
- **Evening Star**: inverso do Morning Star
- **Doji no topo**: indecisão após tendência de alta → atenção

### Continuação
- **Three White Soldiers / Three Black Crows**: 3 velas consecutivas na mesma direção = força
- **Inside Bar**: vela menor dentro da maior → compressão antes de breakout

---

## Suporte e Resistência

**Como identificar:**
1. **Swing Highs/Lows**: máximas e mínimas anteriores relevantes
2. **Números redondos**: R$10, R$20, R$50, R$100 — clusters de stops
3. **Gap**: região de preço não negociada — age como suporte/resistência
4. **Fibonacci**: 38.2%, 50%, 61.8% de retrações — alvos e entradas
5. **Volume Profile (POC)**: região de maior volume negociado = suporte/resistência forte

**Quanto mais vezes o nível foi testado sem romper → mais forte ele é**

---

## Setup completo de trade

```
Contexto: [tendência principal — bull/bear/lateral]
Padrão:   [candle pattern ou indicador que gerou o sinal]
Entrada:  R$[preço]
Stop:     R$[preço] (ATR × multiplicador)
Alvo 1:   R$[preço] (1R)
Alvo 2:   R$[preço] (2-3R — parcial exit)
Alvo 3:   R$[preço] (5R+ — trailing stop)
R:R:      [X:1]
Sizing:   [% do portfólio — veja references/risco-portfolio.md]
Validade: [até quando o setup é válido]
```

**Partial exits (aumenta Win Rate: 33% → 73% — evidência Atlas Trader):**
- Exit 33% em Alvo 1 (1R)
- Exit 33% em Alvo 2 (2R)
- Trailing stop no restante (deixar correr)

---

## Indicadores avançados (Smart Money Concepts)

### OFI Proxy (Order Flow Imbalance — Two Sigma/Citadel)
```python
ofi = (close - low) / (high - low) * volume  # se high > low
# Alto OFI positivo = institucional comprando = LONG
# OFI negativo = institucional vendendo = SHORT
```

### ITI (Informed Trade Index — Kyle 1985)
```python
iti = abs(close - open) / (high - low)  # corpo / range
# ITI > 0.65: informed trader no controle
# ITI < 0.30: ruído / retail
```

### Delta Divergence
```
Preço faz novo máximo (HH) mas OFI faz novo mínimo (LL):
→ Institucional distribuindo → SHORT iminente

Preço faz novo mínimo (LL) mas OFI faz novo máximo (HH):
→ Institucional acumulando → LONG iminente
```
