---
title: "Genesys Developer Center"
chapter: false
weight: 10
locale: "pt-BR"
---

> **O Genesys Cloud Developer Center contém vários recursos para desenvolvedores, como exemplos de código, blueprints, SDKs e, como veremos em breve, um robusto explorador de API.**





## Identifique e execute sua chamada de API

Nesta seção, encontraremos e executaremos uma chamada para encontrar a presença de todos os usuários em uma determinada fila.

Depois de navegar e fazer login no **[Developer Center](https://developer.genesys.cloud/)**, selecione o bloco API Explorer exibido abaixo.

![image](/images/devcenter.PNG)

O Explorador de API (API Explorer) permite que você filtre e pesquise em todos os endpoints de API acessíveis a chamada que está procurando e execute essa chamada sem exigir uma ação de dados totalmente configurada.

Começaremos filtrando as APIs que achamos que podem conter as informações que procuramos.
>**Pode ser necessário trabalhar com chamadas de API que você acredita serem relevantes e executá-las para determinar qual endpooint fornece as informações de que você precisa. Também pode haver inúmeras chamadas que fornecem os dados que você está procurando. Neste workshop vamos navegar diretamente para a chamada que precisamos.**

1. No lado direito, limpe todas as categorias e adicione a categoria “Roteamento” (Routing) de volta ao filtro.
2. Pesquise por “Fila” (Queue) na caixa de pesquisa.
3. Marque a caixa “GET” para filtrar apenas solicitações GET.

![image](/images/explorerfilter.PNG)

Depois de encontrarmos nossa chamada, neste caso **/api/v2/routing/queues/{queueId}/members**, precisamos configurá-la antes da execução.

1. Desative o modo de leitura, isso nos permitirá executar diretamente do centro do desenvolvedor
2. Insira seu id da fila (queueId)
>**LA maioria dos componentes de configuração no Genesys Cloud recebe um GUID ou identificador globalmente exclusivo. Ao navegar para a fila do painel de administração, você pode encontrar o GUID na URL.**

![image](/images/queueguid.PNG)
**Salve este GUID de fila em um bloco de notas para referência posterior.**

3. Expanda a chamada para exibir informações da variável Presença (Presence)
>**Algumas chamadas ocultam informações adicionais por padrão, mas podem ser expandidas ou filtradas para adicionar ou remover informações.**

![image](/images/explorerconfig.PNG)

Agora que nossa chamada foi configurada, vamos executar a chamada e verificar se as informações que procuramos estão contidas no corpo da resposta.
>**Sabemos que encontramos a chamada correta porque podemos ver a presença do usuário no corpo da resposta.**

![image](/images/explorerexecute.PNG)

