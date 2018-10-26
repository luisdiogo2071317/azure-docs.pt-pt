---
title: Diferenças e considerações para máquinas virtuais no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre as diferenças e considerações ao trabalhar com as máquinas virtuais no Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: cb02ecb06728f5f36a0d51a3ec22cc8ba5cb44e7
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094760"
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Considerações sobre a utilização de máquinas virtuais no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Máquinas de virtuais do Azure Stack fornecem recursos de computação a pedido e dimensionáveis. Antes de implementar máquinas virtuais (VMs), deve compreender as diferenças entre as funcionalidades de máquina virtual disponíveis no Azure Stack e o Microsoft Azure. Este artigo descreve essas diferenças e identifica as principais considerações de planeamento de implementações de máquina virtual. Para saber mais sobre das principais diferenças entre o Azure Stack e o Azure, consulte a [considerações da chave](azure-stack-considerations.md) artigo.

## <a name="cheat-sheet-virtual-machine-differences"></a>Referência rápida: diferenças de Máquina Virtual

| Funcionalidade | (Global) do Azure | Azure Stack |
| --- | --- | --- |
| Imagens de máquinas virtuais | O Azure Marketplace contém imagens que pode utilizar para criar uma máquina virtual. Consulte a [do Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) página para ver a lista de imagens que estão disponíveis no Azure Marketplace. | Por predefinição, existem não estão quaisquer imagens disponíveis no mercado do Azure Stack. O administrador da nuvem do Azure Stack deve publicar ou transferir imagens para o mercado do Azure Stack, antes dos utilizadores podem utilizá-los. |
| Tamanhos de máquinas virtuais | O Azure suporta uma grande variedade de tamanhos de máquinas virtuais. Para saber mais sobre os tamanhos disponíveis e as opções, consulte a [tamanhos de máquinas virtuais do Windows](../../virtual-machines/virtual-machines-windows-sizes.md) e [tamanhos de máquinas virtuais do Linux](../../virtual-machines/linux/sizes.md) tópicos. | O Azure Stack suporta um subconjunto de tamanhos de máquinas virtuais que estão disponíveis no Azure. Para ver a lista de tamanhos suportados, consulte a [tamanhos de máquinas virtuais](#virtual-machine-sizes) seção deste artigo. |
| Quotas de máquina virtual | [Limites de quota](../../azure-subscription-service-limits.md#service-specific-limits) são definidas pela Microsoft | O administrador da nuvem do Azure Stack tem de atribuir quotas antes que oferece as máquinas virtuais aos seus usuários. |
| Extensões da máquina virtual |O Azure suporta uma grande variedade de extensões de máquina virtual. Para saber mais sobre as extensões disponíveis, consulte a [extensões de máquina virtual e funcionalidades](../../virtual-machines/windows/extensions-features.md) artigo.| O Azure Stack suporta um subconjunto de extensões que estão disponíveis no Azure e cada um a extensão ter versões específicas. O administrador da nuvem do Azure Stack pode escolher quais as extensões para ser colocados à disposição para os seus utilizadores. Para ver a lista de extensões suportadas, consulte a [extensões de máquina virtual](#virtual-machine-extensions) seção deste artigo. |
| Rede de máquinas virtuais | Endereços IP públicos atribuídos à máquina virtual de inquilino são acessíveis através da Internet.<br><br><br>Máquinas virtuais do Azure tem um nome DNS fixo | Endereços IP públicos atribuídos a uma máquina virtual do inquilino estão acessíveis no ambiente do Kit de desenvolvimento do Azure Stack apenas. Um utilizador tem de ter acesso para o Development Kit do Azure Stack através de [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) ou [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) para ligar a uma máquina virtual que é criada no Azure Stack.<br><br>Máquinas virtuais criadas dentro de uma instância específica do Azure Stack tem um nome DNS com base no valor que é configurado pelo administrador na nuvem. |
| Armazenamento de máquina virtual | Suporta [discos geridos.](../../virtual-machines/windows/managed-disks-overview.md) | Discos geridos são suportados no Azure Stack com a versão 1808 e posterior. |
| Desempenho de discos da máquina virtual | Depende do tipo de disco e o tamanho. | Depende do tamanho VM da VM que os discos estão anexados para fazer referência a [tamanhos de máquinas virtuais suportados no Azure Stack](azure-stack-vm-sizes.md) artigo.
| Versões da API | O Azure tem sempre as versões de API mais recentes para todas as funcionalidades de máquina virtual. | O Azure Stack oferece suporte a serviços específicos do Azure e as versões de API específicas para estes serviços. Para ver a lista de versões de API suportadas, consulte a [versões de API](#api-versions) seção deste artigo. |
|Conjuntos de disponibilidade de máquinas virtuais|Vários domínios de falha (2 ou 3 por região)<br>Vários domínios de atualização<br>Suporte de disco gerenciados|Vários domínios de falha (2 ou 3 por região)<br>Vários domínios de atualização (até 20)<br>Não existe suporte de disco gerido|
|Conjuntos de dimensionamento de máquinas virtuais|Suportada de dimensionamento automático|Dimensionamento automático não suportado.<br>Adicione mais instâncias para um conjunto de dimensionamento com o portal, modelos do Resource Manager ou do PowerShell.

## <a name="virtual-machine-sizes"></a>Tamanhos de máquinas virtuais

O Azure Stack impõe limites de recursos para evitar ao longo do consumo de recursos (servidor local e o nível de serviço.) Estes limites melhorar a experiência de inquilino ao reduzir o impacto do consumo de recursos por outros inquilinos.

- Para funcionamento em rede de saída da VM, existem limites de largura de banda no local. Limites no Azure Stack são os mesmos que os limites no Azure.
- Para recursos de armazenamento, o Azure Stack implementa limites de IOPS de armazenamento para evitar básico consumo excessivo de recursos por inquilinos para acesso de armazenamento.
- Para VMs com vários discos de dados anexados, o débito máximo de cada disco de dados é de 500 IOPS para HDDs e IOPS 2300 para os SSDs.

A tabela seguinte lista as VMs que são suportadas no Azure Stack, juntamente com a respetiva configuração:

| Tipo           | Tamanho          | Intervalo de tamanhos suportados |
| ---------------| ------------- | ------------------------ |
|Fins gerais |Básico A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Fins gerais |Standard A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Fins gerais |Série D       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Fins gerais |Série Dv2     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Fins gerais |Série DS      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Fins gerais |Série DSv2    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Com otimização de memória|Série D       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Com otimização de memória|Série DS      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Com otimização de memória|Série Dv2     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Com otimização de memória|Série DSv2-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Tamanhos de máquinas virtuais e as quantidades de recursos associados são consistentes entre o Azure Stack e o Azure. Isto inclui a quantidade de memória, o número de núcleos e o tamanho/número de discos de dados que podem ser criadas. No entanto, o desempenho de VMs com o mesmo tamanho depende as características subjacentes de um ambiente específico do Azure Stack.

## <a name="virtual-machine-extensions"></a>Extensões da máquina virtual

 O Azure Stack inclui um pequeno conjunto de extensões. Atualizações e extensões adicionais estão disponíveis por meio de distribuição de mercado.

Utilize o seguinte script do PowerShell para obter a lista de extensões de máquinas virtuais que estão disponíveis no seu ambiente do Azure Stack:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>Versões da API

Funcionalidades de máquina virtual no Azure Stack suportam as seguintes versões de API:

![Tipos de recursos VM](media/azure-stack-vm-considerations/vm-resoource-types.png)

Pode utilizar o seguinte script do PowerShell para obter as versões de API para as funcionalidades de máquina virtual que estão disponíveis no seu ambiente do Azure Stack:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```

A lista de tipos de recurso suportados e as versões de API pode variar se o operador da nuvem atualiza o seu ambiente do Azure Stack para uma versão mais recente.

## <a name="windows-activation"></a>Ativação do Windows

Produtos do Windows tem de ser utilizados em conformidade com direitos de uso do produto e os termos de licenciamento da Microsoft. Utiliza o Azure Stack [ativação automática de VM](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) (AVMA) para ativar máquinas virtuais do Windows Server (VMs).

- Anfitrião de pilha do Azure ativa o Windows com chaves AVMA para o Windows Server 2016. Todas as VMs que executam o Windows Server 2012 ou posterior serão automaticamente ativadas.
- As VMs que a execução do Windows Server 2008 R2 não são automaticamente ativados e tem de ser ativado ao utilizar [a ativação da MAK](https://technet.microsoft.com/library/ff793438.aspx). Para utilizar a ativação da MAK, tem de fornecer sua própria chave de produto.

Microsoft Azure utiliza a ativação do KMS para ativar VMs do Windows. Se mover uma VM a partir do Azure Stack para o Azure e encontro ativar problemas, consulte [problemas de ativação de máquina virtual do Windows do Azure de resolução de problemas](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Informações adicionais podem ser encontradas no [falhas de ativação de resolução de problemas do Windows em VMs do Azure](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) post do blogue de equipa de suporte do Azure.

## <a name="next-steps"></a>Passos Seguintes

[Criar uma máquina virtual do Windows com o PowerShell no Azure Stack](azure-stack-quick-create-vm-windows-powershell.md)
