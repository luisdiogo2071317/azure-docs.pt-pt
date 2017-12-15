---
title: "Extensão de VM de diagnóstico de desempenho do Azure para Windows | Microsoft Docs"
description: "Apresenta a extensão de VM de diagnóstico de desempenho do Azure para Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: 5a7dc313f1d6453562e4d5a11ceca03e4459b043
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Extensão de VM de diagnóstico de desempenho do Azure para Windows

Extensão de VM de diagnóstico de desempenho do Azure ajuda a recolher dados de diagnóstico de desempenho de VMs do Windows. A extensão efetua a análise e fornece um relatório de findings e recomendações para identificar e resolver problemas de desempenho na máquina virtual. Esta extensão instala uma ferramenta de resolução de problemas chamada [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Pré-requisitos

Esta extensão pode ser instalada no Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016. Também pode ser instalado no Windows 8.1 e Windows 10.

## <a name="extension-schema"></a>Esquema de extensão
O seguinte JSON mostra o esquema de extensão de VM de diagnóstico de desempenho do Azure. Esta extensão necessita do nome e a chave para uma conta do storage armazenar os resultados de diagnóstico e o relatório. Estes valores são sensíveis e devem ser armazenados no interior de uma configuração de definição protegido. Dados de definição de extensão protegido de VM do Azure são encriptados e apenas é desencriptado na máquina virtual de destino. Tenha em atenção que **storageAccountName** e **storageAccountKey** diferenciam maiúsculas de minúsculas. Outros parâmetros necessários estão listados na secção seguinte.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Valores de propriedade

|   **Nome**   |**Valor / exemplo**|       **Descrição**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|A versão da API.
|Fabricante|Microsoft.Azure.Performance.Diagnostics|O espaço de nomes de publicador para a extensão.
|tipo|AzurePerformanceDiagnostics|O tipo de extensão de VM.
|typeHandlerVersion|1.0|A versão de processador de extensão.
|performanceScenario|básica|O cenário de desempenho para o qual pretende capturar os dados. Os valores válidos são: **básico**, **vmslow**, **azurefiles**, e **personalizado**.
|traceDurationInSeconds|300|A duração de rastreios, se qualquer uma das opções de rastreio estão selecionadas.
|perfCounterTrace|P|Opção para ativar o rastreio de contador de desempenho. Os valores válidos são **p** ou valor de vazio. Se não pretender capturar este rastreio, deixe o valor como vazio.
|networkTrace|n|Opção para ativar o rastreio de rede. Os valores válidos são  **n**  ou valor de vazio. Se não pretender capturar este rastreio, deixe o valor como vazio.
|xperfTrace|x|Opção para ativar o rastreio XPerf. Os valores válidos são **x** ou valor de vazio. Se não pretender capturar este rastreio, deixe o valor como vazio.
|storPortTrace|s|Opção para ativar o rastreio StorPort. Os valores válidos são **s** ou valor de vazio. Se não pretender capturar este rastreio, deixe o valor como vazio.
|srNumber|123452016365929|O suporte número da permissão, se disponível. Deixe o valor como vazio se não o tiver.
|storageAccountName|mystorageaccount|O nome da conta do storage para armazenar os registos de diagnóstico e os resultados.
|storageAccountKey|lDuVvxuZB28NNP … hAiRF3voADxLBTcc = =|A chave da conta do storage.

## <a name="install-the-extension"></a>Instalar a extensão

Siga estes passos para instalar a extensão em máquinas virtuais do Windows:

1. Inicie sessão no [Portal do Azure](http://portal.azure.com).
2. Selecione a máquina virtual onde pretende instalar esta extensão.

    ![Portal de captura de ecrã do Azure, com as máquinas virtuais realçado](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Selecione o **extensões** painel e selecione **adicionar**.

    ![Painel de captura de ecrã de extensões, com adicionar realçada](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Selecione **diagnósticos de desempenho do Azure**, reveja os termos e condições e selecione **criar**.

    ![Captura de ecrã do novo ecrã de recursos, com o Azure Diagnostics de desempenho realçados](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Forneça os valores de parâmetros para a instalação e selecione **OK** instalar a extensão. Para obter mais informações sobre cenários suportados, consulte [como utilizar PerfInsights](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Captura de ecrã de instalar caixa de diálogo de extensão](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Quando a instalação for bem sucedida, verá uma mensagem a indicar este estado.

    ![Mensagem de captura de ecrã de aprovisionamento com êxito](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > A extensão é executada quando o aprovisionamento é concluída com êxito. Demora dois minutos ou menos para concluir o cenário básico. Para obter outros cenários, é executada através de duração especificada durante a instalação.

## <a name="remove-the-extension"></a>Remova a extensão
Para remover a extensão de uma máquina virtual, siga estes passos:

1. Iniciar sessão para o [portal do Azure](http://portal.azure.com), selecione a máquina virtual a partir do qual pretende remover esta extensão e, em seguida, selecione o **extensões** painel. 
2. Selecione o (**...** ) para a entrada de extensão de diagnóstico de desempenho na lista e selecione **desinstalação**.

    ![Painel de captura de ecrã de extensões, com a desinstalação realçada](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Também pode selecionar a entrada de extensão e selecione o **desinstalação** opção.

## <a name="template-deployment"></a>Implementação de modelos
Extensões de máquina virtual do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior, pode ser utilizado num modelo Azure Resource Manager. Isto é executado a extensão de VM de diagnóstico de desempenho do Azure durante uma implementação de modelo Azure Resource Manager. Segue-se um modelo de exemplo:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>Implementação de PowerShell
O `Set-AzureRmVMExtension` comando pode ser utilizado para implementar a extensão de VM de diagnóstico de desempenho do Azure para uma máquina virtual existente. Antes de executar o comando, armazene as configurações de públicas e privadas numa tabela hash do PowerShell.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informações sobre os dados capturados
A ferramenta de PerfInsights recolhe vários registos, configuração e dados de diagnóstico, consoante o cenário selecionado. Para obter mais informações, consulte o [PerfInsights documentação](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Ver e partilhar os resultados

Resultado da extensão do é armazenado numa pasta. A pasta é denominada log_collection e pode ser encontrada na unidade temporária (normalmente D:\log_collection) por predefinição. Esta pasta, pode ver ficheiros zip que contém os registos de diagnóstico e um relatório com findings e recomendações.

Também pode encontrar o ficheiro zip na conta de armazenamento que indicou durante a instalação. São partilhados por 30 dias utilizando [assinaturas de acesso partilhado (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Um ficheiro de texto com o nome *zipfilename*_saslink.txt também é criado na pasta log_collection. Este ficheiro contém a ligação SAS criada para transferir o ficheiro zip. Qualquer pessoa que tenha esta ligação é capaz de transferir o ficheiro zip.

Para ajudar o engenheiro de suporte a trabalhar num pedido de suporte, Microsoft poderá utilizar esta ligação SAS para transferir os dados de diagnóstico.

Para ver o relatório, a extrair o ficheiro zip e abra o **PerfInsights Report.html** ficheiro.

Também poderá transferir o ficheiro zip diretamente a partir do portal, selecionando a extensão.

![Estado detalhado da captura de ecrã do diagnóstico de desempenho](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> A ligação SAS apresentada no portal do poderá não funcionar. Isto pode ser causado por um URL incorreto durante as operações de codificação e descodificação. Em vez disso, pode obter a ligação diretamente a partir do ficheiro *_saslink.txt da VM.

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

- Estado de implementação da extensão (na área de notificação) poderá mostrar "Em curso a implementação", apesar da extensão é aprovisionada com êxito.

    Este problema pode ser ignorado com segurança, desde que o estado da extensão indica que a extensão é aprovisionada com êxito.
- Pode resolver alguns problemas durante a instalação utilizando os registos de extensão. Resultado da execução de extensão é registado para ficheiros encontrados no diretório seguinte:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/)e selecione **obter suporte**. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
