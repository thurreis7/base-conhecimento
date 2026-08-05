---
tipo: contexto
area: CONTEXTO-MESTRE
dominio: financeiro-receitas-carteira
empresa: COMPARTILHADO
atualizado: 2026-08-05
fonte: 00-fonte-completa.md §8-14, §24-26, §31
---

# Financeiro — receitas, carteira, contas a receber e LTV

## A planilha V4 (template Fórmula Gestão Digital / Erico Rocha)
Arquitetura de 4 camadas; plano previa R$ 12,5M/ano, R$ 3,7M de lucro.
**Grafo de dependências:** Base de Dados = mãe (4.430 refs) → Planejamento 2026
(1.679 refs; Custos Vinculados puxa 909 fórmulas) → Receitas Operacionais ↔
Recorrentes (acopladas nas duas direções) → Impostos ← Receitas · Folha → Pessoal
· Dívidas → Financeiras → BS consolida 13 abas.
**Ordem de construção (montante→jusante):** Base → Planejamento → Receitas 3+4 →
Custos Vinculados → Eventos+Viagens → Folha+Pessoal → Serviços+Gerais →
Dívidas+Financeiras → Entregáveis+Invest. → Impostos → BS/dashboard.

## Aba 1 — Base de Dados (a mãe)
Linhas = pares **produto × estratégia** (travessão separa a origem: "Funil|STEND"
× "Prospecção|STEND" — mesmo produto, CACs diferentes). Colunas: ticket médio ·
cash collect médio · **CAC = tráfego pago + disparos de API (WhatsApp/Meta — até
30% do CAC hoje)** · CAC teto (hoje "disposição a pagar" do Judah → vira fórmula:
**CAC teto = f(margem de contribuição × multiplicador LTV)**). LTV validado: CTM
≥1,5× a compra; renovação ~50%+.
Erros/gambiarras: L36–38 são custos (padrinho/analista/professor) na coluna de CAC
(saem para regras de escala); CAC 2.600 na L5 pertence à L4.

## Aba 2 — Planejamento 2026 (quantidade é o único input humano)
Por mês: **Qtd (input) · Venda = qtd×ticket · Receita = qtd×coleta.** Venda ≠
Receita porque a esteira de parcelas dos tickets altos entra na receita sem venda
nova. Bloco de tráfego = qtd×CAC com **gross-up de imposto ÷0,88**. Furo confessado:
projetou tudo no M3 (ousado) sem M1/M2 → **V5 nasce com 3 camadas por linha, eventos
no M1.**

## Aba 3 — Receitas Operacionais (C.Collect)
Linha 34 = **quanto dinheiro entra por mês cumprindo a estratégia** (a mais
importante). Dúvida do Judah: atraso/inadimplência considerados? (parcialmente; a
V5 aplica fatores de realização reais por meio de pagamento).

## Aba 4 — Receitas Recorrentes (o motor de carteira, a "mágica")
ENTRADA (do Planejamento) → SAÍDA → CARTEIRA ATIVA (anterior + entradas − saídas) →
REEMBOLSO/CANCELAMENTO → RECORRÊNCIA. Regras literais e realidade:
- **INI:** projeção fechar dez com 168 clientes/R$ 30k MRR; **real ~30 ativos (25
  pagantes anuais a R$ 97).** Churn sugerido M2 = 5%.
- **Afatécnicos:** projeção R$ 42k/ano MRR; **real 6 clientes × R$ 79.**
- **RICA:** recorrência += (venda − coleta) × 75% recebível ÷ 9 meses.
- **STEND:** saída = 5% da carteira/mês. Erro: cancelamento × ticket (deveria ×
  cash collect).
- **CTM:** saída fator = (carteira+entradas)×3% (churn contínuo). Mudança: **renovado
  FICA na carteira** (não sai no mês 12). Régua: renova até 150k, sobe pro MM acima.
- **MM:** "Saída não renovação 50%" (renovação de 50% já assumida).
- Erros de fórmula a corrigir: **ISCA = SUM dupla contagem (~R$ 6M/ano fantasma)** ·
  FGI T135 (referência escorregou → recorrência negativa) · Saída Ajustada morta.
- Indicadores: **ascensão** e **receita perdida** por produto → painel do setor de
  produto + bonificação das navegadoras.

## Ciclos e gestão financeira
Ver `04-comercial-funis-e-eventos.md` (curvas semanais + ciclos bimestrais).

## Contas a Receber (16.319 títulos, 2020–2027)
**Vocabulário (3 camadas):** venda contratada → coleta na venda (cash collect) →
**entradas do mês** (coleta + esteira de parcelas + recorrência) = o que paga conta.
Meta reversa roda em ENTRADAS; motor de alavancagem roda em COLETA.
**Entradas 2026/mês:** jan 503 · fev 305 · mar 579 · abr 717 · mai 444 · jun 818k =
**R$ 3,37M no semestre, média R$ 566k** (julho no export só R$ 31k = export cortado).
**Realização por meio (valor):** Pix 98,4% · REDE ~100% · Pix Hotmart 100% · DOM
96,8% · Hotmart recorrente 95,8% · **Boleto interno 87,7%** · **Boleto TMB 64,7%
(R$ 167k aberto — pior ativo).**
**Por safra (a média escondia):** boleto interno 2023 95% · 2024 97% · 2025 94% ·
**2026 = 61% (R$ 411k aberto)**; TMB 2026 = 50%.
**Pontualidade (atraso ≤15d; inadimplência = +30d):** em dia 63% · atraso ≤15d
**27%** · inadimplente final 4,3%. **Leitura: o problema do boleto não era perda —
era PONTUALIDADE; ¼ do dinheiro chega atrasado, e no S1 esse atraso era financiado
a 7,22%/mês (explica o cheque especial melhor que os flops).**
**Backlog:** R$ 1,22M a vencer (539 títulos) + R$ 138k em 2027. **Vencido em aberto:
R$ 1,0M (1.103 títulos, 334 inadimplentes, 30 em protesto)** — força-tarefa de
cobrança = caixa sem CAC. Régua de confiabilidade por cliente (verde ≥85% · amarelo
50–85% · vermelho <50%; lógica binária por título, nunca haircut fracionado).

## O.S. × AR (7.451 O.S.) — o cruzamento
Match AR↔OS = **96%**. **~36% das entradas 2026 (R$ 1,21M) SEM O.S. = Hotmart**
(LTVs abaixo são PISO).
**Entradas jan–jul/26 por linha:** MM 947.658 (27,9%) · sem O.S. 1.209.678 (35,6%)
· CTM 556.784 (16,4%) · MPS 204.030 · FGI 144.847 · CV Sign ingressos 107.494 ·
Black 70.088 · STEND 36.630 · INI 24.875 · Planilha 11.544 · **TOTAL 3.397.883.**
**Realização por produto:** CV Sign ingresso 100% · MPS 98,8% · **MM 90,9% (231k
aberto)** · **FGI 87,7% (143k)** · **CTM 87,6% (128k)** · **Black 85% (65k)** →
inadimplência concentrada nos 4 tickets altos parcelados no boleto (R$ 567k), que
são os mais high-touch (padrinho/analista = canal de cobrança pronto).

## LTV por porta de entrada (piso; caixa-vida via AR pago)
| Porta | Clientes | LTV médio | Mediana |
|---|---:|---:|---:|
| MM direto | 72 | 12.967 | 4.100 |
| TDI | 18 | 8.518 | 3.933 |
| Banco de Dados | 21 | 8.175 | 6.000 |
| CTM direto | 76 | 7.316 | 2.667 |
| FAN | 265 | 4.981 | 4.104 |
| MPS | 32 | 4.616 | 250 |
| Black | 73 | 3.537 | 3.333 |
| FGI | 287 | 3.336 | 2.997 |
| Afacom Plus | 191 | 2.796 | 997 |
| **CV Sign ingresso** | **496** | **2.316** | **396** |
| Tactic Day | 236 | 1.324 | 102 |
| Planilha | 792 | 297 | 67 |
**Leituras:** CV Sign média 6× a mediana → ~1 em 5 sobe e paga o canal inteiro ·
só 72 entraram direto no MM → MM é produto de **ascensão** (valida os 90–95% via
evento) · **cadastro do ingresso ≠ cadastro da ascensão (CPF×CNPJ) → números CV
Sign são PISO** (conserto: fuzzy match + captura CPF+CNPJ no credenciamento).

## Notas Fiscais (3.249 NFs, 2 CNPJs)
CFOP 5102/6102 (mercadoria — não captura infoprodutos Hotmart). Receita NF: 2025
R$ 3,1M · 2026 até jun R$ 2,0M. **Recompra 61%.** Curva A = 306 clientes (32%) →
80% da receita. Vida média do recorrente 6,3 meses. 983 CNPJs compradores = semente
do estudo de mercado (TAM).

## Motor de carteira parametrizado (V5 — §31)
Carteira(m) = Carteira(m−1) + Entradas − SaídaTérmino − SaídaChurn [(Carteira+
Entradas)×churn% da Base] + Renovações [NOVO: CTM/MM renovado fica]. Recorrência =
Carteira×ticket OU esteira high ticket += (Venda−Coleta)×%recebível÷meses (RICA/CTM/
MM 75%÷9 · Black 40%÷12). Cancelamento = Saídas×COLETA. **Ponto de partida = carteira
REAL 31/07, nunca a projetada.**
