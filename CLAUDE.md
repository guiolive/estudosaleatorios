# Guia de Estudo — Neuroplasticidade & Dopamina

## O que é este projeto
Uma página HTML única, em **pt-BR**, que sintetiza dois episódios do *Huberman Lab Essentials*
num guia de estudo bonito e navegável:
- **Parte I — Neuroplasticidade** (seções 01–08)
- **Parte II — Dopamina & Motivação** (seções 09–16)

Tudo vive num só arquivo: **`index.html`** (HTML + CSS + JS inline, sem build, sem dependências
além de Google Fonts). Abrir no navegador já funciona.

## Estado atual
- `index.html` está **completo e funcional**: hero, 16 seções, flashcards interativos (18 cartões),
  barra de progresso, nav sticky com scroll-spy, e fontes verificadas.
- Visual atual = **tema escuro "neon"**: fundo quase preto com gradientes, e um sistema de
  **cor = função** (ver abaixo).

## Sistema de design atual (o que NÃO pode se perder no significado)
As cores codificam conteúdo — não são decoração:
| Cor | Var CSS | Significado |
|-----|---------|-------------|
| Laranja | `--alerta` `#FFB454` | Epinefrina / alerta (locus coeruleus) |
| Turquesa | `--foco` `#3FD8C8` | Acetilcolina / foco (tronco encefálico) |
| Roxo | `--marca` `#A98BFF` | Acetilcolina / marcação (núcleo basal) + córtex pré-frontal |
| Rosa | `--dopa` `#FF5C8A` | Dopamina (toda a Parte II) |

Elementos-assinatura: a **"equação química"** no hero da Parte I e a **curva pico↔linha-de-base**
(SVG) na abertura da Parte II. Ambos codificam a ideia central de cada parte.

Classes úteis já existentes: `.callout` (+ `.note` roxo, `.apply` laranja), `.exp` (bloco de
experimento), `.steps/.step`, `.card` (`.plastic`/`.fixed`), `.bars/.bar` (`.warn`), `.take`
(resumo numerado), `.fc` (flashcards), `.src` (fontes). Seções da Parte II têm a classe `.dopa`,
que reaplica o rosa a numeração/callouts/listas.

---

## TAREFA ATUAL: reskin para o tema do **Medium**
O usuário quer trocar a pele visual para o estilo da home do Medium (medium.com).
Ver `reference/medium-theme.md` para a especificação exata (cores, fontes, botões).

### O ponto de decisão crítico
O tema Medium é **monocromático** (preto sobre creme). O guia depende de **4 cores funcionais**.
Não jogue fora a codificação de cor — traduza-a. Duas rotas possíveis (proponha e mostre 1, não as duas):

1. **Acentos discretos** (recomendado): manter fundo creme + serifa preta do Medium, mas usar
   versões *dessaturadas/terrosas* das 4 cores só em detalhes pequenos (filete lateral de callout,
   marcador de lista, número da seção, traço do SVG). O corpo fica preto-sobre-creme, fiel ao Medium.
2. **Tipográfico puro**: abandonar cor e recodificar a função via **hierarquia tipográfica**
   (itálico, small-caps, rótulos monoespaçados, filetes). Mais fiel ao Medium, mas perde o
   "mapa de cores" que ajuda a memorizar. Só siga por aqui se o usuário pedir.

### Regras do reskin
- **Siga o brief do Medium à risca.** Ele foi pedido explicitamente — creme + serifa é a escolha
  do cliente, não um default a ser "melhorado". Não troque por outra estética.
- Preserve **toda a estrutura, o conteúdo e a interatividade** (flashcards, scroll-spy, progresso).
  É um *reskin*, não uma reescrita.
- Mantenha acessível: responsivo até mobile, foco de teclado visível, `prefers-reduced-motion`.
- Os elementos-assinatura (equação e curva) devem ser **repensados no vocabulário do Medium**
  (provavelmente mais tipográficos e com menos brilho), não só recoloridos.

### Como pré-visualizar
```bash
# qualquer um destes:
python3 -m http.server 8000      # abre http://localhost:8000
# ou apenas abra index.html no navegador
```
Peça ao Claude Code para tirar screenshots enquanto itera (ele consegue rodar um headless).

## Fontes / precisão
Números e estudos já estão conferidos e embutidos em `index.html` (ver a seção "Fontes" no fim do
arquivo e `reference/sources.md`). **Não invente dados novos**; se precisar acrescentar, verifique.
