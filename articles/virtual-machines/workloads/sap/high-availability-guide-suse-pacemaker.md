---
title: Configurar Pacemaker no SUSE Linux Enterprise Server no Azure | Documentos da Microsoft
description: Como configurar Pacemaker no SUSE Linux Enterprise Server no Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: sedusch
ms.openlocfilehash: 1fa69cc09772b9f90e6de05820c823f0409d926e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070349"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>Como configurar Pacemaker no SUSE Linux Enterprise Server no Azure

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#memory-preserving-maintenance
[virtual-machines-windows-maintenance]:../../windows/maintenance-and-updates.md#memory-preserving-maintenance

Existem duas opções para configurar um cluster de Pacemaker no Azure. Pode utilizar um agente de delimitação por barreiras, que trata da reinicialização de um nó com falha através das APIs do Azure ou pode utilizar um dispositivo SBD.

O dispositivo SBD necessita de uma máquina virtual adicional que age como um servidor de destino iSCSI e fornece um dispositivo SBD. Este servidor de destino iSCSI no entanto pode ser partilhado com outros clusters Pacemaker. A vantagem de utilizar um dispositivo SBD é um menor tempo de ativação pós-falha e, se estiver a utilizar SBD dispositivos no local, não requer quaisquer alterações no modo de funcionamento do cluster pacemaker. A delimitação por barreiras SBD ainda pode utilizar o agente do Azure de cerca de cópia de segurança barreiras mecanismo, no caso do servidor de destino iSCSI não está disponível.

Se não pretender que a investir numa máquina virtual adicional, também pode utilizar o agente de cerca de Azure. A desvantagem é que uma ativação pós-falha pode demorar entre 10 a 15 minutos, se falha de parar um recurso ou os nós do cluster não é possível comunicar que uns aos outros mais.

![Pacemaker no Descrição geral do SLES](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

>[!IMPORTANT]
> Utilizar um dispositivo SBD para o seu cluster Pacemaker, é essencial para a confiabilidade global do cluster completo que o encaminhamento entre as VMs envolvidos e a VM (s) que aloja o SBD dispositivo (s) não está a passar através de quaisquer outros dispositivos, como [NVAs](https://azure.microsoft.com/solutions/network-appliances/). Caso contrário, os problemas com a NVA podem ter um impacto negativo sobre a estabilidade e a confiabilidade da configuração do cluster geral. Para evitar esses obstáculos, investigar as regras de encaminhamento de NVAs e [regras de encaminhamento definido pelo utilizador](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) ao planear e implementar dispositivos SBD.
>


## <a name="sbd-fencing"></a>A delimitação por barreiras SBD

Se pretender utilizar um dispositivo SBD para delimitação por barreiras, siga estes passos.

### <a name="set-up-an-iscsi-target-server"></a>Configurar um servidor de destino iSCSI

Tem primeiro de criar uma máquina de virtual de destino iSCSI, se ainda não tiver um. servidores de destino iSCSI podem ser compartilhados com vários clusters Pacemaker.

1. Implementar um novo SP1 de 12 do SLES ou superior numa máquina virtual e ligar à máquina através de ssh. A máquina não tem de ser grande. Um tamanho de máquina virtual como Standard_E2s_v3 ou Standard_D2s_v3 é suficiente.

1. Atualizar SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. Remover pacotes

   Para evitar um problema conhecido com targetcli e SLES 12 SP3, desinstale os seguintes pacotes. Pode ignorar erros sobre os pacotes que não podem ser encontradas
   
   <pre><code>
   sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>
   
1. Instalar pacotes de destino iSCSI

   <pre><code>
   sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. Ativar o serviço de destino iSCSI

   <pre><code>   
   sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>Criar dispositivo do iSCSI no servidor de destino iSCSI

Anexe um disco de dados novo à máquina de virtual de destino iSCSI que pode ser utilizada para este cluster. O disco de dados pode ser tão pequeno como 1 GB e tem de ser colocado numa conta de armazenamento Premium ou um disco gerido de Premium para beneficiar a [único SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines).

Execute o seguinte comando **VM de destino iSCSI** para criar um disco iSCSI para o novo cluster. No exemplo a seguir **cl1** é utilizado para identificar o novo cluster e **cl1-prod-0** e **cl1-prod-1** são os nomes de anfitrião de nós do cluster. Substituí-los com os nomes de anfitrião dos nós do cluster.

<pre><code>
# List all data disks with the following command
sudo ls -al /dev/disk/azure/scsi1/

# total 0
# drwxr-xr-x 2 root root  80 Mar 26 14:42 .
# drwxr-xr-x 3 root root 160 Mar 26 14:42 ..
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun0 -> ../../../<b>sdc</b>
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun1 -> ../../../sdd

# Then use the disk name to list the disk id
sudo ls -l /dev/disk/by-id/scsi-* | grep sdc

# lrwxrwxrwx 1 root root  9 Mar 26 14:42 /dev/disk/by-id/scsi-14d53465420202020a50923c92babda40974bef49ae8828f0 -> ../../sdc
# lrwxrwxrwx 1 root root  9 Mar 26 14:42 <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0 -> ../../sdc</b>

# Use the data disk that you attached for this cluster to create a new backstore
sudo targetcli backstores/block create <b>cl1</b> <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0</b>

sudo targetcli iscsi/ create iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/luns/ create /backstores/block/<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-0.local:prod-cl1-0</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-1.local:prod-cl1-1</b>

# save the targetcli changes
sudo targetcli saveconfig
</code></pre>

### <a name="set-up-sbd-device"></a>Configurar o dispositivo SBD

Ligar ao dispositivo iSCSI que foi criado no último passo do cluster.
Execute os seguintes comandos em nós do novo cluster que pretende criar.
Os seguintes itens são prefixados com ambos **[A]** - aplicáveis a todos os nós, **[1]** – apenas aplicável no nó 1 ou **[2]** – apenas aplicável a nó 2.

1. **[A]**  Ligar para os dispositivos de iSCSI

   Em primeiro lugar, ative os serviços SBD e iSCSI.

   <pre><code>
   sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]**  Alterar o nome de iniciador no primeiro nó

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Alterar o conteúdo do ficheiro de acordo com as ACLs que utilizou quando criou o dispositivo de iSCSI no servidor de destino iSCSI

   <pre><code>   
   InitiatorName=<b>iqn.2006-04.prod-cl1-0.local:prod-cl1-0</b>
   </code></pre>

1. **[2]**  Alterar o nome de iniciador para o segundo nó

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Alterar o conteúdo do ficheiro de acordo com as ACLs que utilizou quando criou o dispositivo de iSCSI no servidor de destino iSCSI

   <pre><code>
   InitiatorName=<b>iqn.2006-04.prod-cl1-1.local:prod-cl1-1</b>
   </code></pre>

1. **[A]**  Reiniciar o serviço iSCSI

   Agora, reinicie o serviço iSCSI para aplicar a alteração
   
   <pre><code>
   sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   Ligue os dispositivos de iSCSI. No exemplo abaixo, 10.0.0.17 é o endereço IP do servidor de destino iSCSI e 3260 é a porta predefinida. <b>iqn.2006 04.cl1.local:cl1</b> é o nome de destino que está listado ao executar o primeiro comando.

   <pre><code>
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>
   
   sudo iscsiadm -m node -T <b>iqn.2006-04.cl1.local:cl1</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   Certifique-se de que o dispositivo de iSCSI está disponível bem como observar feito o nome do dispositivo (no exemplo seguinte/desenvolvimento/sde)

   <pre><code>
   lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  cl1              4.0   /dev/sde</b>
   </code></pre>

   Agora, obter o ID do dispositivo iSCSI.

   <pre><code>
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   </code></pre>

   O comando lista as três identificações de dispositivo. Recomendamos que está a utilizar o ID que começa com scsi-3, no exemplo acima isso
   
   **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**

1. **[1]**  Criar o dispositivo SBD

   Utilize o ID de dispositivo do dispositivo iSCSI para criar um novo dispositivo SBD no primeiro nó de cluster.

   <pre><code>
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 10 -4 20 create
   </code></pre>

1. **[A]**  Adaptar-se a configuração SBD

   Abra o ficheiro de configuração SBD

   <pre><code>
   sudo vi /etc/sysconfig/sbd
   </code></pre>

   Alterar a propriedade do dispositivo SBD, permitir a integração de pacemaker e alterar o modo de início de SBD.

   <pre><code>
   [...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   </code></pre>

   Criar o ficheiro de configuração softdog

   <pre><code>
   echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Agora, carregar o módulo

   <pre><code>
   sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Instalação de cluster

Os seguintes itens são prefixados com ambos **[A]** - aplicáveis a todos os nós, **[1]** – apenas aplicável no nó 1 ou **[2]** – apenas aplicável a nó 2.

1. **[A]**  Atualizar SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ativar o ssh acesso

   <pre><code>
   sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

2. **[2]**  Ativar o ssh acesso

   <pre><code>
   sudo ssh-keygen
   
   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]**  Ativar o ssh acesso

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Agentes de cerca de instalar
   
   <pre><code>
   sudo zypper install fence-agents
   </code></pre>

1. **[A]**  Configurar a resolução de nomes de anfitrião   

   Pode utilizar um servidor DNS ou modificar os /etc/hosts em todos os nós. Este exemplo mostra como utilizar o ficheiro /etc/hosts.
   Substitua o endereço IP e o nome de anfitrião nos seguintes comandos. A vantagem de utilizar /etc/hosts é que o seu cluster se torna independentes do DNS que também poderia ser um ponto único de falhas.

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Insira as seguintes linhas ao /etc/hosts. Alterar o endereço IP e o nome de anfitrião para corresponder ao seu ambiente   
   
   <pre><code>
   # IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]**  Instalar Cluster
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> Press ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> Press ENTER
   # Multicast port [5405] -> Press ENTER
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Adicionar nó ao cluster
   
   <pre><code> 
   sudo ha-cluster-join
   
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.14
   # /root/.ssh/id_rsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Alterar hacluster palavra-passe para a mesma palavra-passe

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Configurar corosync para utilizar outro transporte e adicionar nodelist. Cluster caso contrário, não funciona.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Adicione o seguinte conteúdo de negrito para o ficheiro se os valores não forem existe ou diferente. Certifique-se alterar o token para 30000 para permitir que a memória preservação da manutenção. Ver [este artigo para Linux] [ virtual-machines-linux-maintenance] ou [Windows] [ virtual-machines-windows-maintenance] para obter mais detalhes.
   
   <pre><code> 
   [...]
     <b>token:          30000
     token_retransmits_before_loss_const: 10
     join:           60
     consensus:      6000
     max_messages:   20</b>
     
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-cl1-0</b>
      ring0_addr:10.0.0.6
     }
     node {
      # IP address of <b>prod-cl1-1</b>
      ring0_addr:10.0.0.7
     } 
   }</b>
   logging {
     [...]
   }
   quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        <b>expected_votes: 2</b>
        <b>two_node: 1</b>
   }
   </code></pre>

   Em seguida, reinicie o serviço de corosync

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[1]**  Alterar as predefinições de pacemaker

   <pre><code>
   sudo crm configure rsc_defaults resource-stickiness="1"   
   </code></pre>

## <a name="create-stonith-device"></a>Criar dispositivo STONITH

O dispositivo STONITH utiliza um Principal de serviço para autorizar com o Microsoft Azure. Siga estes passos para criar um Principal de serviço.

1. Ir para <https://portal.azure.com>
1. Abra o painel Azure Active Directory  
   Vá para propriedades e anote o ID de diretório. Este é o **ID de inquilino**.
1. Clique em registos de aplicações
1. Clique em Adicionar
1. Introduza um nome, selecione o tipo de aplicação "Aplicação/API da Web", introduza um URL de início de sessão (por exemplo http://localhost) e clique em criar
1. O URL de início de sessão não é utilizado e pode ser qualquer URL válido
1. Selecione a nova aplicação e clique em chaves no separador Definições
1. Introduza uma descrição para uma nova chave, selecione "Nunca expira" e clique em Guardar
1. Anote o valor. Ele é usado como o **palavra-passe** para o Principal de serviço
1. Anote o ID da aplicação. Ele é usado como o nome de utilizador (**ID de início de sessão** nos passos abaixo) de Principal de serviço

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Criar uma função personalizada para o agente de cerca

O Principal de serviço não tem permissões para aceder aos seus recursos do Azure por predefinição. Tem de conceder as permissões do Principal de serviço para iniciar e parar (desaloque) todas as máquinas virtuais do cluster. Se já não tiver criado a função personalizada, pode criá-la utilizando [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell#create-a-custom-role) ou [da CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli#create-a-custom-role)

Utilize o seguinte conteúdo para o ficheiro de entrada. Precisa adaptar o conteúdo para as suas subscrições, substitua c276fc76-9cd4-44c9-99a7-4fd71546436e e e91d47c4-76f3-4271-a796-21b4ecfe3624 com os Ids da sua subscrição. Se tiver apenas uma subscrição, remova a segunda entrada assignablescopes.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="1-assign-the-custom-role-to-the-service-principal"></a>**[1]**  Atribuir a função personalizada para o Principal de serviço

Atribua a função personalizada "Linux cerca agente de função" que foi criado no último capítulo para o Principal de serviço. Não utilize a função de proprietário mais!

1. Ir para https://portal.azure.com
1. Abra o painel de todos os recursos
1. Selecione a máquina virtual do primeiro nó de cluster
1. Clique em controle de acesso (IAM)
1. Clique em Adicionar
1. Selecione a função de "Função de agente de cerca de Linux"
1. Introduza o nome da aplicação que criou acima
1. Clique em OK

Repita os passos acima para o segundo nó de cluster.

### <a name="1-create-the-stonith-devices"></a>**[1]**  Criar os dispositivos STONITH

Depois de editar as permissões para as máquinas virtuais, pode configurar os dispositivos STONITH no cluster.

<pre><code>
# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure property stonith-timeout=900

sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

</code></pre>

### <a name="1-create-fence-topology-for-sbd-fencing"></a>**[1]**  Criar cerca de topologia para a delimitação por barreiras SBD

Se pretender utilizar um dispositivo SBD, ainda Recomendamos que utilize um agente de Azure cerca de cópia de segurança no caso do servidor de destino iSCSI não está disponível.

<pre><code>
sudo crm configure fencing_topology \
  stonith-sbd rsc_st_azure

</code></pre>
### **[1] ** Permitem a utilização de um dispositivo STONITH

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>


## <a name="next-steps"></a>Passos Seguintes
* [Máquinas de virtuais de planeamento e implementação de SAP do Azure][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP][dbms-guide]
* Para saber como estabelecer a elevada disponibilidade e o plano de recuperação após desastre do SAP HANA em VMs do Azure, veja [disponibilidade elevada do SAP HANA em máquinas virtuais do Azure (VMs)][sap-hana-ha]
