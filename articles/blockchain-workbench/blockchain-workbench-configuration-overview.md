---
title: Referência da configuração do Blockchain Workbench do Azure
description: Blockchain Workbench aplicação configuração descrição geral do Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 178c2c1d4f727241338d6d933cd5eecbbffe65bb
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
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
| AllowedInstanceRoles | Lista de funções de utilizador que participam ou especificado no contrato inteligente permitido para iniciar a transição. Funções de instância são definidas no **propriedades** dentro de fluxos de trabalho. AllowedInstanceRoles representar um utilizador a participar numa instância de um contrato inteligente. AllowedInstanceRoles dão-lhe a capacidade para restringir a efetuar uma ação para uma função de utilizador numa instância de contrato.  Por exemplo, poderá apenas pretender permitir que o utilizador que criou o contrato (InstanceOwner) para conseguir terminar em vez de todos os utilizadores no tipo de função (Owner), se especificou a função na AllowedRoles. | Não |
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

Transferência de recurso é um cenário de contrato inteligente para comprar e vender ativos de valor elevado, o que exige um inspector e appraiser. Sellers pode listar os respetivos recursos ao instanciar um contrato de smart de transferência de recurso. Buyers pode efetuar ofertas, efetuando uma ação no contrato inteligente e outras entidades podem efetuar ações para inspecionar ou appraise o elemento. Assim que o recurso está marcado como ambos inspecionado e appraised, o comprador e vendedor confirma venda novamente antes do contrato é definido como concluir. Em cada fase do processo, todos os participantes tem visibilidade sobre o estado do contrato que é atualizado. 

Para obter mais informações, incluindo os ficheiros de código, consulte [amostra de transferência do recurso para o Azure Blockchain Workbench](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)

É o seguinte ficheiro de configuração para o exemplo de transferência do recurso:

``` json
{
  "ApplicationName": "AssetTransfer",
  "DisplayName": "Asset Transfer",
  "Description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
  "ApplicationRoles": [
    {
      "Name": "Appraiser",
      "Description": "User that signs off on the asset price"
    },
    {
      "Name": "Buyer",
      "Description": "User that places an offer on an asset"
    },
    {
      "Name": "Inspector",
      "Description": "User that inspects the asset and signs off on inspection"
    },
    {
      "Name": "Owner",
      "Description": "User that signs off on the asset price"
    }
  ],
  "Workflows": [
    {
      "Name": "AssetTransfer",
      "DisplayName": "Asset Transfer",
      "Description": "Handles the business logic for the asset transfer scenario",
      "Initiators": [ "Owner" ],
      "StartState":  "Active",
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
        },
        {
          "Name": "AskingPrice",
          "DisplayName": "Asking Price",
          "Description": "The asking price for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "OfferPrice",
          "DisplayName": "Offer Price",
          "Description": "The price being offered for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "InstanceAppraiser",
          "DisplayName": "Instance Appraiser",
          "Description": "The user that appraises the asset",
          "Type": {
            "Name": "Appraiser"
          }
        },
        {
          "Name": "InstanceBuyer",
          "DisplayName": "Instance Buyer",
          "Description": "The user that places an offer for this asset",
          "Type": {
            "Name": "Buyer"
          }
        },
        {
          "Name": "InstanceInspector",
          "DisplayName": "Instance Inspector",
          "Description": "The user that inspects this asset",
          "Type": {
            "Name": "Inspector"
          }
        },
        {
          "Name": "InstanceOwner",
          "DisplayName": "Instance Owner",
          "Description": "The seller of this particular asset",
          "Type": {
            "Name": "Owner"
          }
        }
      ],
      "Constructor": {
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
      },
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
        },
        {
          "Name": "MakeOffer",
          "DisplayName": "Make Offer",
          "Description": "Place an offer for this asset",
          "Parameters": [
            {
              "Name": "inspector",
              "Description": "Specify a user to inspect this asset",
              "DisplayName": "Inspector",
              "Type": {
                "Name": "Inspector"
              }
            },
            {
              "Name": "appraiser",
              "Description": "Specify a user to appraise this asset",
              "DisplayName": "Appraiser",
              "Type": {
                "Name": "Appraiser"
              }
            },
            {
              "Name": "offerPrice",
              "Description": "Specify your offer price for this asset",
              "DisplayName": "Offer Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Reject",
          "DisplayName": "Reject",
          "Description": "Reject the user's offer",
          "Parameters": []
        },
        {
          "Name": "AcceptOffer",
          "DisplayName": "Accept Offer",
          "Description": "Accept the user's offer",
          "Parameters": []
        },
        {
          "Name": "RescindOffer",
          "DisplayName": "Rescind Offer",
          "Description": "Rescind your placed offer",
          "Parameters": []
        },
        {
          "Name": "ModifyOffer",
          "DisplayName": "Modify Offer",
          "Description": "Modify the price of your placed offer",
          "Parameters": [
            {
              "Name": "offerPrice",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Accept",
          "DisplayName": "Accept",
          "Description": "Accept the inspection/appraisal results",
          "Parameters": []
        },
        {
          "Name": "MarkInspected",
          "DisplayName": "Mark Inspected",
          "Description": "Mark the asset as inspected",
          "Parameters": []
        },
        {
          "Name": "MarkAppraised",
          "DisplayName": "Mark Appraised",
          "Description": "Mark the asset as appraised",
          "Parameters": []
        }
      ],
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
          "Name": "OfferPlaced",
          "DisplayName": "Offer Placed",
          "Description": "Offer has been placed for the asset",
          "PercentComplete": 30,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Accept the proposed offer for the asset",
              "Function": "AcceptOffer",
              "NextStates": [ "PendingInspection" ],
              "DisplayName": "Accept Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you previously placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Modify the price that you specified for your offer",
              "Function": "ModifyOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Modify Offer"
            }
          ]
        },
        {
          "Name": "PendingInspection",
          "DisplayName": "Pending Inspection",
          "Description": "Asset is pending inspection",
          "PercentComplete": 40,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark this asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "Inspected" ],
              "DisplayName": "Mark Inspected"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "Appraised" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Inspected",
          "DisplayName": "Inspected",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Appraised",
          "DisplayName": "Appraised",
          "Description": "Asset has been appraised, now awaiting inspection",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark the asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Inspected"
            }
          ]
        },
        {
          "Name": "NotionalAcceptance",
          "DisplayName": "Notional Acceptance",
          "Description": "Asset has been inspected and appraised, awaiting final sign-off from buyer and seller",
          "PercentComplete": 50,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "SellerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "BuyerAccepted" ],
              "DisplayName": "BuyerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "BuyerAccepted",
          "DisplayName": "Buyer Accepted",
          "Description": "Buyer has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            }
          ]
        },
        {
          "Name": "SellerAccepted",
          "DisplayName": "Seller Accepted",
          "Description": "Seller has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "Accepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
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
    }
  ]
}
```
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API de REST do Workbench Blockchain do Azure](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

