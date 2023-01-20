---
title: "Definições básicas"
chapter: false
weight: 10
locale: "pt-BR"
---

## Definição de Ação de Dados
Em poucas palavras, Ações de dados são o que a Genesys define como nosso construtor de chamada de API REST.

As ações de dados são usadas para acessar informações internas e externas à plataforma que, de outra forma, não estariam disponíveis. Essas informações, como fila avançada ou dados de CRM, podem ser usadas para invocar decisões de roteamento poderosas ou fornecer informações avançadas de script/screen-pop para seus agentes (entre muitas outras utilizações). As Ações de Dados podem ser divididas em 2 categorias, **Internas** e **Externas**.

As ações de dados nos permitem a flexibilidade de trabalhar de forma intercambiável com outros aplicativos e passar dados livremente para onde eles precisam estar. Isso pode resultar em cenários como chamar atributos de dados personalizados para apresentar em um script de agente, enviar dados para um armazém de CRM doméstico, exportar dados e automação de processos dentro do ambiente Genesys CX.

**As Ações de Dados Internas - Internal Data Actions** são 'normalmente' usadas para tomar decisões de roteamento ou atualizar recursos e configurações da plataforma.

   * Exemplo: Verificando quantos agentes estão na fila para determinar se deve oferecer ao chamador (cliente) uma opção de retorno de chamada (call-back).

**As Ações de Dados Externos - External Data Actions** podem ser usadas para extrair informações de sistemas externos para preencher scripts ou atualizar sistemas externos com informações que o agente coletou na chamada.

  * Exemplo: Extrair um registro de cliente de um CRM externo para exibir as informações da conta para o agente dentro do script e atualizar esse registro do cliente de dentro do script.