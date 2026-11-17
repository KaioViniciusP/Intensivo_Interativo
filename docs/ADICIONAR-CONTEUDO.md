# Adicionar conteúdo

Guia prático para editar o site sem quebrar nada. Vale para **aulas novas**, **questões**, **flashcards** e **matérias inteiras**.

> Antes de começar: abra o `index.html` no navegador e teste com um servidor local (`python -m http.server 8000`). Não há build — é só salvar e recarregar.

## Índice

1. [Adicionar uma aula](#1-adicionar-uma-aula)
2. [Adicionar questão ao simulado](#2-adicionar-questão-ao-simulado)
3. [Adicionar flashcard](#3-adicionar-flashcard)
4. [Adicionar questão ao Modo Intensivo](#4-adicionar-questão-ao-modo-intensivo)
5. [Criar uma matéria nova](#5-criar-uma-matéria-nova)
6. [Checklist final](#6-checklist-final)

---

## 1. Adicionar uma aula

Localize a seção da matéria (`<section id="vetores" class="tab">`, `id="grafos"`, `id="sistemas"` ou `id="venn"`) e insira um `.aula` **antes** do fechamento `</section>`.

```html
<div class="aula">
  <h3><span class="aula-tag">Aula 13</span> Título curto e direto</h3>
  <div class="nota">Ideia principal em uma ou duas frases, com <span class="hl">destaque</span> no termo-chave.</div>
  <div class="nota atencao"><strong>Observação importante:</strong> o erro clássico que a prova cobra.</div>

  <div class="exemplo">
    <div class="ex-t">📌 Exemplo comentado</div>
    <div class="ex-b">
      <div class="passo"><span class="pn">1</span><p>Primeiro passo do raciocínio.</p></div>
      <div class="passo"><span class="pn">2</span><p>Segundo passo, com a conta feita.</p></div>
      <div class="passo"><span class="pn">3</span><p>Conclusão, destacando o resultado.</p></div>
    </div>
  </div>
</div>
```

Regras de estilo:

- **Numeração**: use o próximo número sequencial da matéria. O contador de aulas do cabeçalho é recalculado automaticamente, mas o texto "Aula N" é manual.
- **Títulos**: frase curta, sem ponto final.
- **Explicações**: prefira exemplos resolvidos em passos a blocos longos de texto.
- **HTML permitido**: `<strong>`, `<em>`, `<span class="formula">`, `<span class="vet">a</span>`, `<code>` e entidades (`&lt;`, `&gt;`, `&amp;`).
- **Não repita a palavra "slide"**: o material do site é um resumo próprio, não uma apresentação. Use "exemplo", "observação", "material" etc.
- **Não adicione** o botão "Revisei": ele é criado por JavaScript em todas as `.aula`.

### Figuras

Use SVG inline, sempre com `role="img"` e `aria-label` descritivo:

```html
<figure class="fig">
  <svg viewBox="0 0 320 200" width="320" role="img" aria-label="Triângulo retângulo com catetos 3 e 4 e hipotenusa 5">
    <!-- formas -->
  </svg>
  <figcaption>Legenda que explica o que a figura mostra.</figcaption>
</figure>
```

- Cores principais: `#2450c4` (azul), `#0e8a4e` (verde), `#cf3f3f` (vermelho), `#111b4b` (tinta).
- Evite largura fixa maior que `800`; o CSS já trata a responsividade.
- Não use imagens externas: o site deve continuar funcionando offline.

---

## 2. Adicionar questão ao simulado

O banco fica em `const BANCO=[ ... ]`, no script do `index.html`. Cada linha é um objeto. A área (`ar`) define em quais simulados a questão aparece.

### Múltipla escolha

```js
{t:'mc',ar:'V',e:'Enunciado com <b>destaque</b>.',a:['Alternativa A','Alternativa B','Alternativa C','Alternativa D'],c:1,x:'Explicação da resposta correta.'},
```

- `c` é o índice da correta, começando em **0** (nesse exemplo, a correta é a segunda).
- Use exatamente 4 alternativas para manter o layout.
- Distratores devem ser plausíveis, não absurdos.

### Resposta numérica

```js
{t:'num',ar:'G',e:'Um grafo tem vértices de graus (4, 3, 3, 2). Quantas <b>arestas</b> ele tem?',r:6,tol:.01,u:'',x:'Soma dos graus = 12 = 2|E| → |E| = 6.'},
```

- `r`: resposta numérica (use ponto decimal no código: `11.36`).
- `tol`: tolerância (use `0.01` para inteiros e `0.06` para arredondamentos).
- `u`: unidade exibida (`'N'`, `'u'`, `'anos'` ou `''`).
- O aluno pode digitar vírgula ou ponto.

### Clique no vértice

```js
{t:'clique',ar:'G',e:'Clique no vértice de <b>maior grau</b>.',svg:svgMaior,nome:'C',x:'C tem grau 4.'},
```

- `svg` é uma função que devolve o SVG (defina-a antes do `BANCO`).
- `nome` precisa ser exatamente o `data-nome` do vértice.

### Montar caminho

```js
{t:'caminho',ar:'G',e:'Monte o <b>menor caminho</b> de A até D.',svg:svgCaminho,seq:['A','C','D'],custo:5,x:'A→C→D custa 5.'},
```

### Escolher grafo bipartido

```js
{t:'bip',ar:'G',e:'Qual dos grafos é <b>bipartido</b>?',opcoes:[{t:svgQuad(),ok:true,d:'Grafo 1'},{t:svgTri(),ok:false,d:'Grafo 2'}],x:'O quadrado é bipartido; o triângulo não.'},
```

### Contagens

As contagens de questões por matéria são atualizadas por JavaScript. Se você adicionar uma matéria, veja a seção 5.

---

## 3. Adicionar flashcard

No array `const FLASH=[ ... ]`:

```js
{s:'V',f:'Pergunta curta na frente da carta?',v:'Resposta objetiva no verso, com <b>palavras-chave</b>.'},
```

- `s` é a matéria (`V`, `G`, `S`, `N`).
- A frente deve caber em até ~120 caracteres; o verso, em até ~300.
- Use `<br>` para separar blocos no verso, não parágrafos longos.
- Ao criar uma matéria nova, adicione o botão de filtro (seção 5).

---

## 4. Adicionar questão ao Modo Intensivo

No array `const BANCO_INT=[ ... ]`:

```js
{e:'Enunciado curto, de leitura rápida.',a:['Alternativa A','Alternativa B','Alternativa C','Alternativa D'],c:2},
```

- O modo sorteia 10 questões por rodada e dá 30 segundos por questão: enunciados longos prejudicam a jogabilidade.
- `c` é o índice da alternativa correta (base 0).
- O banco pode ser maior que 10; todas as questões têm chance de aparecer.

---

## 5. Criar uma matéria nova

Suponha uma matéria chamada **Trigonometria**, com sigla `T`.

### 5.1 Seção de teoria

Copie a estrutura de uma seção existente e ajuste o `id`:

```html
<!-- ============ TRIGONOMETRIA ============ -->
<section id="trigonometria" class="tab">
  <div class="divisor">
    <span class="num">05</span>
    <h2>Trigonometria</h2>
    <span class="fim" id="fimTrig">Material próprio + fontes · 0 aulas</span>
  </div>

  <div class="aula">
    <h3><span class="aula-tag">Aula 1</span> ...</h3>
    ...
  </div>
</section>
```

- O `id` deve ser único e sem acentos/espaços (usado pela navegação e pelo `localStorage`).
- O `<span class="num">` é apenas decorativo; ajuste a sequência.

### 5.2 Menu de navegação

Em `<nav class="aba-nav">`:

```html
<li><button data-aba="trigonometria">Trigonometria</button></li>
```

### 5.3 Card na página inicial

Na `<div class="trilha">`:

```html
<a class="tg" href="#trigonometria" data-aba="trigonometria">
  <h3>📐 Trigonometria <span class="hl-soft" style="font-size:.78rem">N aulas</span></h3>
  <p>Razões no triângulo retângulo · ciclo trigonométrico · identidades · equações.</p>
</a>
```

Adicione uma cor de borda no CSS (opcional):

```css
.trilha a.tg{border-left-color:#7a3ec4}
```

### 5.4 Contador de aulas revisadas

Em `atualizaProgresso()` existe uma lista fixa de módulos:

```js
[['vetores','fimVet','VETORES.pdf + extras'],
 ['grafos','fimGraf','Material + Portal OBMEP'],
 ['sistemas','fimSL','Brasil Escola + Toda Matéria + UNIFAL + Wikipédia'],
 ['venn','fimVenn','Toda Matéria + Wikipédia + UNIVESP']]
```

Acrescente a nova linha com `['trigonometria','fimTrig','Material próprio + fontes']`.

### 5.5 Simulado

a) Em `SIM_MODOS`, adicione:

```js
const SIM_MODOS={
  geral:{nome:'Geral',curto:'Simulado'},
  V:{...}, G:{...}, S:{...}, N:{...},
  T:{nome:'Trigonometria',curto:'Trigonometria',ar:'T'}
};
```

b) Em `SIM_ESTADOS`, adicione `T:null`.

c) Em `AREA_LABEL`, adicione `T:'Trigonometria'`.

d) No HTML de `#simOpcoes`, adicione o botão:

```html
<button type="button" class="sim-opcao" data-simmodo="T">
  <span class="rot"><span aria-hidden="true">📐</span> Trigonometria</span>
  <span class="qtd" id="simQtdT">0 questões</span>
</button>
```

e) Atualize os contadores no script:

```js
document.getElementById('simQtdGeral').textContent=BANCO.length+' questões · 5 módulos';
['V','G','S','N','T'].forEach(k=>{ ... });
```

f) Adicione questões ao `BANCO` com `ar:'T'`.

### 5.6 Flashcards

No HTML da aba Flashcards, adicione o botão:

```html
<button class="btn btn-fino" id="fcTrig">📐 Trigonometria</button>
```

E em `fcBtnOn()`:

```js
[['fcTudo','tudo'],['fcVet','V'],['fcGraf','G'],['fcSist','S'],['fcVenn','N'],['fcTrig','T']]
```

Por fim, adicione cartas com `s:'T'` ao `FLASH`.

### 5.7 Modo Intensivo (opcional)

Se quiser questões da nova matéria no jogo, adicione itens ao `BANCO_INT`. Não há identificação de matéria nesse banco — todas as questões concorrem igualmente.

### 5.8 Página de exercícios (opcional)

`exercicios_vetores_grafos.html` é independente e tem estrutura própria. Para uma matéria nova, o mais simples é criar outra página seguindo o mesmo padrão (`type:'mc'`, `'input'`, `'vertex'`, `'path'`, `'bip'`) e linká-la no rodapé do `index.html`.

---

## 6. Checklist final

Antes de abrir o pull request:

- [ ] Abri o site localmente e naveguei por todas as abas.
- [ ] A nova aula aparece, o botão "Revisei" funciona e o contador do módulo atualizou.
- [ ] O simulado da matéria mostra a quantidade certa de questões.
- [ ] Respondi uma questão de cada tipo novo e a correção/explicação está correta.
- [ ] Testei o tema claro e o noturno.
- [ ] Testei em tela estreita (DevTools → 375 px).
- [ ] Não há `console.log` esquecido, HTML quebrado ou acento corrompido.
- [ ] Não adicionei PDFs, imagens protegidas ou dados pessoais.
- [ ] Rodei uma busca por `slide` para manter o vocabulário do site consistente.
- [ ] Atualizei o README se a mudança afetar números, estrutura ou funcionalidades.

### Verificações rápidas no console do navegador

```js
// contagem total e por matéria
BANCO.length
BANCO.filter(q => q.ar === 'T').length

// flashcards por matéria
FLASH.filter(c => c.s === 'T').length

// simulado atual
simModo
```

Se alguma contagem não bater com o que aparece na tela, procure os contadores fixos citados na seção 5.5.

---

Dúvidas ou algo quebrado? Abra uma issue usando os formulários do repositório.
