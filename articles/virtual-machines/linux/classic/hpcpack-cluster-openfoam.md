---
title: Executar o OpenFOAM com o HPC Pack em VMs do Linux | Documentos da Microsoft
description: Implementar um cluster do Microsoft HPC Pack no Azure e executar uma tarefa de OpenFOAM em vários nós de computação do Linux através de uma rede RDMA.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: a8744afe3ec3e83e4a543942441118356730347c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957246"
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Executar o OpenFoam com o Microsoft HPC Pack num cluster RDMA do Linux no Azure
Este artigo mostra-lhe uma forma de executar o OpenFoam em máquinas virtuais do Azure. Aqui, implementar um cluster do Microsoft HPC Pack connosco de computação do Linux no Azure e executar uma [OpenFoam](http://openfoam.com/) tarefa com o Intel MPI. Pode utilizar com capacidade RDMA VMs do Azure para os nós de computação, para que os nós de computação comunicam através da rede de RDMA do Azure. Outras opções para executar o OpenFoam no Azure incluem imagens comerciais totalmente configuradas e disponível no Marketplace, como do UberCloud [OpenFoam 2.3 no CentOS 6](https://azuremarketplace.microsoft.com/marketplace/apps/cfd-direct.cfd-direct-from-the-cloud)e ao executar num [do Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (para abrir o campo de operação e manipulação) é um pacote de software de código-fonte aberto de fluidos computacionais (CFD) que é amplamente usado na engenharia e ciências, em organizações comerciais e acadêmicas. Ele inclui ferramentas para meshing, notavelmente snappyHexMesh, um mesher em paralelo para geometrias de CAD complexas e para pré e pós-processamento. Quase todos os processos executam em paralelo, permitindo aos utilizadores tirar partido do hardware do computador à sua disposição.  

Microsoft HPC Pack fornece recursos para executar o HPC em larga escala e aplicativos em paralelo, incluindo aplicações MPI, em clusters de máquinas virtuais do Microsoft Azure. HPC Pack também suporta a execução Linux HPC, VMs implementadas num cluster HPC Pack de nó de computação de aplicações no Linux. Ver [começar connosco de computação do Linux num cluster HPC Pack no Azure](hpcpack-cluster.md) para uma introdução ao Linux a utilizar nós de computação com HPC Pack.

> [!NOTE]
> Este artigo ilustra como executar uma carga de trabalho de MPI do Linux com o HPC Pack. Parte do princípio de que tem um pouco familiarizado com a administração do sistema Linux e executar cargas de trabalho MPI em clusters do Linux. Se usar versões de MPI e OpenFOAM diferente das referidas neste artigo, poderá ter de modificar alguns passos de instalação e configuração. 
> 
> 

## <a name="prerequisites"></a>Pré-requisitos
* **Nós de computação de cluster HPC Pack com o Linux com capacidade RDMA** - implementar um cluster HPC Pack com tamanho A8, A9, H16r, ou com qualquer um de nós de computação do H16rm Linux uma [modelo Azure Resource Manager](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) ou um [Azure Script do PowerShell](hpcpack-cluster-powershell-script.md). Ver [começar connosco de computação do Linux num cluster HPC Pack no Azure](hpcpack-cluster.md) para a pré-requisitos e passos para qualquer uma das opções. Se escolher a opção de implementação de script do PowerShell, consulte o ficheiro de configuração de exemplo nos ficheiros de exemplo no final deste artigo. Utilize esta configuração para implementar um cluster HPC Pack baseada no Azure consiste num nó principal do tamanho A8 Windows Server 2012 R2 e 2 nós de computação de tamanho A8 SUSE Linux Enterprise Server 12. Substitua os valores adequados para a sua subscrição e o serviço de nomes. 
  
  **Adicionais que deve saber**
  
  * Para pré-requisitos de sistema de rede RDMA do Linux no Azure, consulte [tamanhos de VM de computação de alto desempenho](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Se utilizar a opção de implementação de script do Powershell, implemente todos os nós de computação do Linux dentro de um serviço cloud para utilizar a ligação de rede RDMA.
  * Depois de implementar os nós do Linux, ligar através de SSH para realizar tarefas administrativas adicionais. Encontre os detalhes de ligação de SSH para cada VM do Linux no portal do Azure.  
* **Intel MPI** - para executar o OpenFOAM em nós de computação de SLES 12 HPC no Azure, tem de instalar o tempo de execução do Intel MPI biblioteca 5 do [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/). (5 de MPI Intel é pré-instalado no imagens baseada em CentOS HPC.)  Num passo posterior, se necessário, instale a MPI Intel nos seus nós de computação do Linux. Para se preparar para este passo, depois de registar com o Intel, siga a ligação no e-mail de confirmação para a página da web relacionados. Em seguida, copie a ligação de transferência para o ficheiro de .tgz para a versão adequada do Intel MPI. Este artigo baseia-se a versão 5.0.3.048 do Intel MPI.
* **Pacote de origem OpenFOAM** -baixar o software de pacote de origem OpenFOAM do Linux a partir de [site OpenFOAM Foundation](http://openfoam.org/download/2-3-1-source/). Este artigo se baseia na versão de pacote de origem 2.3.1, disponível para download como OpenFOAM 2.3.1.tgz. Siga as instruções neste artigo para descompactar e compilar OpenFOAM em nós de computação do Linux.
* **EnSight** (opcional) – para ver os resultados da sua OpenFOAM simulação, transfira e instale o [EnSight](https://www.ansys.com/products/platform/ansys-ensight/data-interfaces) programa de visualização e análise. São informações de licenciamento e o download no EnSight site.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Configurar a fidedignidade bidirecional entre os nós de computação
A execução de uma tarefa de entre nós em vários nós do Linux requer os nós do confiam uns nos outros (por **rsh** ou **ssh**). Quando criar o cluster HPC Pack com o script de implementação do IaaS do Microsoft HPC Pack, o script configura automaticamente confiança mútua permanente para a conta de administrador que especificar. Para os utilizadores de não-administrador que criar no domínio do cluster, tem de definir temporária confiança mútua entre os nós quando uma tarefa atribuída aos mesmos e destruir a relação depois da tarefa estiver concluída. Para estabelecer confiança para cada utilizador, forneça um par de chaves RSA para o cluster que utiliza o HPC Pack para a relação de confiança.

### <a name="generate-an-rsa-key-pair"></a>Gerar um par de chaves RSA
É fácil gerar um par de chaves RSA, que contém uma chave pública e uma chave privada, ao executar a Linux **ssh-keygen** comando.

1. Inicie sessão no computador Linux.
2. Execute o seguinte comando:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Prima **Enter** para utilizar as predefinições, até que o comando é concluído. Não introduza uma frase de acesso aqui; Quando lhe for pedido para uma palavra-passe, basta pressionar **Enter**.
   > 
   > 
   
   ![Gerar um par de chaves RSA][keygen]
3. Altere o diretório para o diretório de ~/.ssh. A chave privada é armazenada no id_rsa e a chave pública no id_rsa.
   
   ![Chaves públicas e privadas][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Adicione o par de chaves para o cluster HPC Pack
1. Estabelecer uma ligação de ambiente de trabalho remoto para o seu nó principal com a sua conta de administrador do HPC Pack (a conta de administrador que definir quando executou o script de implementação).
2. Utilize os procedimentos padrão do Windows Server para criar uma conta de utilizador de domínio no domínio do Active Directory do cluster. Por exemplo, utilize a ferramenta de utilizador do Active Directory e computadores no nó principal. Os exemplos neste artigo partem do princípio de que cria um utilizador de domínio com o nome hpclab\hpcuser.
3. Crie um ficheiro denominado C:\cred.xml e copie os dados de chave RSA para o mesmo. É um ficheiro de cred.xml de exemplo no final deste artigo.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Abra um Prompt de comando e introduza o seguinte comando para definir os dados de credenciais para a conta de hpclab\hpcuser. Utilizar o **extendeddata** parâmetro para passar o nome do ficheiro C:\cred.xml que criou para os dados de chave.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Este comando for concluído com êxito sem saída. Depois de definir as credenciais para as contas de utilizador que tem de executar tarefas, armazene o ficheiro de cred.xml numa localização segura ou eliminá-lo.
5. Se para gerar o par de chaves RSA num de nós do Linux, não se esqueça de eliminar as chaves depois de concluir a usá-los. Se o HPC Pack encontrar um ficheiro de id_rsa ou do ficheiro de id_rsa existente, este não configurado confiança mútua.

> [!IMPORTANT]
> Não é recomendada para executar uma tarefa de Linux como um administrador de cluster num cluster partilhado, como uma tarefa submetido por um administrador é executado sob a conta de raiz em nós do Linux. No entanto, uma tarefa submetido por um utilizador não-administrador é executado sob uma conta de utilizador local do Linux com o mesmo nome que o utilizador de tarefa. Neste caso, o HPC Pack configura a confiança mútua para este utilizador de Linux em todos os nós alocadas para a tarefa. Pode configurar o utilizador de Linux manualmente em nós do Linux antes de executar a tarefa ou o HPC Pack cria o usuário automaticamente quando a tarefa é submetida. Se o HPC Pack cria o utilizador, o HPC Pack elimina-a depois de concluída a tarefa. Para reduzir ameaças de segurança, o HPC Pack remove as chaves após a conclusão da tarefa.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Configurar uma partilha de ficheiros de nós do Linux
Agora configure partilhas SMB padrão numa pasta no nó principal. Para permitir que os nós do Linux aceder aos ficheiros de aplicação com um caminho comum, monte a pasta partilhada em nós do Linux. Se quiser, pode utilizar outra opção, como uma partilha de ficheiros do Azure - recomendada para muitos cenários - ou uma partilha NFS de partilha de ficheiros. Consulte o ficheiro de compartilhamento de informações e passos detalhados [começar connosco de computação do Linux num Cluster HPC Pack no Azure](hpcpack-cluster.md).

1. Crie uma pasta no nó principal e partilhá-la para todos os utilizadores através da definição de privilégios de leitura/escrita. Por exemplo, partilhar C:\OpenFOAM no nó principal como \\ \\SUSE12RDMA HN\OpenFOAM. Aqui, *SUSE12RDMA HN* é o nome de anfitrião do nó principal.
2. Abra uma janela do Windows PowerShell e execute os seguintes comandos:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

O primeiro comando cria uma pasta denominada /openfoam em todos os nós no grupo de LinuxNodes. O segundo comando monta a pasta partilhada //SUSE12RDMA-HN/OpenFOAM em nós do Linux com dir_mode e file_mode definidos para 777. O *nome de utilizador* e *palavra-passe* no comando deve ser as credenciais de um utilizador no nó principal.

> [!NOTE]
> O "\`" símbolo o segundo comando é um símbolo de escape para o PowerShell. "\`," significa "," (caractere de vírgula) é uma parte do comando.
> 
> 

## <a name="install-mpi-and-openfoam"></a>Instalar MPI e OpenFOAM
Para executar o OpenFOAM como uma tarefa MPI na rede RDMA, precisa compilar o OpenFOAM com as bibliotecas do Intel MPI. 

Em primeiro lugar, executar vários **clusrun** comandos para instalar as bibliotecas do Intel MPI (se ainda não estiver instalado) e o OpenFOAM nos seus nós do Linux. Utilize a partilha de nó principal que configurou anteriormente para partilhar os ficheiros de instalação entre os nós do Linux.

> [!IMPORTANT]
> Estes instalação e passos de compilação são exemplos. Tem algum conhecimento de administração do sistema Linux para se certificar de que os compiladores dependentes e as bibliotecas estão corretamente instaladas. Poderá ter de modificar determinadas variáveis de ambiente ou outras definições para as versões do Intel MPI e OpenFOAM. Para obter detalhes, consulte [biblioteca de MPI Intel para o guia de instalação do Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) e [instalação do pacote de origem OpenFOAM](http://openfoam.org/download/2-3-1-source/) para o seu ambiente.
> 
> 

### <a name="install-intel-mpi"></a>Instalar Intel MPI
Guarde o pacote de instalação transferido para a Intel MPI (l_mpi_p_5.0.3.048.tgz neste exemplo) em C:\OpenFoam no nó principal para que os nós do Linux podem aceder este ficheiro de /openfoam. Em seguida, execute **clusrun** para instalar a biblioteca do Intel MPI em todos os nós do Linux.

1. Os seguintes comandos copiar o pacote de instalação e extraia-o para /opt/intel em cada nó.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Para instalar automaticamente a biblioteca do Intel MPI, utilize um ficheiro de silent.cfg. Pode encontrar um exemplo dos ficheiros de exemplo no final deste artigo. Coloque este ficheiro na /openfoam a pasta partilhada. Para obter detalhes sobre o ficheiro de silent.cfg, consulte [biblioteca de MPI Intel para o guia de instalação do Linux – instalação silenciosa](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Certifique-se de que guardar o ficheiro de silent.cfg como um ficheiro de texto com o Linux finais (LF apenas, não a CR LF) de linha. Este passo garante que ele é executado corretamente em nós do Linux.
   > 
   > 
3. Instale a biblioteca do Intel MPI em modo silencioso.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>Configurar MPI
Para teste, deve adicionar as seguintes linhas para /etc/security/limits.conf em cada um de nós do Linux:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Depois de atualizar o ficheiro de limits.conf, reinicie os nós do Linux. Por exemplo, utilize o seguinte **clusrun** comando:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Depois de reiniciar, certifique-se de que a pasta compartilhada está montada como /openfoam.

### <a name="compile-and-install-openfoam"></a>Compilar e instalar o OpenFOAM
Guarde o pacote de instalação transferido para o pacote de origem OpenFOAM (2.3.1.tgz OpenFOAM neste exemplo) C:\OpenFoam no nó principal para que os nós do Linux podem aceder este ficheiro de /openfoam. Em seguida, execute **clusrun** comandos para compilar o OpenFOAM em todos os nós do Linux.

1. Criar uma pasta /opt/OpenFOAM em cada nó de Linux, copie o pacote de origem para esta pasta e extraia-o aí.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. Para compilar o OpenFOAM com a biblioteca de MPI Intel, primeiro definir algumas variáveis de ambiente para a MPI Intel e OpenFOAM. Utilize um script de bash chamado settings.sh para definir as variáveis. Pode encontrar um exemplo dos ficheiros de exemplo no final deste artigo. Coloque este ficheiro (salvo com fins de linha de Linux) em /openfoam a pasta partilhada. Este ficheiro também contém definições para tempos de execução MPI e OpenFOAM que irá utilizar mais tarde para executar uma tarefa de OpenFOAM.
3. Instale pacotes dependentes necessários para compilar o OpenFOAM. Dependendo de sua distribuição do Linux, primeiro poderá ter de adicionar um repositório. Execute **clusrun** comandos semelhantes ao seguinte:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Se necessário, SSH a cada nó do Linux para executar os comandos para confirmar que eles funcionem corretamente.
4. Execute o seguinte comando para compilar o OpenFOAM. O processo de compilação leva algum tempo a concluir e gera uma grande quantidade de informações de registo para a saída padrão, por isso, utilize o **/ intercalado** opção para apresentar a saída intercalada.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > O "\`" símbolo no comando é um símbolo de escape para o PowerShell. "\`&" significa que o "e" é uma parte do comando.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>Preparar para executar uma tarefa de OpenFOAM
Prepare-se agora executar uma tarefa MPI chamada sloshingTank3D, que é um dos exemplos de OpenFoam, em dois nós do Linux. 

### <a name="set-up-the-runtime-environment"></a>Configurar o ambiente de tempo de execução
Para configurar os ambientes de runtime de MPI e OpenFOAM em nós do Linux, execute o seguinte comando numa janela do Windows PowerShell no nó principal. (Este comando é válido para o SUSE Linux apenas).

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Preparar dados de exemplo
Utilize a partilha de nó principal que configurou anteriormente para partilhar ficheiros entre os nós do Linux (montados como /openfoam).

1. Nós de computação de SSH para uma das suas Linux.
2. Execute o seguinte comando para configurar o ambiente de tempo de execução OpenFOAM, se ainda não fez isso.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Copie o exemplo de sloshingTank3D para a pasta partilhada e navegar até ele.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Ao utilizar os parâmetros de predefinição deste exemplo, pode demorar dezenas de minutos para ser executado, pelo que pode modificar alguns parâmetros para que ele seja executado mais rapidamente. É uma opção simple modificar as deltaT de variáveis do tempo passo e writeInterval no ficheiro de sistema/controlDict. Esse arquivo armazena todos os dados de entrada relacionadas com o controle de tempo e de leitura e escrita de dados de solução. Por exemplo, pode alterar o valor de deltaT de 0,05 como 0,5 e o valor de writeInterval de 0,05 como 0,5.
   
   ![Modificar variáveis de passo][step_variables]
5. Especifique os valores pretendidos para as variáveis no ficheiro de sistema/decomposeParDict. Este exemplo utiliza dois nós do Linux cada com 8 núcleos, por isso, defina numberOfSubdomains 16 e n hierarchicalCoeffs para (1 1 16), que significa executa OpenFOAM em paralelo com 16 processos. Para obter mais informações, consulte [Guia do usuário OpenFOAM: 3.4 aplicativos em execução em paralelo](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Decompor processos][decompose]
6. Execute os seguintes comandos a partir do diretório sloshingTank3D para preparar os dados de exemplo.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. No nó principal, deverá ver que os ficheiros de dados de exemplo são copiados para C:\OpenFoam\sloshingTank3D. (C:\OpenFoam é a pasta partilhada no nó principal).
   
   ![Ficheiros de dados no nó principal][data_files]

### <a name="host-file-for-mpirun"></a>Arquivo de Host para mpirun
Neste passo, vai criar um ficheiro de anfitrião (uma lista de nós de computação) que o **mpirun** comando utiliza.

1. Num de nós do Linux, crie um ficheiro denominado hostfile em /openfoam, para que este ficheiro pode ser contatado pelo /openfoam/hostfile em todos os nós do Linux.
2. Escreva seus nomes de nó do Linux para este ficheiro. Neste exemplo, o ficheiro contém os seguintes nomes:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Também pode criar este ficheiro na C:\OpenFoam\hostfile no nó principal. Se escolher esta opção, guarde-o como um arquivo de texto com o Linux linha finais (LF apenas, não a CR LF). Isto garante que ele é executado corretamente em nós do Linux.
   > 
   > 
   
   **Wrapper de script de bash**
   
   Se tiver muitos nós do Linux e pretender que a tarefa para serem executadas em apenas alguns deles, não é uma boa idéia usar um arquivo de anfitrião fixo, porque não sabe que nós serão atribuídos para a sua tarefa. Neste caso, escrever um wrapper de script de bash para **mpirun** para criar o ficheiro de anfitriões automaticamente. Pode encontrar um wrapper de script de bash de exemplo chamado hpcimpirun.sh no final deste artigo e guarde-o como /openfoam/hpcimpirun.sh. Este script de exemplo faz o seguinte:
   
   1. Configura as variáveis de ambiente para **mpirun**e alguns parâmetros de comando de adição para executar o trabalho MPI através da rede RDMA. Neste caso, ele define as variáveis seguintes:
      
      * I_MPI_FABRICS=shm:dapl
      * I_MPI_DAPL_PROVIDER=ofa-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION=0
   2. Cria um ficheiro de anfitriões, de acordo com o ambiente de variável $CCP_NODES_CORES, que é definida ao nó principal HPC quando a tarefa é ativada.
      
      O formato de $CCP_NODES_CORES segue este padrão:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      onde
      
      * `<Number of nodes>` -o número de nós atribuídos a esta tarefa.  
      * `<Name of node_n_...>` -o nome de cada nó alocada para esta tarefa.
      * `<Cores of node_n_...>` -o número de núcleos no nó atribuído a esta tarefa.
      
      Por exemplo, se a tarefa precisa de dois nós para executar, $CCP_NODES_CORES é semelhante a
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Chamadas a **mpirun** de comandos e acrescenta dois parâmetros à linha de comandos.
      
      * `--hostfile <hostfilepath>: <hostfilepath>` -o caminho do arquivo host cria o script
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` -uma variável de ambiente definida pelo nó principal HPC Pack, que armazena o número de total de núcleos alocados para esta tarefa. Neste caso, especifica o número de processos para **mpirun**.

## <a name="submit-an-openfoam-job"></a>Submeter uma tarefa de OpenFOAM
Agora pode submeter um trabalho no Gestor de clusters de HPC. Tem de introduzir o script hpcimpirun.sh as linhas de comando para algumas das tarefas de trabalho.

1. Ligar ao seu nó principal do cluster e iniciar o Gestor de clusters HPC.
2. **Na gestão de recursos**, certifique-se de que os nós de computação do Linux estão a **Online** estado. Se não estiverem, selecione-os e clique em **colocar Online**.
3. Na **gerenciamento de trabalhos**, clique em **nova tarefa**.
4. Introduza um nome para a tarefa, tal como *sloshingTank3D*.
   
   ![Detalhes da tarefa][job_details]
5. Na **recursos da tarefa**, escolha o tipo de recurso como "Nó" e definir o mínimo de 2. Esta configuração é executada a tarefa em dois nós do Linux, cada uma delas tiver oito núcleos neste exemplo.
   
   ![Recursos da tarefa][job_resources]
6. Clique em **editar tarefas** na navegação à esquerda e, em seguida, clique **Add** para adicionar uma tarefa ao trabalho. Adicione quatro tarefas ao trabalho com as seguintes linhas de comandos e as definições.
   
   > [!NOTE]
   > Executar `source /openfoam/settings.sh` configura em ambientes de runtime OpenFOAM e MPI, para que cada uma das seguintes tarefas chama antes do comando OpenFOAM.
   > 
   > 
   
   * **Tarefa 1**. Execute **decomposePar** para gerar arquivos de dados para execução **interDyMFoam** em paralelo.
     
     * Atribuir um nó para a tarefa
     * **Linha de comandos** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Diretório de trabalho** -/ openfoam/sloshingTank3D
     
     Consulte a figura a seguir. Configurar as tarefas restantes da mesma forma.
     
     ![Detalhes da tarefa 1][task_details1]
   * **Tarefa 2**. Execute **interDyMFoam** em paralelo para o exemplo de computação.
     
     * Atribuir dois nós para a tarefa
     * **Linha de comandos** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Diretório de trabalho** -/ openfoam/sloshingTank3D
   * **Tarefa 3**. Execute **reconstructPar** para intercalar os conjuntos de diretórios de tempo de cada diretório processor_N_ num único conjunto.
     
     * Atribuir um nó para a tarefa
     * **Linha de comandos** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Diretório de trabalho** -/ openfoam/sloshingTank3D
   * **Tarefa 4**. Execute **foamToEnsight** em paralelo para converter o resultado de OpenFOAM ficheiros para EnSight formatar e colocar os arquivos de EnSight num diretório chamado Ensight no diretório maiúsculas.
     
     * Atribuir dois nós para a tarefa
     * **Linha de comandos** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Diretório de trabalho** -/ openfoam/sloshingTank3D
7. Adicione as dependências para estas tarefas por tarefa ordem ascendente.
   
   ![Dependências de tarefas][task_dependencies]
8. Clique em **submeter** para executar esta tarefa.
   
   Por predefinição, o HPC Pack submete o trabalho como a sua conta de utilizador com sessão iniciada atual. Depois de clicar em **submeter**, poderá ver uma caixa de diálogo solicitando que introduza o nome de utilizador e palavra-passe.
   
   ![Credenciais da tarefa][creds]
   
   Em algumas condições, o HPC Pack lembra as informações do utilizador antes de entrada e não mostrar esta caixa de diálogo. Para tornar o HPC Pack mostrá-lo novamente, introduza o seguinte comando no prompt de comando e, em seguida, submeter a tarefa.
   
   ```
   hpccred delcreds
   ```
9. A tarefa demora a partir de dezenas de minutos a várias horas de acordo com os parâmetros que configurou para o exemplo. No mapa térmico, verá a tarefa em execução em nós do Linux. 
   
   ![Mapa térmico][heat_map]
   
   Em cada nó, oito processos são iniciados.
   
   ![Processos do Linux][linux_processes]
10. Quando a tarefa é concluída, localize os resultados das tarefas em pastas sob C:\OpenFoam\sloshingTank3D e os ficheiros de registo em C:\OpenFoam.

## <a name="view-results-in-ensight"></a>Exibir resultados em EnSight
Opcionalmente, utilize [EnSight](http://www.ensight.com/) visualize e analise os resultados da tarefa OpenFOAM. 

1. Depois de instalar EnSight no nó principal, inicie-o.
2. Abra C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Verá um tanque no Visualizador.
   
   ![Tanque em EnSight][tank]
3. Criar uma **Isosurface** partir **internalMesh**e, em seguida, escolha a variável **alpha_water**.
   
   ![Criar um isosurface][isosurface]
4. Definir a cor para **Isosurface_part** criado no passo anterior. Por exemplo, defina-o como água azul.
   
   ![Editar a cor de isosurface][isosurface_color]
5. Criar uma **Iso-volume** partir **paredes** selecionando **paredes** no **partes** painel e clique no **Isosurfaces** botão na barra de ferramentas.
6. Na caixa de diálogo, selecione **tipo** como **Isovolume** e defina o mínimo de **Isovolume intervalo** como 0,5. Para criar o isovolume, clique em **criar com partes selecionados**.
7. Definir a cor para **Iso_volume_part** criado no passo anterior. Por exemplo, defina-o como água profunda azul.
8. Definir a cor para **paredes**. Por exemplo, defina-o para branco transparente.
9. Agora, clique em **reproduzir** para ver os resultados da simulação.
   
    ![Resultado de tanque][tank_result]

## <a name="sample-files"></a>Ficheiros de exemplo
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Ficheiro de configuração do XML de exemplo para implementação de cluster por script do PowerShell
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Ficheiro de cred.xml de exemplo
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Ficheiro de silent.cfg de exemplo para instalar o MPI
```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Exemplo de script settings.sh
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Exemplo de script hpcimpirun.sh
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
