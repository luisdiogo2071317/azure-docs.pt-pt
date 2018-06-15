---
title: Selecione as imagens de VM do Windows no Azure | Microsoft Docs
description: Saiba como utilizar o Azure PowerShell para determinar o publicador, oferta, SKU e versão para imagens de VM do Marketplace.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.openlocfilehash: 885ee10bc63b65d936f5b433a18c4435b2503720
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
ms.locfileid: "32313448"
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Como localizar as imagens de VM do Windows no Azure Marketplace com o Azure PowerShell

Este artigo descreve como utilizar o Azure PowerShell para localizar as imagens VM no Azure Marketplace. Utilize estas informações para especificar uma imagem do Marketplace, quando criar uma VM através de programação com o PowerShell, modelos do Resource Manager ou outras ferramentas.

Certifique-se de que é instalado e configurado a versão mais recente [módulo Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Tabela de imagens do Windows utilizadas normalmente
| Publicador | Oferta | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |Centro de dados de 2016 |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter com contentores |
| MicrosoftWindowsServer |WindowsServer |Servidor de Nano de 2016 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="navigate-the-images"></a>Navegue de imagens

Outra forma de encontrar uma imagem numa localização está a ser executado o [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), e [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets na sequência. Com estes comandos, determinar estes valores:

1. Listar os publicadores de imagem.
2. Para um determinado publicador, liste as respetivas ofertas.
3. Para uma determinada oferta, liste as respetivas SKUs.

Em seguida, para um SKU selecionado, executar [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) para listar as versões para implementar.

Em primeiro lugar, liste os publicadores com os seguintes comandos:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Preencha o nome do publicador escolhido e execute os seguintes comandos:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Preencha o nome da oferta escolhida e execute os seguintes comandos:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Preencha o nome SKU que escolheu e execute os seguintes comandos:

```powershell
$skuName="<SKU>"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Da saída do `Get-AzureRMVMImage` comando, pode selecionar uma imagem de versão para implementar uma nova máquina virtual.

Os comandos seguintes mostram um exemplo completo:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Saída:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Para o *MicrosoftWindowsServer* publicador:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Saída:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Para o *WindowsServer* oferecem:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Saída:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
2016-Nano-Server
```

Em seguida, para o *2016 Datacenter* SKU:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Agora pode combinar o selecionada publicador, oferta, SKU e versão num URN (valores separados por:). Passar esta URN com o `--image` parâmetro ao criar uma VM com o [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet. Lembre-se de que, opcionalmente, pode substituir o número de versão a URN por "mais recente". Esta versão é sempre a versão mais recente da imagem. Também pode utilizar o URN com o [conjunto AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet do PowerShell. 

Se implementar uma VM com um modelo do Resource Manager, defina os parâmetros de imagem no individualmente a `imageReference` propriedades. Consulte o [referência ao modelo](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]


### <a name="view-plan-properties"></a>Ver propriedades do plano

Para ver informações de plano de compra de uma imagem, execute o `Get-AzureRMVMImage` cmdlet. Se o `PurchasePlan` propriedade na saída não é `null`, a imagem tem termos tem de aceitar antes da implementação programática.  

Por exemplo, a imagem do Datacenter do Windows Server 2016 não tem termos adicionais, porque o `PurchasePlan` informações `null`:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Saída:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Executar um comando semelhante para os dados de ciência de Máquina Virtual - imagem do Windows 2016 mostra o seguinte `PurchasePlan` propriedades: `name`, `product`, e `publisher`. (Algumas imagens de ter também um `promotion code` propriedade.) Para implementar esta imagem, consulte as secções seguintes para aceitar os termos de licenciamento e ativar a implementação programática.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Saída:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Aceitar os termos

Para ver os termos de licenciamento, utilize o [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) cmdlet e passar na compra planear parâmetros. O resultado mostra se anteriormente aceitou os termos e fornece uma ligação para os termos de licenciamento para a imagem do Marketplace. Por exemplo:

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Saída:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Utilize o [conjunto AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) cmdlet para aceitar ou rejeitar os termos de licenciamento. Só tem de aceitar os termos de licenciamento, uma vez por subscrição para a imagem. Por exemplo:

```powershell

$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept

```

Saída:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : VNMTRJK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBJSS4GQ
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Implementar utilizando os parâmetros do plano de compra
Depois de a aceitar os termos de licenciamento para a imagem, pode implementar uma VM na subscrição. Como é mostrado no seguinte fragmento, utilize o [conjunto AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) cmdlet para definir as informações do plano de Marketplace para o objeto VM. Para um script completado criar definições de rede para a VM e concluir a implementação, consulte o [exemplos de script do PowerShell](powershell-samples.md).

```powershell
...
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information
$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher "imagePlanPublisher" -Product "imagePlanProduct" -Name "imagePlanName"

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "imagePublisher" -Offer "imageOffer" -Skus "imageSku" -Version "imageVersion"
...
```
Em seguida, passa a configuração de VM, juntamente com objetos de configuração de rede para o `New-AzureRmVM` cmdlet.

## <a name="next-steps"></a>Passos Seguintes
Para criar uma máquina virtual rapidamente com `New-AzureRmVM` utilizando as informações da imagem básica, consulte [criar máquina virtual do Windows com o PowerShell](quick-create-powershell.md).

Ver um exemplo de script do PowerShell para [criar uma máquina virtual totalmente configurada](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


