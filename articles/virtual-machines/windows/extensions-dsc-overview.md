---
title: "Configuração para descrição geral do Azure do estado pretendido | Microsoft Docs"
description: "Descrição geral para utilizar o processador de extensão do Microsoft Azure para a configuração de estado pretendido do PowerShell. Incluindo a pré-requisitos, arquitetura, os cmdlets..."
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Introdução para o processador de extensão de configuração de estado pretendido do Azure

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

O agente da VM do Azure e as extensões associadas fazem parte dos serviços de infraestrutura do Microsoft Azure.
Extensões de VM são os componentes de software que expandem as funcionalidades VM e simplificam a várias operações de gestão de VM.

O principal caso de utilização para a extensão de configuração de estado pretendido é arranque de uma VM para o [serviço Automation DSC do Azure](../../automation/automation-dsc-overview.md) que fornece [vantagens](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) , incluindo a gestão contínua da configuração de VM e a integração com outras ferramentas operacionais, como a monitorização do Azure.

Também é possível utilizar a extensão de DSC independentemente do serviço do Automation DSC do Azure, no entanto, esta é uma ação único que ocorre durante a implementação e não existe nenhuma comunicação em curso ou a gestão da configuração diferente localmente dentro da VM.

Este artigo contém informações sobre cenários, DSC extensão para a integração da automatização do Azure e como utilizar a extensão de DSC como uma ferramenta para atribuição de configurações para máquinas virtuais utilizando o SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **Máquina local** - para interagir com a extensão de VM do Azure, tem de utilizar o portal do Azure ou o Azure PowerShell SDK.
- **O agente convidado** -a VM do Azure que está configurado na configuração de DSC tem de ser um sistema operativo que suporta o Windows Management Framework (WMF) 4.0 ou posterior. A lista completa de versões de SO suportadas pode ser encontrada em página [histórico de versões de extensão de DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Conceitos e termos de licenciamento

Este guia presumes familiaridade com os seguintes conceitos:

- **Configuração** -documento de configuração de A DSC.
- **Nó** -um destino para uma configuração de DSC. Neste documento, "nó" sempre refere-se para uma VM do Azure.
- **Dados de configuração** - um ficheiro. psd1 que contenha dados ambientais para uma configuração.

## <a name="architectural-overview"></a>Descrição geral da arquitetura

A extensão de DSC do Azure utiliza o framework de agente da VM do Azure para fornecer, enact e relatórios sobre configurações de DSC em execução em VMs do Azure.
A extensão de DSC aceita um documento de configuração e um conjunto de parâmetros.
Se não for fornecido nenhum ficheiro, um [predefinida de script de configuração](###default-configuration-script) está incorporada com a extensão que é utilizada apenas para a definição de metadados no [Gestor de configuração locais](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).

Quando a extensão é chamada pela primeira vez, é instalada uma versão do Windows Management Framework (WMF) utilizando a seguinte lógica:

1. Se o SO de VM do Azure é o Windows Server 2016, não é necessária nenhuma ação. Windows Server 2016 já tem a versão mais recente do PowerShell instalada.
2. Se o `wmfVersion` é especificada a propriedade e que versão do WMF está instalada, a menos que é incompatível com SO a VM.
3. Se não `wmfVersion` propriedade for especificada, a versão mais recente aplicável do WMF está instalada.

Instalação do WMF requer um reinício.
Após o reinício, a extensão transfere o ficheiro. zip especificado no `modulesUrl` propriedade se fornecido.
Se esta localização no armazenamento de Blobs do Azure, um token SAS pode ser especificado o `sasToken` propriedade para aceder ao ficheiro.
Depois da. zip que é transferido e descompactado, a função de configuração definida no `configurationFunction` é executado para gerar um ficheiro MOF.
A extensão, em seguida, executa `Start-DscConfiguration -Force` utilizando o ficheiro MOF gerado.
A extensão de saída de captura e escreve-o para o canal de estado do Azure.

### <a name="default-configuration-script"></a>Script de configuração predefinida

A extensão de DSC do Azure inclui uma configuração predefinida script se destina a ser utilizado quando a integração uma VM para o serviço do Automation DSC do Azure.
Os parâmetros do script estão alinhados com as propriedades configuráveis de [Gestor de configuração locais](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).
Os parâmetros do script são [documentados](extensions-dsc-template.md##default-configuration-script) e o script completo está disponível no [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-arm-templates"></a>Extensão de DSC nos modelos ARM

Os modelos de implementação do Azure Resource Manager (ARM) são a forma esperada para trabalhar com a extensão de DSC na maior parte dos cenários.
Para obter informações e exemplos, incluindo a extensão de DSC nos modelos de implementação do ARM, consulte a página de documentação dedicado [extensão de configuração de estado pretendido com modelos Azure Resource Manager](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>Cmdlets do PowerShell de extensão de DSC

Os cmdlets do PowerShell para gerir a extensão de DSC são melhor utilizados para interativa de resolução de problemas e cenários de recolha de informações.
Os cmdlets pode ser utilizados para o pacote, publicar e monitorizar as implementações de extensão de DSC.
Tenha em atenção que os cmdlets para a extensão de DSC ainda não foram atualizados para trabalhar com o [Script de configuração predefinido](###default-configuration-script).

`Publish-AzureRMVMDscConfiguration`aceita um ficheiro de configuração, analisa-lo para recursos de DSC dependentes e cria um ficheiro. zip que contém a configuração e os recursos de DSC necessários para enact a configuração.
Também pode criar o pacote localmente, utilizando o `-ConfigurationArchivePath` parâmetro.
Caso contrário, publica o ficheiro. zip no blob storage do Azure e protege-lo com um token SAS.

O ficheiro. zip criado por este cmdlet tem o script de configuração. ps1, na raiz da pasta de arquivo.
Recursos tem a pasta de módulo colocada na pasta de arquivo.

`Set-AzureRMVMDscExtension`injects as definições necessárias para a extensão de DSC do PowerShell para um objeto de configuração de VM.

`Get-AzureRMVMDscExtension`obtém o estado da extensão DSC de uma VM específica.

`Get-AzureRMVMDscExtensionStatus`obtém o estado da configuração DSC enacted pelo processador de extensão de DSC.
Esta ação pode ser efetuada num única VM, ou grupo de VMs.

`Remove-AzureRMVMDscExtension`Remove o processador de extensão de uma máquina virtual especificada.
Este cmdlet **não** remover a configuração, desinstale o WMF ou alterar as definições aplicadas na máquina virtual.
Remove apenas o processador de extensão. 

Informações importantes sobre os cmdlets de extensão de DSC AzureRM:

- Cmdlets do Gestor de recursos do Azure são síncronos.
- ResourceGroupName, VMName, ArchiveStorageAccountName, versão e a localização estão todos os parâmetros necessários.
- ArchiveResourceGroupName é um parâmetro opcional. Pode especificar este parâmetro, quando a sua conta de armazenamento pertence a um grupo de recursos diferente daquela onde a máquina virtual é criada.
- O comutador de AutoUpdate permite que a atualização automática do processador de extensão para a versão mais recente como e quando estiver disponível. Nota que este parâmetro tem o potencial de causar reinicia a VM quando for lançada uma nova versão do WMF.

### <a name="getting-started-with-cmdlets"></a>Introdução aos Cmdlets

A extensão de DSC do Azure está apto a utilizar os documentos de configuração de DSC diretamente para configurar as VMs do Azure durante a implementação, embora isto não irá registar o nó a automatização do Azure para o nó será **não*-geridas centralmente.

Segue-se um exemplo simple de uma configuração.
Guarde-o localmente como "IisInstall.ps1":

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Os seguintes passos colocar o script de IisInstall.ps1 na VM especificada, executar a configuração e devolver relatórios sobre o estado.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Funcionalidade de Portal do Azure

Navegue para uma VM. Em Definições -> geral clique em "As extensões."
É criado um novo painel.
Clique em "Adicionar" e selecione o PowerShell DSC.

O portal tem de entrada.
**Módulos de configuração ou Script**: Este campo é obrigatório (o formato não tiver sido atualizado para o [Script de configuração predefinido](###default-configuration-script).
Necessita de um ficheiro. ps1, que contém um script de configuração ou um ficheiro. zip com um script de configuração. ps1 no raiz e todos os recursos dependentes, nas pastas de módulo dentro de. zip.
Podem ser criado com o `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet incluído no SDK do PowerShell do Azure.
O ficheiro. zip é carregado para o armazenamento de BLOBs de utilizador protegido por um token SAS.

**Ficheiro de configuração de dados PSD1**: Este campo é opcional.
Se a configuração necessita de um ficheiro de dados de configuração. psd1, utilize este campo para selecioná-lo e carregá-la para o armazenamento de BLOBs de utilizador, onde é protegida por um token SAS.

**Nome qualificado do módulo de configuração**:. ps1 ficheiros podem ter várias funções de configuração.
Introduza o nome do script. ps1 configuração seguido por um '\' e o nome da função de configuração.
Por exemplo, se o script. ps1 tem o nome "configuration.ps1" e a configuração é "IisInstall", deve introduzir:`configuration.ps1\IisInstall`

**Configuração argumentos**: se a função de configuração necessita de argumentos, introduzi-las a sessão aqui no formato `argumentName1=value1,argumentName2=value2`.
Tenha em atenção de que este formato é um formato diferente que como argumentos de configuração são aceites através de cmdlets do PowerShell ou modelos do Resource Manager.

## <a name="logging"></a>Registo
Os registos são colocados em:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o PowerShell DSC, [visitar o Centro de documentação do PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Examine o [modelo Azure Resource Manager para a extensão de DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Para encontrar funcionalidades adicionais que pode ser geridas com o PowerShell DSC, [procurar na galeria do PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mais recursos de DSC.

Para obter detalhes sobre a transmitir parâmetros confidenciais para as configurações, consulte [gerir credenciais de forma segura com o processador de extensão de DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
