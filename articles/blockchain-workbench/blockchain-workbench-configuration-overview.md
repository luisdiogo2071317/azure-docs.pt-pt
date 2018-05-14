---
title: Referência da configuração do Blockchain Workbench do Azure
description: Blockchain Workbench aplicação configuração descrição geral do Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 3/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 6318b6aafda972242239ce91abb0f67a15721e1e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Referência da configuração do Blockchain Workbench do Azure

 Aplicações do Azure Blockchain Workbench são fluxos de trabalho de múltiplos intervenientes definidos por metadados de configuração e o código de contrato inteligente. Metadados de configuração definem os fluxos de trabalho de alto nível e o modelo de interação da aplicação blockchain. Contratos inteligentes definem a lógica de negócio da aplicação blockchain. Workbench utiliza a configuração e code contrato inteligente para gerar blockchain experiências de utilizador de aplicação.

Metadados de configuração especifica as seguintes informações para cada aplicação blockchain: 

* Nome e descrição da aplicação blockchain
* Funções exclusivas para os utilizadores que podem agir ou participar na aplicação blockchain
* Um ou mais fluxos de trabalho. Cada fluxo de trabalho atua como uma máquina de estado para controlar o fluxo de lógica de negócio. Fluxos de trabalho podem ser independente ou interagir entre si.

Cada fluxo de trabalho definido Especifica o seguinte:

* Nome e descrição do fluxo de trabalho
* Estados de fluxo de trabalho.  Cada Estado é uma fase no fluxo de controlo a lógica de negócio. 
* Ações a efetuar a transição para o estado do seguinte
* As funções de utilizador permitidas para iniciar a cada ação
* Inteligentes contratos que representam a lógica de negócio nos ficheiros de código

## <a name="application"></a>Aplicação

Uma aplicação blockchain contém funções de utilizador, metadados e fluxos de trabalho de configuração que podem agir ou participar na aplicação.

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| ApplicationName | Nome de aplicação único. O contrato inteligente correspondente tem de utilizar o mesmo **ApplicationName** para a classe de contrato aplicável.  | Sim |
| Nome a Apresentar | Nome a apresentar amigável da aplicação. | Sim |
| Descrição | Descrição da aplicação. | Não |
| ApplicationRoles | Coleção de [ApplicationRoles](#application-roles). Funções de utilizador que podem agir ou participar na aplicação.  | Sim |
| Fluxos de trabalho | Coleção de [fluxos de trabalho](#workflows). Cada fluxo de trabalho atua como uma máquina de estado para controlar o fluxo de lógica de negócio. | Sim |

Por exemplo, consulte [exemplo do ficheiro de configuração](#configuration-file-example).

## <a name="workflows"></a>Fluxos de trabalho

Lógica de negócio de uma aplicação pode ser modelada como uma máquina de Estado onde colocar uma ação provoca o fluxo de lógica de negócio para mover de um Estado para outro. Um fluxo de trabalho é uma coleção dessas Estados e ações. Cada fluxo de trabalho é composto por um ou mais contratos inteligentes, que representam a lógica de negócio nos ficheiros de código. Um contrato executável é uma instância de fluxo de trabalho.

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Nome | Nome do fluxo de trabalho exclusivo. O contrato inteligente correspondente tem de utilizar o mesmo **nome** para a classe de contrato aplicável. | Sim |
| Nome a Apresentar | Nome a apresentar amigável do fluxo de trabalho. | Sim |
| Descrição | Descrição do fluxo de trabalho. | Não |
| Iniciadores | Coleção de [ApplicationRoles](#application-roles). Funções que são atribuídas a utilizadores que estão autorizados a criar contratos no fluxo de trabalho. | Sim |
| StartState | Nome do Estado inicial do fluxo de trabalho. | Sim |
| Propriedades | Coleção de [identificadores](#identifiers). Ferramenta de experiência de representa dados que podem ser lidos desativar cadeia ou visualizadas um utilizador. | Sim |
| Construtor | Define os parâmetros de entrada para criar uma instância do fluxo de trabalho. | Sim |
| Funções | Uma coleção de [funções](#functions) que pode ser executado no fluxo de trabalho. | Sim |
| Estados | Uma coleção de fluxo de trabalho [Estados](#states). | Sim |

Por exemplo, consulte [exemplo do ficheiro de configuração](#configuration-file-example).

## <a name="type"></a>Tipo

Tipos de dados suportada.

| Tipo | Descrição |
|-------|-------------|
| Endereço  | Blockchain tratam tipo, tais como *contratos* ou *utilizadores* |
| bool     | Tipo de dados booleano |
| Contrato | Endereço do contrato de tipo |
| Int      | Tipo de dados de número inteiro |
| dinheiro    | Tipo de dados de moeda |
| state    | Estado do fluxo de trabalho |
| cadeia   | Tipo de dados de cadeia |
| Utilizador     | Endereço do tipo de utilizador |
| hora     | Tipo de dados de tempo |
|`[ Application Role Name ]`| Qualquer nome especificado na função de aplicação. Os limites que os utilizadores desse tipo de função. |

## <a name="constructor"></a>Construtor

Define os parâmetros de entrada para uma instância do fluxo de trabalho.

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Parâmetros | Coleção de [identificadores](#identifiers) necessário para iniciar um contrato inteligente. | Sim |

### <a name="constructor-example"></a>Exemplo de construtor

``` json
{
  "Parameters": [
    {
      "Name": "description",
      "Description": "The description of this asset",
      "DisplayName": "Description",
      "Type": {
        "Name": "string"
      }
    },
    {
      "Name": "price",
      "Description": "The price of this asset",
      "DisplayName": "Price",
      "Type": {
        "Name": "money"
      }
    }
  ]
}

```

## <a name="functions"></a>Funções

Define as funções que podem ser executadas no fluxo de trabalho.

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Nome | O nome exclusivo da função. O contrato inteligente correspondente tem de utilizar o mesmo **nome** para a função aplicável. | Sim |
| Nome a Apresentar | Nome a apresentar amigável da função. | Sim |
| Descrição | Descrição da função | Não |
| Parâmetros | Coleção de [identificadores](#identifiers) correspondente para os parâmetros da função. | Sim |

### <a name="functions-example"></a>Exemplo de funções

``` json
"Functions": [
  {
    "Name": "Modify",
    "DisplayName": "Modify",
    "Description": "Modify the description/price attributes of this asset transfer instance",
    "Parameters": [
      {
        "Name": "description",
        "Description": "The new description of the asset",
        "DisplayName": "Description",
        "Type": {
          "Name": "string"
        }
      },
      {
        "Name": "price",
        "Description": "The new price of the asset",
        "DisplayName": "Price",
        "Type": {
          "Name": "money"
        }
      }
    ]
  },
  {
    "Name": "Terminate",
    "DisplayName": "Terminate",
    "Description": "Used to cancel this particular instance of asset transfer",
    "Parameters": []
  }
]

```

## <a name="states"></a>Estados

Uma coleção de Estados exclusivos dentro de um fluxo de trabalho. Cada Estado de captura um passo no fluxo de controlo a lógica de negócio. 

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Nome | Nome exclusivo do Estado. O contrato inteligente correspondente tem de utilizar o mesmo **nome** para o estado aplicável. | Sim |
| Nome a Apresentar | Nome a apresentar amigável do Estado. | Sim |
| Descrição | Descrição do Estado. | Não |
| PercentComplete | Um número inteiro apresentado na interface de utilizador de Blockchain Workbench para mostrar o progresso no fluxo de controlo de lógica de negócio. | Sim |
| Estilo | Sugestão visual que indica se o estado representa um Estado de êxito ou falha. Existem dois valores válidos: `Success` ou `Failure`. | Sim |
| Transições | Coleção de disponíveis [transições](#transitions) do estado atual para o conjunto seguinte de Estados. | Não |

### <a name="states-example"></a>Exemplo de Estados

``` json
"States": [
    {
      "Name": "Active",
      "DisplayName": "Active",
      "Description": "The initial state of the asset transfer workflow",
      "PercentComplete": 20,
      "Style": "Success",
      "Transitions": [
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Cancels this instance of asset transfer",
          "Function": "Terminate",
          "NextStates": [ "Terminated" ],
          "DisplayName": "Terminate Offer"
        },
        {
          "AllowedRoles": [ "Buyer" ],
          "AllowedInstanceRoles": [],
          "Description": "Make an offer for this asset",
          "Function": "MakeOffer",
          "NextStates": [ "OfferPlaced" ],
          "DisplayName": "Make Offer"
        },
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Modify attributes of this asset transfer instance",
          "Function": "Modify",
          "NextStates": [ "Active" ],
          "DisplayName": "Modify"
        }
      ]
    },
    {
      "Name": "Accepted",
      "DisplayName": "Accepted",
      "Description": "Asset transfer process is complete",
      "PercentComplete": 100,
      "Style": "Success",
      "Transitions": []
    },
    {
      "Name": "Terminated",
      "DisplayName": "Terminated",
      "Description": "Asset transfer has been cancelled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Transições

Ações disponíveis para o estado do seguinte. Uma ou mais funções de utilizador podem executar uma ação em cada Estado, onde uma ação poderá transição de estado para outro Estado no fluxo de trabalho. 

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| AllowedRoles | Lista de funções de aplicações permitidas para iniciar a transição. Todos os utilizadores da função especificada podem ser capazes de efetuar a ação. | Não |
| AllowedInstanceRoles | Lista de funções de utilizador que participam ou especificado no contrato inteligente permitido para iniciar a transição. Funções de instância são definidas no **propriedades** dentro de fluxos de trabalho. Estes utilizadores representam um utilizador que participam especificados, ou no contrato inteligente, por oposição a todos os utilizadores de um tipo de função. | Não |
| Nome a Apresentar | Nome a apresentar amigável da transição. | Sim |
| Descrição | Descrição da transição. | Não |
| Função | O nome da função para iniciar a transição. | Sim |
| NextStates | Uma coleção de Estados seguintes potenciais após uma transição com êxito. | Sim |

### <a name="transitions-example"></a>Exemplo de transições

``` json
"Transitions": [
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Cancels this instance of asset transfer",
    "Function": "Terminate",
    "NextStates": [ "Terminated" ],
    "DisplayName": "Terminate Offer"
  },
  {
    "AllowedRoles": [ "Buyer" ],
    "AllowedInstanceRoles": [],
    "Description": "Make an offer for this asset",
    "Function": "MakeOffer",
    "NextStates": [ "OfferPlaced" ],
    "DisplayName": "Make Offer"
  },
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Modify attributes of this asset transfer instance",
    "Function": "Modify",
    "NextStates": [ "Active" ],
    "DisplayName": "Modify"
  }
]

```

## <a name="application-roles"></a>Funções da aplicação

Funções da aplicação definam um conjunto de funções que podem ser atribuídos a utilizadores que pretendem atuar ou participar na aplicação. Funções da aplicação podem ser utilizadas para restringir as ações e participação dentro de blockchain aplicação e fluxos de trabalho correspondentes. 

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Nome | O nome exclusivo da função de aplicação. O contrato inteligente correspondente tem de utilizar o mesmo **nome** para a função aplicável. Os nomes de tipo base estão reservados. Não é possível nome uma função de aplicação com o mesmo nome que [tipo](#type)| Sim |
| Descrição | Descrição da função de aplicação. | Não |

### <a name="application-roles-example"></a>Exemplo de funções de aplicação

``` json
"ApplicationRoles": [
  {
    "Name": "Appraiser",
    "Description": "User that signs off on the asset price"
  },
  {
    "Name": "Buyer",
    "Description": "User that places an offer on an asset"
  }
]
```
## <a name="identifiers"></a>Identificadores

Identificadores representam uma coleção de informações utilizadas para descrever as propriedades de fluxo de trabalho, construtor e os parâmetros de função. 

| Campo | Descrição | Necessário |
|-------|-------------|:--------:|
| Nome | O nome exclusivo a propriedade ou o parâmetro. O contrato inteligente correspondente tem de utilizar o mesmo **nome** para a propriedade aplicável ou o parâmetro. | Sim |
| Nome a Apresentar | Nome a apresentar amigável para a propriedade ou o parâmetro. | Sim |
| Descrição | Descrição da propriedade ou parâmetro. | Não |

### <a name="identifiers-example"></a>Exemplo de identificadores

``` json
"Properties": [
  {
    "Name": "State",
    "DisplayName": "State",
    "Description": "Holds the state of the contract",
    "Type": {
      "Name": "state"
    }
  },
  {
    "Name": "Description",
    "DisplayName": "Description",
    "Description": "Describes the asset being sold",
    "Type": {
      "Name": "string"
    }
  }
]
```

## <a name="configuration-file-example"></a>Exemplo de ficheiro de configuração

O exemplo seguinte define uma aplicação básica de resposta-pedido no qual um requerente envia que um pedido e um dispositivo de resposta enviar uma resposta ao pedido.

``` json
{
  "ApplicationName": "HelloBlockchain",
  "DisplayName": "Hello, Blockchain!",
  "Description": "A simple application to send request and get response",
  "ApplicationRoles": [
    {
      "Name": "Requestor",
      "Description": "A person sending a request."
    },
    {
      "Name": "Responder",
      "Description": "A person responding to a request"
    }
  ],
  "Workflows": [
    {
      "Name": "RequestResponse",
      "DisplayName": "Request Response",
      "Description": "A simple workflow to send a request and receive a response.",
      "Initiators": [ "Requestor" ],
      "StartState": "Request",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract.",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Requestor",
          "DisplayName": "Requestor",
          "Description": "A person sending a request.",
          "Type": {
            "Name": "Requestor"
          }
        },
        {
          "Name": "Responder",
          "DisplayName": "Responder",
          "Description": "A person sending a response.",
          "Type": {
            "Name": "Responder"
          }
        },
        {
          "Name": "RequestMessage",
          "DisplayName": "Request Message",
          "Description": "A request message.",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "ResponseMessage",
          "DisplayName": "Response Message",
          "Description": "A response message.",
          "Type": {
            "Name": "string"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "message",
            "Description": "...",
            "DisplayName": "Request Message",
            "Type": {
              "Name": "string"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "SendRequest",
          "DisplayName": "Request",
          "Description": "...",
          "Parameters": [
            {
              "Name": "requestMessage",
              "Description": "...",
              "DisplayName": "Request Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        },
        {
          "Name": "SendResponse",
          "DisplayName": "Response",
          "Description": "...",
          "Parameters": [
            {
              "Name": "responseMessage",
              "Description": "...",
              "DisplayName": "Response Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        }
      ],
      "States": [
        {
          "Name": "Request",
          "DisplayName": "Request",
          "Description": "...",
          "PercentComplete": 50,
          "Value": 0,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": ["Responder"],
              "AllowedInstanceRoles": [],
              "Description": "...",
              "Function": "SendResponse",
              "NextStates": [ "Respond" ],
              "DisplayName": "Send Response"
            }
          ]
        },
        {
          "Name": "Respond",
          "DisplayName": "Respond",
          "Description": "...",
          "PercentComplete": 90,
          "Value": 1,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": ["Requestor"],
              "Description": "...",
              "Function": "SendRequest",
              "NextStates": [ "Request" ],
              "DisplayName": "Send Request"
            }
          ]
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementar o Azure Blockchain Workbench](blockchain-workbench-deploy.md)

