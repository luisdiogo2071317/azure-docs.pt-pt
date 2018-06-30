---
title: Executar scripts personalizados em VMs do Linux no Azure | Microsoft Docs
description: Automatizar tarefas de configuração de VM com Linux utilizando a extensão de Script personalizado v1
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/25/2018
ms.author: danis
ms.openlocfilehash: 526021ca238be7bc934e639c34d3e49879279a6a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127657"
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>Utilizar o Azure personalizada Script extensão versão 1 com máquinas virtuais do Linux
O Script de personalizado extensão versão 1 transfere e executa os scripts em máquinas virtuais do Azure. Esta extensão é útil para configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Pode transferir scripts do Storage do Azure ou noutra localização acessível da internet ou pode fornecê-los para o tempo de execução da extensão. 

A extensão de Script personalizado integra-se com modelos Azure Resource Manager. Também pode executá-lo utilizando a CLI do Azure, PowerShell, o portal do Azure ou a API de REST de máquinas virtuais do Azure.

Este artigo fornece detalhes sobre como utilizar a extensão de Script personalizado da CLI do Azure e como executar a extensão utilizando um modelo Azure Resource Manager. Este artigo também fornece passos de resolução de problemas para os sistemas Linux.


Existem duas extensões de Script de Linux personalizada:
* Versão 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Versão 2 - Microsoft.Azure.Extensions.CustomScript

Mude implementações novas e existentes para utilizar a nova versão ([Microsoft.Azure.Extensions.CustomScript](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-linux)) em vez disso. A nova versão se destina a ser um substituto drop-in. Por conseguinte, a migração é tão fácil como alterar o nome e versão, não terá de alterar a configuração de extensão.

 

### <a name="operating-system"></a>Sistema Operativo
Distribuições Linux suportadas:

- CentOS 6.5 e superiores
- Debian 8 e superior
    - Debian 8.7 não são enviados juntamente com Python2 nas imagens mais recentes, que quebra CustomScriptForLinux.
- FreeBSD
- OpenSUSE 13.1 e superior
- Oracle Linux 6.4 e superior
- SUSE Linux Enterprise Server 11 SP3 e superiores
- Ubuntu 12.04 e superior

### <a name="script-location"></a>Localização do script

Pode utilizar a extensão para utilizar as suas credenciais do armazenamento de Blobs do Azure, para aceder ao armazenamento de Blobs do Azure. Em alternativa, a localização do script pode ser qualquer where, desde que a VM pode encaminhar a esse ponto final, por exemplo, o GitHub, o servidor de ficheiros interna etc.

### <a name="internet-connectivity"></a>Conectividade Internet
Se precisar de transferir um script externamente, como o GitHub ou o armazenamento do Azure, em seguida, firewall/rede adicionais segurança grupo de portas tem de ser aberta. Por exemplo, se o script está localizado no armazenamento do Azure, pode permitir acesso utilizando as etiquetas de serviço do Azure NSG para [armazenamento](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

Se o script está num servidor local, em seguida, ainda pode precisar de firewall/rede adicionais segurança portas de grupo tem de ser aberta.

### <a name="tips-and-tricks"></a>Sugestões e Truques
* A taxa de falhas mais elevada para esta extensão é devido a erros de sintaxe no script, teste o script é executado sem erros e também colocar registo para o script para tornar mais fácil localizar onde falha adicional.
* Escrever scripts que são idempotent, pelo que o se obter executar novamente mais do que uma vez acidentalmente, não fará com que as alterações de sistema.
* Certifique-se de que os scripts não necessitam de intervenção do utilizador quando são executadas.
* 90 minutos permitidos para o script a executar, existe mais nada resultará num aprovisionar falha da extensão.
* Não coloque reinícios dentro do script, isto causará problemas com outras extensões que estão a ser instalados e post reinício, a extensão não irá continuar após o reinício. 
* Se tiver um script que irá fazer com que um reinício, em seguida, instalar aplicações e executar scripts, etc. Deve agendar o reinício de uma tarefa de Cron, usar ou as ferramentas, como o DSC ou Chef, extensões de Puppet.
* A extensão só será executado um script de uma só vez, se pretender executar um script em cada arranque, em seguida, pode utilizar [nuvem init imagem](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/using-cloud-init) e utilizar um [Scripts por arranque](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) módulo. Em alternativa, pode utilizar o script para criar uma unidade de serviço Systemd.
* Se pretender agendar quando um script é executado, deve utilizar a extensão para criar uma tarefa de Cron. 
* Quando o script está em execução, apenas verá um Estado de extensão 'transição' do portal do Azure ou a CLI. Se pretender que as atualizações de estado mais frequentes de um script em execução, terá de criar a sua própria solução.
* A extensão de Script personalizada não nativamente suporta servidores proxy, no entanto, pode utilizar uma ferramenta de transferência de ficheiros que suporta servidores de proxy no seu script, tais como *Curl*. 
* Lembre-se de que localizações de diretório predefinido não que os scripts ou comandos poderão baseiam-se, ter lógica para lidar com isto.



## <a name="extension-schema"></a>Esquema de extensão

A configuração de extensão de Script personalizado especifica coisas como a localização do script e o comando ser executado. Pode armazenar esta configuração nos ficheiros de configuração, especifique-o na linha de comandos ou especificá-la num modelo Azure Resource Manager. 

Pode armazenar dados confidenciais numa configuração protegida, que é encriptada e desencriptada apenas dentro da máquina virtual. A configuração protegida é útil quando o comando de execução inclui segredos como uma palavra-passe.

Estes itens devem ser tratados como dados confidenciais e especificados na configuração da definição protegido de extensões. Dados da definição de extensão protegido de VM do Azure é encriptados e desencriptados apenas na máquina virtual de destino.

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
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "<url>"
      ],
      "enableInternalDNSCheck": true
    },
    "protectedSettings": {
      "storageAccountName": "<storage-account-name>",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "<command>"
    }
  }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | data |
| Fabricante | Microsoft.OSTCExtensions | cadeia |
| tipo | CustomScriptForLinux | cadeia |
| typeHandlerVersion | 1.5 | Int |
| fileUris (por exemplo) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (por exemplo) | Python MyPythonScript.py < my param1 > | cadeia |
| enableInternalDNSCheck | true | boolean |
| storageAccountName (por exemplo) | examplestorageacct | cadeia |
| storageAccountKey (por exemplo) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | cadeia |

### <a name="property-value-details"></a>Detalhes de valor de propriedade
* `fileUris`: (opcional, matriz de cadeia) a lista de uri de scripts
* `enableInternalDNSCheck`: (opcional, bool) predefinido é verdadeiro, definido como falso para desativar a verificação DNS.
* `commandToExecute`: (opcional, cadeia) o script de ponto de entrada para executar
* `storageAccountName`: (opcional, cadeia) o nome da conta de armazenamento
* `storageAccountKey`: (opcional, cadeia) a chave de acesso da conta de armazenamento

Os seguintes valores podem ser definidos nas definições de públicas ou protegidas, não pode ter estes valores abaixo definido nas definições públicas e protegidas.
* `commandToExecute`

Utilizar definições público pode ser útil para depuração, mas é vivamente recomendado que utilize definições protegidas.

Definições de público são enviadas em texto não encriptado para a VM em que o script será executado.  Definições protegidas estão encriptadas com uma chave conhecida apenas para o Azure e a VM. As definições são guardadas para a VM que foram enviadas, ou seja, se as definições foram encriptadas que estas sejam guardadas encriptados na VM. O certificado utilizado para desencriptar os valores encriptados é armazenado na VM e, utilizado para desencriptar as definições (se necessário) em tempo de execução.


## <a name="template-deployment"></a>Implementação de modelos
Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo Azure Resource Manager para executar a extensão de Script personalizado durante uma implementação de modelo Azure Resource Manager. 


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
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": ["http://MyAccount.blob.core.windows.net/vhds/MyShellScript.sh"]
    },
    "protectedSettings": {
      "storageAccountName": "MyAccount",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "sh MyShellScript.sh"
    }
  }
}
```

>[!NOTE]
>Estes nomes de propriedade são sensíveis a maiúsculas. Para evitar problemas de implementação, utilize os nomes, conforme mostrado aqui.

## <a name="azure-cli"></a>CLI do Azure
Quando estiver a utilizar a CLI do Azure para executar a extensão de Script personalizado, crie um ficheiro de configuração ou de ficheiros. No mínimo, tem de ter 'commandToExecute'.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

Opcionalmente, pode especificar as definições no comando, como uma cadeia de formatação JSON. Isto permite que a configuração ser especificado durante a execução e sem um ficheiro de configuração diferente.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Exemplos da CLI do Azure

#### <a name="public-configuration-with-no-script-file"></a>Configuração pública com nenhum ficheiro de script

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Comando CLI do Azure:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Ficheiros de configuração pública e protegido

Utilize um ficheiro de configuração pública para especificar o ficheiro de script URI. Utilize um ficheiro de configuração protegido para especificar o comando ser executado.

Ficheiro de configuração pública:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"]
}
```

Ficheiro de configuração protegido:  

```json
{
  "commandToExecute": "./config-music.sh <param1>"
}
```

Comando CLI do Azure:

```azurecli
az vm extension set 
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Resolução de problemas
Quando é executada a extensão de Script personalizado, o script é criado ou transferido para um diretório que é semelhante ao seguinte exemplo. A saída do comando também é guardada para este diretório no `stdout` e `stderr` ficheiros. 

```bash
/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1
```

Para resolver problemas, consulte o registo do agente Linux pela primeira vez, certifique-se de que executou a extensão, consulte:

```bash
/var/log/waagent.log 
```

Deve ter o aspeto da execução da extensão, será algo semelhante ao seguinte:
```text
2018/04/26 15:29:44.835067 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Target handler state: enabled
2018/04/26 15:29:44.867625 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] [Enable] current handler state is: notinstalled
2018/04/26 15:29:44.959605 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Download, message=Download succeeded, duration=59
2018/04/26 15:29:44.993269 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Initialize extension directory
2018/04/26 15:29:45.022972 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Update settings file: 0.settings
2018/04/26 15:29:45.051763 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Install extension [customscript.py -install]
2018/04/26 15:29:45 CustomScriptForLinux started to handle.
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] cwd is /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Change log file to /var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log
2018/04/26 15:29:46.088212 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Install, message=Launch command succeeded: customscript.py -install, duration=1005
2018/04/26 15:29:46.133367 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable extension [customscript.py -enable]
2018/04/26 15:29:46 CustomScriptForLinux started to handle.
..
2018/04/26 15:29:47.178163 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Enable, message=Launch command succeeded: customscript.py -enable, duration=1012
```
Alguns pontos a ter em atenção:
1. Ativar é quando o comando entra em execução.
2. Transferência está relacionada com a transferência do pacote de extensão CustomScript do Azure, não os ficheiros de script especificado no fileUris.
3. Também pode ver o ficheiro de registo-lo é ao escrever no, '/var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log'.

Passo seguinte consiste em Ir uma verificação do ficheiro de registo, este é o formato:
```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Deve ter o aspeto da execução de induvidual, este será algo semelhante ao seguinte:
```text
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable,transitioning,0,Launching the script...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] sequence number is 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] setting file path is/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/config/0.settings
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] JSON config: {"runtimeSettings": [{"handlerSettings": {"protectedSettings": "MIIB0AYJKoZIhvcNAQcDoIIBwTCCAb0CAQAxggF+hnEXRtFKTTuKiFC8gTfHKupUSs7qI0zFYRya", "publicSettings": {"fileUris": ["https://dannytesting.blob.core.windows.net/demo/myBash.sh"]}, "protectedSettingsCertThumbprint": "4385AB21617C2452FF6998C0A37F71A0A01C8368"}}]}
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Config decoded correctly.
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Will try to download files, number of retries = 10, wait SECONDS between retrievals = 20s
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Downloading,transitioning,0,Downloading files...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] No azure storage account and key specified in protected settings. Downloading scripts from external links...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Converting /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh from DOS to Unix formats: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Removing BOM of /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Succeeded to download files, retry count = 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Internal DNS is ready, retry count = 0
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Daemon,success,0,Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 
```
Aqui pode ver:
* O comando de ativação a partir é este registo
* As definições transmitidas para a extensão
* A transferir o ficheiro e o resultado da que a extensão.
* O comando que está a ser executado e o resultado.

Também pode obter o estado de execução da extensão de Script personalizado utilizando a CLI do Azure:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

A saída aparente ser o seguinte texto:

```azurecli
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Passos Seguintes
Para ver o código, atuais problemas e as versões, consulte [repositório de extensão CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript).

