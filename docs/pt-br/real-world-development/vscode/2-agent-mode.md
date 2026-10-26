---
title: "Exercício 2 - Adicionar funcionalidades com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Mesmo as atualizações mais simples de um aplicativo podem exigir mudanças em vários arquivos e operações como executar testes. Normalmente, a pessoa desenvolvedora localiza os arquivos necessários, faz as alterações, executa testes, depura, identifica arquivos que faltaram e repete o processo. A lista continua.

É aí que entra o Agent Mode do Copilot.

O Agent Mode trabalha com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente problemas encontrados. Vamos usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, permitindo feedback rápido e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o Agent Mode explora o projeto, identifica arquivos relevantes e coordena alterações.
- o Agent Mode implementa recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você criou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o Agent Mode.

## Executar o site Tailspin Toys

Antes de alterar o código, explore o site para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele lista jogos e exibe detalhes. O projeto é uma única aplicação Astro que gera páginas HTML estáticas durante o build. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM; não há uma API de backend separada nem um framework de UI no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um `<script>` Astro pequeno e específico, com APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o único servidor de desenvolvimento Astro. Execute-o no GitHub Codespace:

1. Volte ao codespace. Após o exercício anterior, você deve estar na branch `main`.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um banner com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se deseja abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespaces, selecionar o link localhost no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`. Esse túnel privado disponibiliza o servidor web do codespace.

### Explorar o site

Com o site em execução, explore os recursos principais:

- **Página inicial**: exibe jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá informações como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará as opções necessárias.

O site inicial funciona, mas queremos adicionar recursos. Comece explorando o backlog de issues criado automaticamente quando você configurou o projeto a partir do modelo.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> A autoria deste workshop não indica preferência por um modelo específico. Usamos Claude Sonnet 4.5 ao criar o workshop para tentar manter as sugestões relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes dos exemplos. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente, mas ainda deve concluir a tarefa.

3. Envie ao Copilot este prompt para consultar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou as issues com base no que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. O arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar orientações, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações:

- adicionar ou aperfeiçoar a lógica restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para adicionar a interface de filtragem.

Os testes também precisam passar antes da integração. O Agent Mode pode executar essas tarefas.

1. Continue a conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da exibição Chat.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot explorar o projeto, localizar os helpers de acesso a dados, a interface e os testes, alterar os arquivos e executar os testes.

   ![Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot explorará o projeto, editará arquivos e executará testes. Isso pode levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer cada alteração; não é necessário selecionar esses botões enquanto o agente ainda trabalha.

7. Quando solicitado, selecione **Continue** para executar os testes.
8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.
9. Examine o código gerado em busca de possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você explorou como usar o Agent Mode do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o Agent Mode implementa recursos na interface e na camada de dados.
- o Agent Mode explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar as alterações e os testes gerados antes de integrá-los à base de código.

Agora, você [testará o recurso com o servidor MCP do Playwright][next-lesson] e abrirá um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use o que aprendeu para pedir ao Copilot que implemente esse recurso. Considere:

- seguir as boas práticas existentes, incluindo os arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias para a implementação desejada.
- iterar, solicitar alterações e fornecer contexto adicional. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
