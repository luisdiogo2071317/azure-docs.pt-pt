---
title: Configuração de controladores da série N do Azure para Windows | Documentos da Microsoft
description: Como configurar controladores de NVIDIA GPU para VMs de série N que executam o Windows Server ou Windows no Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/19/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50d9ea88afc0e7d96d71b2ab26c8a8489ae41fee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442268"
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows"></a>Configurar controladores GPU para VMs de série N que executam o Windows 
Para tirar partido das capacidades de GPU de VMs de série N do Azure a executar o Windows, tem de estar instalados controladores NVIDIA GPU. O [extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) instala os controladores adequados NVIDIA CUDA ou GRADE numa VM de série N. Instalar ou gerir a extensão com o portal do Azure ou ferramentas, tais como modelos do Azure PowerShell ou do Azure Resource Manager. Consulte a [documentação da extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) para sistemas operativos suportados e os passos de implementação.

Se optar por instalar manualmente os controladores de GPU, este artigo fornece sistemas operativos suportados, drivers e passos de instalação e verificação. Informações de configuração manual de driver também estão disponíveis para [VMs do Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para especificações básicas, as capacidades de armazenamento e detalhes do disco, consulte [tamanhos de VM do Windows de GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Instalação de controladores

1. Estabeleça ligação ao ambiente de trabalho remoto para cada VM da série N.

2. Transferir, extrair e instale o controlador suportado para o seu sistema operativo do Windows.

Em VMs do Azure NV, é necessário um reinício após a instalação de driver. Em VMs do NC, não é necessário um reinício.

## <a name="verify-driver-installation"></a>Verificar a instalação de driver

Pode verificar a instalação de controladores no Gerenciador de dispositivos. O exemplo seguinte mostra uma configuração com êxito do cartão Tesla K80 numa VM de NC do Azure.

![Propriedades do controlador GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Para consultar o estado do dispositivo GPU, execute o [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) instalado com o driver o utilitário da linha de comandos.

1. Abra um prompt de comando e altere para o **C:\Program Files\NVIDIA Corporation\NVSMI** diretório.

2. Execute `nvidia-smi`. Se o controlador está instalado, verá que o resultado semelhante ao seguinte. Tenha em atenção que **GPU Util** mostra **0%** , a menos que estiver executando atualmente uma carga de trabalho GPU na VM. Sua versão do controlador e detalhes GPU podem ser diferentes daqueles mostrados.

![Estado do dispositivo NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>Conectividade de rede RDMA

Conectividade de rede RDMA pode ser ativada em VMs de série N com capacidade RDMA como NC24r implementadas no mesmo conjunto de disponibilidade ou num único grupo de colocação num conjunto de dimensionamento VM. A extensão de HpcVmDrivers deve ser adicionada ao instalar controladores de dispositivo de rede do Windows que permitem a conectividade RDMA. Para adicionar a extensão de VM a uma VM de série N com RDMA ativado, utilize [do Azure PowerShell](/powershell/azure/overview) cmdlets para o Azure Resource Manager.

Para instalar a versão mais recente 1.1 HpcVMDrivers extensão numa VM com capacidade RDMA existente com o nome myVM na região Oeste dos E.U.A.:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades para o Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

A rede RDMA suporta tráfego de Message Passing Interface (MPI) para aplicações em execução com [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) ou do Intel MPI 5.x. 


## <a name="next-steps"></a>Passos Seguintes

* Os desenvolvedores que criam aplicativos acelerado de GPU para as GPUs NVIDIA Tesla também podem transferir e instalar a versão mais recente [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Para obter mais informações, consulte a [guia de instalação CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


