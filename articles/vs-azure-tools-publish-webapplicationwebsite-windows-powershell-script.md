---
title: Publicar-WebApplicationWebSite (script do Windows PowerShell) | Microsoft Docs
description: Saiba como publicar um projeto web para um Web site do Azure. Este script cria os recursos necessários na sua subscrição do Azure, caso não existam.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: b540734f17ed11e4c438e1248ed9612fb892e89a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publicar-WebApplicationWebSite (script do Windows PowerShell)
## <a name="syntax"></a>Sintaxe
Publica um projeto web para um site do Azure. O script cria os recursos necessários na sua subscrição do Azure, caso não existam.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Configuração
O caminho para o ficheiro de configuração JSON que descreve os detalhes da implementação.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |true |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="subscriptionname"></a>Nome da Subscrição
O nome da subscrição do Azure que pretende criar o Web site na.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
O caminho para o pacote de implementação web para publicar no Web site. Pode criar este pacote utilizando o Assistente Publicar Web no Visual Studio. Para obter mais informações, consulte [introdução ao Cloud Services do Azure e ao ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
O nome de utilizador e palavra-passe para a base de dados do SQL Server no Azure.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Se for VERDADEIRO, impressão as mensagens a partir do script no fluxo de saída.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |false |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="remarks"></a>Observações
Para obter uma explicação completa de como utilizar o script para criar ambientes de desenvolvimento e teste, consulte [utilizando Scripts do Windows PowerShell para publicar a ambientes de teste e desenvolvimento](vs-azure-tools-publishing-using-powershell-scripts.md).

O ficheiro de configuração JSON Especifica os detalhes do que está a ser implementada. Inclui as informações que especificou quando criou o projeto, tais como o nome e o nome de utilizador para o Web site. Também inclui a base de dados para aprovisionar, se aplicável. O código seguinte mostra um ficheiro de configuração do JSON de exemplo:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Pode editar o ficheiro de configuração JSON para alterar o que é implementado. Não é necessária uma secção de Web site, mas a secção de base de dados é opcional.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [publicar-WebApplicationVM (script do Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)

