---
title: Publicar-WebApplicationWebSite (script do Windows PowerShell) | Documentos da Microsoft
description: Saiba como publicar um projeto da web para um site do Azure. Este script cria os recursos necessários na sua subscrição do Azure, caso não existam.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: 7199e382721fed786157b3dcc1d5930d5fdd49cb
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969490"
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publicar-WebApplicationWebSite (script do Windows PowerShell)
## <a name="syntax"></a>Sintaxe
Publica um projeto da web para um site do Azure. O script cria os recursos necessários na sua subscrição do Azure, caso não existam.

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
O caminho para o pacote de implementação web para publicar no Web site. Pode criar este pacote utilizando o assistente Publish Web no Visual Studio. Para obter mais informações, consulte [introdução aos serviços Cloud do Azure e ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
O nome de utilizador e palavra-passe para a base de dados SQL no Azure.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Se for VERDADEIRO, imprimir mensagens de scripts para o fluxo de saída.

| Parâmetro | Valor predefinido |
| --- | --- |
| Aliases |nenhum |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |false |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="remarks"></a>Observações
Para obter uma explicação completa sobre como utilizar o script para criar ambientes de desenvolvimento e teste, consulte [usando Scripts do Windows PowerShell para publicar no desenvolvimento e ambientes de teste](vs-azure-tools-publishing-using-powershell-scripts.md).

O ficheiro de configuração JSON Especifica os detalhes do que está a ser implantado. Ele inclui as informações que especificou quando criou o projeto, como o nome e o nome de utilizador para o Web site. Ele também inclui a base de dados para aprovisionar, se houver. O código seguinte mostra um arquivo de configuração do JSON de exemplo:

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

Pode editar o ficheiro de configuração JSON para alterar o que é implementado. Uma seção do Web site é necessária, mas a seção de banco de dados é opcional.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [Publish-WebApplicationVM (script do Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)

