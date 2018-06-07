---
title: Série N controlador a configuração do Azure para Windows | Microsoft Docs
description: Como configurar os controladores de NVIDIA GPU para VMs N série com o Windows Server ou Windows no Azure
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
ms.date: 05/29/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bc4b9cb9940f073034df01143f4d9e77a47cb19b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654390"
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows"></a>Configurar controladores GPU para VMs N série com o Windows 
Para tirar partido das funcionalidades GPU do Azure N série VMs em execução uma versão suportada do Windows Server ou Windows, os controladores da placa NVIDIA tem de estar instalados. Este artigo fornece os passos de configuração de controlador depois de implementar uma VM de série N. As informações de configuração do controlador também estão disponíveis para [VMs com Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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

Conectividade de rede RDMA pode ser ativada em VMs de série N com capacidade RDMA, tais como NC24r implementados no mesmo conjunto de disponibilidade ou num grupo de posicionamento única num conjunto de dimensionamento VM. Tem de adicionar a extensão de HpcVmDrivers para instalar controladores de dispositivos de rede do Windows que ativar a conetividade RDMA. Para adicionar a extensão VM para uma VM de série N com capacidade RDMA, utilize [Azure PowerShell](/powershell/azure/overview) cmdlets do Azure Resource Manager.

Para instalar a versão mais recente 1.1 HpcVMDrivers extensão numa VM com capacidade RDMA existente com o nome myVM na região EUA oeste:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades para o Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

A rede RDMA suporta tráfego da Interface de passagem de mensagens (MPI) para aplicações em execução com [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) ou Intel MPI 5. x. 


## <a name="next-steps"></a>Passos Seguintes

* Os programadores criar aplicações para a GPU-acelerados para GPUs de Tesla NVIDIA também podem transferir e instalar a versão mais recente [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Para obter mais informações, consulte o [guia de instalação CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


