---
title: Utilizar a API de REST de Azure Blockchain Workbench
description: Cenários para saber como utilizar a API de REST do Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 27ed94b3ce14c57e369b0c80d4c53b72a5ae40a8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Utilizando a API de REST de Azure Blockchain Workbench 

API de REST de Workbench Blockchain do Azure fornece aos programadores e técnicos de informação uma forma para criar integrações avançadas para aplicações de blockchain. Este documento explica vários métodos de chaves da API de REST do Workbench. Considere um cenário onde pretende criar um cliente blockchain personalizado, o que permite aos utilizadores com sessão iniciada ver e interagir com as respetivas aplicações blockchain atribuído um programador. O cliente permite aos utilizadores ver instâncias de contrato e executar ações contratos inteligentes. O cliente utiliza a API de REST do Workbench no contexto do utilizador com sessão iniciada para fazer o seguinte:

* Lista de aplicações
* Lista os fluxos de trabalho para uma aplicação
* Lista de instâncias de contrato inteligente para um fluxo de trabalho
* Lista de ações disponíveis para um contrato
* Executar uma ação de um contrato

## <a name="list-applications"></a>Lista de aplicações

Depois de um utilizador tem sessão iniciada para o cliente blockchain, é a primeira tarefa obter todas as aplicações de blockchain Blockchain Workbench para o utilizador. Neste cenário, o utilizador tem acesso às duas aplicações:

1.  Transferência de recurso
2.  Transportes refrigerated

Utilize o [aplicações GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

Resposta apresenta uma lista de todas as aplicações de blockchain às quais um utilizador tem acesso no Blockchain Workbench. Os administradores do Blockchain Workbench obter todas as aplicações de blockchain, enquanto não Workbench administradores obter todos os blockchains para o qual têm a função de, pelo menos, uma aplicação associada ou uma função de instância de contrato inteligente associado.

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

## <a name="list-workflows-for-an-application"></a>Lista os fluxos de trabalho para uma aplicação

Depois de um utilizador seleccionar blockchain aplicável aplicação, neste caso, transferência de recurso, o cliente blockchain obtém todos os fluxos de trabalho da aplicação blockchain específico. Os utilizadores podem selecionar, em seguida, o fluxo de trabalho aplicável antes de a ser mostrada todas as instâncias de contrato inteligente para o fluxo de trabalho. Cada aplicação blockchain tem um ou mais fluxos de trabalho e cada fluxo de trabalho tem zero ou smart instâncias de contrato. Ao criar blockchain aplicações cliente, é recomendado para ignorar o fluxo de experiência de utilizador, permitindo que os utilizadores selecionem o fluxo de trabalho adequado quando existe apenas um fluxo de trabalho para a aplicação de blockchain. Neste caso, a transferência do recurso tem apenas um fluxo de trabalho, também designado por recurso de transferência.

Utilize o [fluxos de trabalho de aplicações GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Resposta apresenta uma lista de todos os fluxos de trabalho da aplicação blockchain especificado para o qual um utilizador tem acesso no Blockchain Workbench. Administradores de Blockchain Workbench obter todos os fluxos de trabalho de blockchain, enquanto não Workbench administradores obter todos os fluxos de trabalho para o qual têm pelo menos uma função de aplicação associada ou associada uma função de instância de contrato inteligente.

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

## <a name="list-smart-contract-instances-for-a-workflow"></a>Lista de instâncias de contrato inteligente para um fluxo de trabalho

Depois de um utilizador seleciona o fluxo de trabalho aplicável, este cenário Asset transferência, o cliente blockchain irá obter todas as instâncias de contrato inteligente para o fluxo de trabalho especificado. Pode utilizar estas informações para mostrar todas as instâncias de contrato inteligente para o fluxo de trabalho e permitir que os utilizadores Descrição profunda em qualquer das instâncias do contrato inteligente mostrado. Neste exemplo, considere que um utilizador pretende interagir com uma das instâncias contrato inteligente para tomar medidas.

Utilize o [contrai GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Resposta apresenta uma lista de todas as instâncias de contrato inteligente do fluxo de trabalho especificado. Os administradores do Workbench obter todas as instâncias de contrato inteligente, enquanto não Workbench administradores obter todos os smart instâncias de contrato para o qual têm pelo menos uma função de aplicação associada ou estão associados uma função de instância de contrato inteligente.

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

## <a name="list-available-actions-for-a-contract"></a>Lista de ações disponíveis para um contrato

Depois de um utilizador decidiu descrição profunda dos contratos, o cliente blockchain, em seguida, pode mostrar todas as ações disponíveis para o utilizador dado o estado do contrato. Neste exemplo, o utilizador está à procura disponíveis em todas as ações para um novo contrato inteligente que estes criados:

* Modificar: Permite que o utilizador modifique a descrição e o preço de um recurso.
* Terminar: Permite que o utilizador terminar o contrato do elemento.

Utilize o [contrato ação GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Resposta apresenta uma lista de todas as ações para os quais um utilizador pode demorar dada o estado atual da instância do contrato inteligente especificado. Os utilizadores obtêm a todas as ações aplicáveis se o utilizador tem uma função de aplicação associada ou associada uma função de instância de contrato inteligente para o estado atual da instância do contrato inteligente especificado.

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

## <a name="execute-an-action-for-a-contract"></a>Executar uma ação de um contrato

Um utilizador, em seguida, pode optar por adotar ação para a instância de contrato inteligente especificado. Neste caso, considere o cenário em que um utilizador pretende modificar a descrição e o preço de um ativo para o seguinte:

* Descrição: "os meus atualizado carro"
* Preço: 54321

Utilize o [contrato ação POST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost):

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

Os utilizadores apenas são possível executar a ação dada o estado atual da instância especificada contrato inteligente e função de aplicação associada ao utilizador ou função de instâncias de contrato inteligente. Se a publicação for bem sucedida, é devolvida uma resposta de HTTP 200 OK com nenhum corpo de resposta.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Passos Seguintes

* [Referência da API de REST do Workbench Blockchain do Azure](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)