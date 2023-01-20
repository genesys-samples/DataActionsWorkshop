---
title: "Ações de Dados dentro dos fluxos do Architect"
chapter: false
weight: 50
locale: "pt-BR"
---


Com nosso Data Action testado e validado, o último passo é realmente utilizá-lo! Para esta seção, criaremos um fluxo de chamada de entrada básico que chama nossa ação de dados navegando para Administrador > Arquiteto > Fluxos de chamada de entrada > + Adicionar > Criar Fluxo.

![image](/images/ArchitectFlowCreate.PNG)

Dentro de nosso fluxo, criaremos uma tarefa reutilizável, que é a 'tela em branco' do roteamento e permite uma lógica mais avançada do que um menu padrão. Selecione Adicionar tarefa reutilizável aqui > Caixa de ferramentas > Tarefa.

![image](/images/architectreusable.PNG)

Selecione os 3 pontos ao lado da tarefa e atribua-a como a tarefa inicial. Isso significa que todas as chamadas que entrarem no fluxo começarão com esta tarefa.

![image](/images/architectsetstart.PNG)

Na caixa de ferramentas, adicionaremos os 5 itens a seguir nesta ordem -

1. (Call Data Action) Ação de dados de chamada - logo no início
2. (Decision) Decisão - Dentro do caminho de sucesso da ação de dados
3. (Transfer to ACD) Transferência para ACD - Dentro do caminho **Sim** da Decisão
4. (Transfer to Voicemail) Transferência para correio de voz - Dentro do caminho **Não** da Decisão
5. Uma desconexão bem no fundo do fluxo

Quando terminar, seu fluxo deve se parecer com a imagem abaixo -

![image](/images/architectflowoutline.PNG)

Vamos agora configurar cada um desses elementos. Em "Chamar Ações de Dados", selecione seu tipo de integração (provavelmente "Genesys Cloud Data Actions") e encontre sua ação de dados pelo nome.

Forneceremos o ID da fila para a ação de dados alterando a entrada QueueID para "Literal" e colando em nosso GUID da fila.
O último item é definir a saída para uma variável de fluxo para referência em nossa decisão, por exemplo **Flow.presences**. Quando concluída, sua configuração de ação de dados deve se parecer com - 

![image](/images/architectdataaction.PNG)

O próximo item a configurar é a **Decisão (Decision)**, que é onde aplicaremos a lógica em relação à lista retornada de presenças de usuários na fila. Altere a expressão de booleano para expressão -

![image](/images/architectdecision.PNG)

Cole o trecho de código a seguir no campo de expressão - 

```
Contains(ToString(Flow.presences), "On Queue")
```

Este é um bloco lógico muito simplista que converte nosso Array ou matriz de presença (Presence Array) em uma única string e verifica se há algum agente aparecendo como na FIla (**On Queue**).

Apontaremos agora os blocos Transferir para ACD (Transfer to ACD) e Trasnferir para Correio de Voz (Transfer to Voicemail) para a fila usada na pesquisa de ação de dados. Depois de definir isso, seu fluxo está pronto para ser publicado e deve se parecer com - 

![image](/images/architectfinalflow.PNG)

Você pode testar essa lógica entrando ou saindo da fila e chamando esse fluxo.