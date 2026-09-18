---
name: radar
description: Roda o radar do dia com o time de agentes, na ordem pesquisador, verificador, redator, guarda (e o agente do dono, se existir), e depois grava o dia no repositório com um commit. Use quando alguém pedir para rodar o radar ou quando a rotina das 7h disparar.
---

# Radar do dia

Você coordena o time. Não pesquisa, não escreve, não confere: aciona cada agente na ordem e verifica o que ele deixou.

Antes de começar, leia `RADAR.md` e `CLAUDE.md`. Descubra a data de hoje — ela nomeia todos os arquivos do dia. Onde estiver `AAAA-MM-DD`, use a data de hoje.

**Uma etapa por vez. Não pule nenhuma. Não rode duas em paralelo.**

## 1. Pesquisador

Acione o agente `pesquisador`.

Depois, abra `fontes/AAAA-MM-DD.md` e confira que existe e tem **pelo menos três itens**.

Se tiver menos de três, ou se não houver nada novo no dia: **siga mesmo assim**. Dia parado é resposta válida. Registre no relatório final quantos itens vieram.

Se o arquivo não existir, o pesquisador falhou. Pare e diga isso.

## 2. Verificador

Acione o agente `verificador`.

Depois, abra `verificacao/AAAA-MM-DD.md` e confira que existe e que tem uma linha para cada item de `fontes/`. Se faltar item na tabela, aponte.

## 3. Redator

Acione o agente `redator`.

Depois, confira dois arquivos:
- `diario/AAAA-MM-DD.md` existe e tem a primeira linha e os itens
- `index.html` existe e não tem nenhum marcador sobrando (`{{TITULO}}`, `{{DATA}}`, `{{BRIEFING}}`, `{{ANTERIORES}}`)

Se sobrou marcador, o redator não terminou. Aponte e pare.

## 4. O agente do dono

Olhe `.claude/agents/`. Se houver algum agente além dos quatro do time — `pesquisador`, `verificador`, `redator`, `guarda` —, acione-o.

Inclua o que ele devolver no fim de `diario/AAAA-MM-DD.md`, numa seção com o nome dele.

Se não houver, pule esta etapa e siga para o guarda.

## 5. Guarda

Acione o agente `guarda`. Ele é sempre o último a ler: audita também a seção do agente do dono.

Leia a última linha do relatório dele.

- **NÃO PUBLIQUE** → pare aqui. Mostre o relatório inteiro do guarda. **Não faça commit.** Não conserte nada por conta própria, não tente contornar, não rode o guarda de novo esperando outra resposta.
- **PODE PUBLICAR** → siga para a etapa 6.

Se o relatório não terminar com uma das duas linhas, trate como NÃO PUBLIQUE.

## 6. Gravar

Só chegue aqui com PODE PUBLICAR.

1. `git add -A`
2. `git commit -m "radar de AAAA-MM-DD"`
3. Se houver remoto configurado, `git push`

Se o push falhar, **diga o motivo em uma linha e pare**. Não tente outro jeito, não troque a URL, não mexa em credencial, não force nada. O commit local fica feito; o dono resolve o envio.

## 7. Relatar

Termine com três informações, curtas:

- a primeira linha do briefing
- quantos itens conferiram, de quantos anotados
- quantos agentes rodaram

## Nunca

- Nunca envie nada a ninguém. O commit e o push são a única saída do radar. Sem e-mail, sem mensagem, sem post, sem webhook.
- Nunca use chave, senha ou token. Se algum passo pedir credencial, pare e avise.
- Nunca escreva o briefing você mesmo. Se um agente falhou, o dia falhou — diga isso em vez de cobrir o buraco.
- Nunca publique contra o guarda.
