---
title: Extensão de Script personalizado do Azure para Windows | Microsoft Docs
description: Automatizar tarefas de configuração de VM do Windows utilizando a extensão de Script personalizado
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/24/2018
ms.author: danis
ms.openlocfilehash: 80f9ecd40c5b9504a6554b95bf374046d8253933
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809782"
---
# <a name="custom-script-extension-for-windows"></a>Extensão de Script personalizado para o Windows

A extensão de Script personalizado transfere e executa os scripts em máquinas virtuais do Azure. Esta extensão é útil para a configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Os scripts podem ser transferidos a partir do armazenamento do Azure ou do GitHub, ou fornecidos para o portal do Azure no runtime da extensão. A extensão de Script Personalizado é integrada em modelos do Azure Resource Manager, podendo também ser executada ao utilizar a CLI do Azure, PowerShell, portal do Azure ou API REST de Máquinas Virtuais do Azure.

Este documento fornece detalhes sobre como utilizar a extensão de Script personalizado com o módulo Azure PowerShell, os modelos Azure Resource Manager e detalhes de resolução de problemas de passos em sistemas Windows.

## <a name="prerequisites"></a>Pré-requisitos

> [!NOTE]  
> Não utilize a extensão de Script personalizado para executar a atualização-AzureRmVM com a mesma VM que o respectivo parâmetro, uma vez que este irá esperar em si próprio.  
>   
> 

### <a name="operating-system"></a>Sistema Operativo

A extensão de Script personalizado para o Linux será executada a extensão de extensão suportado do SO, para obter mais informações, consulte este [artigo](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Localização do script

Pode utilizar a extensão para utilizar as suas credenciais do armazenamento de Blobs do Azure, para aceder ao armazenamento de Blobs do Azure. Em alternativa, a localização do script pode ser qualquer where, desde que a VM pode encaminhar a esse ponto final, por exemplo, o GitHub, o servidor de ficheiros interna etc.


### <a name="internet-connectivity"></a>Conectividade Internet
Se precisar de transferir um script externamente, como o GitHub ou o armazenamento do Azure, em seguida, firewall/rede adicionais segurança grupo de portas tem de ser aberta. Por exemplo, se o script está localizado no armazenamento do Azure, pode permitir acesso utilizando as etiquetas de serviço do Azure NSG para [armazenamento](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Se o script está num servidor local, em seguida, ainda pode precisar de firewall/rede adicionais segurança portas de grupo tem de ser aberta.

### <a name="tips-and-tricks"></a>Sugestões e Truques
* A taxa de falhas mais elevada para esta extensão é devido a erros de sintaxe no script, teste o script é executado sem erros e também colocar registo para o script para tornar mais fácil localizar onde falha adicional.
* Escrever scripts que são idempotent, pelo que o se obter executar novamente mais do que uma vez acidentalmente, não fará com que as alterações de sistema.
* Certifique-se de que os scripts não necessitam de intervenção do utilizador quando são executadas.
* 90 minutos permitidos para o script a executar, existe mais nada resultará num aprovisionar falha da extensão.
* Não coloque reinícios dentro do script, isto causará problemas com outras extensões que estão a ser instalados e post reinício, a extensão não irá continuar após o reinício. 
* Se tiver um script que irá fazer com que um reinício, em seguida, instalar aplicações e executar scripts, etc. Deve agendar o reinício utilizando uma tarefa agendada do Windows ou utilizando ferramentas como o DSC ou Chef, extensões de Puppet.
* A extensão só será executado um script de uma só vez, se pretender executar um script em cada arranque, em seguida, tem de utilizar a extensão para criar uma tarefa agendada do Windows.
* Se pretender agendar quando um script é executado, deve utilizar a extensão para criar uma tarefa agendada do Windows. 
* Quando o script está em execução, apenas verá um Estado de extensão 'transição' do portal do Azure ou a CLI. Se pretender que as atualizações de estado mais frequentes de um script em execução, terá de criar a sua própria solução.
* A extensão de Script personalizada não nativamente suporta servidores proxy, no entanto, pode utilizar uma ferramenta de transferência de ficheiros que suporta servidores de proxy no seu script, tais como *Curl* 
* Lembre-se de que localizações de diretório predefinido não que os scripts ou comandos poderão baseiam-se, ter lógica para lidar com isto.


## <a name="extension-schema"></a>Esquema de extensão

A configuração de extensão de Script personalizado especifica coisas como a localização do script e o comando ser executado. Pode armazenar esta configuração nos ficheiros de configuração, especifique-o na linha de comandos ou especificá-la num modelo Azure Resource Manager. 

Pode armazenar dados confidenciais numa configuração protegida, que é encriptada e desencriptada apenas dentro da máquina virtual. A configuração protegida é útil quando o comando de execução inclui segredos como uma palavra-passe.

Estes itens devem ser tratados como dados confidenciais e especificados na configuração da definição protegido de extensões. Dados da definição de extensão protegido de VM do Azure é encriptados e desencriptados apenas na máquina virtual de destino.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```
**Tenha em atenção** -apenas uma versão de uma extensão pode ser instalada numa VM, um ponto no tempo, especificando o script personalizado duas vezes no mesmo modelo do Resource Manager para a mesma VM irá falhar. 

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | data |
| Fabricante | Microsoft.Compute | cadeia |
| tipo | CustomScriptExtension | cadeia |
| typeHandlerVersion | 1.9 | Int |
| fileUris (por exemplo) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| commandToExecute (por exemplo) | PowerShell sem restrições - ExecutionPolicy - configurar-música-app.ps1 de ficheiros | cadeia |
| storageAccountName (por exemplo) | examplestorageacct | cadeia |
| storageAccountKey (por exemplo) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | cadeia |

>[!NOTE]
>Estes nomes de propriedade são sensíveis a maiúsculas. Para evitar problemas de implementação, utilize os nomes, conforme mostrado aqui.

#### <a name="property-value-details"></a>Detalhes de valor de propriedade
 * `commandToExecute`: (**necessário**, cadeia) o script de ponto de entrada para executar. Utilize este campo em vez disso, se o comando contém segredos como palavras-passe, ou se o fileUris confidenciais.
* `fileUris`: (opcional, matriz de cadeia) os URLs para os ficheiros a ser transferido.
* `storageAccountName`: (opcional, cadeia) o nome da conta de armazenamento. Se especificar credenciais do armazenamento, todas as `fileUris` tem de ser URLs de Blobs do Azure.
* `storageAccountKey`: (opcional, cadeia) a chave de acesso da conta de armazenamento

Os seguintes valores podem ser definidos nas definições de públicas ou protegidas, a extensão irão rejeitar qualquer configuração onde os valores abaixo são estipulados nas definições de públicas e protegidas.
* `commandToExecute`

Utilizar definições público pode ser útil para depuração, mas é vivamente recomendado que utilize definições protegidas.

Definições de público são enviadas em texto não encriptado para a VM em que o script será executado.  Definições protegidas estão encriptadas com uma chave conhecida apenas para o Azure e a VM. As definições são guardadas para a VM que foram enviadas, ou seja, se as definições foram encriptadas que estas sejam guardadas encriptados na VM. O certificado utilizado para desencriptar os valores encriptados é armazenado na VM e, utilizado para desencriptar as definições (se necessário) em tempo de execução.

## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo Azure Resource Manager para executar a extensão de Script personalizado durante uma implementação de modelo Azure Resource Manager. Um modelo de exemplo que inclui a extensão de Script personalizado pode ser encontrado aqui, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Implementação de PowerShell

O `Set-AzureRmVMCustomScriptExtension` comando pode ser utilizado para adicionar a extensão de Script personalizado para uma máquina virtual existente. Para obter mais informações, consulte [conjunto AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Mais exemplos

### <a name="using-multiple-script"></a>Utilizar Script de vários
Neste exemplo, tem três scripts que são utilizados para construir o seu servidor, as chamadas de 'commandToExecute' o script primeiro, disponíveis opções como outros são denominados, por exemplo, pode ter um script principal que controla a execução, com o erro direita o processamento, registo e gestão do Estado.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzureRmVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Executar scripts a partir de uma partilha local
Neste exemplo, pode pretender utilizar um servidor do SMB local para a sua localização do script, tenha em atenção de que não é necessário transmitir noutra outras definições, exceto *commandToExecute*.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzureRmVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Como executar scripts personalizados mais do que uma vez com a CLI
Se pretender executar a extensão de script personalizado mais do que uma vez, só pode fazer isto nas seguintes condições:
1. O parâmetro de 'Name' de extensão é o mesmo que a implementação anterior da extensão.
2. Tem de atualizar a configuração, caso contrário, o comando será não voltar a executar, por exemplo, pode adicionar numa propriedade dinâmica para o comando, tal como um carimbo. 

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando o módulo Azure PowerShell. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Resultado da execução de extensão é registado para ficheiros encontrados no diretório de seguinte na máquina virtual de destino.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Os ficheiros especificados são transferidos para o diretório seguinte na máquina virtual de destino.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
onde `<n>` é um número inteiro decimal que pode ser alterada entre execuções da extensão.  O `1.*` valor corresponde à atual real, `typeHandlerVersion` valor da extensão.  Por exemplo, poderia ser o diretório real `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Ao executar o `commandToExecute` comando, a extensão define neste diretório (por exemplo, `...\Downloads\2`) como o diretório de trabalho atual. Isto permite a utilização de caminhos relativos para localizar os ficheiros transferidos através de `fileURIs` propriedade. Consulte a tabela abaixo para obter exemplos.

Uma vez que o caminho de transferência absoluto pode variar devido ao longo do tempo, é melhor optar ativamente por participar para caminhos de script/ficheiro relativo no `commandToExecute` string, sempre que possível. Por exemplo:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Informações de caminho após o primeiro segmento URI é mantido para os ficheiros transferidos através de `fileUris` lista de propriedades.  Como é mostrado na tabela abaixo, os ficheiros transferidos estão mapeados em subdiretórios de transferência para refletir a estrutura do `fileUris` valores.  

#### <a name="examples-of-downloaded-files"></a>Exemplos de ficheiros transferidos

| URI no fileUris | Caminho relativo de localização de transferência | Localização de transferência absoluto * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Como acima, os caminhos de diretório absoluto alteram ao longo da duração da VM, mas não uma execução única da extensão CustomScript.

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
