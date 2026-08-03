# LEIA PRIMEIRO — Mapa da base de conhecimento

Este arquivo orienta qualquer Claude que consulte esta base via Project.
Leia-o antes de responder qualquer pergunta sobre o conteúdo.

## O que é esta base e como ela é alimentada

Esta é a base de conhecimento das reuniões da **AFATEC / AFACOM**. Ela
guarda, de forma organizada e consultável, o que foi tratado, decidido e
deixado pendente em cada reunião gravada no Zoom.

**Apenas uma rotina automatizada escreve aqui.** Ela roda no Claude Code,
lê as gravações transcritas do Zoom, reescreve a transcrição, gera uma
síntese e atualiza o contexto acumulado de cada setor. **Ninguém deve
fazer commit manual.** Sócios apenas consomem (leem) via Project.

## Hierarquia de pastas

```
EMPRESA / SETOR / CLASSE / ANO / MES / SEMANA / DIA
```

- **Empresas:** AFATEC · AFACOM · COMPARTILHADO · PESSOAL
- **Setores (lista fechada):** TECH · PRODUTO · COMERCIAL · FINANCEIRO ·
  AULAS · RH · SOCIETARIO
- **Classes:** REUNIAO · NAO-REUNIAO
- `_TRANSVERSAL/` guarda ponteiros para gravações que tocam mais de um
  setor — o conteúdo real mora sempre no setor principal, nunca duplicado.

Pastas de setor nascem sob demanda, na primeira gravação classificada
naquele setor.

## Os quatro tipos de arquivo

1. **Transcrição reescrita** (`AAAA-MM-DD__slug__transcricao.md`) — a fala
   da reunião, reescrita e limpa, com falante e timestamp. Não é resumo:
   guarda o literal do que foi dito. Vá nela só quando precisar da fonte.
2. **Síntese** (`AAAA-MM-DD__slug.md`) — do que se tratou, decisões,
   pendências, crítica e questionamentos de uma reunião específica.
3. **Contexto acumulado** (`_contexto-acumulado.md`, um por setor) — o
   estado vigente do setor: o que é verdade hoje, decisões em vigor,
   decisões revogadas e questões abertas recorrentes. É a memória viva.
4. **Índice e pendências** (`_indice.md` e `_pendencias-abertas.md`, um par
   por setor) — a lista de todas as gravações e a lista de tarefas em
   aberto/fechadas do setor.

## ORDEM DE LEITURA — sempre nesta sequência

1. **`_contexto-acumulado.md`** do setor — comece sempre por ele.
2. **`_pendencias-abertas.md`** e **`_indice.md`** do setor.
3. As **sínteses** relevantes.
4. A **transcrição reescrita** — só quando precisar do literal do que foi
   dito.

## Dois avisos que não podem ser ignorados

- **`NÃO DEFINIDO`** significa que aquilo não foi dito na reunião. Nunca
  preencha por inferência. Se está NÃO DEFINIDO, a resposta correta é que
  não foi definido — não invente responsável, prazo ou número.
- Decisão listada em **"Decisões revogadas"** **não vale mais**. Ela fica
  registrada apenas para explicar por que a decisão mudou; não a trate
  como vigente.
