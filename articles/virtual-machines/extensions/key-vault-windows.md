---
title: A chave de cofre extensão da VM do Windows | Microsoft Docs
description: Implemente um agente efetuar a atualização automática de segredos do Cofre de chaves em máquinas virtuais com uma extensão da máquina virtual.
services: virtual-machines-windows
documentationcenter: ''
author: dragav
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: dragosav
ms.openlocfilehash: 19e177f086ea9fa817a36e67ace77aed39ee89b5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Extensão de máquina virtual do Cofre de chaves para o Windows

## <a name="overview"></a>Descrição geral

A extensão da VM do Cofre de chave fornece a atualização automática de segredos armazenados num cofre de chaves do Azure. Especificamente, os monitores de extensão observados de uma lista de certificados armazenados no cofres de chaves e, após a detetar uma alteração, obtém e instala os certificados correspondentes. A extensão da VM do Cofre de chave é publicada e suportada pela Microsoft, atualmente em VMs do Windows. Este documento fornece detalhes sobre as plataformas suportadas, configurações e opções de implementação para a extensão da VM do Cofre de chave para o Windows. 

## <a name="prerequisites"></a>Pré-requisitos

A extensão da VM do Cofre de chave depende da extensão de VM de identidade de serviço geridas, para se autenticar perante o serviço Cofre de chaves. Consulte a documentação para a extensão da VM MSI para obter mais detalhes.

### <a name="operating-system"></a>Sistema operativo

A extensão de VM do Cofre de chave suporta atualmente todas as versões do Windows.

| Distribuição | Versão |
|---|---|
| Windows | Principal |

### <a name="internet-connectivity"></a>Conectividade Internet

A extensão da VM do Cofre de chave para o Windows requer que a máquina virtual de destino está ligada à internet. 

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a extensão da VM do Cofre de chave. A extensão não necessita de definições protegidas - todas as respetivas definições são consideradas informações sem impacto de segurança. A extensão requer uma lista de segredos monitorizados, frequência de consulta e o arquivo de certificados de destino. Especificamente:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados |
| ---- | ---- | ---- |
| apiVersion | 2016-10-01 | data |
| Fabricante | Microsoft.Azure.KeyVault.Edp | cadeia |
| tipo | KeyVaultForWindows | cadeia |
| typeHandlerVersion | 0.0 | Int |
| pollingIntervalInS | 3600 | Int |
| certificateStoreName | A MINHA | cadeia |
| certificateStoreLocation  | LOCAL_MACHINE | cadeia |
| observedCertificates  | ["https://myvault.vault.azure.net:443/secrets/mycertificate"] | Matriz de cadeia


## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. Os modelos são ideais quando implementar um ou mais máquinas virtuais que necessitam de post implementação de atualização de certificados. A extensão pode ser implementada em VMs individuais ou VM conjuntos de dimensionamento. O esquema e de configuração são comuns para ambos os tipos de modelo. 

A configuração de JSON para uma extensão da máquina virtual deve ser aninhada o fragmento de recurso de máquina virtual do modelo, especificamente `"resources": []` objeto da máquina virtual.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

## <a name="azure-powershell-deployment"></a>Implementação do PowerShell do Azure

O Azure PowerShell podem ser utilizado para implementar a extensão da VM do Cofre de chave para uma máquina virtual existente ou um conjunto de dimensionamento da máquina virtual. 

* Para implementar a extensão numa VM:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMName> + '/KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Set-AzureRmVmExtension -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Para implementar a extensão de um conjunto de dimensionamento VM, por exemplo, como parte de um cluster do Service Fabric:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMSSName> + '_KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Add-AzureRmVmssExtension -VirtualMachineScaleSet <VMSS> -Name $extName -Publisher $extPublisher -Type $extType -Setting $settings
    
    ```

. Tenha em atenção os seguintes requisitos/restrições:
- A implementação tem de ser efetuada na região Sul Central-nos
- Restrições do Cofre de chaves:
    - Tem de existir no momento da implementação 
    - Tem de estar localizado na mesma região e grupo de recursos como a implementação
    - Tem de estar ativado para implementação e a implementação de modelo

## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure

Exemplos para implementar a extensão da VM do Cofre de chave na CLI do Azure estará disponíveis em breve. 

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando o Azure PowerShell. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando com o Azure PowerShell.

```powershell
Get-AzureRMVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

Resultado da execução de extensão é registado para o ficheiro seguinte:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.Edp.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
