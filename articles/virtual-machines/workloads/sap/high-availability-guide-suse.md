---
title: Azure máquinas virtuais elevada disponibilidade para SAP NetWeaver no SUSE Linux Enterprise Server para SAP aplicações | Microsoft Docs
description: Guia de elevada disponibilidade para SAP NetWeaver no SUSE Linux Enterprise Server para aplicações SAP
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: 12efeba68f30aa8723acc32449ae05ffac4c1ac4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658762"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>Elevada disponibilidade para SAP NetWeaver em VMs do Azure no SUSE Linux Enterprise Server para aplicações SAP

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
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

Este artigo descreve como implementar as máquinas virtuais, configurar as máquinas virtuais, instale a estrutura de cluster e instalar um sistema de SAP NetWeaver 7.50 altamente disponível.
Nas configurações de exemplo, comandos de instalação etc. Número de instância ASCS 00, ERS instância número 02 e SAP sistema ID NW1 é utilizado. Os nomes de recursos (por exemplo, as máquinas virtuais, redes virtuais) no exemplo partem do princípio de que utiliza o [convergido modelo] [ template-converged] com ID NW1 para criar os recursos do sistema de SAP.

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
* [SAP HANA SR desempenho otimizado do cenário][suse-hana-ha-guide]  
  O guia contém todas as informações necessárias para configurar a replicação do sistema do SAP HANA no local. Utilize este guia como uma linha de base.
* [Altamente NFS armazenamento disponível com DRBD e Pacemaker] [ suse-drbd-guide] o guia contém informações de todas as necessárias para configurar um servidor NFS elevada. Utilize este guia como uma linha de base.


## <a name="overview"></a>Descrição geral

Para alcançar a elevada disponibilidade, SAP NetWeaver requer um servidor NFS. O servidor NFS está configurado num cluster separado e pode ser utilizado por vários sistemas SAP.

![Descrição geral do SAP NetWeaver elevada disponibilidade](./media/high-availability-guide-suse/img_001.png)

O servidor NFS, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS e a base de dados SAP HANA utilizam o nome de anfitrião virtual e endereços IP virtuais. No Azure, é necessário um balanceador de carga a utilizar um endereço IP virtual. A lista seguinte mostra a configuração de (A) SCS e ERS o Balanceador de carga.

### <a name="ascs"></a>(A) SCS

* Configuração de front-end
  * Endereço IP 10.0.0.7
* Configuração de back-end
  * Ligado a interfaces de rede principal de todas as máquinas virtuais que deve fazer parte do (A) cluster SCS/ERS
* Porta da sonda
  * Porta 620**&lt;nr&gt;**
* Regras de carga
  * 32**&lt;nr&gt;** TCP
  * 36**&lt;nr&gt;** TCP
  * 39**&lt;nr&gt;** TCP
  * 81**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

### <a name="ers"></a>ERS

* Configuração de front-end
  * Endereço IP 10.0.0.8
* Configuração de back-end
  * Ligado a interfaces de rede principal de todas as máquinas virtuais que deve fazer parte do (A) cluster SCS/ERS
* Porta da sonda
  * Porta 621**&lt;nr&gt;**
* Regras de carga
  * 33**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Configurar um servidor NFS elevada

SAP NetWeaver requer um armazenamento partilhado para o diretório de transporte e perfil. Leitura [elevada disponibilidade para NFS em VMs do Azure no SUSE Linux Enterprise Server] [ nfs-ha] sobre como configurar um servidor NFS para SAP NetWeaver.

## <a name="setting-up-ascs"></a>Configurar a (A) SCS

Pode utilizar um modelo do Azure a partir do github para implementar todos os recursos do Azure necessários, incluindo as máquinas virtuais, conjunto de disponibilidade e o Balanceador de carga ou pode implementar os recursos manualmente.

### <a name="deploy-linux-via-azure-template"></a>Implementar o Linux através do modelo do Azure

O Azure Marketplace contém uma imagem para o SUSE Linux Enterprise Server para SAP aplicações 12, que pode utilizar para implementar novas máquinas virtuais. A imagem do marketplace contém o agente de recursos para SAP NetWeaver.

Pode utilizar um dos modelos de início rápido no github para implementar todos os recursos necessários. O modelo implementa as máquinas virtuais, o Balanceador de carga, etc de conjunto de disponibilidade. Siga estes passos para implementar o modelo:

1. Abra o [modelo ASCS/SCS várias SID] [ template-multisid-xscs] ou [convergido modelo] [ template-converged] do Azure portal ASCS/SCS o modelo apenas cria as regras de balanceamento de carga do SAP NetWeaver ASCS/SCS e instâncias ERS (apenas Linux), enquanto o modelo convergido também cria as regras de balanceamento de carga para uma base de dados (por exemplo o Microsoft SQL Server ou o SAP HANA). Se planear instalar um sistema NetWeaver SAP com base e também instalar a base de dados nas mesmas máquinas, utilize o [convergido modelo][template-converged].
1. Introduza os seguintes parâmetros
   1. Prefixo de recursos (apenas modelo ASCS/SCS várias SID)  
      Introduza o prefixo que pretende utilizar. O valor é utilizado como um prefixo para os recursos que são implementados.
   3. ID de sistema de SAP (apenas modelo convergido)  
      Introduza o ID de sistema SAP do sistema SAP que pretende instalar. O ID é utilizado como um prefixo para os recursos que são implementados.
   4. Tipo de pilha  
      Selecione o tipo de pilha do SAP NetWeaver
   5. Tipo de SO  
      Selecione um das distribuições de Linux. Para este exemplo, selecione o SLES 12 BYOS
   6. Tipo de base de dados  
      Selecione HANA
   7. Tamanho do sistema de SAP  
      A quantidade de SAPS fornece o novo sistema. Se tiver a não certeza SAPS quantos requer que o sistema, peça ao seu parceiro de tecnologia de SAP ou integrador de sistema
   8. Disponibilidade do sistema  
      Selecione HA
   9. Nome de utilizador de administrador e a palavra-passe de administrador  
      Um novo utilizador é criado que podem ser utilizadas para iniciar sessão na máquina.
   10. ID de sub-rede  
   O ID de sub-rede à qual as máquinas virtuais devem ser ligadas.  Deixe em branco, se pretender criar uma nova rede virtual ou selecione a mesma sub-rede que utilizada ou criada como parte da implementação de servidor NFS. O ID de aspeto normalmente /subscriptions/{targetsubscriptionid}/resourcegroups/{targetresourcegroupname}**&lt;ID de subscrição&gt;**/resourceGroups/**&lt;nome do grupo de recursos&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;nome da rede virtual&gt;**/subnets/**&lt;nome de sub-rede&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Implementar manualmente o Linux através do portal do Azure

Terá primeiro de criar as máquinas virtuais para este cluster NFS. Seguidamente, pode criar um balanceador de carga e utiliza máquinas virtuais nos agrupamentos de back-end.

1. Criar um Grupo de Recursos
1. Criar uma rede Virtual
1. Criar um conjunto de disponibilidade  
   Domínio de atualização máximo do conjunto
1. Criar Máquina Virtual 1   
   Utilizar, pelo menos, SLES4SAP 12 SP1 nesta imagem de exemplo SP1 de 12 SLES4SAP https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES para SAP aplicações 12 SP1 é utilizado  
   Selecione o conjunto de disponibilidade criado anteriormente  
1. Criar Máquina Virtual 2   
   Utilizar, pelo menos, SLES4SAP 12 SP1 nesta imagem de exemplo SP1 de 12 SLES4SAP https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES para SAP aplicações 12 SP1 é utilizado  
   Selecione o conjunto de disponibilidade criado anteriormente  
1. Adicione pelo menos um disco de dados para ambas as máquinas virtuais  
   Os discos de dados são utilizados para o /usrsap/`<SAPSID`> diretório
1. Criar um balanceador de carga (interno)  
   1. Criar os endereços IP de front-end
      1. Endereço IP 10.0.0.7 para o ASCS
         1. Abra o Balanceador de carga, selecione o conjunto IP de front-end e clique em Adicionar
         1. Introduza o nome do novo conjunto IP de front-end (por exemplo **nw1-ascs-front-end**)
         1. Como a atribuição estática e introduza o endereço IP (por exemplo **10.0.0.7**)
         1. Clique em OK
      1. Endereço IP 10.0.0.8 para o ERS ASCS
         * Repita os passos acima para criar um endereço IP para o ERS (por exemplo **10.0.0.8** e **nw1-aers-back-end**)
   1. Criar os conjuntos de back-end
      1. Criar um conjunto de back-end para o ASCS
         1. Abra o Balanceador de carga, selecione os conjuntos de back-end e clique em Adicionar
         1. Introduza o nome do novo conjunto de back-end (por exemplo **nw1-ascs-back-end**)
         1. Clique em Adicionar uma máquina virtual.
         1. Selecione o conjunto de disponibilidade que criou anteriormente
         1. Selecione as máquinas virtuais de (A) SCS cluster
         1. Clique em OK
      1. Criar um conjunto de back-end para o ERS ASCS
         * Repita os passos acima para criar um conjunto de back-end para o ERS (por exemplo **nw1-aers-back-end**)
   1. Criar as sondas de estado de funcionamento
      1. Porta 620**00** para ASCS
         1. Abra o Balanceador de carga, selecione sondas de estado de funcionamento e clique em Adicionar
         1. Introduza o nome da sonda de estado de funcionamento novo (por exemplo **nw1-ascs-hp**)
         1. Selecione TCP como protocolo, porta 620**00**, mantenha o intervalo de 5 e limiar de mau estado de funcionamento 2
         1. Clique em OK
      1. Porta 621**02** para ASCS ERS
         * Repita os passos acima para criar uma sonda do Estado de funcionamento para o ERS (por exemplo 621**02** e **nw1-aers-hp**)
   1. Regras de carga
      1. 32**00** TCP para ASCS
         1. Abra o Balanceador de carga, selecione as regras de balanceamento de carga e clique em Adicionar
         1. Introduza o nome da nova regra de Balanceador de carga (por exemplo **nw1-lb-3200**)
         1. Selecione o endereço IP de front-end, o conjunto back-end e a sonda de estado de funcionamento que criou anteriormente (por exemplo **nw1-ascs-front-end**)
         1. Manter protocolo **TCP**, introduza a porta **3200**
         1. Aumentar o tempo limite de inatividade e 30 minutos
         1. **Certifique-se de ativar o IP flutuante**
         1. Clique em OK    
      1. Portas adicionais para o ASCS
         * Repita os passos acima para portas 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 e TCP para o ASCS
      1. Portas adicionais para o ERS ASCS
         * Repita os passos acima para portas 33**02**, 5**02**13, 5**02**14, 5**02**16 e TCP para o ERS ASCS

### <a name="create-pacemaker-cluster"></a>Criar Pacemaker cluster

Siga os passos no [configurar Pacemaker no SUSE Linux Enterprise Server no Azure](high-availability-guide-suse-pacemaker.md) para criar um basic Pacemaker para este (A) SCS do servidor de clusters.

### <a name="installation"></a>Instalação

Os seguintes itens são o prefixo um **[A]** - aplicáveis a todos os nós, **[1]** - só é aplicável ao nó de 1 ou **[2]** - apenas aplicável a nó 2.

1. **[A]**  Instalar SUSE conector
   
   <pre><code>
   sudo zypper install sap_suse_cluster_connector
   </code></pre>

1. **[A]**  Agentes de recursos de SAP da atualização  
   
   É necessário uma correção para o pacote de recursos agentes para utilizar a nova configuração, o que é descrita neste artigo. Pode verificar, se o patch já estiver instalado com o seguinte comando

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   O resultado deve ser semelhante a

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Se o comando grep não localize o parâmetro IS_ERS, terá de instalar o patch listado no [o SUSE página de transferência](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Configurar a resolução de nome de anfitrião   

   Pode utilizar um servidor DNS ou modificar os /etc/hosts em todos os nós. Este exemplo mostra como utilizar o ficheiro /etc/hosts.
   Substituir o endereço IP e o nome de anfitrião nos seguintes comandos

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Insira as seguintes linhas ao /etc/hosts. Alterar o endereço IP e nome de anfitrião para corresponder ao seu ambiente   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>Preparar para a instalação do SAP NetWeaver

1. **[A]**  Criar os diretórios partilhados

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NW1</b>/SYS
   sudo mkdir -p /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo mkdir -p /usr/sap/<b>NW1</b>/ERS<b>02</b>

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NW1</b>/SYS
   sudo chattr +i /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chattr +i /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[A]**  Configurar autofs
 
   <pre><code>
   sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Criar um ficheiro com o

   <pre><code>
   sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   /usr/sap/<b>NW1</b>/ASCS<b>00</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCS
   /usr/sap/<b>NW1</b>/ERS<b>02</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS
   </code></pre>

   Reiniciar autofs montar novas partilhas

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[A]**  Configurar TROCAR ficheiros

   <pre><code>
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Reinicie o agente para ativar a alteração

   <pre><code>
   sudo service waagent restart
   </code></pre>


### <a name="installing-sap-netweaver-ascsers"></a>Instalar o SAP NetWeaver ASCS/ERS

1. **[1]**  Criar um recurso de IP virtual e a sonda de estado de funcionamento para a instância ASCS

   <pre><code>
   sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Certifique-se de que o estado de cluster está ok e de que todos os recursos são iniciados. Não é importante no nó que os recursos estão em execução.

   <pre><code>
   sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]**  Instalar SAP NetWeaver ASCS  

   Instalar o SAP NetWeaver ASCS como raiz no primeiro nó utilizando um nome de anfitrião virtual que está mapeado para o endereço IP da configuração de front-end de Balanceador de carga para ASCS, por exemplo <b>nw1 ascs</b>, <b>10.0.0.7</b> e o instância número que utilizou para a sonda do Balanceador de carga, por exemplo <b>00</b>.

   Pode utilizar o parâmetro de sapinst SAPINST_REMOTE_ACCESS_USER para permitir que um utilizador não raiz ao ligar ao sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Se a instalação falha ao criar uma subpasta na /usr/sap/**NW1**/ASCS**00**, tente definir o proprietário e o grupo do ASCS**00** pasta e tente novamente.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Criar um recurso de IP virtual e a sonda de estado de funcionamento para a instância ERS

   <pre><code>
   sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_NW1_ASCS,nc_NW1_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>
 
   Certifique-se de que o estado de cluster está ok e de que todos os recursos são iniciados. Não é importante no nó que os recursos estão em execução.

   <pre><code>
   sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]**  Instalar SAP NetWeaver ERS  

   Instalar o SAP NetWeaver ERS como raiz no nó segundo utilizando um nome de anfitrião virtual que está mapeado para o endereço IP da configuração de front-end de Balanceador de carga para ERS, por exemplo <b>nw1 aers</b>, <b>10.0.0.8</b> e o instância número que utilizou para a sonda do Balanceador de carga, por exemplo <b>02</b>.

   Pode utilizar o parâmetro de sapinst SAPINST_REMOTE_ACCESS_USER para permitir que um utilizador não raiz ao ligar ao sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > Utilize SWPM SP 20 LP 05 ou superior. Versões inferiores não corretamente as permissões e a instalação irá falhar.

   Se a instalação falha ao criar uma subpasta na /usr/sap/**NW1**/ERS**02**, tente definir o proprietário e o grupo do ERS**02** pasta e tente novamente.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>
   > 

1. **[1]**  Adapt ASCS/SCS e ERS instância perfis
 
   * Perfil ASCS/SCS

   <pre><code> 
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Perfil ERS

   <pre><code> 
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. **[A]**  Configurar Keep Alive

   A comunicação entre o servidor de aplicações do SAP NetWeaver e ASCS/SCS é encaminhada através de um balanceador de carga de software. O Balanceador de carga desliga ligações inativas após um limite de tempo configurável. Para evitar esta situação tem de definir um parâmetro no perfil do SAP NetWeaver ASCS/SCS e alterar as definições de sistema do Linux. Leitura [1410736 de nota SAP] [ 1410736] para obter mais informações.
   
   O ASCS/SCS perfil parâmetro colocar/encni/set_so_keepalive já foi adicionado no último passo.

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  Configurar os utilizadores SAP após a instalação
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nw1</b>adm   
   </code></pre>

1. **[1]**  Adicionar os serviços ASCS e ERS SAP para o ficheiro sapservice

   Adicione os ASCS service entrada para o segundo nó e copie a entrada de serviço ERS para o primeiro nó.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  Criar os recursos de cluster do SAP

   <pre><code>
   sudo crm configure property maintenance-mode="true"   
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure location loc_sap_<b>NW1</b>_failover_to_ers rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NW1</b> eq 1
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

   Certifique-se de que o estado de cluster está ok e de que todos os recursos são iniciados. Não é importante no nó que os recursos estão em execução.

   <pre><code>
   sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>Preparação de servidor do SAP NetWeaver aplicação

Algumas bases de dados requerem que a instalação de instância de base de dados é executada num servidor de aplicações. Prepare as máquinas de virtuais do servidor de aplicação para poder utilizá-las nestes casos.

O passos abaixo partem do princípio de que instala o servidor de aplicações num servidor diferente do que os servidores ASCS/SCS e HANA. Caso contrário, alguns dos passos abaixo (como configurar a resolução de nome de anfitrião) não são necessários.

1. Configurar a resolução de nome de anfitrião

   Pode utilizar um servidor DNS ou modificar os /etc/hosts em todos os nós. Este exemplo mostra como utilizar o ficheiro /etc/hosts.
   Substituir o endereço IP e o nome de anfitrião nos seguintes comandos
   ```bash
   sudo vi /etc/hosts
   ```
   Insira as seguintes linhas ao /etc/hosts. Alterar o endereço IP e nome de anfitrião para corresponder ao seu ambiente    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   # IP address of all application servers
   <b>10.0.0.20 nw1-di-0</b>
   <b>10.0.0.21 nw1-di-1</b>
   </code></pre>

1. Criar o diretório de sapmnt

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Configurar autofs
 
   <pre><code>
   sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Criar um novo ficheiro com o

   <pre><code>
   sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   </code></pre>

   Reiniciar autofs montar novas partilhas

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. Configurar o ficheiro de comutação
 
   <pre><code>
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Reinicie o agente para ativar a alteração

   <pre><code>
   sudo service waagent restart
   </code></pre>

## <a name="install-database"></a>Instalar a base de dados

Neste exemplo, SAP NetWeaver está instalado no SAP HANA. Pode utilizar cada base de dados suportado para esta instalação. Para obter mais informações sobre como instalar o SAP HANA no Azure, consulte [disponibilidade elevada do SAP HANA em Azure Virtual Machines (VMs)][sap-hana-ha]. Para obter uma lista de bases de dados suportadas, consulte [1928533 de nota SAP][1928533].

1. Executar a instalação de instância de base de dados SAP

   Instalar a instância de base de dados SAP NetWeaver como raiz com um nome de anfitrião virtual que mapeia para o endereço IP da configuração de front-end de Balanceador de carga para a base de dados, por exemplo <b>nw1-db</b> e <b>10.0.0.13</b>.

   Pode utilizar o parâmetro de sapinst SAPINST_REMOTE_ACCESS_USER para permitir que um utilizador não raiz ao ligar ao sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>Instalação do servidor de aplicação do SAP NetWeaver

Siga estes passos para instalar um servidor de aplicações SAP. 

1. Preparar o servidor de aplicações

Siga os passos de capítulo [preparação de servidor de aplicação do SAP NetWeaver](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) acima para preparar o servidor de aplicações.

1. Instalar o servidor de aplicações do SAP NetWeaver

   Instale um servidor de aplicações do SAP NetWeaver primário ou adicional.

   Pode utilizar o parâmetro de sapinst SAPINST_REMOTE_ACCESS_USER para permitir que um utilizador não raiz ao ligar ao sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. Atualizar o arquivo seguro de SAP HANA

   Atualize o arquivo seguro de SAP HANA para apontar para o nome virtual da configuração de replicação do sistema do SAP HANA.

   Execute o seguinte comando para listar as entradas
   <pre><code>
   hdbuserstore List
   </code></pre>

   Isto deve listar todas as entradas e deve ter um aspeto semelhante a
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: HN1
   </code></pre>

   O resultado mostra que o endereço IP da entrada de predefinição está a apontar para a máquina virtual e não para o endereço IP do Balanceador de carga. Esta entrada tem de ser alterado para apontar para o nome virtual do anfitrião do Balanceador de carga. Certifique-se de que utiliza a mesma porta (**30313** no resultado acima)!

   <pre><code>
   su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Passos Seguintes
* [Azure máquinas virtuais de planeamento e implementação de SAP][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP][dbms-guide]
* Para saber como estabelecer elevada disponibilidade e o plano de recuperação de desastres do SAP HANA no Azure (instâncias de grande), consulte o artigo [SAP HANA (instâncias de grande) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md).
* Para saber como estabelecer elevada disponibilidade e o plano de recuperação de desastres do SAP HANA em VMs do Azure, consulte o artigo [disponibilidade elevada do SAP HANA em Azure Virtual Machines (VMs)][sap-hana-ha]
