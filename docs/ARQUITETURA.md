# Arquitetura do projeto

Documento técnico para quem quer manter, adaptar ou contribuir com o Intensivo Interativo.

## Visão geral

O projeto é um **site estático de página única** (SPA por abas, sem roteador):

- `index.html` — aplicação principal. Todo o CSS e o JavaScript ficam embutidos no arquivo.
- `exercicios_vetores_grafos.html` — página independente com 20 exercícios de Vetores e Grafos.
- Nenhum build, bundler, framework ou dependência de runtime.
- Hospedagem: **GitHub Pages**, build `legacy`, servindo a raiz da branch `main`.

```text
Navegador
└── index.html
    ├── CSS inline (tokens + componentes + tema noturno)
    ├── HTML das seções <section class="tab">
    └── JavaScript inline
        ├── navegação e tema
        ├── checklist de aulas
        ├── ferramentas SVG (grafos, vetores, decomposição, bipartido)
        ├── banco do simulado (BANCO)
        ├── flashcards (FLASH)
        └── modo intensivo (BANCO_INT)
```

Consequência prática: **qualquer alteração de conteúdo é uma edição de HTML/JS**, sem etapa de compilação. O custo é que o arquivo principal é grande (~220 KB) e mistura estrutura, estilo e dados — por isso este guia e o [ADICIONAR-CONTEUDO.md](ADICIONAR-CONTEUDO.md) existem.

## Navegação por abas

As seções usam a classe `.tab`; apenas a que tem `.on` aparece. Os botões do menu carregam `data-aba="<id-da-seção>"`.

```html
<li><button data-aba="vetores">Vetores</button></li>

<section id="vetores" class="tab"> ... </section>
```

O clique é delegado no `document`:

```js
document.addEventListener('click', e => {
  const t = e.target.closest('[data-aba]');
  if (t) { e.preventDefault(); irPara(t.dataset.aba); }
});
```

`irPara(aba)`:

1. alterna `.on` nas seções e nos botões do menu;
2. salva a aba em `localStorage.abaSite`;
3. se for `ferramentas`, recalcula o `viewBox` dos SVGs;
4. rola para o topo.

Ao carregar, a última aba salva é restaurada. Não há URL por seção — link compartilhado sempre abre na última aba local (ou em Início).

## Estrutura de uma aula

```html
<div class="aula">
  <h3><span class="aula-tag">Aula 1</span> Título da aula</h3>
  <div class="nota">Texto explicativo com <span class="hl">destaque</span>.</div>
  <div class="exemplo">
    <div class="ex-t">📌 Exemplo</div>
    <div class="ex-b">
      <div class="passo"><span class="pn">1</span><p>Primeiro passo.</p></div>
    </div>
  </div>
  <figure class="fig">
    <svg viewBox="0 0 300 200" width="300" role="img" aria-label="Descrição da figura">...</svg>
    <figcaption>Legenda da figura.</figcaption>
  </figure>
</div>
```

Componentes disponíveis no CSS:

| Classe | Uso |
| --- | --- |
| `.nota` / `.nota.atencao` | caixa de observação (a variante `atencao` é a faixa vermelha) |
| `.exemplo` + `.ex-t` + `.ex-b` + `.passo` + `.pn` | exemplo resolvido em passos numerados |
| `.fig` + `figcaption` | figura SVG com legenda |
| `.formula` | fórmula em fonte monoespaçada |
| `.hl` | destaque amarelo no texto |
| `.grid2` | duas colunas responsivas |
| `.aula-tag` | etiqueta "Aula N" |

O título de cada `.aula` recebe automaticamente um botão **"Revisei"** via JavaScript. Não é preciso adicioná-lo no HTML.

## Esquemas de dados

### Simulado — `BANCO`

Array de objetos. Cada questão tem um `t` (tipo) e uma `ar` (área/matéria).

| Campo | Obrigatório | Descrição |
| --- | --- | --- |
| `t` | sim | `mc`, `num`, `clique`, `caminho` ou `bip` |
| `ar` | sim | sigla da matéria (`V`, `G`, `S`, `N`; novas matérias usam uma sigla própria) |
| `e` | sim | enunciado em HTML |
| `x` | sim | explicação exibida após a resposta |
| `a` / `c` | `mc` | alternativas e índice da correta (base 0) |
| `r` / `tol` / `u` | `num` | resposta, tolerância e unidade |
| `svg` | opcional | função que devolve o SVG da questão |
| `nome` | `clique` | nome do vértice correto |
| `seq` / `custo` | `caminho` | sequência de vértices e custo total |
| `opcoes` | `bip` | lista `{t, ok, d}` com SVGs alternativos |

As questões recebem um `id` em tempo de execução (`q.id`) para salvar respostas.

Tipos suportados pelo renderizador (`RENDER`):

- `mc` — múltipla escolha;
- `num` — entrada numérica (aceita vírgula, tolerância configurável);
- `clique` — clique no vértice correto do SVG;
- `caminho` — montar o caminho clicando nos vértices e digitar o custo;
- `bip` — escolher qual dos grafos é bipartido.

### Flashcards — `FLASH`

```js
{s:'V', f:'Pergunta da frente', v:'Resposta do verso em HTML'}
```

`s` é a matéria (`V`, `G`, `S`, `N`). O filtro da interface é montado por `fcBtnOn`/`fcMonta`; para uma matéria nova, inclua o botão correspondente.

### Modo Intensivo — `BANCO_INT`

```js
{e:'Enunciado', a:['alternativa A','B','C','D'], c:1}
```

A cada rodada, `embaralha(BANCO_INT).slice(0,10)` sorteia 10 questões. Cada acerto vale 10 pontos mais bônus de tempo; o cronômetro começa em 30 s.

## Ferramentas interativas

### Construtor de grafos

Estado em memória: `gVerts`, `gArest`, `modoG`, `selG`. Os vértices são desenhados em SVG e podem ser arrastados. Ações:

- **Criar vértice**: clique no quadro.
- **Ligar vértices**: clique em dois vértices; o peso vem do campo `#inpPeso`.
- **Dijkstra**: menor caminho do primeiro ao último vértice criado.
- **Kruskal**: árvore geradora mínima (union-find simples).
- **Limpar tudo**: reinicia o estado.

Não há persistência: recarregar a página limpa o grafo.

### Visualizador de vetores

Lê as componentes de **a** e **b**, desenha os vetores com origem no centro, a soma tracejada e calcula módulos, produto escalar e ângulo. A conversão de coordenadas usa o centro do `viewBox` e uma escala fixa.

### Laboratório de decomposição

Dois sliders (`módulo` e `ângulo`) atualizam em tempo real o vetor e suas componentes `Ax = A·cos θ` e `Ay = A·sen θ`, incluindo o sinal por quadrante.

### Pintar bipartido

Grafos predefinidos em `BIP_PRESETS`, duas tintas e borracha. **Verificar** valida a coloração; **Mostrar solução** pinta um grafo biparticionável; **Gerar outro grafo** troca o preset. Se o grafo tiver ciclo ímpar, a coloração correta é impossível — o que é justamente a lição.

## Persistência (`localStorage`)

| Chave | Conteúdo | Onde é usada |
| --- | --- | --- |
| `abaSite` | id da última aba | navegação |
| `temaSite` | `dia` ou `noite` | tema |
| `aulasVistas` | array `["vetores-0", "grafos-2", ...]` | checklist |
| `simSalvo` | `{modo, estados}` com respostas por modo | simulado |
| `flashDificeis` | array de ids/índices de cartas marcadas | flashcards |

Tudo é local ao navegador. Não existe sincronização, conta ou back-end. O código sempre envolve leitura/escrita em `try/catch` para não quebrar em modo privado.

## Tema e acessibilidade

- Tokens CSS em `:root`; o tema noturno sobrescreve os tokens em `body.noturno`.
- O botão `#btnTema` alterna e salva a preferência.
- Figuras têm `role="img"` e `aria-label`; botões usam elementos nativos; foco visível com `:focus-visible`.
- `prefers-reduced-motion` desativa animações.
- Layout responsivo em um único breakpoint principal (`max-width: 760px`).

## Página de exercícios

`exercicios_vetores_grafos.html` é independente do site principal:

- 20 questões (10 de Vetores, 10 de Grafos);
- tipos: múltipla escolha, entrada numérica, clique em vértice, montagem de caminho e escolha de grafo bipartido;
- correção imediata com explicação;
- **não** persiste progresso (recarregar reinicia);
- tem link de volta para o site principal no rodapé.

## Publicação (GitHub Pages)

O GitHub Pages está configurado em **Settings → Pages** com:

- Source: *Deploy from a branch*
- Branch: `main`
- Pasta: `/ (root)`

Fluxo de publicação:

```bash
git add .
git commit -m "Descrição da mudança"
git push origin main
```

O Pages reconstrói automaticamente em alguns minutos. O arquivo `.nojekyll` na raiz desativa o processamento do Jekyll, garantindo que todos os arquivos estáticos sejam servidos como estão.

### PDFs e direitos autorais

`*.pdf` está no `.gitignore`: os materiais originais não são publicados. Se for adicionar novas fontes, prefira **resumos próprios** e **links** para o material original, não cópias integrais.

## Limitações conhecidas

- Arquivo único grande: mudanças de estilo e conteúdo convivem no mesmo HTML.
- Sem URL por aba/matéria: links compartilhados não abrem uma seção específica.
- Sem testes automatizados: as contagens exibidas são calculadas em tempo de execução, mas o gabarito é validado manualmente.
- Progresso local: não acompanha o usuário entre dispositivos.
- Dependência de fontes externas do Google (o restante funciona offline após o carregamento).
