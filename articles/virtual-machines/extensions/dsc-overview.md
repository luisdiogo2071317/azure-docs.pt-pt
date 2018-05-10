---
title: Configuração de estado pretendido de descrição geral do Azure
description: Saiba como utilizar o processador de extensão do Microsoft Azure para PowerShell pretendido Estado Configuration (DSC). O artigo inclui os pré-requisitos, arquitetura e cmdlets.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 356356288823c86c77e46fdfba2cf81a7ef11e0d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Introdução para o processador de extensão de configuração de estado pretendido do Azure

O agente da VM do Azure e as extensões associadas são parte dos serviços de infraestrutura do Microsoft Azure. Extensões VM são os componentes de software que expandem a funcionalidade de VM e simplificam a várias operações de gestão de VM.

O principal caso de utilização para a extensão de configuração de estado pretendido ' (DSC) da Azure é de arranque de uma VM para o [serviço Automation DSC do Azure](../../automation/automation-dsc-overview.md). Fornece bootstrapping uma VM [vantagens](/powershell/dsc/metaconfig#pull-service) que incluem a gestão contínua da configuração da VM e a integração com outras ferramentas operacionais, tais como a monitorização do Azure.

Pode utilizar a extensão de DSC independentemente do serviço de DSC de automatização. No entanto, isto envolve a uma único ação que ocorre durante a implementação. Nenhuma comunicação em curso ou a gestão de configuração está disponível, excetuando localmente na VM.

Este artigo fornece informações sobre os dois cenários: através da extensão de DSC para a integração da automatização e utilizar a extensão de DSC como uma ferramenta para atribuir as configurações a VMs através do SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **Máquina local**: para interagir com a extensão de VM do Azure, tem de utilizar o portal do Azure ou o Azure PowerShell SDK.
- **O agente convidado**: A VM do Azure que está configurado na configuração de DSC tem de ser um sistema operativo que suporta o Windows Management Framework (WMF) 4.0 ou posterior. Para obter a lista completa de versões de SO suportadas, consulte o [histórico de versões de extensão de DSC](/powershell/dsc/azuredscexthistory).

## <a name="terms-and-concepts"></a>Conceitos e termos de licenciamento

Este guia pressupõe familiaridade com os seguintes conceitos:

- **Configuração**: documento de configuração de A DSC.
- **Nó**: um destino para uma configuração de DSC. Neste documento, *nó* sempre refere-se a VM do Azure.
- **Dados de configuração**: um ficheiro. psd1 que tenha dados ambientais para uma configuração.

## <a name="architecture"></a>Arquitetura

A extensão de DSC do Azure utiliza o framework de agente da VM do Azure para fornecer, enact e relatórios sobre configurações de DSC em execução em VMs do Azure. A extensão de DSC aceita um documento de configuração e um conjunto de parâmetros. Se não for fornecido nenhum ficheiro, um [predefinida de script de configuração](#default-configuration-script) está incorporada com a extensão. O script da configuração predefinida é utilizado apenas para definir os metadados em [Gestor de configuração locais](/powershell/dsc/metaconfig).

Quando a extensão é chamada pela primeira vez, instala uma versão do WMF utilizando a seguinte lógica:

- Se o SO de VM do Azure é o Windows Server 2016, não é necessária nenhuma ação. Windows Server 2016 já tem a versão mais recente do PowerShell instalada.
- Se o **wmfVersion** é especificada a propriedade e que versão do WMF está instalada, a menos que a versão é incompatível com SO a VM.
- Se não **wmfVersion** propriedade for especificada, a versão aplicável mais recente do WMF está instalada.

Instalar o WMF requer um reinício. Depois de reiniciar, a extensão transfere o ficheiro. zip que é especificado o **modulesUrl** propriedade, se fornecidas. Se esta localização é Blob storage do Azure, pode especificar um token SAS no **sasToken** propriedade para aceder ao ficheiro. Depois da. zip que é transferido e descompactado, a função de configuração definida no **configurationFunction** executa para gerar um ficheiro. MOF. A extensão, em seguida, executa `Start-DscConfiguration -Force` utilizando o ficheiro. MOF gerado. A extensão de saída de captura e escreve-o para o canal de estado do Azure.

### <a name="default-configuration-script"></a>Script de configuração predefinida

A extensão de DSC do Azure inclui um script de configuração predefinidas que tem se destina a ser utilizado quando a carregar uma VM para o serviço do Automation DSC do Azure. Os parâmetros do script estão alinhados com as propriedades configuráveis de [Gestor de configuração locais](/powershell/dsc/metaconfig). Para os parâmetros do script, consulte [predefinida de script de configuração](extensions-dsc-template.md#default-configuration-script) no [extensão de configuração de estado pretendido com modelos Azure Resource Manager](extensions-dsc-template.md). Para o script completo, consulte o [modelo de início rápido do Azure no GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>Extensão de DSC nos modelos do Resource Manager

Na maioria dos cenários, os modelos de implementação do Resource Manager são a forma esperada para trabalhar com a extensão de DSC. Para obter mais informações e para obter exemplos de como a incluir a extensão de DSC nos modelos de implementação do Resource Manager, consulte [extensão de configuração de estado pretendido com modelos Azure Resource Manager](dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>Extensão de DSC cmdlets do PowerShell

Os cmdlets do PowerShell que são utilizados para gerir a extensão de DSC são melhor utilizados na resolução de problemas interativa e cenários de recolha de informações. Pode utilizar os cmdlets para o pacote, publicar e monitorizar as implementações de extensão de DSC. Cmdlets para a extensão de DSC ainda não estão a ser atualizados para funcionar com o [predefinida de script de configuração](#default-configuration-script).

O **publicar AzureRmVMDscConfiguration** cmdlet aceita um ficheiro de configuração, analisa-lo para recursos de DSC dependentes e, em seguida, cria um ficheiro. zip. O ficheiro. zip contém a configuração e os recursos de DSC são necessárias para enact a configuração. O cmdlet também pode criar o pacote localmente ao utilizar o *- OutputArchivePath* parâmetro. Caso contrário, o cmdlet publica o ficheiro. zip ao blob storage e protege-o com um token SAS.

O script de configuração. ps1 que o cmdlet cria existe no ficheiro. zip na raiz da pasta de arquivo. A pasta do módulo é colocada na pasta de arquivo de recursos.

O **conjunto AzureRmVMDscExtension** cmdlet injects as definições que requer que a extensão de DSC do PowerShell para um objeto de configuração de VM.

O **Get-AzureRmVMDscExtension** cmdlet obtém o estado da extensão DSC de uma VM específica.

O **Get-AzureRmVMDscExtensionStatus** cmdlet obtém o estado da configuração DSC é enacted pelo processador de extensão de DSC. Esta ação pode ser efetuada numa única VM ou num grupo de VMs.

O **remover AzureRmVMDscExtension** cmdlet Remove o processador de extensão de VM específica. Este cmdlet *não* remover a configuração, desinstalar WMF ou alterar as definições aplicadas na VM. Remove apenas o processador de extensão. 

Informações importantes sobre cmdlets do Gestor de recursos do DSC extensão:

- Cmdlets do Gestor de recursos do Azure são síncronos.
- O *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *versão*, e *localização*parâmetros são todos requeridos.
- *ArchiveResourceGroupName* é um parâmetro opcional. Pode especificar este parâmetro, quando a sua conta de armazenamento pertence a um grupo de recursos diferente daquela onde será criada a VM.
- Utilize o **AutoUpdate** comutador para atualizar automaticamente o processador de extensão para a versão mais recente, quando estiver disponível. Este parâmetro tem o potencial de causar reinícios na VM quando for lançada uma nova versão do WMF.

### <a name="get-started-with-cmdlets"></a>Introdução aos cmdlets

A extensão de DSC do Azure pode utilizar os documentos de configuração de DSC configurar diretamente as VMs do Azure durante a implementação. Este passo não registar o nó de automatização. O nó é *não* gerida centralmente.

O exemplo seguinte mostra um exemplo simples de uma configuração. Guarde a configuração localmente como IisInstall.ps1.

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

Os seguintes comandos colocam o script de IisInstall.ps1 na VM especificada. Os comandos também executar a configuração e, em seguida, informar sobre o estado.

```powershell
$resourceGroup = 'dscVmDemo'
$location = 'westus'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate $true -ConfigurationName 'IISInstall'
```

## <a name="azure-portal-functionality"></a>Funcionalidade de portal do Azure

Para configurar o DSC no portal:

1. Aceda a uma VM.
2. Em **definições**, selecione **extensões**.
3. Na nova página que é criada, selecione **+ adicionar**e, em seguida, selecione **configuração de estado pretendido do PowerShell**.
4. Clique em **criar** na parte inferior da página de informações de extensão.

O portal recolhe a entrada seguinte:

- **Módulos de configuração ou Script**: Este campo é obrigatório (o formato não tiver sido atualizado para o [predefinida de script de configuração](#default-configuration-script)). Módulos de configuração e scripts necessitam de um ficheiro. ps1, que tem um script de configuração ou de um ficheiro. zip com um script de configuração. ps1 na raiz. Se utilizar um ficheiro. zip, todos os recursos dependentes têm de ser incluídos nas pastas de módulo no. zip. Pode criar o ficheiro. zip, utilizando o **publicar AzureVMDscConfiguration - OutputArchivePath** cmdlet que está incluído no SDK do Azure PowerShell. O ficheiro. zip é carregado para o armazenamento de BLOBs de utilizador e protegido por um token SAS.

- **Nome qualificado do módulo de configuração**: pode incluir várias funções de configuração num ficheiro. ps1. Introduza o nome do script. ps1 configuração seguido \\ e o nome da função de configuração. Por exemplo, se o script. ps1 configuration.ps1 o nome e a configuração é **IisInstall**, introduza **configuration.ps1\IisInstall**.

- **Configuração argumentos**: se a função de configuração necessita de argumentos, introduzi-las aqui no formato **argumentName1 = value1, argumentName2 = value2**. Este formato é um formato diferente em que configuração argumentos são aceites os cmdlets do PowerShell ou modelos do Resource Manager.

- **Ficheiro de configuração de dados PSD1**: Este campo é opcional. Se a configuração necessita de um ficheiro de dados de configuração. psd1, utilize este campo para selecionar o campo de dados e carregá-la para o armazenamento de Blobs do utilizador. O ficheiro de dados de configuração está protegido por um token SAS no blob storage.

- **Versão do WMF**: Especifica a versão do Windows Management Framework (WMF) que deve ser instalado na VM. A definição desta propriedade como mais recente instala a versão mais recente do WMF. Atualmente, os valores só possíveis para esta propriedade são 4.0, 5.0, 5.1 e mais recente. Estes valores possíveis são sujeitos a atualizações. O valor predefinido é **mais recente**.

- **Recolha de dados**: determina se a extensão irá recolher telemetria. Para obter mais informações, consulte [recolha de dados de extensão de DSC do Azure](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/).

- **Versão**: Especifica a versão da extensão DSC para instalar. Para obter informações sobre as versões, consulte [histórico de versões de extensão de DSC](/powershell/dsc/azuredscexthistory).

- **Atualizar a versão secundária de auto**: Este campo está mapeado para o **AutoUpdate** mudar os cmdlets e permite que a extensão atualizar automaticamente para a versão mais recente durante a instalação. **Sim** instruirá o processador de extensão para utilizar a versão mais recente e **não** forçará o **versão** especificado para ser instalada. Selecionar nenhum **Sim** nem **não** é o mesmo que selecionar **não**.

## <a name="logs"></a>Registos

Os registos para a extensão são armazenados na seguinte localização: `C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre o PowerShell DSC, vá para o [Centro de documentação do PowerShell](/powershell/dsc/overview).
- Examine o [modelo do Resource Manager para a extensão de DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Para obter mais funcionalidades que pode gerir utilizando o PowerShell DSC e mais recursos de DSC, procure o [galeria do PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Para obter detalhes sobre a transmitir parâmetros confidenciais para as configurações, consulte [gerir credenciais de forma segura com o processador de extensão de DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).