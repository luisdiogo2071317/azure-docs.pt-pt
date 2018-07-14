---
title: Configurar um cluster RDMA do Linux para executar aplicações MPI | Documentos da Microsoft
description: Criar um cluster do Linux de tamanho H16r, H16mr, A8 ou A9 VMs para utilizar a rede de RDMA do Azure para executar aplicações MPI
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/11/2018
ms.author: danlep
ms.openlocfilehash: 471fd4095fe45e76f94df8c61a07eeb9bbc1c120
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990732"
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Configurar um cluster RDMA do Linux para executar aplicações MPI

Saiba como configurar um cluster RDMA do Linux no Azure com o [tamanhos de VM de computação de alto desempenho](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para executar aplicações paralelas de Message Passing Interface (MPI). Este artigo fornece passos para preparar uma imagem de HPC do Linux para executar a MPI Intel num cluster. Depois de preparação, implementar um cluster de VMs com esta imagem e um da [tamanhos de VM do Azure com capacidade RDMA](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances). Utilize o cluster para executar aplicações MPI que se comuniquem com eficiência através de uma rede de baixa latência e alto débito com base na tecnologia de acesso (RDMA) de memória direta remoto.

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [do Azure Resource Manager](../../../azure-resource-manager/resource-manager-deployment-model.md) e clássico. Este artigo cobre a utilização do modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.

## <a name="cluster-deployment-options"></a>Opções de implementação de cluster
Seguem-se métodos diferentes que pode utilizar para criar um cluster RDMA do Linux, com ou sem um programador de tarefas.

* **Scripts CLI do Azure**: conforme mostrado posteriormente neste artigo, utilize o [interface de linha de comandos do Azure](../../../cli-install-nodejs.md) (CLI) para a implementação de um cluster de VMs com capacidade RDMA do script. A CLI no modo de gestão do serviço cria os nós de cluster em série no modelo de implementação clássica, para que a implementação de vários nós de computação pode demorar alguns minutos. Para permitir a ligação de rede RDMA, quando utiliza o modelo de implementação clássica, implemente as VMs no mesmo serviço cloud.
* **Modelos Azure Resource Manager**: também pode utilizar o modelo de implementação do Resource Manager para implementar um cluster de VMs com capacidade RDMA, que liga à rede RDMA. Pode [criar seu próprio modelo](../../../resource-group-authoring-templates.md), ou verifique o [modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/) para modelos que tenham feito parte da Microsoft ou da Comunidade para implantar a solução que pretende. Modelos do Resource Manager podem fornecer uma forma rápida e fiável para implementar um cluster do Linux. Para ativar a ligação de rede RDMA quando utiliza o modelo de implementação do Resource Manager, implemente as VMs no mesmo conjunto de disponibilidade ou conjunto de dimensionamento de máquina virtual.
* **HPC Pack**: criar um cluster do Microsoft HPC Pack no Azure e adicionar nós de computação com capacidade RDMA que executam uma distribuição Linux suportada para acessar a rede RDMA. Para obter mais informações, consulte [começar connosco de computação do Linux num cluster HPC Pack no Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Passos de implementação de exemplo no modelo clássico
Os passos seguintes mostram como utilizar a CLI do Azure para implementar uma VM de HPC do SUSE Linux Enterprise Server (SLES) 12 do Azure Marketplace, personalizá-lo e criar uma imagem VM personalizada. Em seguida, pode utilizar a imagem para a implementação de um cluster de VMs com capacidade RDMA do script.

> [!TIP]
> Utilize passos semelhantes para implementar um cluster de VMs com capacidade RDMA com base nas imagens baseada em CentOS HPC no Azure Marketplace. Algumas etapas diferem ligeiramente, conforme indicado.
>

### <a name="prerequisites"></a>Pré-requisitos
* **Computador cliente**: precisa de um computador de cliente Mac, Linux ou Windows para comunicar com o Azure. Estes passos partem do princípio de que está a utilizar um cliente Linux.
* **Subscrição do Azure**: Se não tiver uma subscrição, pode criar um [conta gratuita](https://azure.microsoft.com/free/) em apenas alguns minutos. Para clusters maiores, considere uma subscrição pay as you go ou outras opções de compra.
* **Disponibilidade de tamanho VM**: verifique [produtos disponíveis por região](https://azure.microsoft.com/regions/services/) para disponibilidade da série H ou outros tamanhos de VM com capacidade RDMA em regiões do Azure.
* **Quota de núcleos**: poderá ter de aumentar a quota de núcleos para implementar um cluster de VMs de computação intensiva. Por exemplo, precisa, pelo menos, 128 núcleos se pretender implementar 8 A9 VMs, conforme mostrado neste artigo. A sua subscrição também pode limitar o número de núcleos que pode implementar em determinadas famílias de tamanhos VM, incluindo a série H. Para pedir um aumento de quota [abra um pedido de suporte do cliente online](../../../azure-supportability/how-to-create-azure-support-request.md) sem encargos.
* **CLI do Azure**: [instale](../../../cli-install-nodejs.md) a CLI do Azure e [ligar à sua subscrição do Azure](/cli/azure/authenticate-azure-cli) do computador cliente.

Reveja também as considerações de implementação para o [tamanhos de VM do Azure com capacidade RDMA](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances).

### <a name="provision-an-sles-12-hpc-vm"></a>Aprovisionar uma VM de HPC SLES 12
Depois de iniciar sessão no Azure com a CLI do Azure, execute `azure config list` para confirmar que a saída mostra o modo de gestão de serviço. Se não existir, defina o modo ao executar este comando:

    azure config mode asm


Escreva o seguinte para listar todas as subscrições que estão autorizados a utilizar:

    azure account list

A subscrição atual do Active Directory é identificada com `Current` definido como `true`. Se esta subscrição não é aquela que pretende utilizar para criar o cluster, defina o ID de subscrição apropriado que o Active Directory da subscrição:

    azure account set <subscription-Id>

Para ver as imagens de SLES 12 HPC publicamente disponíveis no Azure, execute um comando semelhante ao seguinte, supondo que o suporta de ambiente de shell **grep**:

    azure vm image list | grep "suse.*hpc"

Aprovisione uma VM com capacidade RDMA com uma imagem de SLES 12 SP3 HPC, executando um comando semelhante ao seguinte:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-v20170913

Em que:

* O tamanho (A9 neste exemplo) é um dos tamanhos de VM com capacidade RDMA.
* O número da porta SSH externo (22 neste exemplo, o que é o padrão SSH) é qualquer número de porta válido. O número da porta SSH interno é definido como 22.
* Um novo serviço cloud é criado na região do Azure especificada pela localização. Especifique uma localização na qual o tamanho da VM que escolher está disponível, tais como "EUA Oeste".
* Para obter suporte de prioridade SUSE (o que leva a custos adicionais), o nome da imagem SLES 12 atualmente pode ser uma das opções que tem `priority` o nome, tais como: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-priority-v20170913`

### <a name="customize-the-vm"></a>Personalizar a VM
Depois da VM termina o aprovisionamento, o SSH para a VM com o endereço IP externo da VM (ou nome DNS) e a porta externa número configurado e, em seguida, personalizá-lo. Para obter detalhes de ligação, veja [como iniciar sessão a uma máquina virtual em execução no Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Execute comandos que o utilizador que tiver configurado na VM, a menos que o acesso à raiz é necessário para concluir uma etapa.

> [!IMPORTANT]
> Microsoft Azure não fornece acesso de raiz para VMs do Linux. Para obter acesso administrativo quando estiver ligado como utilizador para a VM, execute comandos utilizando `sudo`.
>
>

* **Atualizações**: Instale atualizações utilizando o zypper. Pode também querer instalar os utilitários NFS.

  > [!IMPORTANT]
  > Uma VM de HPC do SLES 12, recomendamos que não aplicar as atualizações de kernel, que podem causar problemas com o RDMA do Linux drivers.
  >
  >
* **A MPI Intel**: concluir a instalação do Intel MPI na VM ao executar o seguinte comando:

    ```bash
    sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
    ```

* **Bloquear memória**: códigos de MPI para bloquear a memória disponível para RDMA, adicionar ou alterar as seguintes definições no ficheiro /etc/security/limits.conf. Precisa de acesso de raiz para editar este ficheiro.

    ```bash
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Para fins de teste, também pode definir memlock para ilimitado. Por exemplo: `* hard memlock unlimited`. Para obter mais informações, consulte [métodos mais conhecidos para definição bloqueado o tamanho da memória](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **Chaves SSH para VMs de SLES**: gerar chaves SSH para estabelecer confiança para a sua conta de utilizador entre os nós de computação no SLES do cluster durante a execução de trabalhos de MPI. Se tiver implementado uma VM de HPC baseada em CentOS, não siga este passo. Consulte as instruções neste artigo para configurar a passe SSH fidedignidade entre os nós de cluster depois de capturar a imagem e implementar o cluster.

  Para criar chaves SSH, execute o `ssh-keygen` comando. Quando lhe for pedido para entrada, selecione **Enter** para gerar as chaves na localização predefinida sem definir uma palavra-passe.

  Acrescente a chave pública para o ficheiro de authorized_keys de chaves públicas conhecidas.

  ```bash
  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ```

  No diretório ~/.ssh, editar ou criar o `config` ficheiro. Forneça o intervalo de endereços IP da rede privada que pretende utilizar no Azure (10.32.0.0/16 neste exemplo):

  ```bash
  host 10.32.0.*
  StrictHostKeyChecking no
  ```

  Em alternativa, liste o endereço IP de rede privada de cada VM no seu cluster da seguinte forma:

  ```bash
  host 10.32.0.1
  StrictHostKeyChecking no
  host 10.32.0.2
  StrictHostKeyChecking no
  host 10.32.0.3
  StrictHostKeyChecking no
  ...
  ```

  > [!NOTE]
  > Configurar `StrictHostKeyChecking no` pode criar um potencial risco de segurança quando não for especificado um endereço IP específico ou um intervalo.
  >
  >
* **Aplicativos**: instalar nenhum aplicativo precisar ou efetuar outras personalizações antes de capturar a imagem.

### <a name="capture-the-image"></a>Capturar a imagem
Para capturar a imagem, execute o seguinte comando na VM do Linux. Este comando desprovisiona a VM, mas mantém a contas de utilizador e as chaves SSH que configurou.

```bash
sudo waagent -deprovision
```

A partir do computador cliente, execute os seguintes comandos da CLI do Azure, para capturar a imagem. Para obter mais informações, consulte [como capturar uma máquina virtual do Linux clássica como uma imagem](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Depois de executar estes comandos, a imagem VM é capturada para a sua utilização e a VM é eliminada. Agora tem a sua imagem personalizada pronta para implementar um cluster.

### <a name="deploy-a-cluster-with-the-image"></a>Implementar um cluster com a imagem
Modifique o seguinte script de Bash com os valores adequados para o seu ambiente e executá-lo a partir do computador cliente. Porque o Azure implementa as VMs em série no modelo de implementação clássica, demora alguns minutos para implementar as VMs A9 oito sugerido nesse script.

```bash
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a classic VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>;
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Considerações para um cluster HPC do CentOS
Se pretender configurar um cluster com base em uma das imagens baseadas em CentOS HPC no Azure Marketplace em vez de 12 do SLES para HPC, siga os passos gerais na secção anterior. Quando aprovisionar e configurar a VM, tenha em atenção as seguintes diferenças:

- A MPI Intel já está instalada numa VM aprovisionada a partir de uma imagem baseada em CentOS HPC.
- As definições de memória de bloqueio já foram adicionadas no arquivo de /etc/security/limits.conf da VM.
- Não é gere chaves SSH na VM Aprovisiona para captura. Em vez disso, recomendamos que configurar a autenticação baseada no utilizador depois de implementar o cluster. Para obter mais informações, consulte a secção seguinte.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Configurar a fidedignidade passe de SSH no cluster
Num cluster HPC baseada em CentOS, existem dois métodos para estabelecer confiança entre os nós de computação: autenticação baseada em anfitrião e a autenticação baseada no utilizador. Autenticação baseada em anfitrião está fora do escopo deste artigo e geralmente deve ser feita por meio de um script de extensão durante a implementação. Autenticação baseada no utilizador é conveniente para estabelecer confiança após a implementação e requer a geração e a partilha de chaves SSH entre os nós de computação no cluster. Esse método é normalmente conhecido como início de sessão SSH início e é necessário quando a execução de trabalhos de MPI.

Um script de exemplo `user_authentication.sh` para ativar a autenticação de utilizador com base num HPC baseada em CentOS cluster segue:

```bash
#!/bin/bash
# For CentOS user must first install epel-release, sshpass, and nmap (sshpass and nmap are available from epel-release for CentOS)

# usage ./user_authentication.sh [username] [password] [internalIP prefix]
# ./user_authentication.sh azureuser Azure@123 10.32.0
USER=$1
PASS=$2
IPPRE=$3
HEADNODE=`hostname`

mkdir -p .ssh
echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''

echo 'Host *' >> .ssh/config
echo 'StrictHostKeyChecking no' >> .ssh/config
chmod 400 .ssh/config
chown azureuser:azureuser /home/azureuser/.ssh/config

nmap -sn $IPPRE.* | grep $IPPRE. | awk '{print $5}' > nodeips.txt
for NAME in `cat nodeips.txt`; do sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'hostname' >> nodenames.txt;done

NAMES=`cat nodenames.txt` #names from names.txt file
for NAME in $NAMES; do
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 /home/$USER/nodenames.txt $USER@$NAME:/home/$USER/
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "mkdir .ssh && chmod 700 .ssh"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'touch /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo "Host *" >  /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo StrictHostKeyChecking no >> /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 400 /home/'$USER'/.ssh/config'
    cat .ssh/id_rsa.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'cat >> .ssh/authorized_keys'
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 $USER@$NAME:/home/$USER/.ssh/id_rsa.pub .ssh/sub_node.pub

    for SUBNODE in `cat nodeips.txt`; do
         sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'mkdir -p .ssh'
         cat .ssh/sub_node.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'cat >> .ssh/authorized_keys'
    done
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 700 .ssh/'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 640 .ssh/authorized_keys'
done
```

Execute este script utilizando os seguintes passos. Também pode modificar o script ou utilizar outro método para estabelecer passe autenticação SSH entre os nós de computação de cluster.

Para executar o script, precisa saber o prefixo de seus endereços IP de sub-rede. Obtenha o prefixo ao executar o seguinte comando em um de nós do cluster. A saída deve ser algo como 10.1.3.5 e o prefixo é o 10.1.3 parte.

```bash
ifconfig eth0 | grep -w inet | awk '{print $2}'
```

Agora, execute o script usando três parâmetros: o nome de utilizador comum em nós de computação, a palavra-passe comum para esse utilizador em nós de computação e o prefixo de sub-rede que foi devolvido do comando anterior.

```bash
./user_authentication.sh <myusername> <mypassword> 10.1.3
```

Este script faz o seguinte:

* Cria um diretório no nó de anfitrião com o nome. SSH, que é necessário para início de sessão de início.
* Cria um ficheiro de configuração no diretório. SSH que instrui o início de sessão passe para permitir o início de sessão de qualquer nó no cluster.
* Cria ficheiros que contêm os nomes de nó e endereços IP do nó para todos os nós do cluster. Estes ficheiros são deixados depois do script é executado para futura referência.
* Cria um par de chaves público e privado para cada nó de cluster (incluindo o nó de anfitrião) e cria entradas no arquivo authorized_keys.

> [!WARNING]
> Executar este script pode criar um potencial risco de segurança. Certifique-se de que as informações da chave públicas no ~/.ssh não são distribuídas.
>

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Executar o MPI num cluster de dois nós básico
Se ainda não o fez, primeiro configure o ambiente para Intel MPI.

```bash
# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Execute um comando MPI
Execute um comando MPI em um de nós de computação para mostrar que MPI se encontra corretamente instalado e pode se comunicar entre, pelo menos, que dois nós de computação. O seguinte procedimento **mpirun** comando é executado o **nome do anfitrião** comando em dois nós. Para o `-hosts` parâmetro, passar os endereços IP dos dois nós na VNet do Azure (por exemplo, 10.32.0.4,10.32.0.5).

```bash
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
O resultado deve listar os nomes de todos os nós que é passada como entrada para `-hosts`. Por exemplo, um **mpirun** comando com dois nós devolve o resultado semelhante ao seguinte:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Executar um benchmark MPI
O seguinte comando do Intel MPI executa um parâmetro de comparação pingpong para verificar a configuração de cluster e a ligação à rede RDMA.

```bash
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Num cluster de trabalhar com dois nós, deverá ver um resultado semelhante ao seguinte. Na rede do Azure RDMA, espere até 512 bytes de tamanhos de latência igual ou abaixo a 3 microssegundos de mensagem.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 6 17:16:46 2018
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```
### <a name="sample-mpi-run-script"></a>MPI executar script de exemplo
Eis um script de Bash de exemplo para configurar as variáveis necessárias para executar um aplicativo de MPI. Alterar o caminho para `mpivars.sh` conforme necessário para a instalação do Intel MPI.

```bash
#!/bin/bash -x

# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the MPI job. Substitute with values appropriate for your application.

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Segue-se o formato do ficheiro de anfitriões. Adicione uma linha para cada nó do cluster. Especifique os endereços IP privados da rede virtual definido anteriormente, não os nomes DNS. Por exemplo, para dois anfitriões com endereços IP 10.32.0.4 e 10.32.0.4, o ficheiro contém o seguinte:

```bash
10.32.0.4:16
10.32.0.5:16
```


## <a name="next-steps"></a>Passos Seguintes
* Implementar e executar aplicações de MPI do Linux no seu cluster do Linux.
* Consulte a [documentação da biblioteca do Intel MPI](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) para obter orientações sobre a MPI Intel.
* Tente uma [modelo de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) para criar um cluster de Intel Lustre utilizando uma imagem baseada em CentOS HPC. 
