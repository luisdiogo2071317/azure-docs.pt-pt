---
title: Utilizar a API REST do Azure Blockchain Workbench
description: Cenários para saber como utilizar a API REST do Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: cec2ab862a34a8753601dfeef3081ae9e9ca9fd9
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Utilizar a API REST do Azure Blockchain Workbench 

A API REST do Azure Blockchain Workbench proporciona aos programadores e técnicos de informação uma forma de criar integrações avançadas para aplicações de blockchain. Este documento explica vários métodos essenciais da API REST do Workbench. Considere um cenário em que um programador pretende criar um cliente blockchain personalizado, que permite aos utilizadores com sessão iniciada ver e interagir com as respetivas aplicações de blockchain atribuídas. O cliente permite aos utilizadores ver instâncias de contrato e executar ações em contratos inteligentes. O cliente utiliza a API REST do Workbench no contexto do utilizador com sessão iniciada para fazer o seguinte:

* Listar aplicações
* Listar fluxos de trabalho para uma aplicação
* Listar instâncias de contrato inteligente para um fluxo de trabalho
* Listar ações disponíveis para um contrato
* Executar uma ação para um contrato

## <a name="list-applications"></a>Listar aplicações

Depois de um utilizador iniciar sessão no cliente blockchain, a primeira tarefa consiste em obter todas as aplicações de blockchain do Blockchain Workbench para o utilizador. Neste cenário, o utilizador tem acesso a duas aplicações:

1.  Asset Transfer
2.  Refrigerated Transportation

Utilize a [API GET de Aplicações](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

A resposta apresenta uma lista de todas as aplicações de blockchain às quais um utilizador tem acesso no Blockchain Workbench. Os administradores do Blockchain Workbench obtêm todas as aplicações de blockchain, enquanto os utilizadores não administradores do Workbench obtêm todos os blockchains para os quais têm, pelo menos, uma função de aplicação associada ou uma função de instância de contrato inteligente associada.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Listar fluxos de trabalho para uma aplicação

Depois de um utilizador selecionar a aplicação de blockchain aplicável, neste caso, Asset Transfer, o cliente blockchain obtém todos os fluxos de trabalho da aplicação de blockchain específica. Em seguida, os utilizadores podem selecionar o fluxo de trabalho aplicável antes de serem apresentadas todas as instâncias de contrato inteligente para o fluxo de trabalho. Cada aplicação de blockchain tem um ou mais fluxos de trabalho e cada fluxo de trabalho tem zero ou instâncias de contrato inteligente. Ao criar aplicações cliente do blockchain, é recomendado ignorar o fluxo de experiência de utilizador, que permite que os utilizadores selecionem o fluxo de trabalho adequado quando existe apenas um fluxo de trabalho para a aplicação de blockchain. Neste caso, Asset Transfer tem apenas um fluxo de trabalho, também chamado Asset Transfer.

Utilize a [API Workflows GET de Aplicações](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

A resposta apresenta uma lista de todos os fluxos de trabalho da aplicação de blockchain especificada, aos quais um utilizador tem acesso no Blockchain Workbench. Os administradores do Blockchain Workbench obtêm todos os fluxos de trabalho do blockchain, enquanto os utilizadores não administradores do Workbench obtêm todos os fluxos de trabalho para os quais têm, pelo menos, uma função de aplicação associada ou que estão associados a uma função de instância de contrato inteligente.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>Listar instâncias de contrato inteligente para um fluxo de trabalho

Depois de um utilizador selecionar o fluxo de trabalho aplicável, neste caso, Asset Transfer, o cliente blockchain irá obter todas as instâncias de contrato inteligente para o fluxo de trabalho especificado. Pode utilizar estas informações para mostrar todas as instâncias de contrato inteligente para o fluxo de trabalho e permitir que os utilizadores explorem mais a fundo qualquer uma das instâncias de contrato inteligente apresentadas. Neste exemplo, considere que um utilizador pretende interagir com uma das instâncias de contrato inteligente para tomar medidas.

Utilize a [API GET de Contratos](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

A resposta apresenta uma lista de todas as instâncias de contrato inteligente do fluxo de trabalho especificado. Os administradores do Workbench obtêm todas as instâncias de contrato inteligente, enquanto os utilizadores não administradores do Workbench obtêm todas as instâncias de contrato inteligente para as quais têm, pelo menos, uma função de aplicação associada ou que estão associados a uma função de instância de contrato inteligente.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Listar ações disponíveis para um contrato

Depois de um utilizador decidir explorar a fundo um dos contratos, o cliente blockchain poderá mostrar, em seguida, todas as ações disponíveis para o utilizador, tendo em conta o estado do contrato. Neste exemplo, o utilizador está a ver todas as ações disponíveis para um novo contrato inteligente que criou:

* Modificar: permite que o utilizador modifique a descrição e o preço de um recurso.
* Terminar: permite que o utilizador termine o contrato do recurso.

Utilize a [API Contract Action GET](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

A resposta apresenta uma lista de todas as ações que o utilizador pode executar, tendo em conta o estado atual da instância de contrato inteligente especificada. Os utilizadores obtêm todas as ações aplicáveis se o utilizador tiver uma função de aplicação associada ou estiver associado a uma função de instância de contrato inteligente para o estado atual da instância de contrato inteligente especificada.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Executar uma ação para um contrato

Em seguida, um utilizador pode decidir agir em relação à instância de contrato inteligente especificada. Neste caso, considere o cenário em que um utilizador pretende modificar a descrição e o preço de um recurso para o seguinte:

* Descrição: "My updated car"
* Preço: 54321

Utilize a [API Contract Action POST](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost):

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Os utilizadores apenas poderão executar a ação tendo em conta o estado atual da instância de contrato inteligente especificada e a função de aplicação ou a função de instância de contrato inteligente associada do utilizador. Se a publicação for bem sucedida, é devolvida uma resposta de HTTP 200 OK sem corpo da resposta.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência da API REST do Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)