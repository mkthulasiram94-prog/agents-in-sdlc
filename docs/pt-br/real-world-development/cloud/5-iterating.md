---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar recursos. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Agora, revise as alterações e, se necessário, dê feedback ao Copilot para melhorar o resultado.

### Cenário

Os fundamentos do design de software e das práticas de desenvolvimento e operações (DevOps) não mudam com a inclusão da inteligência artificial (IA) generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Com isso em mente, revise as propostas do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à ramificação que usará para o código.
- O agente de nuvem executa o trabalho no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar os fluxos de trabalho do GitHub Actions antes da execução.
- O [acesso a recursos externos é limitado por padrão][agent-firewall], inclusive para servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o que adiciona documentação ao código. Você fará essa revisão usando a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**: por padrão, o Copilot tem acesso limitado a recursos externos, inclusive a chamadas de servidores MCP. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite o intervalo para descansar ou refletir sobre o que já aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui novos comentários TSDoc (documentação de código para TypeScript) e outra documentação. As alterações exatas podem variar.

   Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Confirme se os fluxos de trabalho foram adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencioná-lo em comentários do pull request ou em comentários sobre linhas de código, como faria com outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único a recomendar. Algumas ideias de solicitações:

- Adicione no início de cada arquivo de código um cabeçalho com uma breve descrição de sua finalidade.
- Adicione comentários TSDoc a arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou solicite outra melhoria na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Opção Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, procure um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

6. Se ainda não estiver satisfeito, continue iterando. Quando a revisão estiver concluída, marque o PR de rascunho como pronto para revisão e faça o merge na ramificação principal.

![Opção para marcar um PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para resolver o problema sobre a exibição de jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Confirme se os fluxos de trabalho foram adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** mude para essa ramificação no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir. Substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O agente de nuvem também permite iterar sobre o recurso de jogos relacionados e solicitar atualizações. Por exemplo, você pode pedir uma alteração no número de jogos exibidos ou solicitar cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Um comentário na guia **Conversation** inicia uma nova sessão do Copilot.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no Mission Control no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre modos;
   - uso do armazenamento local para manter as preferências;
   - alterações de folhas de estilo em cascata (CSS) ou de outros estilos para os modos de alto contraste e claro;
   - atributos Accessible Rich Internet Applications (ARIA), como rótulos e navegação por teclado;
   - código JavaScript ou TypeScript responsável pela alternância entre modos.
6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Confirme se os fluxos de trabalho foram adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para essa ramificação no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir. Substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências continuam salvas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot para implementar os recursos seguindo práticas de acessibilidade. Se encontrar problemas ou melhorias possíveis, mencione **@copilot** em um comentário para solicitar alterações, como fez nos outros PRs.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode atribuir itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais alguns problemas no repositório e atribua-os ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na página de listagem de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o ambiente do agente de nuvem. Ao longo destas lições, você:

- **inspecionou os arquivos de instruções personalizadas do repositório** para observar seus efeitos no trabalho do agente de nuvem;
- **atribuiu problemas ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona;
- **criou e usou um agente personalizado** para acessibilidade, com alternâncias para os modos de alto contraste e claro;
- **usou a página de agentes do Copilot como Mission Control** para acompanhar e direcionar a sessão de acessibilidade;
- **revisou os pull requests do agente de nuvem e iterou sobre eles**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu o ambiente do agente de nuvem. Para continuar explorando, os outros ambientes complementam o que praticou:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Ambiente do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem um problema de refatoração ou cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir problemas do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
