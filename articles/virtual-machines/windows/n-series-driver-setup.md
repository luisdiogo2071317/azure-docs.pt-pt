---
title: "Série N controlador a configuração do Azure para Windows | Microsoft Docs"
description: "Como configurar os controladores de NVIDIA GPU para VMs N série com o Windows no Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/12/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49e367d4bf1ae2e060b77b0259771403c81a56d6
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Configurar controladores GPU para VMs N série com o Windows Server
Para tirar partido das capacidades de GPU de VMs de série N do Azure com o Windows Server 2016 ou o Windows Server 2012 R2, instale controladores de gráficos NVIDIA suportados. Este artigo fornece os passos de configuração de controlador depois de implementar uma VM de série N. As informações de configuração do controlador também estão disponíveis para [VMs com Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para especificações básicas, as capacidades de armazenamento e detalhes do disco, consulte [tamanhos de VM do Windows para a GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Instalação de controlador

1. Estabelecer ligação ao ambiente de trabalho remoto para cada VM de série N.

2. Transferir, extraia e instalar o controlador suportado para o sistema operativo Windows.

Em VMs do Azure NV, é necessário um reinício após a instalação de controlador. Em VMs do NC, não é necessário um reinício.

## <a name="verify-driver-installation"></a>Certifique-se a instalação de controlador

Pode verificar a instalação de controladores no Gestor de dispositivos. O exemplo seguinte mostra a configuração com êxito do cartão Tesla K80 numa VM de NC do Azure.

![Propriedades do controlador GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Para consultar o estado do dispositivo para a GPU, execute o [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) instalado com o controlador o utilitário da linha de comandos.

1. Abra uma linha de comandos e mude para o **c:\Programas\Microsoft Files\NVIDIA Corporation\NVSMI** diretório.

2. Execute `nvidia-smi`. Se o controlador estiver instalado, verá o resultado semelhante ao seguinte. Tenha em atenção que **GPU Util** mostra **0%** , a menos que estiver a executar uma carga de trabalho para a GPU na VM. A versão do controlador e detalhes GPU poderão ser diferentes das mostrado.

![Estado do dispositivo NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>Conectividade de rede RDMA

Conectividade de rede RDMA, pode ser ativada em série N VMs o RDMA ativado como NC24r implementadas no mesmo conjunto de disponibilidade. Tem de adicionar a extensão de HpcVmDrivers para instalar controladores de dispositivos de rede do Windows que ativar a conetividade RDMA. Para adicionar a extensão VM para uma VM de série N com capacidade RDMA, utilize [Azure PowerShell](/powershell/azure/overview) cmdlets do Azure Resource Manager.

> [!NOTE]
> Atualmente, apenas o Windows Server 2012 R2 suporta a rede RDMA em série N VMs.
> 

Para instalar a versão mais recente 1.1 HpcVMDrivers extensão numa VM com capacidade RDMA existente com o nome myVM na região EUA oeste:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades para o Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

A rede RDMA suporta tráfego da Interface de passagem de mensagens (MPI) para aplicações em execução com [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) ou Intel MPI 5. x. 


## <a name="next-steps"></a>Passos Seguintes

* Os programadores criar aplicações para a GPU-acelerados para GPUs de Tesla NVIDIA também podem transferir e instalar o [CUDA Toolkit 9.1](https://developer.nvidia.com/cuda-downloads). Para obter mais informações, consulte o [guia de instalação CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


