# Macro Brasil — Referência

## Estado atual (atualizar via AI Router)

```bash
# Buscar dados macro atualizados
ssh claw@100.112.103.77 "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"SELIC atual e expectativa Focus, IPCA 12m, câmbio USD/BRL, spread CDS Brasil, Ibovespa nível atual. Macro Brasil março 2026.\"}],\"tier\":\"research\"}'"
```

**Referências fixas (base 2026):**
- SELIC: ~14.25% (pico do ciclo, expectativa Focus: 12–13% fim 2026)
- IPCA 12m: ~3.81% (meta 3.0%, banda 1.5–4.5%)
- USD/BRL: ~5.70–5.90 (volatil com fluxo)
- Ibovespa: ~130.000–140.000 pontos

---

## Framework de análise macro

### Ciclo de juros SELIC

**Alta de juros (aperto monetário):**
- Prejudica: Growth, varejo endividado, construtoras, FIIs tijolo
- Beneficia: Bancos (spread maior), FIIs papel/CRI, Tesouro Direto, exportadoras
- Câmbio tende a apreciar (BRL se fortalece com juros altos)

**Queda de juros (afrouxamento monetário):**
- Beneficia: Growth, varejo, construtoras, FIIs tijolo, utilities, ações de longa duration
- Prejudica: Bancos (spread cai), FIIs papel (DY cai com índices)
- Câmbio tende a depreciar (BRL enfraquece)
- **2026**: mercado precifica cortes de 300bps → oportunidade em FIIs tijolo

### IPCA e inflação

**Inflação alta (acima de 4.5%):**
- Pressão para BCB subir juros → negativo para bolsa
- Beneficia: FIIs papel indexados ao IPCA/IGP-M, commodities, exportadoras
- Prejudica: consumo doméstico, varejo, empresas com custos em BRL e receita em BRL

**Inflação controlada (2–4%):**
- Espaço para COPOM cortar juros → positivo para renda variável
- Beneficia: ciclo de crescimento sustentável

### Câmbio USD/BRL

**BRL fraco (USD alto):**
- Beneficia: VALE3, PETR4, exportadoras (receita em USD)
- Prejudica: importadores, varejo com produto importado, empresas com dívida em USD
- Impacto Ibovespa: correlação negativa (Ibovespa sobe quando BRL aprecia)

**BRL forte:**
- Beneficia: importadores, companhias aéreas (combustível em USD)
- Prejudica: exportadoras

### Risco-país e CDS

- CDS Brasil (Credit Default Swap): custo de seguro da dívida soberana
- CDS < 150bps: risco baixo, fluxo estrangeiro positivo
- CDS > 250bps: risco alto, estrangeiro vendendo Brasil
- Monitor: embi+ Brasil, CDS 5 anos

### Cenário fiscal

- Meta fiscal: déficit primário de 0% do PIB (meta 2026)
- Resultado primário acima da meta → positivo para BRL e bolsa
- Risco fiscal → venda de BRL, alta de juros longos

---

## Correlações importantes

| Ativo | Correlação com | Direção |
|-------|---------------|---------|
| VALE3 | Minério de ferro (iron ore) | + (sobe junto) |
| VALE3 | USD/BRL | + (BRL fraco = VALE mais cara em BRL) |
| PETR4 | Petróleo Brent | + |
| PETR4 | Câmbio | + (exportadora) |
| FIIs tijolo | SELIC | - (juros sobem = FIIs caem) |
| FIIs papel IPCA | IPCA | + |
| Bancos (ITUB, BBDC) | Spread bancário | + |
| Ibovespa | Risco EM global | - (risk-off global = vende Brasil) |
| Small caps | PIB Brasil | + |

---

## Calendário macro relevante

### Brasil
- **COPOM** (SELIC): reuniões a cada 45 dias
  - 2026: jan, mar, mai, jun, ago, set, nov, dez
- **IPCA**: divulgado todo mês ~dia 9
- **IPCA-15** (prévia): divulgado ~dia 23
- **PIB**: trimestral, 60 dias após o trimestre
- **Resultado Primário**: mensal (~dia 28)
- **Focus** (expectativas de mercado): toda segunda-feira

### EUA (impacto no Brasil)
- **FOMC** (Fed Funds Rate): 8 reuniões/ano (~6 semanas)
- **Payroll** (emprego EUA): 1ª sexta do mês — move câmbio
- **CPI** (inflação EUA): mensal — move expectativas do Fed
- **GDP**: trimestral

---

## Interpretando o Boletim Focus

O Focus do BCB traz medianas de expectativas do mercado:

| Indicador | O que monitorar |
|-----------|----------------|
| SELIC fim de ano | Se revisando para baixo → positivo para bolsa |
| IPCA | Se subindo → BCB pode subir juros |
| USD/BRL | Expectativa de câmbio do mercado |
| PIB | Expectativa de crescimento econômico |

**Leitura bullish**: SELIC caindo + IPCA no centro da meta + PIB acelerando + fiscal equilibrado

**Leitura bearish**: SELIC subindo + IPCA acima do teto + fiscal deteriorando + CDS explodindo

---

## Fluxo de capital estrangeiro

```bash
# Fluxo estrangeiro na B3 (semanal)
ssh claw@100.112.103.77 "curl -s -X POST http://127.0.0.1:3080/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{\"messages\":[{\"role\":\"user\",\"content\":\"Fluxo de capital estrangeiro na B3 nas últimas 4 semanas. Ibovespa performance vs. MSCI EM. Risk-on ou risk-off global?\"}],\"tier\":\"social\"}'"
```

Estrangeiro representa ~50% do volume da B3 → monitoramento essencial para timing.
