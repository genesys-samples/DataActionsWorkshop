---
title: "Construção da Ação de Dados"
chapter: false
weight: 40
locale: "pt-BR"
---


Neste ponto, você identificou sua chamada de API e a analisou para os dados que está tentando recuperar, agora precisamos amarrar tudo em uma ação de dados que pode ser usada nos fluxos ou scripts do Architect.

Começaremos navegando para Administrador > Ações e criando uma nova ação de dados no tipo de integração que você criou.

![image](/images/DAinputcontract.PNG)

Variável de campo de contrato de entrada - 
```
QueueID
```

Você então definirá seu contrato de saída, embora existam várias maneiras de construir contratos de saída para permitir dados adicionais, você construirá um objeto que é uma matriz de strings.

![image](/images/DAoutputcontract.PNG)

Variáveis de campo do contrato de saída - 
```
OutputPresence
```
```
Presences
```
```
Presence
```

## Configuração
Na guia de configuração, começaremos definindo nosso modelo de URL de solicitação usando o URL da API que reunimos no centro do desenvolvedor; nesse caso, parecerá -
```
/api/v2/routing/queues/INSERTQUEUEGUIDHERE/members?expand=presence
```
Precisaremos usar nossas entradas disponíveis para substituir o GUID da fila estática pela variável de entrada que criamos de QueueID. Você pode fazer isso destacando o GUID da fila e clicando na entrada "QueueID (String)" no lado direito da tela.

![image](/images/DAconfiginput.PNG)

O campo de resposta é onde construiremos nosso mapa de tradução para analisar (parse) a resposta, os padrões do mapa de tradução para lidar graciosamente com respostas vazias e modelos de sucesso para mapear nossa tradução para nossa variável de saída. Abaixo está um exemplo de código de uma configuração de resposta.

```
{
  "translationMap": {
    "Presence": "$.entities[*].user.presence.presenceDefinition.systemPresence"
  },
  "translationMapDefaults": {
    "Presence": "null"
  },
  "successTemplate": "{\"Presences\" : $Presence}"
}
```

Dentro da seção do mapa de tradução, temos um par de chave-valor de presença e o caminho para a matriz de presenças do usuário que descobrimos anteriormente. 

O modelo de sucesso é configurado para mapear o valor do par de valores-chave 'Presence' para a matriz de saída que construímos na área do contrato.

Agora podemos testar essa configuração para garantir que a ação de dados esteja funcionando!

## Testando

Ao inserir manualmente um QueueID, podemos realizar um teste em nossa configuração para garantir que os dados estejam retornando conforme o esperado.
Na imagem abaixo, podemos ver uma execução bem-sucedida com nossos valores de presença mapeados corretamente para nossa matriz de saída.

![image](/images/DAtest.PNG)

Se tivéssemos cometido um erro de digitação ou erro na configuração, o teste retornaria o erro e a etapa do processo de execução para nos auxiliar na solução de problemas. Nos exemplos abaixo, coloquei um erro de digitação no valor do meu par de valores de chave de presença, criando uma falha na etapa 10 do processo REST.

![image](/images/DAtestfailure1.PNG)

Navegando até a etapa 10, podemos ver uma análise do erro real
```
{
  "message": "Transform failed to process result using 'successTemplate' template due to error:'Variable $Presences has not been set at successTemplate[line 1, column 20]'\n Template:'{\"PresenceArray\" : $Presences}'.",
  "code": "bad.request",
  "status": 400,
  "messageParams": {},
  "contextId": "fc7c43df-fb93-4651-83b2-2dc942e623d7",
  "details": [
    {
      "errorCode": "ACTION.PROCESSING"
    }
  ],
  "errors": []
}
```
Este exemplo afirma "$Presences não foi definido no modelo de sucesso", ajudando-me a identificar que tenho uma incompatibilidade entre meu mapa de tradução e o modelo de sucesso. No exemplo abaixo, a linha 8 tem presenças (Presences) no plural, enquanto a linha 3 tem singular.

![image](/images/DAtestfailure2.PNG)

Quando os testes são aprovados e executados conforme o esperado, você pode publicar sua ação de dados para uso em fluxos ou scripts