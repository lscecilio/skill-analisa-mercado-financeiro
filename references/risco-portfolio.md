# Risco e Portfólio — Referência

## Métricas de performance (calcular sempre)

### Sharpe Ratio
```
Sharpe = (Retorno Portfólio - Taxa Livre de Risco) / Desvio Padrão

Taxa livre de risco Brasil = SELIC/252 ao dia

Interpretação:
< 0.5  — ruim
0.5–1.0 — aceitável
1.0–2.0 — bom
> 2.0  — muito bom (Atlas Trader v3: Sharpe 3.729)
> 3.0  — excelente — difícil de manter
```

### Sortino Ratio (melhor que Sharpe para não-normais)
```
Sortino = (Retorno - Risk-Free) / Downside Deviation
→ Penaliza só a volatilidade negativa
→ Preferível ao Sharpe em estratégias com assimetria
```

### Maximum Drawdown (MaxDD)
```
MaxDD = (Pico - Vale) / Pico × 100

Referências:
< 5%:  excelente (Atlas Trader v3: -9.38%)
5–15%: bom para estratégia ativa
15–25%: tolerável para portfólio diversificado
> 30%: inaceitável — revisar estratégia

Calmar Ratio = Retorno Anual / |MaxDD|
→ > 1.0: estratégia gerencia bem o risco
```

### Win Rate e Payoff
```
Win Rate: % de trades vencedores
Payoff:   Média de ganho / Média de perda

Sistema lucrativo pode ter WR baixo se payoff alto:
WR 40% com Payoff 3:1 → esperança positiva
WR 70% com Payoff 1:1 → também lucrativo

Atlas Trader v3: WR 75.5%, Payoff ~2:1 → excelente
```

---

## Sizing (tamanho da posição)

### Kelly Criterion (teórico)
```python
kelly = win_rate - (1 - win_rate) / payoff_ratio
# Usar metade do Kelly para maior segurança (Half-Kelly)
```

### Volatility Targeting (Barroso & Santa-Clara 2015)
```python
# Método mais robusto usado no Atlas Trader
vol_realizada = std(returns_21d) * sqrt(252)
size = (vol_target / vol_realizada) * size_base

vol_target recomendado: 15–25% para estratégia B3
cap: 0.3x a 3.0x do size_base (evita posições extremas)

# Parâmetros otimizados Atlas Trader v3:
vol_target_annual = 0.375 (37.5%)
risk_pct = 3.77% por trade
```

### Regras práticas de sizing
```
Posição máxima por ativo:     5–10% do portfólio
Posição máxima por setor:     20–25% do portfólio
Alocação em ações:            conforme perfil de risco
Reserva de emergência:        nunca investir fundo de emergência

Critério do 1%: nunca arriscar mais de 1% do portfólio em 1 trade
→ Stop em 1% = position size = 1% / (entrada - stop) × capital
```

---

## Correlação e Diversificação

### Correlação entre ativos (B3)
```
Alta correlação (> 0.7): setor bancário entre si, commodities entre si
Baixa correlação: ações + FIIs tijolo, ações + renda fixa
Correlação negativa: USD/BRL vs. exportadoras vs. importadoras

Portfólio bem diversificado: correlação média < 0.4 entre ativos
```

### Matriz de correlação (tickers LSC-Lab)
```
PETR4 × VALE3:    ~0.55 (ambas commodities, mas produtos diferentes)
PETR4 × USD/BRL:  ~0.80 (exportadora)
VALE3 × China PMI: ~0.75 (ferro para China)
ITUB4 × SELIC:    ~0.60 (spread bancário)
FII tijolo × SELIC: ~-0.65 (inversamente correlacionados)
Ibovespa × S&P500: ~0.50 (correlação moderada)
```

### Diversificação ótima (teoria de Markowitz)
- Portfólio eficiente: máximo Sharpe para dado nível de risco
- Com 15–20 ativos pouco correlacionados: risco idiossincrático eliminado
- Risco sistemático (mercado): permanece — só hedge elimina

---

## VaR (Value at Risk)

```
VaR 95% (histórico): quanto o portfólio pode perder em 1 dia com 95% de confiança

VaR = percentil 5% dos retornos históricos × valor da carteira

Exemplo: portfólio R$100k, VaR 95% = 2%
→ 95% dos dias a perda não excede R$2.000
→ 5% dos dias (1 em 20) pode perder mais de R$2.000

CVaR (Conditional VaR): média das perdas além do VaR
→ Mais robusto que VaR para caudas gordas
```

---

## Estratégia LSC-Lab — Atlas Trader v4

> Estratégia proprietária para B3. Serviço em staging (:3097) + ML service (:3096).
> Produção atual: v3 em :3095 (Node.js). V4 ainda em paper trading (30 dias requeridos).

### Arquitetura V4 (8 fases completas — 2026-03-24)

```
atlas-trader-v4 (:3097, Node.js)
├── Regime Detector      → classifica TRENDING / RANGING / STRESS / CRISE
├── Signal Composer      → técnico + sentimento + ML composite
├── Sentiment Pipeline   → Perplexity Sonar + OpenAI fallback
├── XGBoost ML Filter    → modelo treinado semanal (segunda 02h BRT)
├── Pairs Trading        → cointegração + z-score (mean-reversion)
├── Position Sizer       → Kelly fracionário ajustado por regime
├── Pre-trade Risk Check → validação antes de qualquer entrada
└── Optuna Optimizer     → otimização semanal (segunda 22h BRT)

atlas-trader-ml (:3096, Python/FastAPI)
├── backtester.py        → backtesting vetorizado com ML inline
├── optimizer.py         → Optuna (Sortino/Sharpe/Return)
├── xgboost_model.py     → modelo treinado com 12 features
└── pairs_analysis.py    → Engle-Granger, z-score
```

### Regime atual (live — atualiza a cada 15min)

```bash
# Ver regime em tempo real
curl -s http://atlas-capital.lsc-lab.com/api/trader/regime 2>/dev/null | python3 -m json.tool
# Ou direto no Claw:
ssh claw@100.112.103.77 "curl -s http://127.0.0.1:3097/regime"
```

**Regime em 2026-03-26:** RANGING | SELIC 14.75% | VIX 28.01 | ADX 22.08
- RANGING = modo seletivo: só alta convicção entra

### MELHOR RESULTADO ATUAL — LONG-only ML Optuna (2026-03-25) ⭐

**14.87%/mês | Sharpe 4.14 | WR 85.9% | MaxDD -7.36%**

```python
# Parâmetros do backtester — LONG-only ML Optuna 2026-03-25
BEST_PARAMS_V4_LONGONLY = {
    "atr_stop_mult":     5.20,    # stop mais largo (melhor survival)
    "atr_target_mult":   4.51,    # alvo ajustado LONG-only
    "risk_pct":          0.033,   # 3.3% por trade (vol_target alto auto-ajusta)
    "score_threshold":   2,       # limiar mais baixo (ML já filtra)
    "rsi_oversold":      36.4,
    "rsi_overbought":    84.1,    # raro tocar — favorece LONG
    "cs_momentum_min":   0.33,    # top 67% por residual momentum
    "cs_momentum_max":   0.0,     # SHORT desabilitado (LONG-only mode)
    "vol_target_annual": 0.583,   # 58.3% — agressivo mas gerenciado por Kelly
    "max_hold_days":     30,
    "ml_threshold":      0.5018,  # XGBoost: prob ≥ 50.18% para entrar
}
```

### Checkpoint histórico V3 — Dual-Alpha+COPOM (2026-03-24)
**11.07%/mês | Sharpe 3.729 | WR 75.5% | MaxDD -9.38%**

```python
# Parâmetros do signal-composer.js (atualmente em uso no live)
SIGNAL_COMPOSER_V3 = {
    "rsi_oversold":      27.6,
    "rsi_overbought":    76.6,
    "atr_stop_mult":     3.42,
    "atr_target_mult":   6.24,
    "confidence_threshold": 0.75,
    "adx_trend_min":     25.0,
    "ml_weight":         0.25,
    "sentiment_weight":  0.25,
}
```

### Outros checkpoints (histórico)
| Versão | Mensal | Sharpe | MaxDD | WR | Destaque |
|--------|--------|--------|-------|----|---------|
| **LONG-only ML** | **14.87%** | **4.14** | **-7.36%** | **85.9%** | **Atual (backtester)** |
| ML XGBoost 120t | 12.13% | 3.60 | — | 78.8% | Optuna 120 trials |
| Dual-Alpha v3 | 11.07% | 3.729 | -9.38% | 75.5% | COPOM+ResidMomentum |
| Optuna 80t | 5.09% | 3.21 | -4.4% | 73.6% | 12 tickers |

### Sistema de score (condições de entrada)
```
Score threshold = 2 pontos mínimos (ML já filtra ~50% dos trades):

Alpha 1 — Institucional:
+2 pontos: Liquidity Sweep LONG
+2 pontos: FVG (Fair Value Gap) LONG
+1 ponto:  OFI Proxy positivo
+1 ponto:  Delta Divergence LONG (acumulação)
+1 ponto:  ITI > 0.65 (tubarão presente)
+1 ponto:  RSI < 36.4 E %B < 25%
+1 ponto:  Trend up (preço > MMA)

Alpha 2 — Contra-Retail:
+1 ponto:  Stop Cluster Score > 3 (varejo em trap)
+1 ponto:  Round Number Sweep iminente
+1 ponto:  Disposition Effect detectado

BÔNUS: Alpha1 + Alpha2 alinhados = confidence × 1.2

ML Filter: XGBoost prob ≥ 0.5018 (bloqueia entrada se modelo diz não)
Regime Filter: STRESS = só bullComposite > 0.68 | CRISE = neutro forçado
```

### Universo de ativos B3 (20 tickers — otimizados)
```python
B3_20_TICKERS = [
    'PETR4.SA', 'VALE3.SA', 'ITUB4.SA', 'BBDC4.SA', 'BBAS3.SA',
    'WEGE3.SA', 'RENT3.SA', 'PRIO3.SA', 'ABEV3.SA', 'GGBR4.SA',
    'GOLD11.SA', 'IVVB11.SA',  # hedge: ouro e S&P
    'CSAN3.SA', 'CMIG4.SA', 'TAEE11.SA', 'HAPV3.SA',
    'UGPA3.SA', 'SBSP3.SA', 'KLBN11.SA',
    # ENBR3.SA excluído (delisted)
]
```

### Partial exits (implementado — WR 33% → 75%)
```
33% sai no Alvo 1 (1R)
33% sai no Alvo 2 (2R)
33% trailing stop (maior contribuição ao resultado)
```

### Drawdown monitor automático
| Threshold | Ação automática |
|-----------|----------------|
| 8% | Alerta Telegram + sizing −50% |
| 15% | Suspende novos trades |
| 22% | Liquida todas as posições |

### Status de produção
- **:3095** → V3 produção (Node.js — atlas-trader.js) — ATIVO
- **:3097** → V4 staging (app.js) — 30 dias paper trading requeridos
- **:3096** → ML service (Python) — Backtester + Optuna + XGBoost

### Critérios para promover V4 para produção
- Sharpe > 1.0 por 30 dias consecutivos em paper trading
- MaxDD < 15% no período
- WR > 45% | Profit Factor > 1.3
- Zero crashes em 7 dias | Latência scan < 120s

---

## Gestão emocional e disciplina

Regras que protegem o capital:
1. **Nunca mover o stop** para baixo (aceitar o loss pré-definido)
2. **Daily loss limit**: parar de operar se perder 3× o risco por trade no dia
3. **Drawdown limit**: se carteira cair > 15% do pico, reduzir sizing 50%
4. **Review mensal**: checar se métricas (WR, Sharpe, MaxDD) continuam dentro do esperado
5. **Não operar** na hora do COPOM, Payroll, IPCA sem setup de alta convicção
