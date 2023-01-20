---
title: "Analisando Respostas da API"
chapter: false
weight: 20
locale: "pt-BR"
---

## Entenda os caminhos JSON (JSON Path)
As respostas JSON são séries de objetos e matrizes em uma estrutura hierárquica. Você começa com um objeto base, no exemplo abaixo, o objeto base será “Thestore”. Nosso exemplo “Thestore” vende uma variedade de diferentes alimentos e talheres. Se fizermos uma consulta ao inventário da “Thestore” receberemos uma resposta contendo todos os objetos que a “Thestore” vende e quaisquer atributos associados a esses objetos, como a sua categoria (frutas, legumes, etc.) e seus preços.

![image](/images/storeexample.PNG)

E se nos importarmos apenas com as frutas que a “Thestore” vende? Se estamos em um orçamento de compras limitado e só queremos ver o que a loja tem por menos de 2 dolares? Definir um JSON Path contra a resposta nos permitirá isolar apenas os itens com os quais estamos preocupados.

Um exemplo de caminho JSON para encontrar toda a comida que “Thestore” oferece seria semelhante a -
```
$.Thestore.food[*]
```
**'$'** - Diz para começar no objeto raiz de "Thestore".

**'.food'** - Indica para entrar no próximo objeto ou array ("comida") dentro de "Thestore".

**'[*]'** - é uma notação de posição de array, informando para retornar todos os objetos dentro do array food, isso pode ser alterado para **'[0]'** para retornar apenas o primeiro objeto dentro deste array. 
>**Nota: arrays na maioria dos idiomas indexam, ou começam, em 0, com 0 sendo o primeiro item.**

Um exemplo de caminho (path) que retorna apenas objetos abaixo de $ 2,00 dolares - 
```
$.Thestore.food[?(@.price<2.00)]
```
A única diferença aqui é que não estamos mais definindo uma posição de array e agora aplicamos um filtro **' [?(@.price<2.50)] '** que mira la clave de precio y filtra sólo los objetos que son inferiores a 2,50 dólares.

## Analisando o corpo da resposta

Agora que você encontrou uma chamada de API que retorna os dados que precisamos, precisamos determinar o caminho que retornará apenas as informações de presença (Presence) que procuramos.
>**Como você viu na resposta no centro do desenvolvedor (Developer Center), muito parecido com nosso exemplo de consulta de loja acima, essa chamada específica retornou muitas linhas de dados que são irrelevantes para nosso objetivo de verificar a presença na fila. Dependendo da chamada específica, ou para esta chamada - quantos membros estão na fila, você pode retornar milhares de linhas de dados.**

Existem muitas ferramentas externas que podem ajudá-lo a analisar as respostas JSON para os dados de que você precisa. Embora a Genesys não tenha ferramentas preferenciais para executar essas ações, abaixo estão algumas ferramentas acessíveis ao público que serão usadas para executar essas ações para este workshop. 
>**Essas ferramentas não são necessárias, mas podem ajudar durante o aprendizado da análise sintática (Parsing) do JSON.**

  * https://goessner.net/articles/JsonPath/index.html#e2 - Pode fornecer definições de funções JSON e exemplos de análise.

  * https://jsonpathfinder.com/ - Pode ajudar a encontrar caminhos para objetos específicos nas respostas JSON

  * http://jsonpath.com/ - Pode fornecer representações visuais de quais dados serão retornados de caminhos específicos.

Ao colar a resposta do nosso centro de desenvolvimento (developer center) em ferramentas como o localizador de caminho, você pode ver uma divisão/hierarquia visual dos objetos e arrays na resposta JSON.

No exemplo abaixo, podemos ver que a ferramenta Path Finder identificou 2 entidades em nossa resposta JSON, que se correlacionam com os 2 usuários que temos na fila

![image](/images/pathfinder1.PNG)

Ao expandir os campos abaixo de uma das entidades, podemos eventualmente encontrar o campo de presença. Ao selecionar o campo de presença, vemos que a ferramenta preencheu um “Caminho” para este campo para nós.

![image](/images/pathfinder2.PNG)

Agora que temos nosso caminho, é importante visualizar os dados e filtrar ou expandir nosso caminho quando necessário. Mudando para o **JSONPath Online Evaluator**, colando a resposta completa do centro do desenvolvedor no campo de entrada e colando o caminho que descobrimos do **JSONPathfinder** no campo de caminho, podemos visualizar o que o caminho real avalia.
>**você precisará substituir o “X” que o JSONPathFinder coloca no início do caminho por um “$”**

![image](/images/Jsonpath1.PNG)

A resposta aqui mostra um único status offline; contudo, sabemos que existem vários usuários nesta fila e precisamos das informações de presença de todos eles. O caminho que estamos usando tem uma posição de array de **"[0]"**, o que significa que está retornando apenas a presença da primeira entidade ou usuário dentro da resposta. Ao alterar isso para um curinga - **"[*]"**, podemos retornar as informações de presença para todos os usuários.

![image](/images/Jsonpath2.PNG)

### Para resumir o que realizamos até agora
Nós -

1. Identificamos a chamada a API dentro do Centro de Desenvolvedor (developer center)
2. Testamos essa chamada a API para garantir que fornecia a presença do usuário que estávamos buscando
3. Usamos JSON Pathfinder para determinar o caminho (path) necessário para a presença do usuário
4. Usamos o JSON Path Online Evaluator para confirmar o caminho (path) retorna os dados que precisamos