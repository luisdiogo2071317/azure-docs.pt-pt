---
title: Executar scripts personalizados em VMs do Linux no Azure | Microsoft Docs
description: Automatizar tarefas de configuração de VM com Linux utilizando a extensão de Script personalizado
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 88f337df14b7e2647a76cce7ef91ec6f8950bbc6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30906832"
---
# <a name="use-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Utilizar a extensão de Script personalizado do Azure com máquinas virtuais do Linux
A extensão de Script personalizado transfere e executa os scripts em máquinas virtuais do Azure. Esta extensão é útil para configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Pode transferir scripts do Storage do Azure ou noutra localização acessível da internet ou pode fornecê-los para o tempo de execução da extensão. 

A extensão de Script personalizado integra-se com modelos Azure Resource Manager. Também pode executá-lo utilizando a CLI do Azure, PowerShell, o portal do Azure ou a API de REST de máquinas virtuais do Azure.

Este artigo fornece detalhes sobre como utilizar a extensão de Script personalizado da CLI do Azure e como executar a extensão utilizando um modelo Azure Resource Manager. Este artigo também fornece passos de resolução de problemas para os sistemas Linux.

## <a name="extension-configuration"></a>Configuração de extensão
A configuração de extensão de Script personalizado especifica coisas como a localização do script e o comando ser executado. Pode armazenar esta configuração nos ficheiros de configuração, especifique-o na linha de comandos ou especificá-la num modelo Azure Resource Manager. 

Pode armazenar dados confidenciais numa configuração protegida, que é encriptada e desencriptada apenas dentro da máquina virtual. A configuração protegida é útil quando o comando de execução inclui segredos como uma palavra-passe.

### <a name="public-configuration"></a>Configuração pública
Segue-se o esquema para a configuração pública.

>[!NOTE]
>Estes nomes de propriedade são sensíveis a maiúsculas. Para evitar problemas de implementação, utilize os nomes, conforme mostrado aqui.

* **commandToExecute** (necessário, string): O script de ponto de entrada a executar.
* **fileUris** (matriz de cadeia opcional,): O URL para ficheiros a ser transferido.
* **Timestamp** (opcional, número inteiro): O carimbo de hora do script. Altere o valor deste campo apenas se pretender acionar uma volte a executar o script.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Configuração protegida
Segue-se o esquema para a configuração protegido.

>[!NOTE]
>Estes nomes de propriedade são sensíveis a maiúsculas. Para evitar problemas de implementação, utilize os nomes, conforme mostrado aqui.

* **commandToExecute** (opcional, cadeia): O script de ponto de entrada a executar. Utilize este campo se o comando contém segredos como palavras-passe.
* **storageAccountName** (opcional, cadeia): O nome da conta de armazenamento. Se especificar credenciais do armazenamento, todos os ficheiros URIs tem de ser URLs para os blobs do Azure.
* **storageAccountKey** (opcional, cadeia): A chave de acesso da conta de armazenamento.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>CLI do Azure
Quando estiver a utilizar a CLI do Azure para executar a extensão de Script personalizado, crie um ficheiro de configuração ou de ficheiros. No mínimo, os ficheiros de configuração contém o ficheiro URI e o comando de execução do script.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Opcionalmente, pode especificar as definições no comando, como uma cadeia de formatação JSON. Isto permite que a configuração ser especificado durante a execução e sem um ficheiro de configuração diferente.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Exemplos da CLI do Azure

#### <a name="public-configuration-with-script-file"></a>Configuração pública com o ficheiro de script

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Comando CLI do Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-configuration-with-no-script-file"></a>Configuração pública com nenhum ficheiro de script

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Comando CLI do Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Ficheiros de configuração pública e protegido

Utilize um ficheiro de configuração pública para especificar o ficheiro de script URI. Utilize um ficheiro de configuração protegido para especificar o comando ser executado.

Ficheiro de configuração pública:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Ficheiro de configuração protegido:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Comando CLI do Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Modelo do Resource Manager
Pode executar a extensão de Script personalizado do Azure no momento da implementação de máquina virtual utilizando um modelo do Resource Manager. Para tal, adicione corretamente formatada JSON para o modelo de implementação.

### <a name="resource-manager-examples"></a>Exemplos de Gestor de recursos

#### <a name="public-configuration"></a>Configuração pública

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

#### <a name="execution-command-in-protected-configuration"></a>Comando de execução na configuração protegido

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Para obter um exemplo completado, consulte o [demonstração da loja de música .NET](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Resolução de problemas
Quando é executada a extensão de Script personalizado, o script é criado ou transferido para um diretório que é semelhante ao seguinte exemplo. A saída do comando também é guardada para este diretório no `stdout` e `stderr` ficheiros.

```bash
/var/lib/waagent/custom-script/download/0/
```

A extensão de Script do Azure produz um registo, o que pode encontrar aqui:

```bash
/var/log/azure/custom-script/handler.log
```

Também pode obter o estado de execução da extensão de Script personalizado utilizando a CLI do Azure:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

A saída aparente ser o seguinte texto:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre outras extensões de script VM, consulte [descrição geral de extensão de script do Azure para Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

