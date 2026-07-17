# Guia de Estudo — Biblioteca de neurociência & psicologia (pt-BR)

## O que é este projeto
Uma **biblioteca de guias de estudo** em pt-BR, cada capítulo destilando um episódio de podcast
(sobretudo *Huberman Lab*) em **prosa que ensina o mecanismo** — não um resumo, não um deck de revisão.
O objetivo do dono (Guilherme, virando programador, vindo do "tutorial hell") é **ler para aprender de
verdade**, com os conceitos complexos explicados como o convidado fala no vídeo.

Tudo vive num único arquivo: **`index.html`** (HTML + CSS + JS inline, sem build; só Google Fonts).
Abrir no navegador funciona. Deploy é automático via GitHub Pages.

### Capítulos atuais (todos aprofundados, exceto onde indicado)
- **01 · Neuroplasticidade** (seções 01–08) — "How to Focus to Change Your Brain"
- **02 · Dopamina & Motivação** (09–16) — ep. 39, versão longa
- **03 · Como o cérebro aprende** (17–24) — Terry Sejnowski — **AINDA NO FORMATO DECK; falta aprofundar
  e falta limpar os travessões.** Precisa da transcrição.
- **04 · Autocontrole & motivação** (25–32) — Kentaro Fujita

As seções são numeradas de forma **contínua** pela biblioteca inteira (01–32). Um capítulo novo continua
a numeração (o próximo começaria em 33).

---

## COMO ADICIONAR / APROFUNDAR UM CAPÍTULO (o fluxo que funciona)

1. **Consiga a transcrição** (o usuário cola; é do YouTube, sem problema de direitos para uso pessoal).
   Salve em `reference/transcripts/NN-nome.md` com cabeçalho (episódio, URL, link YouTube, nota de que é
   rendering limpo, não verbatim). É a matéria-prima.

2. **VERIFIQUE as citações ANTES de escrever.** Isto não é opcional: a verificação já pegou **erros
   factuais que iam ao ar** (e um que foi ao ar). Dispare um subagente (`general-purpose`, em background)
   com WebSearch/WebFetch para checar cada estudo em PubMed/Crossref: autores, ano, título, journal,
   páginas, PMID/DOI — e, crucialmente, **se a afirmação do episódio bate com o que o paper mostra**.
   Huberman/convidados às vezes exageram ou fundem estudos. Erros reais já pegos: "20 min de descanso >
   uma noite de sono" (sem fonte), frio "3 horas"/adrenalina disparando (era noradrenalina), plasticidade
   "migra para o córtex auditivo" (estudo nem mapeou A1), cafeína "uso regular" (era dose única), estudo
   da *Neuron* sobre crenças (não existe). **Prefira DERRUBAR uma citação a imprimir errado.**

3. **Escreva em prosa explicativa**, no registro em que o convidado fala. Alvo ~3.000–4.500 palavras por
   capítulo. Prosa é a espinha dorsal; os blocos visuais (callout/duo/steps/bars) são **acento**, não a
   estrutura. Desenvolva o mecanismo, a intuição contraintuitiva, o experimento, e as ressalvas honestas.
   Inclua um callout de "onde a versão popular exagera" sempre que a checagem tiver pego algo.

4. **Regras de estilo (rígidas):**
   - **SEM travessões (—).** O projeto os removeu de propósito. Use vírgula, dois-pontos, parênteses.
     (En-dash `–` em faixas numéricas e `·` em referências são OK.)
   - **Branding:** o nome "Huberman" só aparece em **citação** (seção Referências, sources.md). Na prosa,
     use "o episódio" / "o apresentador", nunca "Huberman conta". Nada de contexto pessoal do usuário
     (código, "tutorial hell", nome próprio) na prosa — exemplos genéricos de estudo/treino/metas.
   - Mantenha **acessível**: responsivo, foco de teclado visível, `prefers-reduced-motion`.

5. **Preserve estrutura e interatividade.** É um site com editor CRUD, busca, barra de progresso,
   scroll-spy. Não quebre o JS. Cada capítulo é um
   `<article class="chapter" data-key="..." data-rev="N" data-num="NN" data-tag="...">` dentro do
   `<template id="seed">`. Costuma abrir com um **elemento-assinatura** SVG.

6. **Bump o `data-rev`** do capítulo sempre que mudar o conteúdo dele (ver mecanismo abaixo). Sem isso, a
   mudança **não chega** a quem já tem a biblioteca em cache.

7. **Atualize as fontes:** `reference/sources.md` (com as ressalvas da verificação) e a seção "Referências"
   do próprio capítulo, com PMIDs/DOIs verificados. Documente o que foi descartado e por quê.

8. **Teste no navegador** (`python3 -m http.server`, Browser tools): (a) instalação nova = capítulos na
   versão atual; (b) simular cache antigo/deck no localStorage → recarregar → deve atualizar sozinho;
   (c) simular capítulo editado à mão → deve ser preservado; (d) recarregar de novo → idempotente; (e)
   console sem erros; (f) `node --check` no `<script>` extraído.

9. **Commit assinado → push branch → deploy** (ver Deploy abaixo).

---

## Mecanismo de auto-atualização (CRÍTICO de entender)
O conteúdo dos capítulos vive no **`localStorage`** do leitor, não é lido do HTML a cada visita. Por isso:
- Cada `<article>` semente tem **`data-rev`** (inteiro). Ao carregar, o JS **atualiza no lugar** os
  capítulos-semente cuja `data-rev` é maior que a última aplicada — **preservando id, posição e leitura**.
- Só atualiza se o usuário **não editou** aquele capítulo à mão. Edição é detectada por **hash (djb2)** do
  corpo vs. o hash da última semente aplicada, guardado em `localStorage['biblioteca:seedmeta']`.
- Capítulos criados/editados pelo usuário nunca são tocados. Capítulos-semente novos são adicionados.
- **Regra prática:** mudou o conteúdo de um capítulo? **Bump o `data-rev`.** Senão o deploy é invisível.
- Chaves no localStorage: `biblioteca:chapters`, `biblioteca:seedkeys`, `biblioteca:seedmeta`,
  `biblioteca:read`, `biblioteca:last`.

## Sistema de design (cor = função; não é decoração)
Tema **Medium**: fundo creme, serifa preta. Vars CSS das 4 cores funcionais (versões terrosas):
`--f-key` roxo (ideia-chave / marcação / pré-frontal), `--f-apply` ocre (aplicar / alerta / epinefrina),
`--f-focus` verde-azulado (foco / termo / acetilcolina), `--f-warn` argila-rosa (cuidado / dopamina).
Classes: `.callout` (`.key`/`.apply`/`.term`/`.warn`), `.duo`/`.d-card` (`.plastic`/`.fixed`),
`.steps`/`.step`, `.bars`/`.bar` (`.warn`), `.stats`/`.stat`, `.facts`/`.f-row`, `.equation`, `.curve`
(SVG), `.takeaways`, `.sources`. Ênfases inline: `.k-alerta`/`.k-foco`/`.k-marca`/`.k-dopa`.
Elementos-assinatura por capítulo: a "equação" (neuroplasticidade), a curva pico↔base (dopamina), o laço
do aprendizado (Sejnowski), as curvas porquê×como (autocontrole).

## Deploy (GitHub Pages, automático a partir da `main`)
- Repo: **`guiolive/estudosaleatorios`** (foi renomeado de `huberman-guia`).
- Site no ar: **https://guiolive.github.io/estudosaleatorios/**
- Commits são **assinados** (SSH via 1Password) — não desabilite a assinatura; se falhar, o 1Password
  provavelmente travou (pedir para o usuário desbloquear).
- Fluxo: commit na branch → `git push` → `git push origin HEAD:main` (fast-forward). Pages faz o build
  sozinho; confirmar com `gh api repos/guiolive/estudosaleatorios/pages/builds/latest --jq .status`.
- **A `main` recebe commits de outras sessões** entre um deploy e outro (já aconteceu: branding,
  generalização). Sempre `git fetch origin main` e mesclar antes; conflitos costumam ser no `index.html`.

## Precisão / fontes
Números e estudos são **verificados** (ver `reference/sources.md`, com as ressalvas). **Não invente dados.**
Se precisar acrescentar, verifique via PubMed/Crossref antes. `reference/transcripts/` guarda a
matéria-prima. `reference/medium-theme.md` documenta o tema visual.

## Como me ajudar (Guilherme)
Estou virando programador e venho do tutorial hell. Me empurre a **codar/aplicar**, não a mais conteúdo.
Explique e me deixe tentar; em exercício, dê pistas, não a solução pronta. Português (pt-BR). Uma
observação calibrada, depois confie na intenção e siga.
