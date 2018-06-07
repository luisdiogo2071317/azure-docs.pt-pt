---
title: Série N controlador a configuração do Azure para Linux | Microsoft Docs
description: Como configurar os controladores de NVIDIA GPU para VMs de série N executar Linux no Azure
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 807af10c0655d9d1728a80a47d1f8f9c2a16fb84
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654288"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Instalar controladores de NVIDIA GPU em VMs de série N executar Linux

Para tirar partido das funcionalidades GPU do Azure N série as VMs com Linux, os controladores da placa NVIDIA tem de estar instalados. Este artigo fornece os passos de configuração de controlador depois de implementar uma VM de série N. As informações de configuração do controlador também estão disponíveis para [VMs do Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Para a série N VM especificações, as capacidades de armazenamento e detalhes do disco, consulte [tamanhos de VM de Linux GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-on-n-series-vms"></a>Instalar controladores CUDA em série N VMs

Seguem-se passos para instalar controladores CUDA a partir do Toolkit de CUDA NVIDIA em série N VMs. 


Os programadores C e C++, opcionalmente, podem instalar o Toolkit de completo para criar aplicações acelerados de GPU. Para obter mais informações, consulte o [guia de instalação CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

Para instalar controladores CUDA, efetue uma ligação SSH cada VM. Para verificar que o sistema tem uma GPU compatível com CUDA, execute o seguinte comando:

```bash
lspci | grep -i NVIDIA
```
Irá ver o resultado semelhante ao seguinte exemplo (com um cartão NVIDIA Tesla K80):

![saída do comando lspci](./media/n-series-driver-setup/lspci.png)

Em seguida, específico para a distribuição de comandos de instalação de execução.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Transfira e instale os controladores CUDA.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_9.1.85-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  A instalação pode demorar alguns minutos.

2. A opção de instalar o toolkit CUDA completado, escreva:

  ```bash
  sudo apt-get install cuda
  ```

3. Reiniciar a VM e continue para verificar a instalação.

#### <a name="cuda-driver-updates"></a>Atualizações de controladores CUDA

Recomendamos que atualize periodicamente controladores CUDA após a implementação.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux-73-or-74"></a>CentOS ou Red Hat Enterprise Linux 7.3 ou 7.4

1. Atualize o kernel.

  ```
  sudo yum install kernel kernel-tools kernel-headers kernel-devel
  
  sudo reboot

2. Install the latest [Linux Integration Services for Hyper-V and Azure](https://www.microsoft.com/download/details.aspx?id=55106).

  ```bash
  wget https://aka.ms/lis
 
  tar xvzf lis
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Restabeleça a ligação para a VM e continuar a instalação com os seguintes comandos:

  ```bash
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-9.1.85-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  A instalação pode demorar alguns minutos. 

4. A opção de instalar o toolkit CUDA completado, escreva:

  ```bash
  sudo yum install cuda
  ```

5. Reiniciar a VM e continue para verificar a instalação.

### <a name="verify-driver-installation"></a>Certifique-se a instalação de controlador

Para consultar o estado do dispositivo para a GPU, SSH para a VM e execute o [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) instalado com o controlador o utilitário da linha de comandos. 

Se o controlador estiver instalado, verá um resultado semelhante ao seguinte. Tenha em atenção que **GPU Util** mostra 0%, a menos que estiver a executar uma carga de trabalho para a GPU na VM. A versão do controlador e detalhes GPU poderão ser diferentes das mostrado.

![Estado do dispositivo NVIDIA](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>Conectividade de rede RDMA

Conectividade de rede RDMA pode ser ativada em VMs de série N com capacidade RDMA, tais como NC24r implementados no mesmo conjunto de disponibilidade ou num grupo de posicionamento única num conjunto de dimensionamento VM. A rede RDMA suporta tráfego da Interface de passagem de mensagens (MPI) para aplicações em execução com Intel MPI 5. x ou uma versão posterior. Siga a requisitos adicionais:

### <a name="distributions"></a>Distribuições

Implemente com capacidade RDMA série N VMs de uma das imagens no Azure Marketplace que suporte a conectividade RDMA em série N VMs:
  
* **Ubuntu 16.04 LTS** - configurar os controladores RDMA na VM e registar Intel para transferir Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **Com base em centOS 7.4 HPC** -controladores RDMA e Intel MPI 5.1 são instaladas na VM.

## <a name="install-grid-drivers-on-nv-series-vms"></a>Instalar controladores de grelha em série NV VMs

Para instalar controladores de grelha NVIDIA em série NV VMs, efetuar uma ligação SSH para cada VM e siga os passos para a distribuição de Linux. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Execute o `lspci` comando. Certifique-se de que o cartão NVIDIA M60 ou cartões estão visíveis como dispositivos PCI.

2. Instale as atualizações.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Desative o controlador de kernel Nouveau, que é incompatível com o controlador NVIDIA. (Utilize apenas o controlador NVIDIA em NV VMs.) Para tal, crie um ficheiro no `/etc/modprobe.d `denominado `nouveau.conf` com o seguinte conteúdo:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Reinicie a VM e voltar a ligar. Servidor de saída X:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Transfira e instale o controlador da grelha:

  ```bash
  wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-grid.run

  sudo ./NVIDIA-Linux-x86_64-grid.run
  ``` 

6. Quando estiver a pedido que pretende executar o utilitário de nvidia xconfig para atualizar o ficheiro de configuração X, selecione **Sim**.

7. Depois de concluída a instalação, copie /etc/nvidia/gridd.conf.template para um novo gridd.conf de ficheiro na localização /etc/hosts nvidia /

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Adicione o seguinte ao `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Reiniciar a VM e continue para verificar a instalação.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS ou Red Hat Enterprise Linux 

1. Atualize o kernel e DKMS.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Desative o controlador de kernel Nouveau, que é incompatível com o controlador NVIDIA. (Utilize apenas o controlador NVIDIA em NV VMs.) Para tal, crie um ficheiro no `/etc/modprobe.d `denominado `nouveau.conf` com o seguinte conteúdo:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. Reiniciar a VM, voltar a ligar e instalar a versão mais recente [serviços de integração do Linux para Hyper-V e o Azure](https://www.microsoft.com/download/details.aspx?id=55106).
 
  ```bash
  wget https://aka.ms/lis

  tar xvzf lis

  cd LISISO

  sudo ./install.sh

  sudo reboot

  ```
 
4. Restabeleça a ligação para a VM e execute o `lspci` comando. Certifique-se de que o cartão NVIDIA M60 ou cartões estão visíveis como dispositivos PCI.
 
5. Transfira e instale o controlador da grelha:

  ```bash
  wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-grid.run

  sudo ./NVIDIA-Linux-x86_64-grid.run
  ``` 
6. Quando estiver a pedido que pretende executar o utilitário de nvidia xconfig para atualizar o ficheiro de configuração X, selecione **Sim**.

7. Depois de concluída a instalação, copie /etc/nvidia/gridd.conf.template para um novo gridd.conf de ficheiro na localização /etc/hosts nvidia /
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Adicione o seguinte ao `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Reiniciar a VM e continue para verificar a instalação.

### <a name="verify-driver-installation"></a>Certifique-se a instalação de controlador


Para consultar o estado do dispositivo para a GPU, SSH para a VM e execute o [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) instalado com o controlador o utilitário da linha de comandos. 

Se o controlador estiver instalado, verá um resultado semelhante ao seguinte. Tenha em atenção que **GPU Util** mostra 0%, a menos que estiver a executar uma carga de trabalho para a GPU na VM. A versão do controlador e detalhes GPU poderão ser diferentes das mostrado.

![Estado do dispositivo NVIDIA](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 servidor
Se precisar de um X11 servidor para ligações remotas para uma VM de NV [x11vnc](http://www.karlrunge.com/x11vnc/) é recomendada porque permite a aceleração de hardware de gráficos. BusID do dispositivo M60 tem ser adicionado manualmente ao ficheiro xconfig (`etc/X11/xorg.conf` no Ubuntu 16.04 LTS, `/etc/X11/XF86config` em CentOS 7.3 ou Red Hat Enterprise Server 7.3). Adicionar um `"Device"` secção semelhante ao seguinte:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Além disso, atualize o `"Screen"` secção para utilizar este dispositivo.
 
O decimal BusID pode ser encontrado através da execução

```bash
echo $((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))
```
 
O BusID pode ser alteradas quando uma VM obtém reatribuir porque ou reiniciada. Por conseguinte, poderá pretender criar um script para atualizar o BusID no X11 configuração quando uma VM for reiniciada. Por exemplo, criar um script com o nome `busidupdate.sh` (ou outro nome, escolher) com o seguinte conteúdo:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Em seguida, crie uma entrada para o script de atualização no `/etc/rc.d/rc3.d` para o script é invocado como raiz no arranque.

## <a name="troubleshooting"></a>Resolução de problemas

* Pode definir a utilizar o modo de persistência `nvidia-smi` , de modo a saída do comando é mais rápida quando precisar de cartões de consulta. Para definir o modo de persistência, execute `nvidia-smi -pm 1`. Tenha em atenção que se a VM é reiniciada, a definição de modo fica ausente. Pode sempre script a definição de modo a executar após o arranque.

## <a name="next-steps"></a>Passos Seguintes

* Para capturar uma imagem de VM com Linux com os controladores NVIDIA instaladas, consulte [como generalize e capturar uma máquina virtual Linux](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
