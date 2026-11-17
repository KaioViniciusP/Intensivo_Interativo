# Intensivo Interativo

> Caderno de estudos interativo de **Vetores**, **Teoria dos Grafos**, **Sistemas Lineares** e **Diagrama de Venn** — teoria organizada, ferramentas que desenham e calculam, flashcards e simulados com correção na hora.

[![Site online](https://img.shields.io/badge/site-online-0e8a4e)](https://kaioviniciusp.github.io/treino/)
[![GitHub Pages](https://img.shields.io/badge/hospedado-GitHub%20Pages-2450c4)](https://kaioviniciusp.github.io/treino/)
[![Licença MIT](https://img.shields.io/badge/licen%C3%A7a-MIT-ffd60a)](LICENSE)

O projeto nasceu como material de revisão para uma prova e virou um repositório aberto: **qualquer pessoa pode estudar, reutilizar em sala de aula, corrigir um erro ou adicionar uma matéria nova**.

---

## 🌐 Acesse

- **Site:** <https://kaioviniciusp.github.io/treino/>
- **Repositório:** <https://github.com/KaioViniciusP/treino>
- **Treino extra (Vetores e Grafos):** <https://kaioviniciusp.github.io/treino/exercicios_vetores_grafos.html>

Não precisa instalar nada: o site roda inteiro no navegador e funciona offline depois de carregado (exceto as fontes do Google).

---

## 📚 O que tem no site

| Módulo | Aulas | Conteúdo principal |
| --- | :---: | --- |
| ➡️ **Vetores** | 12 | Módulo, direção e sentido · soma e subtração · Pitágoras e Lei dos Cossenos · decomposição · notação cartesiana · produto escalar e ângulo entre vetores |
| 🔗 **Teoria dos Grafos** | 15 | Königsberg e Euler · vértices, arestas e graus · caminhos e ciclos · menor caminho · bipartidos · árvores e florestas · conexidade e paridade |
| 🧮 **Sistemas Lineares** | 12 | Equações e sistemas · SPD/SPI/SI · substituição, adição e comparação · matriz e determinante · Regra de Cramer · escalonamento · homogêneos |
| 🔵 **Diagrama de Venn** | 9 | Linguagem dos conjuntos · união, interseção e diferença · diagramas de 2 e 3 conjuntos · inclusão-exclusão · De Morgan |

**Total:** 48 aulas, 64 questões de simulado (16 por módulo), 39 flashcards e 48 questões no banco do Modo Intensivo.

### Recursos

- **Teoria aula por aula**, com figuras em SVG, exemplos resolvidos passo a passo e checklist de aulas concluídas.
- **Ferramentas interativas**
  - Construtor de grafos com **Dijkstra** (menor caminho) e **Kruskal** (árvore geradora mínima).
  - Visualizador de vetores: soma, módulos, produto escalar e ângulo.
  - Laboratório de decomposição vetorial: `Ax = A·cos θ` e `Ay = A·sen θ` em tempo real.
  - Jogo de colorir grafos bipartidos.
- **Treino**
  - **Flashcards** com filtro por matéria e marcação de cartas difíceis.
  - **Simulado** por matéria ou geral, com correção imediata e explicação.
  - **Modo Intensivo**: 10 questões, 30 segundos cada, com pontuação e bônus de tempo.
- **Tema claro e noturno**, layout responsivo e navegação por teclado.
- **Progresso salvo no próprio navegador** (`localStorage`) — sem cadastro, sem servidor, sem envio de dados.
- **Página extra de exercícios** com 20 questões de Vetores e Grafos (múltipla escolha, resposta numérica, cliques no grafo e montagem de caminhos).

---

## 🚀 Como estudar

1. **Leia as aulas** do módulo escolhido e marque cada uma como concluída ao terminar.
2. **Use as ferramentas** para visualizar o que a teoria descreve (arraste vértices, mude o ângulo, rode o Dijkstra).
3. **Vire os flashcards** na véspera da prova, marcando o que ainda não sabe.
4. **Faça o simulado** e confira as explicações das questões erradas.
5. **Enfrente o Modo Intensivo** para ganhar velocidade.

O progresso fica salvo no navegador usado. Em outro dispositivo, comece do zero (não há conta).

---

## 🛠️ Rodar localmente

Como é um site estático, basta abrir o `index.html` no navegador. Para evitar diferenças de caminho e ter recarregamento automático, prefira um servidor local:

```bash
# Python 3
python -m http.server 8000

# ou com Node.js
npx serve .
```

Depois acesse <http://localhost:8000>.

Para publicar, é só enviar os arquivos para a branch `main` — o GitHub Pages está configurado para servir a raiz do repositório (`main` / `/`).

---

## 📁 Estrutura

```text
treino/
├── index.html                      # site principal (teoria, ferramentas, flashcards, simulado)
├── exercicios_vetores_grafos.html  # banco extra de 20 exercícios interativos
├── README.md                       # esta documentação
├── CONTRIBUTING.md                 # como contribuir
├── LICENSE                         # licença MIT (código)
├── docs/
│   ├── ARQUITETURA.md              # como o site é construído por dentro
│   └── ADICIONAR-CONTEUDO.md       # passo a passo para aulas, questões e matérias novas
└── .github/
    ├── ISSUE_TEMPLATE/             # formulários de erro de conteúdo e sugestão de matéria
    └── PULL_REQUEST_TEMPLATE.md
```

> **Importante:** os PDFs originais (`VETORES.pdf`, `TEORIA DOS GRAFOS.pdf`) **não** são publicados no repositório por direitos autorais — o `.gitignore` bloqueia `*.pdf`. O site contém apenas o resumo de estudo produzido a partir deles e das fontes citadas.

---

## 🧩 Adicionar uma matéria nova

O site foi feito para crescer. O guia completo, com trechos de código prontos, está em **[docs/ADICIONAR-CONTEUDO.md](docs/ADICIONAR-CONTEUDO.md)**.

Em resumo, uma nova matéria exige:

1. Uma `<section class="tab" id="sua-materia">` com as aulas (o mesmo padrão visual das existentes).
2. Um botão no menu `aba-nav` e um card na trilha da página inicial.
3. Uma entrada em `SIM_MODOS` / `SIM_ESTADOS` e questões no `BANCO` com a nova sigla.
4. Cartas no `FLASH` e, se quiser, questões no `BANCO_INT`.
5. Atualização dos contadores exibidos na tela.

O guia mostra exatamente onde fica cada bloco e como testar.

---

## 🏗️ Tecnologia

- **HTML, CSS e JavaScript puros** — um único arquivo principal, sem build, sem framework e sem dependências de runtime.
- **SVG** gerado dinamicamente para grafos, vetores e diagramas de Venn.
- **GitHub Pages** para hospedagem estática.
- Fontes do Google: Bricolage Grotesque, Archivo e JetBrains Mono.

Detalhes de arquitetura, esquemas de dados e chaves de armazenamento: **[docs/ARQUITETURA.md](docs/ARQUITETURA.md)**.

---

## 🔒 Privacidade

O site não tem back-end, analytics ou login. Todo o progresso (aba atual, tema, aulas vistas, simulado e flashcards difíceis) fica no `localStorage` do navegador. Limpar os dados do site apaga esse progresso.

---

## 📖 Fontes e créditos

O conteúdo didático foi organizado a partir de:

- `VETORES.pdf` (material de aula).
- Material de Teoria dos Grafos (Aulas 1–8) e temas extras inspirados no **Portal OBMEP/IMPA**.
- **Brasil Escola**, **Toda Matéria**, **UNIFAL-MG**, **Wikipédia** e **UNIVESP** para os módulos de Sistemas Lineares e Diagrama de Venn.

Os links para as fontes externas aparecem no fim dos módulos correspondentes. Marcas, textos e materiais de terceiros pertencem aos seus autores; este repositório reúne resumos e exercícios autorais para fins de estudo.

---

## 🤝 Contribuindo

Contribuições são bem-vindas — correções de conteúdo, melhorias de acessibilidade, novas questões ou módulos inteiros.

1. Abra uma **issue** descrevendo o erro ou a sugestão (há formulários prontos).
2. Faça um **fork** e crie uma branch: `git checkout -b feat/nova-materia`.
3. Siga o **[CONTRIBUTING.md](CONTRIBUTING.md)** e teste no navegador.
4. Envie o **pull request** explicando o que mudou.

Não envie PDFs, materiais protegidos por direitos autorais nem dados pessoais.

---

## 🗺️ Roadmap

- [ ] Novos módulos (ex.: Trigonometria, Geometria Analítica, Probabilidade).
- [ ] Exportar/importar o progresso entre navegadores.
- [ ] Banco de questões comunitário separado do código.
- [ ] Testes automatizados dos bancos de questões (gabarito e contagens).
- [ ] Versão com URL por matéria (`#vetores`, `#grafos`…) para compartilhar links diretos.

Tem uma ideia? Abra uma issue.

---

## 📄 Licença

O **código** deste repositório está sob a [licença MIT](LICENSE): use, copie, adapte e compartilhe, mantendo o aviso de copyright.

O **conteúdo didático autoral** (resumos, exercícios e explicações produzidos para este projeto) pode ser reutilizado com atribuição ao repositório. Materiais de terceiros citados ou linkados seguem as licenças dos respectivos autores.
