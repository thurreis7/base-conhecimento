# _config.md — Base de Conhecimento de Reuniões

Arquivo de configuração operacional. A rotina de ingestão lê este arquivo
antes de qualquer processamento e o segue LITERALMENTE. Não improvisar
formato, nome de pasta ou nome de setor.

Fuso: America/Sao_Paulo

## 0. Fontes de entrada (camada adaptavel)

O sistema separa CAPTACAO (ler transcricoes de uma fonte) de PROCESSAMENTO
(filtrar, classificar, reescrever, sintetizar, arquivar). O processamento e
IDENTICO para qualquer fonte. Trocar ou somar fonte NAO muda regra, molde ou
pasta — muda so o passo de captacao (PASSO 2 da rotina).

Fontes:
  ZOOM   — ATIVA. Gravacoes em nuvem via conector Zoom (transcricao VTT ou
           AI Companion / Smart Recording).
  PLAUD  — FUTURA (inativa). Transcricoes do dispositivo/app Plaud, lidas do
           repositorio/export que o proprio Plaud disponibiliza (API, pasta
           sincronizada ou arquivo — a definir quando o hardware chegar).
           So processar quando o dono avisar e enviar o PROMPT DE ATIVACAO
           nomeando a fonte. Ate la, ignorar por completo.

Cada item processado registra no YAML:
  fonte:      zoom | plaud | ...
  origem_id:  identificador unico do item NAQUELA fonte

DEDUPE e sempre pelo par (fonte, origem_id), nunca so pelo id. Em
_estado.json, "processadas" e "pendentes" guardam chaves no formato
"fonte:origem_id" (ex.: "zoom:abc123==").

Para ADICIONAR uma fonte nova: (1) descrever aqui como listar os itens e
puxar o texto daquela fonte; (2) o dono envia o prompt de ativacao nomeando
a fonte; (3) nada mais no pipeline muda.

## 1. Hierarquia

EMPRESA / SETOR / CLASSE / ANO / MES / SEMANA / DIA

Empresas: AFATEC · AFACOM · COMPARTILHADO · PESSOAL
Classes:  REUNIAO · NAO-REUNIAO

Setores — LISTA FECHADA, nunca inventar, abreviar ou traduzir:
TECH · PRODUTO · COMERCIAL · FINANCEIRO · AULAS · RH · SOCIETARIO

Pastas de setor nascem sob demanda: na primeira gravação classificada
naquele setor, criar a pasta e os três arquivos de controle
(_contexto-acumulado.md, _pendencias-abertas.md, _indice.md) com
cabeçalho e seções vazias.

Nomes:
  Ano        AAAA                 2026
  Mes        MM-Mes               08-Agosto
  Semana     S<ISO>_DD-a-DD       S32_03-a-09
  Dia        DD                   03
  Sintese    AAAA-MM-DD__slug.md
  Transcricao AAAA-MM-DD__slug__transcricao.md

Slug: minusculas, sem acento, hifen entre palavras, maximo 5 palavras.

Multi-setor: o arquivo mora no setor PRINCIPAL. Em _TRANSVERSAL/ gravar
um .md de uma linha com link relativo para o arquivo real, e registrar a
gravacao tambem no _indice.md do setor secundario. NUNCA duplicar
conteudo.

## 2. Classificacao

EMPRESA (unica) — por assunto, participantes e produto citados. Duvida
real entre AFATEC e AFACOM: usar a do produto mais citado e marcar
confianca media. Assunto societario ou que atravessa as duas:
COMPARTILHADO.

SETOR — um principal (define onde o arquivo mora) e zero ou mais
secundarios. Se nada encaixar na lista fechada, usar o mais proximo e
marcar confianca baixa. NUNCA criar setor novo.

CLASSE:
  REUNIAO       2 ou mais participantes com fala
  NAO-REUNIAO   1 participante — gravacao solo, de tela, ditado, ensaio
  setor AULAS   webinar, ou titulo com Aula / MPS / Mentoria / Turma

CONFIANCA: alta (evidente na fala) · media (inferido por contexto) ·
baixa (ambiguo ou transcricao ruim). Media e baixa recebem [REVISAR] na
linha do _indice.md.

## 3. Filtro de entrada — nesta ordem

1. Titulo contem [CONF] → PULAR INTEIRA. Nao ler transcricao, nao gravar
   nada, nao indexar. Apenas contabilizar como pulada.
2. Par (fonte, origem_id) ja em "processadas" → pular.
3. Duracao menor que 10 minutos → pular.
4. Transcricao indisponivel → registrar em "pendentes" com motivo e
   seguir adiante. NUNCA travar a rotina.
5. Apos 3 tentativas → registrar no _indice.md como [SEM TRANSCRICAO] e
   remover da fila.

## 4. Glossario de normalizacao

Aplicar ANTES de reescrever. Acrescentar sempre que um erro novo
aparecer. Este e o unico arquivo que o dono edita a mao.

  CIA · SIA · si a               -> SIA
  MIPS · EMPS · em pe esse       -> MPS
  ine · i ni                     -> INI
  cavi · ca vi                   -> CAV
  ceteeme · ce te eme            -> CTM
  rica                           -> RICA
  eme-eme · em em                -> MM
  Afatec · afa tec               -> AFATEC
  Afacon · afa com               -> AFACOM
  Afatecnicos                    -> AFATECNICOS
  Afacaste                       -> Afacast
  Jefite · Jefe                  -> Jefte
  Ingrede                        -> Ingrid
  Walace                         -> Wallace
  Luis                           -> Luiz
  erre ce efe                    -> RCF
  pe ge ve                       -> PGV

## 5. Regras rigidas de extracao — inegociaveis

- DECISAO e so o explicitamente fechado na fala. Discussao sem
  fechamento vai para "Na minha mesa", NUNCA para "Decisoes".
- RESPONSAVEL nunca e inferido. Nao nomeado: NAO DEFINIDO.
- PRAZO nunca e inventado. Nao dito: NAO DEFINIDO.
- NUMERO nunca e arredondado nem estimado. Como foi dito.
- FALA nunca e atribuida a quem a transcricao nao identificou.
- Transcricao ruim demais para extrair com seguranca: escrever isso
  explicitamente, em vez de produzir sintese vaga.
- Transcricao truncada: aviso em destaque no topo dos dois arquivos.
- Secao vazia e OMITIDA, nunca preenchida com "nada a registrar".

## 6. Molde — transcricao reescrita

REESCRITA NAO E RESUMIDA. Nada dito pode ser descartado em substancia.
A reescrita: aplica o glossario, identifica o falante, remove muleta de
fala e ruido, organiza em blocos tematicos com timestamp, marca
[inaudivel] onde a transcricao falhou.

---
tipo: transcricao
data: AAAA-MM-DD
gravacao: <titulo original>
empresa: <EMPRESA>
setor_principal: <SETOR>
duracao_min: <n>
fonte: <zoom|plaud|...>
origem_id: "<id na fonte>"
---

## [00:00] <tema do bloco>
**<Falante>:** ...

## Molde — sintese

---
tipo: sintese
data: AAAA-MM-DD
gravacao: <titulo original>
empresa: <EMPRESA>
setor_principal: <SETOR>
setores_secundarios: [<...>]
classe: REUNIAO | NAO-REUNIAO
participantes: [<...>]
duracao_min: <n>
confianca: alta | media | baixa
fonte: <zoom|plaud|...>
origem_id: "<id na fonte>"
transcricao: <nome do arquivo de transcricao>
---

## Sintese
5 a 8 linhas: do que se tratou e o que mudou por causa disso.

## Decisoes tomadas
- decisao — quem decidiu

## Pendencias
- [ ] tarefa — responsavel — prazo

## Na minha mesa
- o que travou esperando o dono da base

## Conexao com o historico
Avanca, repete ou contradiz o que ja estava decidido. Com o NUMERO de
recorrencias quando o assunto ja apareceu antes.

## Pontos frageis das conclusoes
Decisao sem dado, sem responsavel, sem prazo, ou com premissa nao
verificada. Critica direta, nao diplomatica.

## Questionamentos para a proxima
1. pergunta
   *por que:* uma linha de justificativa

## Citacoes-chave
- [00:00:00] **<Falante>:** ... (maximo 3)

AS TRES SECOES ANTES DAS CITACOES SO PODEM SER ESCRITAS DEPOIS DE LER O
_contexto-acumulado.md DO SETOR. Se sairem genericas, o passo de leitura
previa falhou.

## 7. Molde — contexto acumulado (um por setor)

# Contexto acumulado — <EMPRESA> / <SETOR>
Atualizado: AAAA-MM-DD · <n> gravacoes indexadas

## Estado atual
O que e verdade hoje. 5 a 10 linhas. REESCRITO, nunca acrescentado.

## Decisoes vigentes
- AAAA-MM-DD — decisao — quem decidiu

## Decisoes revogadas
- AAAA-MM-DD — decisao antiga
  — [REVOGADA em AAAA-MM-DD: motivo]

## Questoes abertas recorrentes
- assunto — <n> gravacoes — nunca fechou

## Pessoas e papeis
- nome — papel — autonomia observada

## Vocabulario do setor
- termo — o que significa aqui

## Historico condensado
### <mes corrente> (detalhado)
- DD/MM — gravacao: o que fechou, o que travou
### <mes-1>
- paragrafo unico
### <mes-2>
- paragrafo unico
### Trimestre anterior
- paragrafo unico

### Regras do contexto acumulado
CONDENSACAO — obrigatoria:
  mes corrente     uma linha por gravacao
  meses 2 e 3      um paragrafo por mes
  mais de 3 meses  um paragrafo por trimestre
TETO: 800 linhas. Ao atingir, condensar mais o historico antigo.
"Decisoes vigentes" e "Questoes abertas recorrentes" NUNCA sao cortadas.
REESCRITA: o arquivo e reescrito, nao acrescentado. Decisao nova que
contradiz uma antiga move a antiga para "Revogadas" com data e motivo.
NUNCA apagar — e isso que permite responder por que a decisao mudou.
CONTADOR: questao que reaparece incrementa o contador de recorrencias.
E o contador que alimenta as secoes "Conexao com o historico" e
"Questionamentos" da sintese. Sem ele o sistema nao percebe assunto
travado.

## 8. Molde — indice e pendencias

# Indice — <EMPRESA> / <SETOR>
| Data | Gravacao | Classe | Confianca | Sintese | Transcricao |
Mais recente no topo.

# Pendencias abertas — <EMPRESA> / <SETOR>
Atualizado: AAAA-MM-DD

## Vencidas
- [ ] tarefa — responsavel — venceu DD/MM (<n> dias)

## Com prazo
- [ ] tarefa — responsavel — DD/MM

## Sem prazo definido
- [ ] tarefa — responsavel — NAO DEFINIDO

## Fechadas nos ultimos 30 dias
- [x] tarefa — responsavel — fechada em DD/MM

Fechada ha mais de 30 dias sai do arquivo.

## 9. Governanca

- SO A ROTINA ESCREVE. Consumo e leitura.
- Uma rotina, na conta do dono da base (que detem o acesso as fontes).
- Socios consomem via Project apontando para este repositorio.
- Se mais de uma fonte escrever, o contexto acumulado entra em conflito
  e o controle de duplicidade quebra EM SILENCIO.
