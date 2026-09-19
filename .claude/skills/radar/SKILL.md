---
name: radar
description: Roda o radar do dia com o time de agentes, na ordem pesquisador, verificador, redator, guarda (e o agente do dono, se existir), e depois grava o dia no repositório com um commit. Use quando alguém pedir para rodar o radar ou quando a rotina das 7h disparar.
---

# Radar do dia

Você coordena o time. Não pesquisa, não escreve, não confere: aciona cada agente na ordem e verifica o que ele deixou.

Antes de começar, leia `RADAR.md` e `CLAUDE.md`. Descubra a data de hoje — ela nomeia todos os arquivos do dia. Onde estiver `AAAA-MM-DD`, use a data de hoje.

**Uma etapa por vez. Não pule nenhuma. Não rode duas em paralelo.**

## A regra que vale para todas as etapas

**O relatório do agente não é prova.** Um agente pode dizer que gravou e não ter gravado. Depois de cada agente, abra o arquivo que ele deveria ter deixado e confirme com os próprios olhos que o conteúdo está lá.

Se o relatório disser uma coisa e o arquivo disser outra, **o arquivo ganha**. Rode o agente de novo, avisando o que faltou. Na segunda falha, pare e conte o que aconteceu.

## 1. Pesquisador

Acione o agente `pesquisador`.

**Confira:** `fontes/AAAA-MM-DD.md` existe e tem **pelo menos três itens**.

Menos de três, ou dia sem novidade: **siga mesmo assim**. Dia parado é resposta válida. Registre no relatório final quantos itens vieram.

Arquivo inexistente: o pesquisador falhou. Pare e diga isso.

## 2. Verificador

Acione o agente `verificador`.

**Confira:** `verificacao/AAAA-MM-DD.md` existe e tem uma linha de tabela para cada item de `fontes/`. Conte as duas coisas e compare. Se faltar item, aponte.

## 3. Redator — o briefing

Acione o agente `redator` pedindo **só o briefing**, não a página.

**Confira:** `diario/AAAA-MM-DD.md` existe, tem a primeira linha, tem os itens e tem a seção "O que não conferiu".

## 4. O agente do dono

Olhe `.claude/agents/`. Se houver algum agente além dos quatro do time — `pesquisador`, `verificador`, `redator`, `guarda` —, acione-o. Ele acrescenta a seção dele no fim de `diario/AAAA-MM-DD.md`.

**Confira:** abra `diario/AAAA-MM-DD.md` e procure a seção com o nome dele. Não aceite o relatório como prova — esta etapa já falhou em silêncio antes.

Se não houver agente do dono, pule e siga.

## 5. Redator — a página

Acione o agente `redator` de novo, agora para gerar `index.html`.

Esta etapa vem **depois** da etapa 4 de propósito: a página só é gerada quando o briefing está completo, com a seção do agente do dono já dentro. Gerar antes deixa a página sem essa seção.

**Confira:** `index.html` existe, não tem nenhum marcador sobrando (`{{TITULO}}`, `{{DATA}}`, `{{BRIEFING}}`, `{{ANTERIORES}}`) e contém a seção do agente do dono.

Sobrou marcador ou faltou seção: o redator não terminou. Aponte e pare.

## 6. Guarda

Acione o agente `guarda`. Ele é sempre o último a ler: audita também a seção do agente do dono, nos dois arquivos.

Leia a última linha do relatório dele.

- **NÃO PUBLIQUE** → pare aqui. Mostre o relatório inteiro do guarda. **Não faça commit.** Não conserte nada por conta própria, não tente contornar, não rode o guarda de novo esperando outra resposta. Para destravar, acione o agente responsável pelo achado e depois o guarda de novo.
- **PODE PUBLICAR** → siga para a etapa 7.

Se o relatório não terminar com uma das duas linhas, trate como NÃO PUBLIQUE.

## 7. Gravar

Só chegue aqui com PODE PUBLICAR.

1. `git add -A`
2. `git commit -m "radar de AAAA-MM-DD"`
3. Se houver remoto configurado, `git push`

Se o push falhar, **diga o motivo em uma linha e pare**. Não tente outro jeito, não troque a URL, não mexa em credencial, não force nada. O commit local fica feito; o dono resolve o envio.

## 8. Relatar

Termine com três informações, curtas:

- a primeira linha do briefing
- quantos itens conferiram, de quantos anotados
- quantos agentes rodaram

Se algum agente precisou rodar duas vezes, ou se o guarda barrou antes de liberar, diga isso também.

## Nunca

- Nunca envie nada a ninguém. O commit e o push são a única saída do radar. Sem e-mail, sem mensagem, sem post, sem webhook.
- Nunca use chave, senha ou token. Se algum passo pedir credencial, pare e avise.
- Nunca escreva o briefing você mesmo. Se um agente falhou, o dia falhou — diga isso em vez de cobrir o buraco.
- Nunca confie no relatório de um agente sem abrir o arquivo.
- Nunca publique contra o guarda.
