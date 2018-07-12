---
title: Criar e carregar um VHD do Linux no Azure
description: Saiba como criar e carregar um Azure disco rígido virtual (VHD) que contém um sistema operativo Linux.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 17b4df83b141d5365a8d6244c4ab73b0eba5ed73
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972354"
---
# <a name="information-for-non-endorsed-distributions"></a>Informações para Distribuições Não Apoiadas
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

A plataforma do Azure SLA se aplica a máquinas virtuais que executem o sistema operacional Linux, apenas quando um do [distribuições aprovadas pelo](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) é utilizado. Para estes distribuições apoiadas, imagens do Linux são fornecidas no Azure Marketplace com a configuração necessária.

* [Distribuições aprovadas pelo Linux no Azure-](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Suporte para imagens do Linux no Microsoft Azure](https://support.microsoft.com/kb/2941892)

Todas as distribuições em execução no Azure têm de preencher um número de pré-requisitos para ter uma chance de ser executado corretamente na plataforma.  Este artigo não constitui abrangente como cada distribuição for diferente; e é possível que, mesmo se cumprir os critérios abaixo terá de ajustar significativamente o seu sistema de Linux para garantir que ele execute corretamente na plataforma.

É por essa razão que recomendamos que comece com um [Linux em distribuições apoiadas do Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sempre que possível. O seguinte artigo orienta-o como preparar as várias distribuições do Linux apoiadas que são suportadas no Azure:

* **[Distribuições baseada em centOS](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

O restante deste artigo se concentra na documentação de orientação geral para a execução de sua distribuição do Linux no Azure.

## <a name="general-linux-installation-notes"></a>Observações de instalação de Linux geral
* O formato VHDX não é suportado no Azure, apenas **fixo VHD**.  Pode converter o disco para o formato VHD utilizando o Gestor de Hyper-V ou o cmdlet convert-vhd. Se estiver a utilizar o VirtualBox significa selecionando **fixos de tamanho** em oposição a predefinição atribuída dinamicamente ao criar o disco.
* O Azure suporta apenas máquinas virtuais de geração 1. Pode converter uma máquina virtual de geração 1 de VHDX para o formato de ficheiro VHD e de expansão dinâmica num disco de tamanho fixo. Mas não é possível alterar a geração de uma máquina virtual. Para obter mais informações, consulte [deve criar máquinas virtuais de geração 1 ou 2 no Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* O tamanho máximo permitido para o VHD é 1,023 GB.
* Ao instalar o sistema Linux é *recomendado* que utilize partições padrão em vez de LVM (muitas vezes, o padrão para muitas instalações). Isso evitará LVM nome entra em conflito com VMs Clonadas, especialmente se um disco de SO algum dia precisar ser anexados a outra VM idêntico para resolução de problemas. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ou [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pode ser utilizada em discos de dados.
* É necessário suporte de kernel de sistemas de ficheiros do UDF de montagem. No primeiro arranque no Azure a configuração de aprovisionamento é passada para a VM com Linux através de mídia formatada para UDF, que está ligada para o convidado. O agente Linux do Azure tem de ser capaz de montar o sistema de ficheiros UDF para ler a respetiva configuração e aprovisionar a VM.
* Versões de kernel do Linux abaixo 2.6.37 não suportam no Hyper-V com tamanhos de VM maiores. Isso emitir principalmente impactos distribuições mais antigas usando o montante kernel do Red Hat 2.6.32 e foi corrigido no RHEL 6.6 (kernel-2.6.32-504). Sistemas em execução personalizados kernels anteriores a 2.6.37 ou baseado em RHEL kernels mais antigas do que 2.6.32-504 tem de definir o parâmetro de arranque `numa=off` no kernel da linha de comandos no grub.conf. Para obter mais informações, consulte Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Não configure uma partição de troca no disco do SO. O agente Linux pode ser configurado para criar um ficheiro de troca no disco de recursos temporário.  Podem encontrar mais informações sobre este assunto nos passos abaixo.
* Todos os VHDs no Azure tem de ter um tamanho virtual alinhado para 1MB. Ao converter a partir de um disco não processado para o VHD tem de se certificar de que o tamanho de disco bruto é um múltiplo de 1MB antes da conversão. Podem encontrar mais informações nos passos abaixo.

### <a name="installing-kernel-modules-without-hyper-v"></a>Instalar módulos kernel sem Hyper-V
Azure é executado num hipervisor Hyper-V, para que o Linux requer que determinados módulos kernel estão instalados para executar no Azure. Se tiver uma VM que tenha sido criada fora do Hyper-V, os programas de instalação do Linux não podem incluir os controladores para o Hyper-V no ramdisk inicial (initrd ou initramfs), a menos que Deteta que está em execução num ambiente de Hyper-V. Quando utilizar um sistema de Virtualização diferentes (ou seja, Virtualbox, KVM, etc.) para preparar sua imagem do Linux, poderá ter de recriar o initrd para garantir que, pelo menos, o `hv_vmbus` e `hv_storvsc` módulos kernel estão disponíveis no ramdisk inicial.  Este é um problema conhecido, pelo menos, em sistemas com base no montante distribuição do Red Hat.

O mecanismo para recriar a imagem de initrd ou initramfs pode variar consoante a distribuição. Consulte a documentação ou suporte de sua distribuição para o procedimento adequado.  Eis um exemplo de como reconstruir a através de initrd o `mkinitrd` utilitário:

Em primeiro lugar, criar cópias de segurança a imagem de initrd existente:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Em seguida, reconstrua o initrd com o `hv_vmbus` e `hv_storvsc` módulos kernel:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Redimensionar os VHDs
Imagens VHD no Azure tem de ter um tamanho virtual alinhado para 1 MB.  Normalmente, os VHDs criados com o Hyper-V devem já estarão alinhados corretamente.  Se o VHD não está alinhado corretamente, poderá receber uma mensagem de erro semelhante ao seguinte ao tentar criar uma *imagem* a partir de seu VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Para resolver esse comportamento, redimensione a VM utilizando a consola de Gestor de Hyper-V ou o [VHD de redimensionamento](http://technet.microsoft.com/library/hh848535.aspx) cmdlet do Powershell.  Se não estiver a executar num ambiente Windows, é recomendado que utilize qemu img converter (se necessário) e redimensione o VHD.

> [!NOTE]
> Há um bug conhecido em versões de qemu img > = 2.2.1 do buildship que resulta num VHD formatado incorretamente. Foi corrigido o problema em QEMU 2.6. Recomenda-se para utilizar qemu-img 2.2.0 ou inferior, ou atualizar para o 2.6 ou superior. Referência: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. O VHD diretamente com ferramentas, como o redimensionamento `qemu-img` ou `vbox-manage` pode resultar num VHD não inicializável.  Portanto, é recomendado para converter o primeiro o VHD para uma imagem de disco bruto.  Se a imagem de VM já foi criada como imagem de disco bruto (o padrão para alguns Hipervisores como KVM), em seguida, pode ignorar este passo:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Calcule o tamanho necessário da imagem do disco para garantir que o tamanho virtual está em sintonia para 1 MB.  O script de shell de bash seguinte pode ajudar com isso.  O script usa "`qemu-img info`" para determinar o tamanho virtual da imagem do disco e, em seguida, calcula o tamanho de 1 MB seguinte:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Redimensione o disco não processado usando $rounded_size conforme definido no script acima:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Agora, converta o disco não PROCESSADO para um VHD de tamanho fixo:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Ou, com a versão de qemu **2.6 +** incluem o `force_size` opção:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Requisitos de Kernel do Linux
Os controladores de serviços de integração do Linux (LIS) para o Hyper-V e do Azure são enviados diretamente para o kernel do Linux a montante. Muitas distribuições que incluem uma versão de kernel de Linux recente (ou seja, 3.x) já a tiver estes controladores disponíveis ou caso contrário, fornecer versões backported esses Drivers com os kernels.  Estes controladores estão constantemente a ser atualizados no kernel do montante com novas correções e funcionalidades, pelo sempre que possível que é recomendado para executar uma [apoiadas pelo distribuição](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) que inclui estas correções e atualizações.

Se estiver a executar uma variante de versões do Red Hat Enterprise Linux **6.0 6.3**, em seguida, tem de instalar os controladores mais recentes do LIS para Hyper-V. Os controladores podem ser encontrados [nesta localização](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). A partir do RHEL **6.4 +** (e derivados) os controladores LIS já estão incluídos com o kernel e por isso, não existem pacotes de instalação adicionais são necessários para executar esses sistemas no Azure.

Se um kernel personalizado for necessário, é recomendável utilizar uma versão mais recente do kernel (ou seja, **3.8 +**). Para esses distribuições ou fornecedores que mantêm os seus próprios kernel, algum esforço é necessário para backport regularmente a LIS drivers de kernel a montante para o kernel personalizado.  Mesmo se já estiver a executar uma versão de kernel relativamente recente, é altamente recomendado para controlar quaisquer correções a montante nos controladores LIS e backport aqueles conforme necessário. A localização dos ficheiros de origem do driver LIS está disponível na [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) ficheiros na árvore de origem do kernel do Linux:

    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/

No mínimo, a ausência dos patches seguintes é conhecida causar problemas no Azure e por isso, estes têm de ser incluídos no kernel. Esta lista não é exaustiva ou completa para todas as distribuições de:

* [ata_piix: diferir discos para os controladores de Hyper-V por predefinição](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: conta para os pacotes em trânsito no caminho de reposição](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: evitar a utilização de WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: desativar mesmo escrever para RAID e drivers de adaptador de anfitrião virtual](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: ponteiro NULL tirarmos a referência de correção](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: falhas de memória intermédia de anel podem resultar em congelamento de e/s](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: proteger contra execução dupla de __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>O agente Linux do Azure
O [agente Linux do Azure](../extensions/agent-linux.md) (waagent) é necessário para aprovisionar corretamente uma máquina virtual do Linux no Azure. Pode obter a versão mais recente, os problemas de ficheiros ou submeter pedidos pull no [repositório do GitHub de agente do Linux](https://github.com/Azure/WALinuxAgent).

* O agente do Linux é lançado sob a licença Apache 2.0. Muitas distribuições já fornecem deb ou RPM pacotes para o agente e, então, em alguns casos, isso pode ser instalado e atualizado com pouco esforço.
* O agente Linux do Azure requer Python v2.6 +.
* O agente também requer o módulo de python pyasn1. A maioria das distribuições fornecem isso como um pacote separado que pode ser instalado.
* Em alguns casos, o agente Linux do Azure não podem ser compatível com NetworkManager. Muitos dos pacotes RPM/Deb fornecidos pelo distribuições configurar NetworkManager como um conflito ao pacote de waagent e, portanto, serão desinstalado NetworkManager quando instala o pacote de agente do Linux.
* O agente Linux do Azure tem de ser superior à versão mínima suportada, veja este artigo para [detalhes](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Requisitos de sistema de Linux geral

* Modificar a linha de arranque de kernel em GRUB ou GRUB2 para incluir os seguintes parâmetros. Isso também garante que todas as mensagens de consola sejam enviadas para a primeira porta serial, que pode ajudá-lo Azure suporte com a depuração de problemas:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Isso também garante que todas as mensagens de consola sejam enviadas para a primeira porta serial, que pode ajudá-lo Azure suporte com a depuração de problemas.
  
    Além do exposto acima, é recomendado *remover* os seguintes parâmetros se existirem:
  
        rhgb quiet crashkernel=auto
  
    Arranque gráfica e quieto não é útil num ambiente de nuvem onde queremos todos os registos a serem enviados para a porta serial. O `crashkernel` opção pode ser esquerda configurado se assim o desejar, mas tenha em atenção que este parâmetro reduz a quantidade de memória disponível na VM 128 MB ou mais, que pode ser problemático sobre os tamanhos VM mais pequenos.

* Instalar o agente Linux do Azure
  
    O agente Linux do Azure é necessário para o aprovisionamento de uma imagem do Linux no Azure.  Número de distribuições fornece o agente como um pacote Deb ou RPM (o pacote é normalmente chamado de 'WALinuxAgent' ou 'walinuxagent').  O agente também pode ser instalado manualmente, seguindo os passos a [guia de agente do Linux](../extensions/agent-linux.md).

* Certifique-se de que o servidor SSH está instalado e configurado para iniciar no momento da inicialização.  Normalmente, esta é a predefinição.

* Não crie espaço de comutação no disco do SO
  
    O agente Linux do Azure podem configurar automaticamente o espaço de comutação com o disco de recurso local que está ligado à VM após o aprovisionamento no Azure. O disco de recurso local é uma *temporária* disco e poderá ser esvaziada quando a VM é desaprovisionada. Depois de instalar o agente Linux do Azure (consulte o passo anterior), modifique os parâmetros seguintes em /etc/waagent.Conf. adequadamente:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

* Como passo final, execute os seguintes comandos para desaprovisionar a máquina virtual:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Sobre o Virtualbox poderá ver o seguinte erro após a execução "waagent-force - desaprovisionamento ': `[Errno 5] Input/output error`. Esta mensagem de erro não é crítica e pode ser ignorada.
  > 
  > 

* Encerre a máquina virtual e carregar o VHD para o Azure.

