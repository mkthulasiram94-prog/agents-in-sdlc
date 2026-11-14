---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar filtros a partir de um problema e o Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um recurso pequeno ao projeto. Mudanças maiores exigem um processo mais robusto. O GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho da organização e ajudar a desenvolver o que é necessário da maneira adequada. Nesta e nas próximas lições, você seguirá um processo de desenvolvimento orientado por agentes: começará com um problema, verificará os requisitos, validará o código e o comportamento do recurso e, por fim, fará o merge no projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho do recurso usando a mesma conversa e a mesma ramificação. Em geral, seriam usadas ramificações ou pull requests diferentes para cada tipo de arquivo, mas neste workshop vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de um problema do GitHub;
- definir requisitos no modo Plan;
- implementar o recurso no modo Autopilot;
- revisar o código;
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e publicador. O problema no backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programar. Você usará o modo Plan para decidir esses detalhes e, em seguida, autorizará o Autopilot a implementar uma tarefa delimitada.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos; na verdade, torna-os ainda mais importantes. Muitos desenvolvedores seguem um processo parecido com este:

1. Começar com um problema registrado que descreve o que precisa ser feito.
2. Criar um plano do que será desenvolvido.
3. Implementar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma abordagem semelhante.

Ao seguir esse processo padrão, você garante que o código gerado por IA atenda aos requisitos e passe pela mesma revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan e revise o plano. Em seguida, use o Autopilot para implementá-lo.

## Comece pelo problema

Antes de começar a implementar os filtros, volte ao Codespace e prepare o repositório e o terminal.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR de avaliações com estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma ramificação para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os problemas.
8. Use as setas para destacar o problema intitulado **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o problema ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto um problema ou pull request do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem antes de pedir ao Copilot que implemente um recurso ou execute uma tarefa. Para tarefas complexas, é recomendável reservar um tempo para planejar. Agora, alterne para o modo Plan e peça ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência ao problema adicionada na etapa anterior, insira o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a explorar o projeto e a definir a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Autorize o Autopilot

Com o plano escrito e revisado, implemente-o usando o Autopilot. Esse modo permite que o Copilot itere até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme se o indicador abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes de aceitá-lo. Se o Copilot indicar dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de fazer o merge, como faria com qualquer outro código. Analise as alterações e execute o site para conferir o resultado.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador de modo não exibe **Plan** nem **Autopilot**.
2. Insira `/diff` e revise a implementação dos filtros e os testes.
3. Compare o resultado com o problema e com as decisões tomadas durante o planejamento.
4. Quando terminar a revisão do código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explore o novo recurso

O código parece correto, mas será que funciona? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por publicador e as combinações definidas no plano.
4. Confirme se a redefinição dos filtros e os resultados vazios correspondem ao problema e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado por ele.

## Resumo e próxima etapa

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um problema do GitHub;
- definiu requisitos no modo Plan;
- implementou o recurso no modo Autopilot;
- revisou o código;
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, descubra como o código é gerado e como garantir que ele siga as práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
