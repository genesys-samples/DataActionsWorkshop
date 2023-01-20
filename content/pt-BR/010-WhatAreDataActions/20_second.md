---
title: "Definição dos componentes"
chapter: false
weight: 20
locale: "pt-BR"
---

## Componentes das Ações de Dados - Data Actions

LAs ações de dados podem ser divididas nos próximos 4 componentes

**1. Contratos de entrada - Input Contracts**

**2. Contratos de saída - Output Contracts**

**3. URLs de pedido - Request URLs**

**4. Corpos da resposta - Response Bodies**

### Contratos de entrada
Os contratos de entrada são variáveis ou listas de variáveis que fornecemos na chamada REST para informar à API qual dado específico estamos tentando invocar.
   * Exemplo: estamos tratando de recuperar daddos de presença de usuário de uma fila específica, construimos uma variaável de QueueID para para inserir em nossa chamada de API, permitindo-nos fornecer dinamicamente um novo ID de fila em cada chamada.

> **Nem todas as chamadas de API exigem um contrato de entrada definido e as entradas podem ser atribuídas estaticamente.**

Na imagem abaixo, criamos um objeto com uma string de entrada QueueID, essa entrada é atribuída na URL de solicitação para permitir que um valor de ID de fila dinâmico seja atribuído a partir de um fluxo do Architect. Como alternativa, se o valor do ID da fila não precisar ser alterado, ele pode ser atribuído estaticamente.

![image](/images/inputcontracts.PNG)

### Contratos de saída
Em uma resposta de API, podemos receber centenas de linhas de dados, mas nos preocupamos apenas com uma única linha. 
Os Contratos de saída são onde definimos quais dados da chamada queremos executar.

  * Exemplo: Efetuamos uma chamada para nos fornecer todos os detalhes da fila, só precisamos ver a presença do agente, então definimos uma variável de Presença para armazenar essa informação no final da chamada.

  >**Nem todas as chamadas de API requerem um contrato de saída definido; Algumas chamadas podem não retornar dados que desejamos referenciar.**

Na imagem abaixo, construímos nosso contrato de saída e construímos apenas **Presence** (Presença) como uma variável de saída. No corpo da resposta definido posteriormente, podemos analisar a resposta quanto à presença e armazenar apenas esse valor em nossa única variável de saída.

![image](/images/outputcontracts.PNG)

### URLs de pedido
Os URLs de pedido são a chamada real que está sendo executada e contêm o caminho de API específico que estamos invocando. Em nosso construtor de ação de dados, algumas chamadas podem ser definidas na visualização simples, onde definimos apenas a URL e as entradas, enquanto outras chamadas podem exigir predicados adicionais e serem formatadas no campo JSON.

Na imagem abaixo podemos ver os templates de requisição **Simples** e **JSON**. Também podemos ver a variável de entrada QueueID atribuída em ambos os formatos do pedido

![image](/images/requesturls.PNG)

### Corpos da resposta

O corpo da resposta define quais informações da chamada estamos tentando retornar e pode ser dividida em 3 componentes. 
  * Os  mapas de tradução (Translation Maps) nos permite analisar e mapear dados da API Response para variáveis.
  * Os valores padrões (Translatin Maps Default) nos permite definir valores padrão caso nenhuma informação seja retornada.
  * Os modelos de sucesso (Success Templates) nos permitem mapear os dados analisados para as variáveis do contrato de saída que construímos para que os dados possam ser utilizados para roteamento, script, etc.

Na imagem abaixo, construimos -
1. Um mapa de tradução com um par de valor-chave de 'Presença' e o caminho usado para analisar a resposta da API apenas para informações de presença. 
2. Definimos um mapa de tradução para aplicar um valor padrão para 'Presença' de "NOT_SET" no caso de não retornarmos nenhum dado. Isso permitirá que a solicitação falhe normalmente se nenhuma resposta for retornada.
3. Mapeou o valor de 'Presence' de nosso mapa de tradução, para nossa variável de saída *<ins>'Presences<ins>*.

![image](/images/responsebodies.PNG)

### Ferramenta de teste de Ações de Dados 
O construtor de Ações de Dados (Data Action Constructor) fornece uma ferramenta de teste que permite testar chamadas e validar se elas estão configuradas corretamente antes de implantá-las. A ferramenta de teste também contém uma sequência de operações para mostrar onde no processo RESTful a chamada está falhando para facilitar a solução de problemas.

Na imagem abaixo, podemos ver que o teste falhou ao aplicar a transformação de saída, indicando que o mapa de tradução deve ser o ponto de partida para nossa solução de problemas


![image](/images/testtool.PNG)