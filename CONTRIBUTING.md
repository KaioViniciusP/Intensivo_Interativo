# Como contribuir

Obrigado pelo interesse! Este repositório é aberto para quem quiser **estudar**, **corrigir** ou **ampliar** o material.

## Formas de contribuir

- **Reportar um erro** de conteúdo, conta, gabarito ou português.
- **Sugerir uma matéria** ou uma funcionalidade nova.
- **Melhorar acessibilidade**, responsividade ou desempenho.
- **Enviar questões, aulas ou flashcards** seguindo o padrão do site.
- **Compartilhar** o site com quem está estudando.

## Antes de abrir uma issue

1. Verifique se já existe uma issue semelhante.
2. Para erro de conteúdo, informe **módulo, aula/questão e o trecho exato**, e qual seria a correção.
3. Se possível, anexe um print — mas nunca dados pessoais.

Há formulários prontos em **Issues → New issue**.

## Fluxo de contribuição

```bash
# 1. Fork no GitHub, depois clone o seu fork
git clone https://github.com/SEU-USUARIO/treino.git
cd treino

# 2. Crie uma branch descritiva
git checkout -b fix/gabarito-sistemas-aula-5
# ou: feat/modulo-trigonometria

# 3. Faça as mudanças e teste localmente
python -m http.server 8000

# 4. Commit
git add .
git commit -m "Corrige gabarito do sistema da Aula 5"

# 5. Push e pull request
git push origin fix/gabarito-sistemas-aula-5
```

Abra o pull request contra a branch `main` e descreva **o que mudou e por quê**. Se a mudança for visual, inclua um print do antes/depois.

## Padrões do projeto

- **Português (Brasil)** na interface, nos comentários de conteúdo e nas mensagens de commit.
- **HTML/CSS/JS puros**, sem frameworks, bundlers ou dependências novas de runtime.
- **SVG inline** para figuras; nada de imagens externas.
- **Sem a palavra "slide"** no conteúdo: o site é um resumo próprio. Use "exemplo", "material", "observação".
- **Sem PDFs** ou cópias integrais de materiais de terceiros. Resuma e cite a fonte.
- **Sem dados pessoais**, analytics ou chamadas de rede além das fontes já usadas (Google Fonts).

### Estilo de código

- Mantenha a indentação e o estilo do arquivo em volta.
- Use `const`/`let`; não introduza `var` no `index.html`.
- Preserve o padrão de nomes em português (`irPara`, `aulasVistas`, `BANCO`).
- Não reformate blocos inteiros sem necessidade: diffs pequenos são mais fáceis de revisar.

### Conteúdo

- Cada aula deve ter uma ideia central, um exemplo resolvido e, quando fizer sentido, uma observação de erro comum.
- Questões de múltipla escolha: 4 alternativas, distratores plausíveis, explicação obrigatória (`x`).
- Questões numéricas: informe unidade, tolerância e arredondamento.
- Novas matérias: siga o passo a passo de **[docs/ADICIONAR-CONTEUDO.md](docs/ADICIONAR-CONTEUDO.md)**.

## Checklist do pull request

- [ ] O site abre sem erros no console.
- [ ] Naveguei por todas as abas afetadas.
- [ ] Testei tema claro e noturno.
- [ ] Testei em tela estreita (~375 px).
- [ ] Contagens e gabaritos conferem.
- [ ] Não incluí PDFs, imagens protegidas ou dados pessoais.
- [ ] A descrição do PR explica a mudança.

## Licença

Ao contribuir, você concorda que sua contribuição será publicada sob a [licença MIT](LICENSE) para o código e sob os mesmos termos de reutilização com atribuição para o conteúdo autoral. Materiais de terceiros continuam pertencendo aos seus autores.
