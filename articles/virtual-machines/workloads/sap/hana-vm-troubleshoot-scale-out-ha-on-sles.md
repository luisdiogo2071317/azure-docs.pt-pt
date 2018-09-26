---
title: Resolver problemas de configuração de HSR Pacemaker de escalamento horizontal do SAP HANA 2.0 com SLES 12 SP3 em máquinas virtuais do Azure | Documentos da Microsoft
description: Guia para verificar e resolver problemas de uma configuração de escalamento horizontal de elevada disponibilidade de SAP HANA complexa com base no SAP HANA System Replication (HSR) e Pacemaker no SLES 12 SP3 em execução em máquinas virtuais do Azure
services: virtual-machines-linux
documentationcenter: ''
author: hermannd
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/24/2018
ms.author: hermannd
ms.openlocfilehash: 6c0d6397246e8b8db1d59c26229e37a722d49f48
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184980"
---
# <a name="verify-and-troubleshoot-sap-hana-scale-out-high-availability-setup-on-sles-12-sp3"></a>Certifique-se e resolver problemas de configuração de elevada disponibilidade de escalamento horizontal de SAP HANA no SLES 12 SP3 

[sles-pacemaker-ha-guide]:high-availability-guide-suse-pacemaker.md
[sles-hana-scale-out-ha-paper]:https://www.suse.com/documentation/suse-best-practices/singlehtml/SLES4SAP-hana-scaleOut-PerfOpt-12/SLES4SAP-hana-scaleOut-PerfOpt-12.html
[sap-hana-iaas-list]:https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html
[suse-pacemaker-support-log-files]:https://www.suse.com/support/kb/doc/?id=7022702
[azure-linux-multiple-nics]:https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics
[suse-cloud-netconfig]:https://www.suse.com/c/multi-nic-cloud-netconfig-ec2-azure/
[sap-list-port-numbers]:https://help.sap.com/viewer/ports
[sles-12-ha-paper]:https://www.suse.com/documentation/sle-ha-12/pdfdoc/book_sleha/book_sleha.pdf
[sles-zero-downtime-paper]:https://www.suse.com/media/presentation/TUT90846_towards_zero_downtime%20_how_to_maintain_sap_hana_system_replication_clusters.pdf
[sap-nw-ha-guide-sles]:high-availability-guide-suse.md
[sles-12-for-sap]:https://www.suse.com/media/white-paper/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf


Este artigo foi escrito para ajudar a verificar a configuração de cluster Pacemaker para SAP HANA em execução em máquinas virtuais do Azure de escalamento. A configuração de cluster foi realizada em combinação com o SAP HANA System Replication (HSR) e o RPM de SUSE empacotar SAPHanaSR-aumento horizontal. Todos os testes foram realizados em SUSE SLES 12 SP3 apenas. Existem várias seções que abordam áreas diferentes e incluem comandos de exemplo e trechos de ficheiros de configuração. Estes exemplos são recomendados como um método para verificar e verifique a configuração de cluster inteiro.



## <a name="important-notes"></a>Notas importantes

Todos os testes para Escalamento SAP HANA, em combinação com o SAP HANA System Replication e Pacemaker foi feito com o SAP HANA 2.0 apenas. A versão do sistema operativo foi SUSE Linux Enterprise Server 12 SP3 para aplicações SAP. Além disso, o pacote RPM SAPHanaSR ScaleOut mais recente do SUSE foi utilizado para configurar o cluster pacemaker.
SUSE publicado uma descrição detalhada desta configuração de desempenho otimizado, o que pode ser encontrada [aqui][sles-hana-scale-out-ha-paper]

Para tipos de máquina virtual, que são suportados para verificação de escalamento horizontal do SAP HANA a [diretório de IaaS com certificação SAP HANA][sap-hana-iaas-list]

Ocorreu um problema técnico com o SAP HANA aumentar horizontalmente em combinação com várias sub-redes e vNICs e a configuração HSR. É obrigatório para utilizar os patches mais recentes do SAP HANA 2.0 em que este problema foi corrigido. São suportadas as seguintes versões do SAP HANA: 

**REV2.00.024.04 ou superior & rev2.00.032 ou superior.**

No caso de deve haver uma situação, o que necessita de suporte do SUSE siga isso [guia][suse-pacemaker-support-log-files]. Recolha todas as informações sobre o cluster SAP HANA HA, conforme descrito no artigo. Suporte SUSE precisa estas informações para análise adicional.

Durante o teste interno, aconteceu que a configuração do cluster de obteve confuso com um encerramento correto de VM normal através do portal do Azure. Portanto, é recomendado para testar uma ativação pós-falha de cluster por outros métodos. Utilizar métodos como forçar um entre em pânico do kernel ou desligar as redes ou migrar o **msl** recursos (ver detalhes nas secções abaixo). A pressuposição é de que um encerramento padrão acontece com a intenção. O melhor exemplo para um encerramento intencional é manutenção (ver detalhes na seção sobre manutenção planeada).

Durante o teste interno aconteceu que a configuração de cluster ficavam confusos após uma obtenção de controlo manual do SAP HANA, enquanto o cluster estava em modo de manutenção. Recomenda-se de alterá-la novamente manualmente novamente, antes de terminar o modo de manutenção do cluster. Outra opção consiste em acionar uma ativação pós-falha antes de colocar o cluster no modo de manutenção (consulte a secção sobre manutenção planeada para obter mais detalhes). A documentação do SUSE descreve como pode repor o cluster nesse sentido com o comando de crm. Mas a abordagem mencionada antes de que parecia ser robusto durante o teste interno e nunca mostrou quaisquer efeitos colaterais inesperados.

Se utilizar o crm migração comando não perca a limpar a configuração do cluster. Ele adiciona as restrições de localização, que podem não estar atento. Essas restrições tem um impacto sobre o comportamento de cluster (ver mais detalhes na seção sobre manutenção planeada).



## <a name="test-system-description"></a>Descrição do sistema de teste

Para verificação de HA de escalamento horizontal do SAP HANA e certificação de que um programa de configuração foi utilizado, consiste em dois sistemas com três nós de SAP HANA cada - um mestre e dois operadores. Eis a lista de nomes de VMS e endereços IP internos. Todos os exemplos de verificação ainda mais para baixo tivesse terminados nestas VMS. Endereços nos exemplos de comando com estes nomes de VM e o IP devem ajudar a compreender melhor os comandos e suas saídas.


| Tipo de nó | o nome da VM | Endereço IP |
| --- | --- | --- |
| Nó principal no site 1 | hso-hana-vm-s1-0 | 10.0.0.30 |
| Nó de trabalho 1 no site 1 | hso-hana-vm-s1-1 | 10.0.0.31 |
| Nó de trabalho 2 no site 1 | hso-hana-vm-s1-2 | 10.0.0.32 |
| | | |
| Nó principal no site 2 | hso-hana-vm-s2-0 | 10.0.0.40 |
| Nó de trabalho 1 no site 2 | hso-hana-vm-s2-1 | 10.0.0.41 |
| Nó de trabalho 2 no site 2 | hso-hana-vm-s2-2  | 10.0.0.42 |
| | | |
| Nós de criador de principal | hso-hana-dm | 10.0.0.13 |
| Servidor de SBD de dispositivos | hso-hana-sbd | 10.0.0.19 |
| | | |
| Servidor NFS 1 | hso-nfs-vm-0 | 10.0.0.15 |
| Servidor NFS 2 | hso-nfs-vm-1 | 10.0.0.14 |



## <a name="multiple-subnets-and-vnics"></a>Várias sub-redes e vNICs

Ao seguir as recomendações de rede do SAP HANA, três sub-redes foram criadas dentro de uma rede virtual do Azure. Escalamento do SAP HANA no Azure tem de ser instalado no modo não partilhado, o que significa que cada nó usa os volumes de disco local para **/hana/data** e **/hana/do registo**. Devido ao utilizar volumes de disco apenas local não é necessário definir uma sub-rede separada para o armazenamento:

- 10.0.2.0/24 para comunicação entre nós de SAP HANA
- 10.0.1.0/24 para SAP HANA System Replication HSR
- 10.0.0.0/24 para todo o resto

Para obter informações sobre a configuração do SAP HANA relacionada ao uso várias redes Consulte a secção **global.ini** mais abaixo.

Correspondente ao número de sub-redes todas as VMS do cluster tem três vNICs. [Isso] [ azure-linux-multiple-nics] artigo descreve um potencial problema de encaminhamento no Azure ao implementar uma VM do Linux. Este tópico de encaminhamento específico só se aplica a utilização de múltiplos vNICs. O problema foi resolvido pela SUSE por predefinição no SLES 12 SP3. O artigo do SUSE sobre este tópico pode ser encontrado [aqui][suse-cloud-netconfig].


Como uma verificação básica para verificar se o SAP HANA está corretamente configurada para utilização de várias redes, basta execute os comandos abaixo. Primeiro passo é simplesmente verificar novamente no nível do SO que todos os três endereços IP internos para todos os três sub-redes estão ativos. No caso de que definiu as sub-redes com diferentes intervalos de endereços IP tem de se adaptar os comandos:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

Aqui está uma saída de exemplo do segundo nó de trabalho no site 2. Pode ver três diferentes endereços IP internos do eth0, eth1 e eth2:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


Segundo passo é a verificação de portas de SAP HANA para nameserver e HSR. SAP HANA deve escutar as sub-redes correspondentes. Dependendo do número de instância do SAP HANA, terá de se adaptar os comandos. Para o sistema de teste, o número de instância foi **00**. Existem diferentes formas de descobrir que portas são utilizadas. 

Abaixo, ver uma instrução de SQL, que retorna a ID de instância e o número de instância, entre outras informações:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Para localizar os números de porta correto, pode observar, por exemplo, HANA Studio sob "**configuração**" ou através de uma instrução SQL:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

Para localizar cada porta, o que é utilizada na pilha de software do SAP, incluindo SAP HANA, pesquisar [aqui][sap-list-port-numbers].

Dado o número de instância **00** no sistema de teste de SAP HANA 2.0, é o número de porta para o nameserver **30001**. É o número de porta para comunicação de dados de metadados HSR **40002**. É uma opção iniciar sessão para um nó de trabalho e, em seguida, verifique os serviços de nó principal. Aqui, a verificação foi feita no nó de trabalho 2 no site está a tentar ligar ao nó principal no site 2 de 2.

Verifique a porta de nameserver:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

O resultado deve ser semelhante do exemplo de saída abaixo para provar que a comunicação entre nós é utilizar a sub-rede **10.0.2.0/24**.
Apenas a ligar através do sub-rede **10.0.2.0/24** deve ter êxito:

<pre><code>
nc: connect to 10.0.0.40 port 30001 (tcp) failed: Connection refused
nc: connect to 10.0.1.40 port 30001 (tcp) failed: Connection refused
Connection to 10.0.2.40 30001 port [tcp/pago-services1] succeeded!
</code></pre>

Verificar agora para a porta HSR **40002**:

<pre><code>
nc -vz 10.0.0.40 40002
nc -vz 10.0.1.40 40002
nc -vz 10.0.2.40 40002
</code></pre>

O resultado deve ser semelhante do exemplo de saída abaixo para provar que a comunicação de HSR está usando a sub-rede **10.0.1.0/24**.
Apenas a ligar através do sub-rede **10.0.1.0/24** deve ter êxito:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


O ficheiro de configuração corosync tem que estar correto em cada nó do cluster, incluindo o nó de criador da maioria. No caso de associação de um nó de cluster não funcionou como esperado, criar e/ou copie **/etc/corosync/corosync.conf** manualmente/para todos os nós e reinicie o serviço.

Aqui está o conteúdo do **corosync.conf** do sistema de teste como um exemplo.

É a primeira seção **totem** conforme descrito neste [documentação] [ sles-pacemaker-ha-guide] (etapa de instalação de cluster de secção 11). Pode ignorar o valor para **mcastaddr**. Apenas manter a entrada existente. As entradas **token** e **consenso** tem de ser definida de acordo com a documentação do SAP HANA no Microsoft Azure, que pode ser encontrado [aqui][sles-pacemaker-ha-guide]

<pre><code>
totem {
    version:    2
    secauth:    on
    crypto_hash:    sha1
    crypto_cipher:  aes256
    cluster_name:   hacluster
    clear_node_high_bit: yes

    token:      30000
    token_retransmits_before_loss_const: 10
    join:       60
    consensus:  36000
    max_messages:   20

    interface {
        ringnumber:     0
        bindnetaddr: 10.0.0.0
        mcastaddr:  239.170.19.232
        mcastport:  5405

        ttl:        1
    }
    transport:      udpu

}
</code></pre>

A segunda seção **registo** não foi alterado das predefinições de determinado:

<pre><code>
logging {
    fileline:   off
    to_stderr:  no
    to_logfile:     no
    logfile:    /var/log/cluster/corosync.log
    to_syslog:  yes
    debug:      off
    timestamp:  on
    logger_subsys {
        subsys:     QUORUM
        debug:  off
    }
}
</code></pre>

A terceira mostra de secção a **nodelist**. Todos os nós do cluster tem de aparecer com o id do nó:

<pre><code>
nodelist {
  node {
   ring0_addr:hso-hana-vm-s1-0
   nodeid: 1
   }
  node {
   ring0_addr:hso-hana-vm-s1-1
   nodeid: 2
   }
  node {
   ring0_addr:hso-hana-vm-s1-2
   nodeid: 3
   }
  node {
   ring0_addr:hso-hana-vm-s2-0
   nodeid: 4
   }
  node {
   ring0_addr:hso-hana-vm-s2-1
   nodeid: 5
   }
  node {
   ring0_addr:hso-hana-vm-s2-2
   nodeid: 6
   }
  node {
   ring0_addr:hso-hana-dm
   nodeid: 7
   }
}
</code></pre>

Na última seção **quórum**, é importante definir o valor **expected_votes** corretamente. Tem de ser o número de nós, incluindo o nó de criador da maioria. E o valor de **two_node** tem de ser **0**. Não remova a entrada completamente. Basta definir o valor como **0**.

<pre><code>
quorum {
    # Enable and configure quorum subsystem (default: off)
    # see also corosync.conf.5 and votequorum.5
    provider: corosync_votequorum
    expected_votes: 7
    two_node: 0
}
</code></pre>


Reinicie o serviço via **systemctl**:

<pre><code>
systemctl restart corosync
</code></pre>




## <a name="sbd-device"></a>Dispositivo SBD

A documentação sobre como configurar um dispositivo SBD numa VM do Azure é descrita [aqui] [ sles-pacemaker-ha-guide] (secção barreiras sbd).

Primeira coisa a verificar é procurar no servidor SBD VM, se existirem entradas ACL para cada nó no cluster. Execute o seguinte comando no servidor SBD VM:


<pre><code>
targetcli ls
</code></pre>


No sistema de teste, a saída do comando era a aparência do exemplo abaixo. A ACL de nomes, como **iqn.2006-04.hso-db-0.local:hso-db-0** têm de ser introduzidos como o nome de iniciador correspondente nas VMs. Cada VM tem um diferente.

<pre><code>
 | | o- sbddbhso ................................................................... [/sbd/sbddbhso (50.0MiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2006-04.dbhso.local:dbhso ..................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 7]
  |     | o- iqn.2006-04.hso-db-0.local:hso-db-0 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-1.local:hso-db-1 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-2.local:hso-db-2 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-3.local:hso-db-3 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-4.local:hso-db-4 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-5.local:hso-db-5 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-6.local:hso-db-6 .................................................................. [Mapped LUNs: 1]
</code></pre>

Em seguida verifique que os nomes de iniciador em todas as VMs são diferentes e correspondem às entradas mostradas acima. Eis um exemplo do nó de trabalho 1 no site 1:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

A saída a aparência do exemplo abaixo:

<pre><code>
##
## /etc/iscsi/iscsi.initiatorname
##
## Default iSCSI Initiatorname.
##
## DO NOT EDIT OR REMOVE THIS FILE!
## If you remove this file, the iSCSI daemon will not start.
## If you change the InitiatorName, existing access control lists
## may reject this initiator.  The InitiatorName must be unique
## for each iSCSI initiator.  Do NOT duplicate iSCSI InitiatorNames.
InitiatorName=iqn.2006-04.hso-db-1.local:hso-db-1
</code></pre>

Em seguida, certifique-se se o **deteção** funciona corretamente e execute o seguinte comando em cada nó de cluster utilizando o endereço IP do servidor SBD VM:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

O resultado deverá ser semelhante o exemplo abaixo:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Próximo ponto de prova é Certifique-se de que o nó, verá o dispositivo SDB. Dê uma em cada nó, incluindo o nó do criador de maioria:

<pre><code>
lsscsi | grep dbhso
</code></pre>

O resultado deverá ser semelhante o exemplo abaixo. Tenha em atenção que os nomes podem ser diferentes (nome do dispositivo também poderão sofrer alterações depois de VM é reiniciada):

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Consoante o estado do sistema, às vezes, poderá ser útil para reiniciar os serviços de iscsi para resolver problemas. Em seguida, execute os seguintes comandos:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Qualquer nó, pode verificar se todos os nós são **limpar**. Apenas fique atento utilizar o nome de dispositivo correto num nó específico:

<pre><code>
sbd -d /dev/sdm list
</code></pre>

A saída deve mostrar **limpar** para cada nó do cluster:

<pre><code>
0       hso-hana-vm-s1-0        clear
1       hso-hana-vm-s2-2        clear
2       hso-hana-vm-s2-1        clear
3       hso-hana-dm     clear
4       hso-hana-vm-s1-1        clear
5       hso-hana-vm-s2-0        clear
6       hso-hana-vm-s1-2        clear
</code></pre>


Outro SBD verificar é a **despejo** opção do comando sbd. Eis um comando de exemplo e a saída a partir do nó de criador de maioria em que o nome do dispositivo não era **sdm** mas **sdd**:

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

A saída (exceto o nome do dispositivo) deve ser o mesmo em todos os nós:

<pre><code>
==Dumping header on disk /dev/sdd
Header version     : 2.1
UUID               : 9fa6cc49-c294-4f1e-9527-c973f4d5a5b0
Number of slots    : 255
Sector size        : 512
Timeout (watchdog) : 60
Timeout (allocate) : 2
Timeout (loop)     : 1
Timeout (msgwait)  : 120
==Header on disk /dev/sdd is dumped
</code></pre>

Mais uma verificação para SBD é a possibilidade de enviar uma mensagem para outro nó. Execute o seguinte comando no nó de trabalho 1 no site 2 para enviar uma mensagem para o nó de trabalho 2 no site 2:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

No destino do lado do VM - que era **hso-hana-vm-s2-2** neste exemplo - pode encontrar a seguinte entrada **/var/log/messages.**:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

Verifique se as entradas na **/etc/sysconfig/sbd** correspondem à descrição na nossa [documentação] [ sles-pacemaker-ha-guide] (secção barreiras sbd). Certifique-se de que esta definição de arranque **/etc/iscsi/iscsid.conf** é definido como automático.

Entradas importantes **/etc/sysconfig/sbd** (adaptar o valor de id, se necessário):

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


Outro item para verificar é a definição de arranque na **/etc/iscsi/iscsid.conf**. A definição necessária deve ter sucedido pela **iscsiadm** comando mostrado abaixo, que é descrito na documentação. Faz sentido para verificar e talvez adaptá-lo manualmente com **vi** caso seja diferente.

Comando para definir o comportamento de arranque:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

Entrada na **/etc/iscsi/iscsid.conf**:

<pre><code>
node.startup = automatic
</code></pre>

Durante o teste e verificação ocorrências aconteceram onde após o reinício de uma VM dispositivo SBD não era visível mais. Ocorreu uma discrepância entre a configuração de inicialização e quais yast2 mostrou. Para verificar as definições, execute os passos abaixo:

1. Iniciar yast2
2. Selecione **serviços de rede** no lado esquerdo
3. Desloque para baixo no lado direito para **iniciador iSCSI** e selecioná-lo
4. No ecrã seguinte sob o **serviço** separador, deverá ver o nome de iniciador exclusivo para o nó
5. Acima do nome de iniciador, certifique-se de que o **arranque do serviço** valor está definido como **quando inicialização**
6. Se não for o caso, em seguida, defina-o como **quando inicialização** em vez de **manualmente**
7. Em seguida, mude do separador superior para **destinos ligado**
8. No ecrã de destinos ligado deverá ver uma entrada para o dispositivo SBD como este exemplo: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**
9. Verifique se o valor de inicialização está definido "**onboot**"
10. Se não, escolha **editar** e alterá-lo
11. Guardar as alterações e sair yast2



## <a name="pacemaker"></a>Pacemaker

Uma vez que tudo está configurado corretamente, pode executar o seguinte comando em cada nó para verificar o estado do serviço pacemaker:

<pre><code>
systemctl status pacemaker
</code></pre>

Parte superior da saída deverá ser semelhante o exemplo abaixo. É importante que o estado após **Active Directory** é mostrado como **carregado** e **Active Directory (em execução)**. O estado após "Carregar" tem de ser mostrado como **ativada**.

<pre><code>
  pacemaker.service - Pacemaker High Availability Cluster Manager
   Loaded: loaded (/usr/lib/systemd/system/pacemaker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-09-07 05:56:27 UTC; 4 days ago
     Docs: man:pacemakerd
           http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Pacemaker_Explained/index.html
 Main PID: 4496 (pacemakerd)
    Tasks: 7 (limit: 4915)
   CGroup: /system.slice/pacemaker.service
           ├─4496 /usr/sbin/pacemakerd -f
           ├─4499 /usr/lib/pacemaker/cib
           ├─4500 /usr/lib/pacemaker/stonithd
           ├─4501 /usr/lib/pacemaker/lrmd
           ├─4502 /usr/lib/pacemaker/attrd
           ├─4503 /usr/lib/pacemaker/pengine
           └─4504 /usr/lib/pacemaker/crmd
</code></pre>

No caso da definição ainda na **desativada**, execute o seguinte comando:

<pre><code>
systemctl enable pacemaker
</code></pre>

Para ver todos os recursos configurados no pacemaker, execute o seguinte comando:

<pre><code>
crm status
</code></pre>

O resultado deverá ser semelhante o exemplo abaixo. Há problema-se de que os recursos de cln e msl são apresentados como parou no Criador de maioria VM (**hso-hana-dm**). Não existe nenhuma instalação de SAP HANA no nó de criador de maioria. Portanto, o **cln** e **msl** recursos são mostrados como parado. É importante que mostra o número correto de total de VMs (**7**). Todas as VMs que fazem parte do cluster tem de estar listadas com estado **Online**. O nó principal primário atual deve ser reconhecido corretamente (neste exemplo é **hso-hana-vm-s1-0**).

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Tue Sep 11 15:56:40 2018
Last change: Tue Sep 11 15:56:23 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0
</code></pre>

Um recurso importante do pacemaker é colocá-lo no modo de manutenção. Este modo permite fazer modificações (por exemplo um reinício VM) sem fazer uma ação imediata de cluster. Um caso de utilização típica seria a manutenção planeada de SO ou do Azure da infraestrutura (Consulte também a seção à parte sobre manutenção planeada). Utilize o seguinte comando para colocar pacemaker no modo de manutenção:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

Quando a verificação com **status de crm**, repare na saída que todos os recursos são marcados como **não gerenciado**. Neste estado, o cluster não reagir em todas as alterações, como iniciar/parar SAP HANA.
Aqui está uma saída de exemplo do **status de crm** enquanto o cluster está em modo de manutenção de comando:

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Wed Sep 12 07:48:10 2018
Last change: Wed Sep 12 07:46:54 2018 by root via cibadmin on hso-hana-vm-s2-1

7 nodes configured
17 resources configured

              *** Resource management is DISABLED ***
  The cluster will not attempt to start, stop or recover services

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm (unmanaged)
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00] (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-0 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00] (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Master hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm hso-hana-vm-s1-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s2-0 (unmanaged)
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s2-0 (unmanaged)
</code></pre>


E o exemplo de comando abaixo, veja como o modo de manutenção do cluster de fim:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Outro comando crm permite que a configuração de cluster completo num editor com a possibilidade de editar a obter. Depois de guardar as alterações, o cluster começa ações adequadas:

<pre><code>
crm configure edit
</code></pre>

Para examinar a configuração completa do cluster, utilize o crm **mostrar** opção:

<pre><code>
crm configure show
</code></pre>



Depois de falhas de recursos do cluster cujo, o **status de crm** comando mostra uma lista de **ações de falha**. Veja um exemplo para essa saída abaixo:


<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Thu Sep 13 07:30:44 2018
Last change: Thu Sep 13 07:30:20 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm hso-hana-vm-s2-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0

Failed Actions:
* rsc_SAPHanaCon_HSO_HDB00_monitor_60000 on hso-hana-vm-s2-0 'unknown error' (1): call=86, status=complete, exitreason='none',
    last-rc-change='Wed Sep 12 17:01:28 2018', queued=0ms, exec=277663ms
</code></pre>

É necessário fazer uma limpeza de cluster após falhas. Basta utilizar novamente o comando de crm e utilize a opção de comando **limpeza** para se livrar deles falha na atribuição de nomes correspondente de entradas de ação recurso de cluster conforme mostrado abaixo:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

O comando deverá devolver um resultado, o que se parece com o exemplo abaixo:

<pre><code>
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-dm
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-2
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-2
Waiting for 7 replies from the CRMd....... OK
</code></pre>



## <a name="failover--takeover"></a>Ativação pós-falha / obtenção de controlo

Como já mencionado na primeira seção com notas importantes, não deve utilizar um encerramento correto padrão para testar a ativação pós-falha de cluster ou a obtenção de controlo do SAP HANA HSR. Em vez disso, é recomendado para acionar, por exemplo, um entre em pânico do kernel ou forçar uma migração de recursos ou talvez Encerre todas as redes no nível do SO de uma VM. Outro método seria a **crm \<nó\> espera** comando. Consulte também o documento SUSE, o que pode ser encontrado [aqui][sles-12-ha-paper]. Abaixo, ver três comandos de exemplo para forçar uma ativação pós-falha do cluster:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

Também é descrito na seção sobre manutenção planeada, é uma boa forma para monitorizar as atividades de cluster executar **SAPHanaSR showAttr** com o **watch** comando:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Além disso, ele ajuda a ver o estado de paisagem de SAP HANA proveniente de um script de python SAP. Este valor de estado é um, o que a configuração de cluster está à procura de. Fica claro ao pensar numa falha de nó de trabalho. Se um nó de trabalho ficar inativo, o SAP HANA não devolve imediatamente um erro para o estado de funcionamento do sistema totalmente aumentar horizontalmente. Existem algumas repetições para evitar as ativações pós-falha desnecessárias. Apenas se o estado é alterado de Ok (valor de retorno 4) erro (valor de retorno 1) reage o cluster. Por isso é correto, se a saída do **SAPHanaSR showAttr** mostra uma VM com o estado **offline** mas não existe nenhuma atividade de mudar a primária e secundária. Nenhuma atividade do cluster é ativada, desde que SAP HANA não retorna um erro.

Pode monitorizar o estado de funcionamento de paisagem de SAP HANA como usuário \<HANA SID\>adm chamando o python SAP do script da seguinte forma (poderá ter de adaptar o caminho):

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

O resultado deste comando deve ser semelhante o exemplo abaixo. É importante o **estado do anfitrião** coluna, bem como a **geral do Estado de um anfitrião**. O resultado real é, na verdade, mais amplo com colunas adicionais.
Para tornar a tabela de saída mais legível neste documento, a maioria das colunas à direita foram removidos:

<pre><code>
| Host             | Host   | Host   | Failover | Remove | 
|                  | Active | Status | Status   | Status | 
|                  |        |        |          |        | 
| ---------------- | ------ | ------ | -------- | ------ |    .......
| hso-hana-vm-s2-0 | yes    | ok     |          |        |        
| hso-hana-vm-s2-1 | yes    | ok     |          |        |         
| hso-hana-vm-s2-2 | yes    | ok     |          |        |        

overall host status: ok
</code></pre>


Há outro comando para verificar as atividades atuais do cluster. Veja abaixo o comando e a cauda da saída depois do nó principal do site primário foi terminado. Pode ver a lista de ações de transição, como **promover** primeiro nó principal secundário (**hso-hana-vm-s2-0**) como o novo mestre principal. Se tudo está ok e todas as atividades estão concluídas, esta lista de **resumo de transição** tem de estar vazio.

<pre><code>
 crm_simulate -Ls

...........

Transition Summary:
 * Fence hso-hana-vm-s1-0
 * Stop    rsc_SAPHanaTop_HSO_HDB00:1   (hso-hana-vm-s1-0)
 * Demote  rsc_SAPHanaCon_HSO_HDB00:1   (Master -> Stopped hso-hana-vm-s1-0)
 * Promote rsc_SAPHanaCon_HSO_HDB00:5   (Slave -> Master hso-hana-vm-s2-0)
 * Move    rsc_ip_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
 * Move    rsc_nc_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
</code></pre>



## <a name="planned-maintenance"></a>Manutenção planeada 

Existem diferentes casos de utilização que diz respeito à manutenção planeada. Uma pergunta é, por exemplo, se estiver apenas a manutenção de infraestrutura, como alterações no nível do SO e de configuração do disco ou de uma atualização do HANA.
Pode encontrar informações adicionais em documentos do SUSE, como [aqui] [ sles-zero-downtime-paper] ou [aqui outro][sles-12-for-sap]. Esses documentos também incluem exemplos de como migrar manualmente um site primário.

Testes internos intensa foi feito para verificar o caso de utilização de manutenção de infraestrutura. Para evitar qualquer tipo de problema relacionados com a migração primária, foi tomada a decisão de migrar sempre um site primário antes de colocar um cluster no modo de manutenção. Dessa maneira não é necessário para que o cluster esquecer a situação anterior (do lado que foi primário e do lado que foi secundário).

Existem duas situações diferentes nesse aspecto:

1. Manutenção planeada secundário atual. 
   Neste caso, pode simplesmente colocar o cluster no modo de manutenção e fazem o trabalho no secundário sem afetar o cluster

2. Manutenção planeada do primário atual. 
   Para permitir que os utilizadores continuar a trabalhar durante a manutenção, é necessário forçar uma ativação pós-falha. Com essa abordagem tem de acionar a ativação pós-falha do cluster pela pacemaker e não apenas no nível de SAP HANA HSR. A configuração de pacemaker aciona automaticamente a obtenção de controlo do SAP HANA. Além disso, é necessário realizar a ativação pós-falha antes de colocar o cluster no modo de manutenção.

O procedimento para manutenção do site secundário atual gostaria que os passos abaixo:

1. Colocar o cluster no modo de manutenção
2. Realizar o trabalho no site secundário 
3. Fim do modo de manutenção do cluster

O procedimento para manutenção no site primário atual é mais complexo:

1. Manualmente, acionar uma ativação pós-falha / SAP de obtenção de controlo do HANA através de uma migração de recursos de Pacemaker (ver detalhes abaixo)
2. SAP HANA no primeiro site primário é obter encerrado pela configuração do cluster
3. Colocar o cluster no modo de manutenção
4. Depois de terminar o trabalho de manutenção, registe o primário primeiro como o novo site secundário
5. Limpar a configuração de cluster (ver detalhes abaixo)
6. Fim do modo de manutenção do cluster


Migrar um recurso (por exemplo forçar uma ativação pós-falha) adiciona uma entrada para a configuração do cluster. Terá de limpar estas entradas antes de terminar o modo de manutenção. Eis um exemplo:

Primeira etapa é forçar uma ativação pós-falha do cluster ao migrar o recurso de msl ao nó principal secundário atual. O comando abaixo apresenta um aviso que uma restrição"Mover" foi criada.

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


Verifique o processo de ativação pós-falha através do comando **SAPHanaSR showAttr**. O que ajuda a monitorizar o estado de cluster é abrir uma janela do shell dedicado e iniciar o comando com **assista**:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

A saída deve refletir a ativação pós-falha manual. O primeiro nó principal secundário de obteve **promovido** (neste exemplo **hso-hana-vm-s2-0**) e o primeiro site primário foi parado (**lss** valor **1** para o primeiro nó principal primário **hso-hana-vm-s1-0**): 

<pre><code>
Global cib-time                 prim  sec srHook sync_state
------------------------------------------------------------
global Wed Sep 12 07:40:02 2018 HSOS2 -   SFAIL  SFAIL


Sites lpt        lss mns              srr
------------------------------------------
HSOS1 10         1   hso-hana-vm-s1-0 P
HSOS2 1536738002 4   hso-hana-vm-s2-0 P


Hosts            clone_state node_state roles                        score  site
----------------------------------------------------------------------------------
hso-hana-dm                  online
hso-hana-vm-s1-0 UNDEFINED   online     master1::worker:             150    HSOS1
hso-hana-vm-s1-1 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s1-2 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s2-0 PROMOTED    online     master1:master:worker:master 150    HSOS2
hso-hana-vm-s2-1 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
hso-hana-vm-s2-2 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
</code></pre>

Após a ativação pós-falha do cluster e a obtenção de controlo do SAP HANA, coloque o cluster no modo de manutenção, conforme descrito na secção pacemaker.

Os comandos **SAPHanaSR showAttr** ou **status de crm** não indicar qualquer coisa sobre as restrições criadas pela migração de recursos. É uma opção para tornar essas restrições visíveis Mostrar a configuração do recurso de cluster completo com o seguinte comando:

<pre><code>
crm configure show
</code></pre>

Na configuração do cluster, encontrar uma nova restrição de localização causada pela migração manual de recursos anteriores. Eis um exemplo (entrada que comece com **localização cli -**):

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Infelizmente essas restrições podem ter um impacto sobre o comportamento geral do cluster. Por isso é obrigatório para removê-los novamente antes de recolocar todo o sistema de cópia de segurança. Com o **unmigrate** é possível limpar as restrições de localização, que foram criados antes de comando. A nomenclatura pode ser um pouco confusa. Isso não significa que tentaria migrar o recurso de volta para a sua VM original a partir do qual foi migrada. Ele apenas remove as restrições de localização e também retorna informações correspondentes ao executar o comando:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

No final do trabalho de manutenção, pare o modo de manutenção do cluster conforme mostrado na secção pacemaker.



## <a name="hbreport-to-collect-log-files"></a>hb_report para recolher ficheiros de registo

Para analisar problemas de cluster pacemaker, é útil e também pedida pelo suporte do SUSE para executar o **hb_report** utilitário. Recolhe todos os logfiles importantes, que permitem a análise do que aconteceu. Eis uma chamada de exemplo com uma hora de início e fim em que um incidente específico ocorreu (também consulte primeiro a secção sobre notas importantes):

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

O comando spills horizontalmente em que ele coloca os ficheiros de registo comprimido:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

Pode então extrair os ficheiros individuais através do padrão **tar** comando:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

Olhando para os ficheiros extraídos, encontrará todos os ficheiros de registo. A maioria dos editores foram colocar em diretórios separados para cada nó do cluster:

<pre><code>
-rw-r--r-- 1 root root  13655 Sep 13 09:01 analysis.txt
-rw-r--r-- 1 root root  14422 Sep 13 09:01 description.txt
-rw-r--r-- 1 root root      0 Sep 13 09:01 events.txt
-rw-r--r-- 1 root root 275560 Sep 13 09:00 ha-log.txt
-rw-r--r-- 1 root root     26 Sep 13 09:00 ha-log.txt.info
drwxr-xr-x 4 root root   4096 Sep 13 09:01 hso-hana-dm
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-2
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-2
-rw-r--r-- 1 root root 264726 Sep 13 09:00 journal.log
</code></pre>


Dentro do intervalo de tempo, que foi especificado o nó principal atual **hso-hana-vm-s1-0** foi terminado. Na **journal.log** encontrará entradas relacionadas com este evento:

<pre><code>
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: (to hsoadm) root on none
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session opened for user hsoadm by (uid=0)
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 systemd[1]: Started Session c44290 of user hsoadm.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] A new membership (10.0.0.13:120996) was formed. Members left: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] Failed to receive the leave message. failed: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Removing all hso-hana-vm-s1-0 attributes for peer loss
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [QUORUM] Members[6]: 7 2 3 4 5 6
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [MAIN  ] Completed service synchronization, ready to provide service.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 crmd[28315]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 pacemakerd[28308]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:03+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session closed for user hsoadm
</code></pre>

Outro exemplo é o ficheiro de registo pacemaker no mestre de secundário, tornou-se o novo mestre principal. Aqui está um trecho, que mostra que o estado do nó principal primário cancelado foi definido como **offline**.

<pre><code>
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 3 still member of group stonith-ng (peer=hso-hana-vm-s1-2, counter=5.1)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 4 still member of group stonith-ng (peer=hso-hana-vm-s2-0, counter=5.2)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 5 still member of group stonith-ng (peer=hso-hana-vm-s2-1, counter=5.3)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 6 still member of group stonith-ng (peer=hso-hana-vm-s2-2, counter=5.4)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 7 still member of group stonith-ng (peer=hso-hana-dm, counter=5.5)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 1 left group crmd (peer=hso-hana-vm-s1-0, counter=5.0)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: crm_update_peer_proc:     pcmk_cpg_membership: Node hso-hana-vm-s1-0[1] - corosync-cpg is now offline
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: peer_update_callback:     Client hso-hana-vm-s1-0/peer now has status [offline] (DC=hso-hana-dm, changed=4000000)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 2 still member of group crmd (peer=hso-hana-vm-s1-1, counter=5.0)
</code></pre>





## <a name="sap-hana-globalini"></a>SAP HANA global.ini


Abaixo, ver trechos do ficheiro de global.ini SAP HANA no site do cluster 2 como um exemplo para mostrar as entradas de resolução de nome de anfitrião para utilizar redes diferentes para comunicação entre nós de SAP HANA e HSR:

<pre><code>
[communication]
tcp_keepalive_interval = 20
internal_network = 10.0.2/24
listeninterface = .internal
</code></pre>


<pre><code>
[internal_hostname_resolution]
10.0.2.40 = hso-hana-vm-s2-0
10.0.2.42 = hso-hana-vm-s2-2
10.0.2.41 = hso-hana-vm-s2-1
</code></pre>

<pre><code>
[ha_dr_provider_SAPHanaSR]
provider = SAPHanaSR
path = /hana/shared/myHooks
execution_order = 1
</code></pre>

<pre><code>
[system_replication_communication]
listeninterface = .internal

[system_replication_hostname_resolution]
10.0.1.30 = hso-hana-vm-s1-0
10.0.1.31 = hso-hana-vm-s1-1
10.0.1.32 = hso-hana-vm-s1-2
10.0.1.40 = hso-hana-vm-s2-0
10.0.1.41 = hso-hana-vm-s2-1
10.0.1.42 = hso-hana-vm-s2-2
</code></pre>



## <a name="hawk"></a>HAWK

A solução de cluster também fornece uma interface de browser, que oferece uma GUI interessante para as pessoas que preferem menus e gráficos em comparação comparados todos os comandos no nível de shell.
Para utilizar a interface de browser, pegar o URL mostrado abaixo e substitua **\<nó\>** por um nó de SAP HANA real e, em seguida, introduza as credenciais do cluster (utilizador **hacluster**):

<pre><code>
https://&ltnode&gt:7630
</code></pre>

Captura de ecrã seguinte mostra o dashboard do cluster:


![Dashboard de clusters HAWK](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


A segunda captura de ecrã pode ver um exemplo das restrições de localização causado por uma migração de recursos de cluster, conforme explicado na seção de manutenção planeada:


![Restrições de lista HAWK](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


Outro recurso interessante é a possibilidade de carregar uma **hb_report** saída (consulte a secção **hb_report**) no **HAWK** sob **histórico** como mostrado na captura de ecrã seguinte:

![Saída de hb_report HAWK carregamento](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

O **histórico Explorer** , em seguida, permite passar por todas as transições de cluster incluídas no **hb_report** saída:

![HAWK ver transições dentro da saída hb_report](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

A última captura de ecrã, pode ver a secção de detalhes de uma transição única, que mostra que o cluster reagiram numa falha de nó principal primário (nó **hso-hana-vm-s1-0**) e agora está a promover o nó secundário como o novo mestre (**hso-hana-vm-s2-0**):

![HAWK examinar uma transição única](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Passos Seguintes

Este guia de resolução de problemas é sobre a elevada disponibilidade para SAP HANA numa configuração de escalamento horizontal. Outro componente importante dentro de um ambiente SAP além da base de dados é a pilha do SAP NetWeaver. Em seguida deverá ler sobre a elevada disponibilidade para SAP NetWeaver em máquinas virtuais do Azure com o SUSE Enterprise Linux Server na [isso] [ sap-nw-ha-guide-sles] artigo.

