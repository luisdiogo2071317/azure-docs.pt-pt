---
title: Configurar a replicação do sistema de SAP HANA em máquinas de virtuais (VMs) do Azure | Microsoft Docs
description: Estabelecer uma elevada disponibilidade de SAP HANA em máquinas de virtuais (VMs) do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/24/2018
ms.author: sedusch
ms.openlocfilehash: e23c69b020c96dab4d119ebf4f45791e6f63c4af
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656844"
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Elevada disponibilidade de SAP HANA em máquinas de virtuais (VMs) do Azure

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2388694]:https://launchpad.support.sap.com/#/notes/2388694

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

No local, pode utilizar a replicação do sistema HANA ou utilizar o armazenamento partilhado para estabelecer a elevada disponibilidade para SAP HANA.
Na replicação do sistema de HANA de VMs do Azure no Azure é que a única função de elevada disponibilidade suportadas de até ao momento. SAP HANA replicação é composta por um nó primário e pelo menos um nó secundário. As alterações dos dados no nó principal são replicadas para o nó secundário de forma síncrona ou assíncrona.

Este artigo descreve como implementar as máquinas virtuais, configurar as máquinas virtuais, instale a estrutura de cluster, instalar e configurar a replicação do sistema de SAP HANA.
As configurações de exemplo, o número de instância de etc. 03 os comandos de instalação e HANA sistema ID HN1 é utilizado.

Leia primeiro as seguintes notas de SAP e papers

* A nota [1928533], que tem:
  * Lista de tamanhos de VM do Azure que são suportados para a implementação de software do SAP
  * Informações de capacidade importantes para tamanhos de VM do Azure
  * Software SAP suportado e o sistema operativo (SO) e combinações de base de dados
  * Versão necessária de kernel SAP para o Windows e Linux no Microsoft Azure
* A nota [2015553] apresenta uma lista de pré-requisitos suportado de SAP SAP para implementações de software no Azure.
* A nota [2205917] tem definições de SO para SUSE Linux Enterprise Server para aplicações SAP recomendadas
* A nota [1944799] tem SAP HANA diretrizes para SUSE Linux Enterprise Server para aplicações SAP
* A nota [2178632] tem informações detalhadas sobre todas as monitorizações métricas que relatados para SAP no Azure.
* A nota [2191498] tem a versão necessária do agente de anfitrião do SAP para Linux no Azure.
* A nota [2243692] tem informações sobre o licenciamento SAP no Linux no Azure.
* A nota [1984787] tem informações gerais sobre o SUSE Linux Enterprise Server 12.
* A nota [1999351] tem informações adicionais de resolução de problemas para o Azure avançada extensão de monitorização para SAP.
* [WIKI do SAP Comunidade](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tem todas as necessárias SAP notas para Linux.
* [Azure máquinas virtuais de planeamento e implementação de SAP no Linux][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP no Linux (Este artigo)][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP no Linux][dbms-guide]
* [Cenário de com otimização de desempenho do SAP HANA SR] [ suse-hana-ha-guide] o guia contém informações de todas as necessárias para configurar a replicação do sistema do SAP HANA no local. Utilize este guia como uma linha de base.

## <a name="overview"></a>Descrição geral

Para alcançar a elevada disponibilidade, SAP HANA está instalado no duas máquinas virtuais. Os dados são replicados utilizando a replicação do sistema de HANA.

![Descrição geral de SAP HANA elevada disponibilidade](./media/sap-hana-high-availability/ha-suse-hana.png)

As utilizações de configuração de SAP HANA SR um nome de anfitrião virtual dedicado e virtual endereços IP. No Azure, é necessário um balanceador de carga a utilizar um endereço IP virtual. A lista seguinte mostra a configuração de Balanceador de carga.

* Configuração de front-end
  * Endereço IP 10.0.0.13 para hn1-db
* Configuração de back-end
  * Ligado a interfaces de rede principal de todas as máquinas virtuais que deve fazer parte da replicação do sistema de HANA
* Porta da sonda
  * Porta 62503
* Regras de carga
  * 30313 TCP
  * 30315 TCP
  * 30317 TCP

## <a name="deploying-linux"></a>Implementar Linux

O agente de recursos para SAP HANA está incluído no SUSE Linux Enterprise Server para aplicações SAP.
O Azure Marketplace contém uma imagem para o SUSE Linux Enterprise Server para SAP aplicações 12, que pode utilizar para implementar novas máquinas virtuais.

### <a name="deploy-with-template"></a>Implementar com modelo
Pode utilizar um dos modelos de início rápido no github para implementar todos os recursos necessários. O modelo implementa as máquinas virtuais, o Balanceador de carga, etc de conjunto de disponibilidade. Para implementar o modelo, siga estes passos:

1. Abra o [modelo de base de dados] [ template-multisid-db] ou [convergido modelo] [ template-converged] no portal do Azure. 
   O modelo de base de dados cria apenas as regras de balanceamento de carga para uma base de dados, enquanto o modelo convergido também cria as regras de balanceamento de carga para um ASCS/SCS e instância ERS (apenas Linux). Se planear instalar um sistema NetWeaver SAP com base e também instalar a instância ASCS/SCS nas mesmas máquinas, utilize o [convergido modelo][template-converged].
1. Introduza os seguintes parâmetros
    1. ID de sistema do SAP  
       Introduza o ID de sistema SAP do sistema SAP que pretende instalar. O ID de vai ser utilizado como um prefixo para os recursos que são implementados.
    1. Tipo de pilha (apenas é aplicável se utilizar o modelo convergido)   
       Selecione o tipo de pilha do SAP NetWeaver
    1. Tipo de SO  
       Selecione um das distribuições de Linux. Para este exemplo, selecione o SLES 12
    1. Tipo de base de dados  
       Selecione HANA
    1. Tamanho do sistema de SAP  
       A quantidade de SAPS o novo sistema vai fornecer. Se tiver a não certeza SAPS quantos requer que o sistema, peça ao seu parceiro de tecnologia de SAP ou integrador de sistema
    1. Disponibilidade do sistema  
       Selecione HA
    1. Nome de utilizador de administrador e a palavra-passe de administrador  
       Um novo utilizador é criado que podem ser utilizadas para iniciar sessão na máquina.
    1. Sub-rede nova ou existente  
       Determina se devem ser criadas uma nova rede virtual e uma sub-rede ou se deve ser utilizada uma sub-rede existente. Se já tiver uma rede virtual que está ligada à sua rede no local, selecione existente.
    1. ID de sub-rede  
    O ID de sub-rede à qual as máquinas virtuais devem ser ligadas. Para ligar a máquina virtual à sua rede no local, selecione a sub-rede da rede virtual VPN ou Express Route. O ID de aspeto normalmente /subscriptions/{targetsubscriptionid}/resourcegroups/{targetresourcegroupname}`<subscription ID`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

### <a name="manual-deployment"></a>Implementação manual

1. Criar um Grupo de Recursos
1. Criar uma rede Virtual
1. Criar um conjunto de disponibilidade  
   Domínio de atualização máximo do conjunto
1. Criar um balanceador de carga (interno)  
   Selecione a VNET que criou no segundo
1. Criar Máquina Virtual 1  
   Utilizar, pelo menos, SP1 de 12 SLES4SAP, neste exemplo utilizaremos a imagem de SLES4SAP 12 SP2 https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES para SAP 12 SP2 (Premium)  
   Selecione o conjunto de disponibilidade criado anteriormente  
1. Criar Máquina Virtual 2  
   Utilizar, pelo menos, SP1 de 12 SLES4SAP, neste exemplo utilizaremos a imagem de SLES4SAP 12 SP1 BYOS https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES para SAP 12 SP2 (Premium)  
   Selecione o conjunto de disponibilidade criado anteriormente  
1. Adicionar discos de dados
1. Configurar o Balanceador de carga
    1. Criar um conjunto IP de front-end
        1. Abra o Balanceador de carga, selecione o conjunto IP de front-end e clique em Adicionar
        1. Introduza o nome do novo conjunto IP Front-end (por exemplo hana-front-end)
        1. Como a atribuição estática e introduza o endereço IP (por exemplo **10.0.0.13**)
        1. Clique em OK
        1. Depois de criar o novo conjunto IP de front-end, anote o endereço IP
    1. Criar um conjunto de back-end
        1. Abra o Balanceador de carga, selecione os conjuntos de back-end e clique em Adicionar
        1. Introduza o nome do novo conjunto de back-end (por exemplo hana-back-end)
        1. Clique em Adicionar uma máquina virtual
        1. Selecione o conjunto de disponibilidade que criou anteriormente
        1. Selecione as máquinas virtuais do cluster de SAP HANA
        1. Clique em OK
    1. Criar uma sonda de estado de funcionamento
        1. Abra o Balanceador de carga, selecione sondas de estado de funcionamento e clique em Adicionar
        1. Introduza o nome da sonda de estado de funcionamento novo (por exemplo hana-hp)
        1. Selecione TCP como protocolo, porta 625**03**, mantenha o intervalo de 5 e limiar de mau estado de funcionamento 2
        1. Clique em OK
    1. SAP HANA 1.0: Criar regras de balanceamento de carga
        1. Abra o Balanceador de carga, selecione as regras de balanceamento de carga e clique em Adicionar
        1. Introduza o nome da nova regra de Balanceador de carga (por exemplo hana-lb-3**03**15)
        1. Selecione o endereço IP de front-end, o conjunto back-end e a sonda do Estado de funcionamento que criou anteriormente (por exemplo hana-front-end)
        1. Manter o protocolo TCP, introduza a porta 3**03**15
        1. Aumentar o tempo limite de inatividade e 30 minutos
        1. **Certifique-se de ativar o IP flutuante**
        1. Clique em OK
        1. Repita os passos acima para a porta 3**03**17
    1. SAP HANA 2.0: Criar regras para a base de dados do sistema de balanceamento de carga
        1. Abra o Balanceador de carga, selecione as regras de balanceamento de carga e clique em Adicionar
        1. Introduza o nome da nova regra de Balanceador de carga (por exemplo hana-lb-3**03**13)
        1. Selecione o endereço IP de front-end, o conjunto back-end e a sonda do Estado de funcionamento que criou anteriormente (por exemplo hana-front-end)
        1. Manter o protocolo TCP, introduza a porta 3**03**13
        1. Aumentar o tempo limite de inatividade e 30 minutos
        1. **Certifique-se de ativar o IP flutuante**
        1. Clique em OK
        1. Repita os passos acima para a porta 3**03**14
    1. SAP HANA 2.0: Criar regras para primeiro de balanceamento de carga a base de dados do inquilino
        1. Abra o Balanceador de carga, selecione as regras de balanceamento de carga e clique em Adicionar
        1. Introduza o nome da nova regra de Balanceador de carga (por exemplo hana-lb-3**03**40)
        1. Selecione o endereço IP de front-end, o conjunto back-end e a sonda do Estado de funcionamento que criou anteriormente (por exemplo hana-front-end)
        1. Manter o protocolo TCP, introduza a porta 3**03**40
        1. Aumentar o tempo limite de inatividade e 30 minutos
        1. **Certifique-se de ativar o IP flutuante**
        1. Clique em OK
        1. Repita os passos acima para a porta 3**03**41 e 3**03**42

Para mais informações sobre as portas necessárias para SAP HANA, leia o capítulo [ligações às bases de dados do inquilino](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) do [bases de dados do SAP HANA inquilino](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) guia ou [2388694 de nota SAP] [2388694].


## <a name="create-pacemaker-cluster"></a>Criar Pacemaker cluster

Siga os passos no [configurar Pacemaker no SUSE Linux Enterprise Server no Azure](high-availability-guide-suse-pacemaker.md) para criar um cluster de Pacemaker básico para este servidor HANA. Também pode utilizar o mesmo cluster Pacemaker para SAP HANA e SAP NetWeaver (A) SCS.

## <a name="installing-sap-hana"></a>Instalar o SAP HANA

Os seguintes itens são o prefixo um **[A]** - aplicáveis a todos os nós, **[1]** - só é aplicável ao nó de 1 ou **[2]** - apenas aplicável a 2 de nó do Pacemaker cluster.

1. **[A]**  Esquema de disco de configuração
    1. LVM  
       
       Em geral, recomendamos que utilize LVM para volumes que armazenam dados e ficheiros de registo. O seguinte exemplo parte do princípio de que as máquinas virtuais tem quatro dados os discos ligados que devem ser utilizados para criar dois volumes.

       Listar todos os discos disponíveis
       
       <pre><code>
       ls /dev/disk/azure/scsi1/lun*
       </code></pre>
       
       Exemplo de saída
       
       ```
       /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
       ```
       
       Crie volumes de físicos para todos os discos que pretende utilizar.    
       <pre><code>
       sudo pvcreate /dev/disk/azure/scsi1/lun0
       sudo pvcreate /dev/disk/azure/scsi1/lun1
       sudo pvcreate /dev/disk/azure/scsi1/lun2
       sudo pvcreate /dev/disk/azure/scsi1/lun3
       </code></pre>

       Criar um grupo de volume para os ficheiros de dados, um grupo de volume para os ficheiros de registo e outro para o diretório partilhado de SAP HANA

       <pre><code>
       sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
       sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
       sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
       </code></pre>
       
        Crie os volumes lógicos. Linear volume será criada ao utilizar lvcreate sem -i mudar. Sugerimos para criar um volume repartido para um melhor desempenho de e/s, -i argumento deve ser igual ao número do volume físico subjacente. Neste documento, 2 volumes físicos são utilizados para o volume de dados, para o argumento de comutador -i 2. 1 volume físico é utilizado para o volume de registo, por isso, não -i comutador é utilizado explicitamente. Utilize -i mudar e substitua o número para o mesmo número de volume físico subjacente quando estiver a utilizar mais do que 1 volume físico para cada dados, o registo ou a volumes partilhados.

       <pre><code>
       sudo lvcreate <b>-i 2</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
       sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
       sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
       sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       </code></pre>
       
       Criar os diretórios de montagem e copie o UUID de todos os volumes lógicos
       
       <pre><code>
       sudo mkdir -p /hana/data/<b>HN1</b>
       sudo mkdir -p /hana/log/<b>HN1</b>
       sudo mkdir -p /hana/shared/<b>HN1</b>
       # write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       sudo blkid
       </code></pre>
       
       Criação de entradas de fstab para os volumes lógicas três
       
       <pre><code>
       sudo vi /etc/fstab
       </code></pre>
       
       Inserir a linha para /etc/fstab
       
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
       </code></pre>
       
       Montar os novos volumes
       
       <pre><code>
       sudo mount -a
       </code></pre>
    
    1. Discos simples  
       Para sistemas de demonstração, é possível colocar os ficheiros de dados e de registo HANA num disco. Os seguintes comandos crie uma partição no /dev/disk/azure/scsi1/lun0 e formate-o com xfs.

       <pre><code>
       sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
       sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
       
       # write down the ID of /dev/disk/azure/scsi1/lun0-part1
       sudo /sbin/blkid
       sudo vi /etc/fstab
       </code></pre>

       Inserir a linha para /etc/fstab
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
       </code></pre>

       Criar o diretório de destino e montar o disco.

       <pre><code>
       sudo mkdir /hana
       sudo mount -a
       </code></pre>

1. **[A]**  Resolução do nome de anfitrião de configuração para todos os anfitriões  
    Pode utilizar um servidor DNS ou modificar os /etc/hosts em todos os nós. Este exemplo mostra como utilizar o ficheiro /etc/hosts.
   Substituir o endereço IP e o nome de anfitrião nos seguintes comandos
    ```bash
    sudo vi /etc/hosts
    ```
    Insira as seguintes linhas ao /etc/hosts. Alterar o endereço IP e nome de anfitrião para corresponder ao seu ambiente    
    
   <pre><code>
   <b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]**  Pacotes instalar HANA HA  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

Para instalar a replicação do sistema de SAP HANA, siga o capítulo 4 do guia de SAP HANA SR desempenho optimizado do cenário no https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/

1. **[A]**  Executar hdblcm a partir do HANA DVD
    * Escolha a instalação -> 1
    * Selecione os componentes adicionais para instalação-1 >
    * Introduza o caminho de instalação [hana/partilhado]: -> ENTER
    * Introduza o nome de anfitrião Local [.]: -> ENTER
    * Pretende adicionar a anfitriões adicionais para o sistema? (s/n) [n]: -> ENTER
    * Introduza o ID de sistema do SAP HANA: <SID of HANA e.g. HN1>
    * Introduza o número de instância [00]:   
  Número de instância HANA. Utilizar 03 se utilizou o modelo do Azure ou seguido a implementação manual
    * Selecionar o modo de base de dados / introduza índice [1]: -> ENTER
    * Selecionar a utilização do sistema / introduza índice [4]:  
  Selecione o sistema de utilização
    * Introduza a localização de Volumes de dados [/ dados/hana/HN1]: -> ENTER
    * Introduza a localização de Volumes de registo [/ registo/hana/HN1]: -> ENTER
    * Restringir a atribuição de memória máxima? [n]: -> ENTER
    * Introduza o nome de anfitrião de certificado para o anfitrião "..." [...]: -> ENTER
    * Introduza o utilizador de agente de anfitrião do SAP (sapadm) palavra-passe:
    * Confirme o SAP anfitrião de agente de utilizador (sapadm) palavra-passe:
    * Introduza o administrador de sistema (hdbadm) palavra-passe:
    * Confirme (hdbadm) palavra-passe de administrador de sistema:
    * Introduza o diretório de raiz de administrador de sistema [/ usr/sap/HN1/home]: -> ENTER
    * Introduza a Shell de início de sessão de administrador de sistema [/ bin/ostrar]: -> ENTER
    * Introduza o ID de utilizador de administrador de sistema [1001]: -> ENTER
    * Introduza ID do grupo de utilizadores (sapsys) [79]: -> ENTER
    * Introduza a palavra-passe de utilizador (sistema) de base de dados:
    * Confirme a palavra-passe de utilizador (sistema) de base de dados:
    * Reiniciar o sistema após o reinício do computador? [n]: -> ENTER
    * Pretende continuar? (s/n):   
  O resumo de validação e introduza s para continuar

1. **[A]**  Atualizar o agente do anfitrião SAP  
  Transferir o arquivo de agente do anfitrião SAP mais recente do [SAP Softwarecenter] [ sap-swcenter] e execute o seguinte comando para atualizar o agente. Substitua o caminho para o arquivo para apontar para o ficheiro que transferiu.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

## <a name="configure-sap-hana-20-system-replication"></a>Configurar a replicação do SAP HANA sistema 2.0

Os seguintes itens são o prefixo um **[A]** - aplicáveis a todos os nós, **[1]** - só é aplicável ao nó de 1 ou **[2]** - apenas aplicável a 2 de nó do Pacemaker cluster.

1. **[1]**  Criar base de dados do inquilino

   Se estiver a utilizar o SAP HANA 2.0 ou MDC, crie uma base de dados do inquilino para o seu sistema SAP NetWeaver. Substitua o SID do seu sistema SAP NW1.

   Inicie sessão como `<hanasid`> adm e execute o seguinte comando

   <pre><code>
   hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  Configurar a replicação do sistema no primeiro nó
   
   Inicie sessão como `<hanasid`> adm e as bases de dados de cópia de segurança

   <pre><code>
   hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   Copie os ficheiros PKI do sistema para secundário

   <pre><code>
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Crie o site primário.

   <pre><code>
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Configurar a replicação do sistema no segundo nó
    
    Registe o segundo nó para iniciar a replicação do sistema. Inicie sessão como `<hanasid`> adm e execute o seguinte comando

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>Configurar a replicação do SAP HANA 1.0 do sistema

1. **[1]**  Criar os utilizadores necessários

    Inicie sessão como raiz e execute o seguinte comando. Certifique-se de que substitui a negrito cadeias (HANA sistema ID HN1 e instância número 03) com os valores a instalação de SAP HANA.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. **[A]**  Criar keystore entrada
   
    Inicie sessão como raiz e execute o seguinte comando para criar uma nova entrada de keystore.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>

1. **[1]**  Base de dados de cópia de segurança

   Inicie sessão como raiz e as bases de dados de cópia de segurança

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Se utilizar uma instalação de multi-inquilino, também de cópia de segurança da base de dados do inquilino

   <pre><code>   
   hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  Configurar a replicação do sistema no primeiro nó
    
    Inicie sessão como `<hanasid`> adm e criar o site primário.

    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>

1. **[2]**  Configurar a replicação do sistema no nó secundário.

    Inicie sessão como `<hanasid`> adm e registar o site secundário.

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>Criar recursos de cluster de SAP HANA

   Em primeiro lugar, crie a topologia HANA. Execute os seguintes comandos num de nós do cluster Pacemaker.
   
   <pre><code>
   sudo crm configure property maintenance-mode=true

   # replace the bold string with your instance number and HANA system ID
   
   sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
     operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"
   
   sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
   </code></pre>
   
   Em seguida, crie os recursos HANA.
   
   <pre><code>
   # replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
      
   sudo crm configure primitive rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations \$id="rsc_sap_<b>HN1</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
   
   sudo crm configure ms msl_SAPHana_<b>HN1</b>_HDB<b>03</b> rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
   
   sudo crm configure primitive rsc_ip_<b>HN1</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \
     meta target-role="Started" is-managed="true" \
     operations \$id="rsc_ip_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10s" timeout="20s" \
     params ip="<b>10.0.0.13</b>"
   
   sudo crm configure primitive rsc_nc_<b>HN1</b>_HDB<b>03</b> anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g_ip_<b>HN1</b>_HDB<b>03</b> rsc_ip_<b>HN1</b>_HDB<b>03</b> rsc_nc_<b>HN1</b>_HDB<b>03</b>
   
   sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 4000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  
   
   sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
   
   # Cleanup the HANA resources. The HANA resources might have failed because of a known issue.
   sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

   sudo crm configure property maintenance-mode=false
   </code></pre>

   Certifique-se de que o estado de cluster está ok e de que todos os recursos são iniciados. Não é importante no nó que os recursos estão em execução.

   <pre><code>
   sudo crm_mon -r
   
   # Online: [ hn1-db-0 hn1-db-1 ]
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): Started hn1-db-0
   # rsc_st_azure    (stonith:fence_azure_arm):      Started hn1-db-1
   # Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
   #     Started: [ hn1-db-0 hn1-db-1 ]
   # Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
   #     Masters: [ hn1-db-0 ]
   #     Slaves: [ hn1-db-1 ]
   # Resource Group: g_ip_HN1_HDB03
   #     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
   #     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

### <a name="test-cluster-setup"></a>Configuração do cluster de teste
Este capítulo descreve como pode testar a configuração. Cada teste parte do princípio de que são raiz e o mestre de SAP HANA está em execução na máquina virtual hn1-db-0.

#### <a name="fencing-test"></a>Teste fencing

Pode testar a configuração do agente fencing ao desativar a interface de rede no nó hn1-db-0.

<pre><code>
sudo ifdown eth0
</code></pre>

A máquina virtual deve agora obter reiniciada ou parada consoante a configuração de cluster.
Se definir a ação de stonith para desativado, a máquina virtual vai ser parado e os recursos são migrados para a máquina virtual em execução.

Depois de iniciar a máquina virtual novamente, o recurso de SAP HANA falha a iniciação como secundário se definir AUTOMATED_REGISTER = "false". Neste caso, configure a instância HANA como secundária ao executar este comando:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testar uma ativação pós-falha manual

Pode testar uma ativação pós-falha manual, parando o serviço de pacemaker no nó hn1-db-0.
<pre><code>
service pacemaker stop
</code></pre>

Após a ativação pós-falha, pode iniciar o serviço novamente. Se definir AUTOMATED_REGISTER = "false", o recurso de SAP HANA no hn1-db-0 falha ao iniciar como secundária. Neste caso, configure a instância HANA como secundária ao executar este comando:

<pre><code>
service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-migration"></a>Uma migração de teste

Pode migrar o nó mestre de SAP HANA executando o seguinte comando
<pre><code>
crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
crm resource migrate g_ip_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Se definir AUTOMATED_REGISTER = "false", esta sequência de comandos deve migrar o nó mestre de SAP HANA e o grupo que contém o endereço IP virtual para hn1-db-1.
O recurso de SAP HANA hn1-db-0 não consegue iniciar como secundária. Neste caso, configure a instância HANA como secundária ao executar este comando:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

A migração cria as restrições de localização que precisam de ser eliminadas novamente.

<pre><code>
crm configure edited

# Delete location constraints that are named like the following contraint. You should have two constraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HN1</b>_HDB<b>03</b> g_ip_<b>HN1</b>_HDB<b>03</b> role=Started inf: <b>hn1-db-1</b>
</code></pre>

Também terá de limpar o estado do recurso de nó secundário

<pre><code>
# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

## <a name="next-steps"></a>Passos Seguintes
* [Azure máquinas virtuais de planeamento e implementação de SAP][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP][dbms-guide]
* Para saber como estabelecer elevada disponibilidade e o plano de recuperação de desastres do SAP HANA no Azure (instâncias de grande), consulte o artigo [SAP HANA (instâncias de grande) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md). 
