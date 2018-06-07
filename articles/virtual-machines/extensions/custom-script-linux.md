---
title: Executar scripts personalizados em VMs do Linux no Azure | Microsoft Docs
description: Automatizar tarefas de configuração de VM com Linux utilizando a extensão de Script personalizado v2
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
ms.date: 04/25/2018
ms.author: danis
ms.openlocfilehash: 89b3f1184254964a32073c63de3fe69d8a51e292
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652962"
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>Utilizar o Azure personalizado Script extensão versão 2 com máquinas virtuais do Linux
A versão 2 do personalizada Script extensão transfere e executa os scripts em máquinas virtuais do Azure. Esta extensão é útil para configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Pode transferir scripts do Storage do Azure ou noutra localização acessível da internet ou pode fornecê-los para o tempo de execução da extensão. 

A extensão de Script personalizado integra-se com modelos Azure Resource Manager. Também pode executá-lo utilizando a CLI do Azure, PowerShell, o portal do Azure ou a API de REST de máquinas virtuais do Azure.

Este artigo fornece detalhes sobre como utilizar a extensão de Script personalizado da CLI do Azure e como executar a extensão utilizando um modelo Azure Resource Manager. Este artigo também fornece passos de resolução de problemas para os sistemas Linux.


Existem duas extensões de Script de Linux personalizada:
* Versão 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Versão 2 - Microsoft.Azure.Extensions.CustomScript

Mude implementações novas e existentes para utilizar em vez disso, a nova versão 2. A nova versão se destina a ser um substituto drop-in. Por conseguinte, a migração é tão fácil como alterar o nome e versão, não terá de alterar a configuração de extensão.


### <a name="operating-system"></a>Sistema Operativo

A extensão de Script personalizado para o Linux será executada a extensão de extensão suportado do SO, para obter mais informações, consulte este [artigo](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

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
  "type": "Microsoft.Compute/virtualMachines/extensions",
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
      "skipDos2Unix":false,
      "timestamp":"123456789",          
    },
    "protectedSettings": {
       "commandToExecute": "<command-to-execute>",
       "script": "<base64-script-to-execute>",
       "storageAccountName": "<storage-account-name>",
       "storageAccountKey": "<storage-account-key>",
       "fileUris": ["https://.."]  
    }
  }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | data |
| Fabricante | Microsoft.Compute.Extensions | cadeia |
| tipo | CustomScript | cadeia |
| typeHandlerVersion | 2.0 | Int |
| fileUris (por exemplo) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (por exemplo) | Python MyPythonScript.py < my param1 > | cadeia |
| Script | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo = | cadeia |
| skipDos2Unix (por exemplo) | false | boolean |
| Timestamp (por exemplo) | 123456789 | número inteiro de 32 bits |
| storageAccountName (por exemplo) | examplestorageacct | cadeia |
| storageAccountKey (por exemplo) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | cadeia |

### <a name="property-value-details"></a>Detalhes de valor de propriedade
* `skipDos2Unix`: (opcional, booleano) ignorar conversão dos2unix dos URLs de ficheiros baseado em script ou script.
* `timestamp` Este campo apenas para acionar uma volte a executar o script alterando o valor deste campo utilização de (inteiro opcional, 32 bits).  Qualquer valor de número inteiro é aceitável; só tem de ser diferente do valor anterior.
 * `commandToExecute`: (**necessário** se não definido o script, cadeia) o script de ponto de entrada para executar. Utilize este campo em vez disso, se o comando contém segredos como palavras-passe.
* `script`: (**necessário** se commandToExecute não definido, cadeia) um codificado em base64 (e opcionalmente gzip'ed) script executado por /bin/partilhar.
* `fileUris`: (opcional, matriz de cadeia) os URLs para os ficheiros a ser transferido.
* `storageAccountName`: (opcional, cadeia) o nome da conta de armazenamento. Se especificar credenciais do armazenamento, todas as `fileUris` tem de ser URLs de Blobs do Azure.
* `storageAccountKey`: (opcional, cadeia) a chave de acesso da conta de armazenamento


Os seguintes valores podem ser definidos nas definições de públicas ou protegidas, a extensão irão rejeitar qualquer configuração onde os valores abaixo são estipulados nas definições de públicas e protegidas.
* `commandToExecute`
* `script`
* `fileUris`

Utilizar definições público pode ser útil para depuração, mas é vivamente recomendado que utilize definições protegidas.

Definições de público são enviadas em texto não encriptado para a VM em que o script será executado.  Definições protegidas estão encriptadas com uma chave conhecida apenas para o Azure e a VM. As definições são guardadas para a VM que foram enviadas, ou seja, se as definições foram encriptadas que estas sejam guardadas encriptados na VM. O certificado utilizado para desencriptar os valores encriptados é armazenado na VM e, utilizado para desencriptar as definições (se necessário) em tempo de execução.

#### <a name="property-skipdos2unix"></a>Propriedade: skipDos2Unix

O valor predefinido é false, o que significa dos2unix conversão **é** foi executado.

A versão anterior do CustomScript, Microsoft.OSTCExtensions.CustomScriptForLinux, seria automaticamente para converter ficheiros ficheiros UNIX por traduzir `\r\n` para `\n`. Esta conversão ainda existe e está ativada por predefinição. Esta conversão é aplicada a todos os ficheiros transferidos da fileUris ou a definição de script com base em qualquer um dos seguintes critérios.

* Se a extensão é um dos `.sh`, `.txt`, `.py`, ou `.pl` será convertido. A definição de script corresponderá sempre este critério porque é pressupõe-se que um script executado /bin/sh e é guardada como script.sh na VM.
* Se o ficheiro for iniciado com `#!`.

A conversão de dos2unix pode ser ignorada definindo a skipDos2Unix como true.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Propriedade: script

CustomScript suporta a execução do script definidos pelo utilizador. As definições de script para combinar commandToExecute e fileUris para uma única definição. Em vez do ter de configurar um ficheiro para transferência a partir do armazenamento do Azure ou gist do GitHub, pode simplesmente de codificar o script como uma definição. Script pode ser utilizado para commandToExecute substituído e fileUris.

O script **tem** ser codificado em base64.  O script pode **opcionalmente** ser gzip'ed. A definição de script pode ser utilizada nas definições de públicas ou protegidas. O tamanho máximo de dados do parâmetro de script é 256 KB. Se o script exceder este tamanho que não serão executada.

Por exemplo, dado o seguinte script /script.sh/ o ficheiro foi guardado.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

A definição de script CustomScript correta deverá ser construída, efetuando a saída do comando seguinte.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

O script, opcionalmente, pode ser gzip'ed para reduzir o tamanho (na maioria dos casos). (CustomScript auto-Deteta a utilização da compressão de gzip.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript utiliza o seguinte algoritmo para executar um script.

 1. Assert que o comprimento do valor do script não pode exceder 256 KB.
 1. Base64 descodificar o valor do script
 1. _tentativa de_ para gunzip o base64 descodificar o valor
 1. escrever o valor descodificado (e opcionalmente descomprimido) disco (/ var/lib/waagent/custom-script/#/script.sh)
 1. Execute o script utilizar /var/lib/waagent/custom-script/#/script.sh _ / bin/ostrar - c.


## <a name="template-deployment"></a>Implementação de modelos
Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo Azure Resource Manager para executar a extensão de Script personalizado durante uma implementação de modelo Azure Resource Manager. Um modelo de exemplo que inclui a extensão de Script personalizado pode ser encontrado aqui, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


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
      },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <param2>",
      "fileUris": ["https://github.com/MyProject/Archive/MyPythonScript.py"
      ]  
    }
  }
}
```

>[!NOTE]
>Estes nomes de propriedade são sensíveis a maiúsculas. Para evitar problemas de implementação, utilize os nomes, conforme mostrado aqui.

## <a name="azure-cli"></a>CLI do Azure
Quando estiver a utilizar a CLI do Azure para executar a extensão de Script personalizado, crie um ficheiro de configuração ou de ficheiros. No mínimo, tem de ter 'commandToExecute'.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

Opcionalmente, pode especificar as definições no comando, como uma cadeia de formatação JSON. Isto permite que a configuração ser especificado durante a execução e sem um ficheiro de configuração diferente.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

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
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \ 
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Resolução de problemas
Quando é executada a extensão de Script personalizado, o script é criado ou transferido para um diretório que é semelhante ao seguinte exemplo. A saída do comando também é guardada para este diretório no `stdout` e `stderr` ficheiros.

```bash
/var/lib/waagent/custom-script/download/0/
```

Para resolver problemas, consulte o registo do agente Linux pela primeira vez, certifique-se de que executou a extensão, consulte:

```bash
/var/log/waagent.log 
```

Deve ter o aspeto da execução da extensão, será algo semelhante ao seguinte:
```text
2018/04/26 17:47:22.110231 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] [Enable] current handler state is: notinstalled
2018/04/26 17:47:22.306407 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Download, message=Download succeeded, duration=167
2018/04/26 17:47:22.339958 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Initialize extension directory
2018/04/26 17:47:22.368293 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Update settings file: 0.settings
2018/04/26 17:47:22.394482 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Install extension [bin/custom-script-shim install]
2018/04/26 17:47:23.432774 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Install, message=Launch command succeeded: bin/custom-script-shim install, duration=1007
2018/04/26 17:47:23.476151 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Enable extension [bin/custom-script-shim enable]
2018/04/26 17:47:24.516444 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Enable, message=Launch command succeeded: bin/custom-sc
```
Alguns pontos a ter em atenção:
1. Ativar é quando o comando entra em execução.
2. Transferência está relacionada com a transferência do pacote de extensão CustomScript do Azure, não os ficheiros de script especificado no fileUris.


A extensão de Script do Azure produz um registo, o que pode encontrar aqui:

```bash
/var/log/azure/custom-script/handler.log
```

Deve ter o aspeto da execução de induvidual, este será algo semelhante ao seguinte:
```text
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=start
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=pre-check
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="comparing seqnum" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="seqnum saved" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="reading configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="read configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating json schema"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="json schema valid"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsing configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsed configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating configuration logically"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validated configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="creating output directory" path=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="created output directory"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 files=1
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download start"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download complete" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing protected commandToExecute" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executed command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=enabled
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=end
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
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Passos Seguintes
Para ver o código, atuais problemas e as versões, consulte [personalizada script-extensão linux repositório](https://github.com/Azure/custom-script-extension-linux).

