---
title: VMs de computação do Linux num cluster HPC Pack | Documentos da Microsoft
description: Saiba como criar e utilizar um cluster HPC Pack no Azure para Linux de alto desempenho computação a cargas de trabalho (HPC)
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 2d4091d8ad6a778405ee6bb916c399e0b144f21d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441532"
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Introdução aos nós de computação do Linux num cluster HPC Pack no Azure
Configurar uma [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) a executar uma distribuição Linux suportada de nós de computação de cluster no Azure que contém um nó principal que executam o Windows Server e muitos outros. Explore as opções para mover dados entre os nós do Linux e o nó principal do Windows do cluster. Aprenda a submeter tarefas HPC do Linux para o cluster.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Num alto nível, o diagrama seguinte mostra o cluster HPC Pack, criar e trabalhar com.

![Cluster HPC Pack connosco do Linux][scenario]

Para outras opções executar cargas de trabalho de HPC do Linux no Azure, veja [recursos técnicos para batch e a computação de alto desempenho](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Implementar um cluster HPC Pack connosco de computação do Linux
Este artigo mostra-lhe duas opções para implementar um cluster HPC Pack no Azure com o Linux nós de computação. Ambos os métodos usam uma imagem do Marketplace do Windows Server com o HPC Pack para criar o nó principal. 

* **Modelo Azure Resource Manager** -utilizar um modelo do Azure Marketplace ou de um modelo de início rápido da Comunidade, para automatizar a criação do cluster no modelo de implementação do Resource Manager. Por exemplo, o [cluster HPC Pack para cargas de trabalho do Linux](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) modelo no Azure Marketplace cria uma infraestrutura completa de cluster HPC Pack para Linux HPC cargas de trabalho.
* **Script do PowerShell** -utilize o [script de implementação do IaaS do Microsoft HPC Pack](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) para automatizar uma implantação completa do cluster no modelo de implementação clássica. Este script do PowerShell do Azure utiliza uma imagem de VM do HPC Pack no Azure Marketplace para a implementação rápida e fornece um conjunto abrangente de parâmetros de configuração para implementar nós de computação do Linux.

Para obter mais informações sobre as opções de implementação de cluster HPC Pack no Azure, consulte [opções para criar e gerir uma computação de alto desempenho (HPC) do cluster no Azure com o Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Pré-requisitos
* **Subscrição do Azure** -pode utilizar uma subscrição no serviço do Azure Global ou do Azure China. Se não tiver uma conta, pode criar uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) em apenas alguns minutos.
* **Quota de núcleos** -poderá ter de aumentar a quota de núcleos, especialmente se optar por implantar vários nós de cluster com tamanhos de VM com vários núcleos. Para aumentar uma quota, abra um pedido de suporte do cliente online sem encargos.
* **Distribuições do Linux** -atualmente o HPC Pack suporta as seguintes distribuições de Linux para nós de computação. Pode utilizar versões de Marketplace nessas distribuições que estiverem disponíveis, ou fornecer seu próprio.
  
  * **Baseada em centOS**: 6.5, 6.6, 6.7, 7.0, 7.1 e 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6.8 e 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), 12 de SLES para HPC, 12 do SLES para HPC (Premium)
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > Para utilizar a rede de RDMA do Azure com um dos tamanhos de VM com capacidade RDMA, especifique uma imagem do HPC do SUSE Linux Enterprise Server 12 ou baseada em CentOS HPC no Azure Marketplace. Para obter mais informações, consulte [tamanhos de VM de computação de alto desempenho](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

Pré-requisitos adicionais para implementar o cluster utilizando o script de implementação do HPC Pack IaaS:

* **Computador cliente** -necessita de um computador cliente baseado no Windows para executar o script de implementação de cluster.
* **O Azure PowerShell** - [instalar e configurar o Azure PowerShell](/powershell/azure/overview) (versão 0.8.10 ou posterior) no computador cliente.
* **Script de implementação do HPC Pack IaaS** - baixe e Descompacte a versão mais recente do script do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Pode verificar a versão do script ao executar `.\New-HPCIaaSCluster.ps1 –Version`. Este artigo se baseia na versão 4.4.1 ou posterior do script.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Opção de implementação 1. Utilizar um modelo do Resource Manager
1. Vá para o [cluster HPC Pack para cargas de trabalho do Linux](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) modelo no Azure Marketplace e clique em **implementar**.
1. No portal do Azure, reveja as informações e, em seguida, clique em **criar**.
   
    ![Criação do portal][portal]
1. Sobre o **Noções básicas** painel, introduza um nome para o cluster, o que também os nomes de nó principal do VM. Pode escolher um grupo de recursos existente ou crie um grupo para a implantação num local que está disponível para. A localização afeta a disponibilidade de determinados tamanhos de VM e outros serviços do Azure (veja [produtos disponíveis por região](https://azure.microsoft.com/regions/services/)).
1. Sobre o **aceda a definições de nó** painel, para uma primeira implementação, pode aceitar geralmente as predefinições. 
   
   > [!NOTE]
   > O **URL de scripts pós-configuração** é uma definição opcional para especificar um script Windows PowerShell disponível publicamente que pretende executar no nó principal VM depois que for executado. 
   > 
   > 
1. Sobre o **configurações de nó de computação** painel, selecione um tipo de nomenclatura padrão para os nós, o número e tamanho de nós e a distribuição do Linux para implementar.
1. Sobre o **definições de infraestrutura** painel, introduza nomes para a rede virtual e do Active Directory domínio, domínio e as credenciais de administrador da VM e um padrão de nomenclatura para as contas de armazenamento.
   
   > [!NOTE]
   > HPC Pack utiliza o domínio do Active Directory para autenticar os utilizadores de cluster. 
   > 
   > 
1. Depois de executam os testes de validação e rever os termos de utilização, clique em **Compra**.

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Opção de implementação 2. Utilize o script de implementação do IaaS
Seguem-se os pré-requisitos adicionais para implementar o cluster utilizando o script de implementação do HPC Pack IaaS:

* **Computador cliente** -necessita de um computador cliente baseado no Windows para executar o script de implementação de cluster.
* **O Azure PowerShell** - [instalar e configurar o Azure PowerShell](/powershell/azure/overview) (versão 0.8.10 ou posterior) no computador cliente.
* **Script de implementação do HPC Pack IaaS** - baixe e Descompacte a versão mais recente do script do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Pode verificar a versão do script ao executar `.\New-HPCIaaSCluster.ps1 –Version`. Este artigo se baseia na versão 4.4.1 ou posterior do script.

**Ficheiro de configuração XML**

O script de implementação do HPC Pack IaaS utiliza um ficheiro de configuração XML como entrada para descrever o cluster HPC. O ficheiro de configuração de exemplo seguinte especifica um pequeno cluster consiste num nó principal HPC Pack e dois nós de computação de A7 CentOS 7.0 Linux de tamanho. 

Modificar o ficheiro, conforme necessário para o seu ambiente e a configuração de cluster pretendido e guarde-o com um nome como HPCDemoConfig.xml. Por exemplo, terá de fornecer o nome da sua subscrição e um nome de conta de armazenamento exclusivo e nome do serviço de nuvem. Além disso, poderá escolher uma imagem do Linux suportados diferente para os nós de computação. Para obter mais informações sobre os elementos no arquivo de configuração, consulte o arquivo de Manual.rtf na pasta de script e [criar um cluster HPC com o script de implementação do HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**Para executar o script de implementação do HPC Pack IaaS**

1. Abra o Windows PowerShell no computador cliente como administrador.
1. Alteração de diretório para a pasta onde o script está instalado (E:\IaaSClusterScript neste exemplo).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
1. Execute o seguinte comando para implementar o cluster HPC Pack. Este exemplo assume que o ficheiro de configuração está localizado no E:\HPCDemoConfig.xml
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Uma vez que o **AdminPassword** não está especificado no comando anterior, lhe for pedido para introduzir a palavra-passe do utilizador *MyAdminName*.
   
    b. Em seguida, inicia o script validar o ficheiro de configuração. Ele pode demorar vários minutos, consoante a ligação de rede.
   
    ![Validação][validate]
   
    c. Depois de passam validações, o script lista os recursos de cluster para criar. Introduza *Y* para continuar.
   
    ![Recursos][resources]
   
    d. O script começa a implementar o cluster HPC Pack e conclui a configuração sem ainda mais as etapas manuais. O script pode ser executado durante vários minutos.
   
    ![Implementação][deploy]
   
   > [!NOTE]
   > Neste exemplo, o script gera um arquivo de log automaticamente uma vez que o **- o ficheiro de registo** parâmetro não for especificado. Os registos não são escritos em tempo real, mas são recolhidos no final da validação e a implementação. Se o processo de PowerShell estiver parado, enquanto o script é executado, alguns registos serão perdidos.
   > 
   > 

## <a name="connect-to-the-head-node"></a>Ligar ao nó principal
Depois de implementar o cluster HPC Pack no Azure, [estabelecer ligação ao ambiente de trabalho remoto](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ao nó principal do VM através do domínio de credenciais fornecido quando implementou o cluster (por exemplo, *hpc\\clusteradmin*). Gerir o cluster do nó principal.

No nó principal, inicie o Gestor de clusters HPC para verificar o estado do cluster HPC Pack. Pode gerir e nós de computação de monitor de nós de computação de Linux, da mesma forma que trabalha com Windows. Por exemplo, verá os nós do Linux listados na **gestão de recursos** (estes nós são implementados com o **LinuxNode** modelo).

![Gerenciamento de nós][management]

Também verá os nós do Linux no **mapa térmico** vista.

![Mapa térmico][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Como mover dados num cluster connosco do Linux
Existem várias opções para mover dados entre os nós do Linux e o nó principal do Windows do cluster. Seguem-se três métodos comuns, descritos mais detalhadamente nas seções a seguir:

* **Ficheiros do Azure** -expõe uma partilha de ficheiros SMB gerida para armazenar os ficheiros de dados no armazenamento do Azure. Nós do Windows e Linux podem montar uma partilha de ficheiros do Azure como uma unidade ou pasta ao mesmo tempo, mesmo que a serem implementadas em redes virtuais diferentes.
* **Nó principal SMB partilhar** -monta uma pasta compartilhada de Windows padrão do nó principal em nós do Linux.
* **Servidor NFS de nós do head** -fornece uma solução de partilha de ficheiros para um ambiente misto do Windows e Linux.

### <a name="azure-file-storage"></a>Armazenamento de ficheiros do Azure
O [ficheiros do Azure](https://azure.microsoft.com/services/storage/files/) serviço expõe partilhas de ficheiros utilizando o protocolo SMB 2.1 padrão. VMs do Azure e serviços cloud podem partilhar dados de ficheiros entre componentes de aplicações através de partilhas montadas e as aplicações no local podem aceder a dados de ficheiros numa partilha através da API de armazenamento de ficheiros. 

Para obter passos detalhados criar uma partilha de ficheiros do Azure e montá-la no nó principal, consulte [introdução ao armazenamento de ficheiros do Azure no Windows](../../../storage/files/storage-how-to-use-files-windows.md). Para montar a partilha de ficheiros do Azure em nós do Linux, veja [como utilizar o armazenamento de ficheiros do Azure com o Linux](../../../storage/files/storage-how-to-use-files-linux.md). Para configurar a persistência de ligações, veja [Persisting ligações aos ficheiros do Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

No exemplo a seguir, crie uma partilha de ficheiros do Azure numa conta de armazenamento. Para montar a partilha no nó principal, abra uma linha de comandos e introduza os seguintes comandos:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

Neste exemplo, allvhdsje é o nome da sua conta de armazenamento, storageaccountkey é a chave de conta de armazenamento e rdma é o nome da partilha de ficheiros do Azure. A partilha de ficheiros do Azure é montada como z: no nó principal.

Para montar a partilha de ficheiros do Azure em nós do Linux, execute uma **clusrun** comando no nó principal. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  é uma ferramenta útil do HPC Pack para executar tarefas administrativas em vários nós. (Consulte também [Clusrun para nós do Linux](#Clusrun-for-Linux-nodes) neste artigo.)

Abra uma janela do Windows PowerShell e introduza os seguintes comandos:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

O primeiro comando cria uma pasta denominada /rdma em todos os nós no grupo de LinuxNodes. O segundo comando monta a ficheiros do Azure partilha allvhdsjw.file.core.windows.net/rdma para a pasta de /rdma com dir e bits de modo do ficheiro definido para 777. O segundo comando, allvhdsje é o nome da sua conta de armazenamento e storageaccountkey é a chave de conta de armazenamento.

> [!NOTE]
> O "\`" símbolo o segundo comando é um símbolo de escape para o PowerShell. "\`," significa que "," (caractere de vírgula) é uma parte do comando.
> 
> 

### <a name="head-node-share"></a>Partilha de nó principal
Em alternativa, monte uma pasta compartilhada do nó principal em nós do Linux. Uma partilha fornece a forma mais simples para partilhar ficheiros, mas o nó principal e todos os nós do Linux tem de ser implementados na mesma rede virtual. Aqui estão as etapas.

1. Crie uma pasta no nó principal e partilhá-la para todos os utilizadores com permissões de leitura/escrita. Por exemplo, partilhar D:\OpenFOAM no nó principal como \\CentOS7RDMA HN\OpenFOAM. Aqui CentOS7RDMA HN é o nome de anfitrião do nó principal.
   
    ![Permissões de partilha de ficheiros][fileshareperms]
   
    ![Partilha de ficheiros][filesharing]
1. Abra uma janela do Windows PowerShell e execute os seguintes comandos:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

O primeiro comando cria uma pasta denominada /openfoam em todos os nós no grupo de LinuxNodes. O segundo comando monta //CentOS7RDMA-HN/OpenFOAM a pasta partilhada para a pasta com dir e bits de modo do ficheiro definido para 777. O nome de utilizador e palavra-passe no comando devem ser o nome de utilizador e palavra-passe de um utilizador de cluster no nó principal. (Consulte [adicionar ou remover utilizadores de cluster](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> O "\`" símbolo o segundo comando é um símbolo de escape para o PowerShell. "\`," significa que "," (caractere de vírgula) é uma parte do comando.
> 
> 

### <a name="nfs-server"></a>Servidor NFS
O serviço NFS permite-lhe partilhar e migrar arquivos entre computadores que executam o sistema operativo Windows Server 2012 utilizando o protocolo SMB e de computadores baseados em Linux através do protocolo NFS. O servidor NFS e todos os outros nós têm de ser implementada na mesma rede virtual. Ele fornece uma melhor compatibilidade com os nós do Linux em comparação com uma partilha SMB. Por exemplo, ele suporta ligações de ficheiros.

1. Para instalar e configurar um servidor NFS, siga os passos em [servidor para a partilha de primeiro de sistema de ficheiros de rede ponto-a-ponto](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Por exemplo, crie uma partilha NFS com o nome nfs com as seguintes propriedades:
   
    ![Autorização de NFS][nfsauth]
   
    ![Permissões de partilha NFS][nfsshare]
   
    ![Permissões de NTFS de NFS][nfsperm]
   
    ![Propriedades de gestão de NFS][nfsmanage]
1. Abra uma janela do Windows PowerShell e execute os seguintes comandos:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   O primeiro comando cria uma pasta denominada /nfsshared em todos os nós no grupo de LinuxNodes. O segundo comando monta a partilha NFS CentOS7RDMA HN: / nfs para a pasta. Aqui CentOS7RDMA HN: / nfs é o caminho remoto da sua partilha NFS.

## <a name="how-to-submit-jobs"></a>Como submeter tarefas
Existem várias formas de submeter as tarefas para o cluster HPC Pack:

* Gestor de clusters HPC ou GUI de Gestor de tarefas HPC
* HPC web portal
* API REST

Submissão de tarefas para o cluster no Azure através de ferramentas de GUI do HPC Pack e o HPC web portal são as mesmas que nos Windows nós de computação. Ver [Gestor de tarefas do HPC Pack](https://technet.microsoft.com/library/ff919691.aspx) e [como submeter tarefas de um computador de cliente no local](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Para submeter tarefas através da API REST, veja [criar e submeter tarefas ao utilizar a API REST no Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Para submeter tarefas de um cliente Linux, consulte também o exemplo de Python a [SDK do HPC Pack](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Clusrun para nós do Linux
O HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) ferramenta pode ser utilizada para executar comandos em nós do Linux através do Gestor de clusters HPC ou linha de comandos. Seguem-se alguns exemplos básicos.

* Mostra os nomes de utilizador atual em todos os nós do cluster.
  
    ```command
    clusrun whoami
    ```
* Instalar o **gdb** ferramenta de depurador com **yum** em todos os nós no linuxnodes de grupo e, em seguida, reinicie os nós após 10 minutos.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Criar um script de shell apresentar cada número 1 a 10 para um segundo em cada nó do Linux no cluster, executá-lo e mostrar saída de nós de imediato.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Poderá ter de utilizar determinados caracteres de escape num **clusrun** comandos. Como é mostrado neste exemplo, utilizar ^ numa linha de comandos para escapar a ">" símbolo.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
* Tente dimensionar o cluster para um grande número de nós, ou tentar executar uma carga de trabalho do Linux no cluster. Por exemplo, veja [executar NAMD com o Microsoft HPC Pack no Linux nós de computação no Azure](hpcpack-cluster-namd.md).
* Experimentar um cluster com [VMs com capacidade RDMA, computação intensiva](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) executar cargas de trabalho MPI. Por exemplo, veja [cluster de executar o OpenFOAM com o Microsoft HPC Pack num RDMA do Linux no Azure](hpcpack-cluster-openfoam.md).
* Se estiver interessado em trabalhar com nós do Linux num cluster HPC Pack no local, consulte a [documentação de orientação do TechNet](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
