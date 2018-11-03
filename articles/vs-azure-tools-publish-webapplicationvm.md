---
title: WebApplicationVM publicar | Documentos da Microsoft
description: Saiba como implementar uma aplicação web a uma máquina virtual. Este script cria os recursos necessários na sua subscrição do Azure, caso não existam.
services: visual-studio-online
author: ghogen
manager: douge
assetId: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: f2b6a4ce611169c6e03479eb71446d4ac059d83e
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969441"
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publicar-WebApplicationVM (script do Windows PowerShell)
Implementa uma aplicação web para uma máquina virtual. O script cria os recursos necessários na sua subscrição do Azure, caso não existam.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Configuração
O caminho para o ficheiro de configuração JSON que descreve os detalhes da implementação.

| Aliases | nenhum |
| --- | --- |
| Necessário? |true |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="subscriptionname"></a>Nome da Subscrição
O nome da subscrição do Azure na qual pretende criar a máquina virtual.

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |Utiliza a primeira assinatura no ficheiro de subscrição |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="webdeploypackage"></a>WebDeployPackage
O caminho para o pacote de implementação web para publicar para a máquina virtual. Pode criar este pacote utilizando o assistente Publish Web no Visual Studio. Ver [como: criar um pacote de implementação Web no Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="allowuntrusted"></a>AllowUntrusted
Se for VERDADEIRO, permita a utilização de certificados que não são assinados por uma autoridade de raiz fidedigna.

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |false |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="vmpassword"></a>VMPassword
As credenciais da conta de máquina virtual. Exemplo: - VMPassword @{nome = "admin"; Palavra-passe = "password"}

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
As credenciais da base de dados SQL no Azure. Exemplo: - DatabaseServerPassword @{nome = "admin"; Palavra-passe = "password"}

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |nenhum |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Se for VERDADEIRO, imprimir mensagens de scripts para o fluxo de saída.

| Aliases | nenhum |
| --- | --- |
| Necessário? |false |
| Posição |com o nome |
| Valor predefinido |false |
| Aceitar entrada de pipeline? |false |
| Aceitar carateres universais? |false |

## <a name="remarks"></a>Observações
Para obter uma explicação completa sobre como utilizar o script para criar ambientes de desenvolvimento e teste, consulte [usando Scripts do Windows PowerShell para publicar no desenvolvimento e ambientes de teste](vs-azure-tools-publishing-using-powershell-scripts.md).

O ficheiro de configuração JSON Especifica os detalhes do que está a ser implantado. Ele inclui as informações que especificou quando criou o projeto, como o nome, o grupo de afinidade, a imagem VHD e o tamanho da máquina virtual. Também inclui os pontos de extremidade na máquina virtual, as bases de dados para aprovisionar, se houver e os parâmetros de implementação da web. O código seguinte mostra um arquivo de configuração do JSON de exemplo:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Pode editar o ficheiro de configuração JSON para alterar o que está a ser aprovisionado. Uma máquina virtual e um serviço em nuvem são necessárias, mas a seção de banco de dados é opcional.

