---
title: Processador de extensão de configuração do Estado de pretendida do Azure | Microsoft Docs
description: Carregar e aplicar uma configuração de DSC do PowerShell numa VM do Azure utilizando a extensão de DSC
services: virtual-machines-windows
documentationcenter: ''
author: eshaparmar
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: esparmar
ms.openlocfilehash: b34314951980f7dbe2269119883dec52a90a0587
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942705"
---
# <a name="powershell-dsc-extension"></a>Extensão de DSC do PowerShell

## <a name="overview"></a>Descrição geral

A extensão de DSC do PowerShell para o Windows é publicada e suportada pela Microsoft. A extensão carrega e aplica-se uma configuração de DSC do PowerShell numa VM do Azure. A extensão de DSC chama-se no DSC do PowerShell para enact a configuração de DSC recebida na VM. Este documento fornece detalhes sobre as plataformas suportadas, configurações e opções de implementação para a extensão de máquina virtual de DSC para Windows.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo

A extensão de DSC suporta o SO seguinte

Windows Server 2016, o Windows Server 2012R2, Windows Server 2012, o Windows Server 2008 R2 SP1, o cliente do Windows 7/8.1

### <a name="internet-connectivity"></a>Conectividade Internet

A extensão de DSC para o Windows requer que a máquina virtual de destino está ligada à internet. 

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a parte de definições da extensão DSC num modelo Azure Resource Manager. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.73",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | data |
| Fabricante | Microsoft.Powershell.DSC | cadeia |
| tipo | DSC | cadeia |
| typeHandlerVersion | 2.73 | Int |

### <a name="settings-property-values"></a>Valores de propriedade de definições

| Nome | Tipo de Dados | Descrição
| ---- | ---- | ---- |
| settings.wmfVersion | cadeia | Especifica a versão do Windows Management Framework que deve ser instalado na VM. Definir esta propriedade como 'mais recente' irá instalar a versão mais atualizada do WMF. Os valores possíveis apenas atuais para esta propriedade são '4.0', '5.0' e 'mais recente'. Estes valores possíveis são sujeitos a atualizações. O valor predefinido é 'mais recente'. |
| settings.configuration.url | cadeia | Especifica a localização de URL a partir das quais transferir o ficheiro zip de configuração de DSC. Se o URL fornecido necessita de um token SAS para o acesso, terá de definir a propriedade de protectedSettings.configurationUrlSasToken para o valor do seu token SAS. Esta propriedade é necessária se settings.configuration.script e/ou settings.configuration.function estiverem definidos.
| settings.configuration.script | cadeia | Especifica o nome de ficheiro do script que contém a definição da sua configuração de DSC. Este script tem de ser na pasta raiz do ficheiro zip transferido a partir do URL especificado pela propriedade configuration.url. Esta propriedade é necessária se settings.configuration.url e/ou settings.configuration.script estiverem definidos.
| settings.configuration.function | cadeia | Especifica o nome da sua configuração de DSC. A configuração com o nome tem de estar contida no script definido pelo configuration.script. Esta propriedade é necessária se settings.configuration.url e/ou settings.configuration.function estiverem definidos.
| settings.configurationArguments | Coleção | Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade não será encriptada.
| settings.configurationData.url | cadeia | Especifica o URL a partir das quais transferir o ficheiro de configuração de dados (.pds1) para utilizar como entrada para a configuração de DSC. Se o URL fornecido necessita de um token SAS para o acesso, terá de definir a propriedade de protectedSettings.configurationDataUrlSasToken para o valor do seu token SAS.
| settings.privacy.dataEnabled | cadeia | Ativa ou desativa a coleção de telemetria. Os valores só possíveis para esta propriedade são 'Ativar', 'Disable' ", ou $null. Abandonar o fileparser esta propriedade em branco ou nulo irá ativar a telemetria
| settings.advancedOptions.forcePullAndApply | bool | Permite que a extensão de DSC atualizar e enact configurações de DSC quando o modo de atualização é solicitação.
| settings.advancedOptions.downloadMappings | Coleção | Define as localizações alternativas para transferir as dependências, tais como WMF e .NET

### <a name="protected-settings-property-values"></a>Valores de propriedade de definições protegidos

| Nome | Tipo de Dados | Descrição
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | cadeia | Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade será encriptada. |
| protectedSettings.configurationUrlSasToken | cadeia | Especifica o token SAS para aceder ao URL de definido por configuration.url. Esta propriedade será encriptada. |
| protectedSettings.configurationDataUrlSasToken | cadeia | Especifica o token SAS para aceder ao URL de definido por configurationData.url. Esta propriedade será encriptada. |


## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. Os modelos são ideais quando implementar um ou mais máquinas virtuais que necessitam de configuração de implementação de post. Um modelo de Gestor de recursos de exemplo que inclui a extensão de VM de agente do OMS pode ser encontrado no [Galeria de início rápido de Azure](https://github.com/Azure/azure-quickstart-templates/tree/052db5feeba11f85d57f170d8202123511f72044/dsc-extension-iis-server-windows-vm). 

A configuração de JSON para uma extensão da máquina virtual pode ser aninhada o recurso de máquina virtual ou colocada no nível superior de um modelo do Resource Manager JSON de raiz ou. A colocação da configuração JSON afeta o valor do nome do recurso e do tipo. 

Quando o recurso de extensão de aninhamento, JSON é colocado no `"resources": []` objeto da máquina virtual. Quando colocar a extensão JSON na raiz do modelo, o nome do recurso inclui uma referência à máquina virtual principal e o tipo de reflete a configuração aninhada.  


## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure

A CLI do Azure pode ser utilizada para implementar a extensão de VM de agente do OMS para uma máquina virtual existente. Substitua a chave do OMS e o ID do OMS as da sua área de trabalho do OMS. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name Microsoft.Powershell.DSC \
  --publisher Microsoft.Powershell \
  --version 2.73 --protected-settings '{}' \
  --settings '{}'
```

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando a CLI do Azure. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando utilizando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Pacote de extensão é transferido e implementado para esta localização na VM do Azure
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Ficheiro de estado de extensão contém o estado de subchaves e códigos de estado de erros/com êxito, juntamente com o erro detalhado e desciption para cada extensão de executar.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Os registos de saída de extensão são registados no diretório seguinte:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Códigos de erro e os respetivos significados

| Código de Erro | Significado | Ação possíveis |
| :---: | --- | --- |
| 1000 | Erro genérico | A mensagem para este erro é fornecida pela exceção específica nos registos de extensão |
| 52 | Erro de instalação da extensão | A mensagem para este erro é fornecida por uma exceção específica |
| 1002 | Erro de instalação do WMF | Ocorreu um erro ao instalar o WMF. |
| 1004 | Pacote Zip inválido | Zip inválido; Erro unpacking zip |
| 1100 | Erro de argumento | Indica um problema na entrada fornecido pelo utilizador. A mensagem de erro é fornecida por uma exceção específica|


### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).