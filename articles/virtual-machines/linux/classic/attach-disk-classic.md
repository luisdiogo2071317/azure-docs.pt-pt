---
title: Anexar um disco a uma VM do Linux no Azure | Documentos da Microsoft
description: Saiba como anexar um disco de dados para uma VM do Linux utilizar o modelo de implementação clássica e inicializar o disco para que fique pronta para uso
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: b5bb3a9353f83cb569988a068f3ca02da85f739c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929156"
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Como anexar um disco de dados a uma Máquina Virtual do Linux
> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Veja como [anexar um disco de dados com o modelo de implementação do Resource Manager](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Pode anexar discos vazios e discos que contêm dados para as VMs do Azure. Ambos os tipos de discos são ficheiros. vhd que residem numa conta de armazenamento do Azure. Como na adição de qualquer disco a uma máquina Linux, depois de ligar o disco precisa inicializar e formatá-lo para que fique pronta para uso. Este artigo descreve a anexar discos vazios e discos, já que contém dados para as suas VMs, bem como para, em seguida, inicializar e formatar um novo disco.

> [!NOTE]
> É recomendado utilizar um ou mais discos separados para armazenar dados de uma máquina virtual. Quando cria uma máquina virtual do Azure, tem um disco do sistema operativo e um disco temporário. **Não utilize o disco temporário para armazenar dados persistentes.** Como o nome indica, ele fornece apenas um armazenamento temporário. Ele oferece nenhuma cópia de segurança ou a redundância porque ele não reside no armazenamento do Azure.
> O disco temporário é, normalmente, gerido pelo agente de Linux do Azure e automaticamente montado **/mnt/recurso** (ou **/mnt** nas imagens do Ubuntu). Por outro lado, um disco de dados pode ser denominado pelo Linux kernel algo como `/dev/sdc`, e precisa de partição, formatar e montar este recurso. Consulte a [guia de utilizador do agente Azure Linux] [ Agent] para obter detalhes.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Inicializar um novo disco de dados no Linux
1. SSH para a VM. Para obter mais informações, consulte [como iniciar sessão a uma máquina virtual em execução no Linux][Logon].
2. Em seguida, precisa localizar o identificador de dispositivo para o disco de dados de inicialização. Existem duas formas de fazer isso:
   
    a) Grep para dispositivos SCSI nos registos, como o seguinte comando:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Para as distribuições recentes do Ubuntu, poderá ter de utilizar `sudo grep SCSI /var/log/syslog` uma vez que iniciar sessão `/var/log/messages` podem estar desativados por predefinição.
   
    Pode encontrar o identificador do disco de dados última que foi adicionado às mensagens que são apresentadas.
   
    ![Obter as mensagens de disco](./media/attach-disk/scsidisklog.png)
   
    OU
   
    b) utilize o `lsscsi` comando para obter o id do dispositivo. `lsscsi` pode ser instalado através de `yum install lsscsi` (em Red Hat com base em distribuições) ou `apt-get install lsscsi` (no Debian com base em distribuições). Pode encontrar o disco que está procurando pelo respetivo *lun* ou **número de unidade lógica**. Por exemplo, o *lun* para os discos ligados por podem ser facilmente vistos de `azure vm disk list <virtual-machine>` como:

    ```azurecli
    azure vm disk list myVM
    ```

    O resultado é semelhante ao seguinte:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Comparar estes dados com a saída de `lsscsi` para a mesma máquina virtual de exemplo:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    O último número na cadeia de identificação em cada linha é o *lun*. Consulte `man lsscsi` para obter mais informações.
3. Na linha de comandos, escreva o seguinte comando para criar o seu dispositivo:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Quando lhe for pedido, escreva **n** para criar uma partição.

    ![Criar dispositivo](./media/attach-disk/fdisknewpartition.png)

5. Quando lhe for pedido, escreva **p** para tornar a partição a partição primária. Tipo **1** para torná-lo a primeira partição e o tipo, em seguida, introduza para aceitar o valor predefinido para o cilindro. Em alguns sistemas, ele pode mostrar os valores predefinidos do primeiro e último setores, em vez do cilindro. Pode optar por aceitar estas predefinições.

    ![Criar a partição](./media/attach-disk/fdisknewpartdetails.png)


6. Tipo **p** para ver os detalhes sobre o disco que está a ser particionado.

    ![Listar informações de disco](./media/attach-disk/fdiskpartitiondetails.png)


7. Tipo **w** escrever as definições para o disco.

    ![Escrever as alterações de disco](./media/attach-disk/fdiskwritedisk.png)

8. Agora, pode criar o sistema de ficheiros na partição de novo. Acrescentar o número de partição para o ID de dispositivo (no exemplo a seguir `/dev/sdc1`). O exemplo seguinte cria uma partição de ext4 no /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Criar o sistema de ficheiros](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > Sistemas de SuSE Linux Enterprise 11 só suportam o acesso só de leitura para sistemas de ficheiros de ext4. Para estes sistemas, é recomendado para formatar o novo sistema de ficheiros como ext3 em vez de ext4.

9. Torne um diretório para montar o novo sistema de arquivos, da seguinte forma:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Por fim pode montar a unidade, da seguinte forma:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    O disco de dados está agora pronto para utilizar como **/datadrive**.
   
    ![Criar o diretório e Monte o disco](./media/attach-disk/mkdirandmount.png)

11. Adicione nova unidade ao /etc/fstab.:
   
    Para garantir que a unidade é automaticamente remontada após um reinício tem de ser adicionado para o ficheiro/etc/fstab. Além disso, é altamente recomendável que o UUID (Identificador exclusivo universalmente) é usado em /etc/fstab. para fazer referência a unidade em vez de apenas o nome do dispositivo (ou seja, /dev/sdc1). Usar o UUID evita o que está a ser montado para um determinado local se o sistema operacional detetar um erro de disco durante o arranque e de quaisquer discos de dados restantes, em seguida, que está sendo atribuídos essas identificações de dispositivo de disco incorreta. Para localizar o UUID da unidade de novo, pode utilizar o **blkid** utilitário:
   
    ```bash
    sudo -i blkid
    ```
   
    O resultado será semelhante ao seguinte exemplo:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Editar incorretamente o **etc/fstab** ficheiros poderiam resultar num sistema não inicializável. Se não souber, consulte a documentação da distribuição para obter informações sobre como editar corretamente esse arquivo. Também é recomendável que uma cópia de segurança do ficheiro /etc/fstab. é criada antes de editar.

    Em seguida, abra a **etc/fstab** ficheiro num editor de texto:

    ```bash
    sudo vi /etc/fstab
    ```

    Neste exemplo, usamos o valor UUID para a nova **/desenvolvimento/sdc1** dispositivo que foi criado nos passos anteriores e o ponto de montagem **/datadrive**. Adicione a seguinte linha ao final dos **etc/fstab** ficheiro:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Ou, em sistemas com base no SuSE Linux poderá ter de utilizar um formato de um pouco diferente:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > O `nofail` opção garante que a VM é iniciado, mesmo que o sistema de ficheiros está danificado ou o disco não existe no momento da inicialização. Sem esta opção, pode encontrar o comportamento, conforme descrito em [não é possível SSH à VM do Linux devido a erros FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Pode testar que o sistema de ficheiros está montado corretamente ao desmontar e remontar, em seguida, o sistema de ficheiros, ou seja, usando o exemplo do ponto de montagem `/datadrive` criado nos passos anteriores:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Se o `mount` comando produz um erro, verifique o ficheiro de fstab/etc/para a sintaxe correta. Se as unidades de dados adicionais ou partições são criadas, insira-os no/etc/fstab separadamente também.

    Torne a unidade gravável utilizando este comando:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Em seguida, remover um disco de dados sem editar fstab pode fazer com que a VM falhe efetuar o arranque. Se se tratar de uma ocorrência comum, a maioria das distribuições fornecem nenhuma a `nofail` e/ou `nobootwait` fstab opções que permitem que um sistema efetuar o arranque, mesmo se o disco não consegue montar ao tempo de arranque. Para obter mais informações sobre estes parâmetros, consulte a documentação de sua distribuição.

### <a name="trimunmap-support-for-linux-in-azure"></a>Suporte de cortar/UNMAP para Linux no Azure
Alguns kernels de Linux suportam operações de cortar/UNMAP descartar blocos não utilizados no disco. Essas operações são principalmente útil para armazenamento standard para informar o Azure eliminado páginas já não são válido e podem ser descartados. Descarte páginas pode poupar no custo de se criar ficheiros grandes e, em seguida, eliminá-los.

Existem duas formas de ativar a limitação de suporte na sua VM do Linux. Como sempre, consulte sua distribuição para a abordagem recomendada:

* Utilize o `discard` montar opção na `/etc/fstab`, por exemplo:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* Em alguns casos o `discard` opção pode ter implicações de desempenho. Em alternativa, pode executar o `fstrim` comando manualmente a partir da linha de comandos, ou adicione-o para seu crontab para executar regularmente:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Resolução de problemas
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Próximos Passos
Pode ler mais sobre como utilizar a sua VM do Linux nos seguintes artigos:

* [Como iniciar sessão a uma máquina virtual em execução no Linux][Logon]
* [Como desanexar um disco de máquina virtual do Linux](detach-disk-classic.md)
* [Utilizar a CLI do Azure com o modelo de implementação clássica](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Configurar o RAID, numa VM do Linux no Azure](../configure-raid.md)
* [Configurar LVM numa VM do Linux no Azure](../configure-lvm.md)

<!--Link references-->
[Agent]:../../extensions/agent-linux.md
[Logon]:../mac-create-ssh-keys.md
