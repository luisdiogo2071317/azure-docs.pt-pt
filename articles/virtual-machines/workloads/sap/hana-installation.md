---
title: Instalar o SAP HANA no SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Como instalar o SAP HANA num SAP HANA no Azure (instância grande).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d335e135551b7b6faed8ee566acb14b46fd6c81
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107516"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Como instalar e configurar o SAP HANA (instâncias grandes) no Azure

Seguem-se algumas definições que é importante saber antes de ler este guia. Na [descrição geral do SAP HANA (instâncias grandes) e a arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) , apresentamos duas classes diferentes de unidades de instância grande do HANA com:

- S72, S72m, S144, S144m, S192, S192m e S192xm, quais nos Referimos como o "tipo de classe," de SKUs.
- S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm e S960m, o qual nos Referimos como a "classe de tipo II" dos SKUs.

O especificador de classe vai ser utilizado por toda a documentação de instância grande do HANA para, eventualmente, fazer referência a diferentes capacidades e requisitos com base em SKUs de instância grande do HANA.

Outras definições que usamos com frequência são:
- **Carimbo de instância grande:** uma pilha de infraestrutura de hardware que é a TDI do SAP HANA certificadas e dedicado para executar as instâncias do SAP HANA no Azure.
- **SAP HANA no Azure (instâncias grandes):** nome oficial para a oferta no Azure executar o HANA instâncias no TDI do SAP HANA certificadas hardware que é implementada nos carimbos de data / instância grande em diferentes regiões do Azure. O termo relacionado **instância grande do HANA** é a abreviação de SAP HANA no Azure (instâncias grandes) sendo amplamente utilizada neste guia de implementação técnica.


A instalação do SAP HANA é sua responsabilidade e pode começar a atividade após a entrega de um SAP HANA de novo no servidor do Azure (instâncias grandes). E, depois a conectividade entre a vnet ou Vnets do Azure e a instância grande do HANA unidade (s) foi estabelecida. 

> [!Note]
> Por diretiva SAP, a instalação do SAP HANA deve ser executada por uma pessoa certificada para efetuar instalações do SAP HANA. Uma pessoa, o que tiver passado o exame de certificação associar de tecnologia de SAP, o exame de certificação de instalação do SAP HANA, ou por um certificado integrador de sistemas (SI).

Verificar novamente, especialmente quando instalar HANA 2.0, a planear [2235581 de n. º de nota de suporte de SAP - SAP HANA: sistemas operativos suportados](https://launchpad.support.sap.com/#/notes/2235581/E) para certificar-se de que o sistema operacional é suportado com o SAP HANA de versão que optar por instalar. Tenha em atenção que o SO suportado para HANA 2.0 é mais restrito do que o sistema operativo suportado para HANA 1.0. 

> [!IMPORTANT] 
> Unidades de tipo II apenas o SLES 12 SP2 sistema operacional de versão é suportada neste momento. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>Primeiros passos depois de receber a unidade de instância grande do HANA

**Primeiro passo** depois de receber a instância grande do HANA e ter acesso estabelecido e conectividade para as instâncias, é registrar o sistema operacional da instância do seu fornecedor do sistema operacional. Este passo inclui a registar o seu SO do SUSE Linux numa instância do SUSE SMT que tem de ter implementado numa VM no Azure. A unidade de instância grande do HANA, pode ligar a esta instância SMT (ver mais tarde nesta documentação). Ou seu SO do Red Hat tem de ser registado com o Gestor de subscrições do Red Hat precisam de ligar a. Observações também de ver neste [documento](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Este passo também é necessário para conseguir corrigir o sistema operacional. Uma tarefa que é responsabilidade do cliente. Para o SUSE, encontre a documentação para instalar e configurar SMT [aqui](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**Segundo passo** é verificar a existência de novos patches e correções da versão/versão de SO específica. Verifique se o nível de correção da instância grande do HANA está no estado mais recente. Com base no tempo na patch/versões do SO e as alterações à imagem do que Microsoft pode implementar, poderá haver casos em que os patches mais recentes não podem ser incluídos. Por conseguinte, é um passo obrigatório após a demorar mais de uma unidade de instância grande do HANA, para verificar se a patches relevantes de segurança, funcionalidades, disponibilidade e desempenho foram lançados enquanto isso pelo fornecedor do Linux específico e têm de ser aplicadas.

**Terceiro passo** é verificar o SAP Notes relevantes para instalar e configurar o SAP HANA em lançamento/versão de SO específica. Devido à alteração de recomendações ou alterações para SAP Notes ou configurações que são dependentes de cenários de instalação individuais, Microsoft não sempre poderá ter uma unidade de instância grande do HANA configurada perfeitamente. Por conseguinte, é obrigatório para como um cliente, para ler as notas de SAP relacionados com o SAP HANA na sua versão exata do Linux. Também verificar as configurações da versão do SO versão/necessária e aplicar as definições de configuração em que não fez.

Em particular, verifique os seguintes parâmetros e, eventualmente, ajustado para:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- NET.IPv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

A partir do SLES12 SP1 e RHEL 7.2, esses parâmetros devem ser definidos num ficheiro de configuração no diretório /etc/sysctl.d. Por exemplo, um ficheiro de configuração com o nome 91-NetApp-HANA.conf tem de ser criado. Para versões mais antigas de SLES e RHEL, esses parâmetros têm de ser in/etc/sysctl.conf conjunto.

Para RHEL todas as versões e a partir do SLES12, o 
- sunrpc.tcp_slot_table_entries = 128

parâmetro tem de ser definido in/etc/modprobe.d/sunrpc-local.conf. Se o ficheiro não existir, ela primeiro deve ser criada, adicionando a seguinte entrada: 
- options sunrpc tcp_max_slot_table_entries=128

**Quarto passo** é verificar a hora do sistema de sua unidade de instância grande do HANA. As instâncias são implementadas com um fuso horário do sistema que representam a localização da região do Azure, que o carimbo de instância grande do HANA está localizado em. Tem liberdade para alterar a hora do sistema ou o fuso horário das instâncias que é proprietário. Ao fazer isso e ordenação mais instâncias no seu inquilino, esteja preparado-se de que necessita de adaptar o fuso horário das instâncias recentemente entregues. Operações da Microsoft têm não existem informações sobre o fuso horário do sistema que configurou com as instâncias depois de entregar o direito de permanência. Por conseguinte, as instâncias recentemente implementadas não podem ser definidas no mesmo fuso horário que aquela alterada para. Como resultado, é sua responsabilidade de cliente para verificar e se for necessário adaptar o fuso horário da instância (s) passada. 

**Quinto passo** é verificar etc/anfitriões. Como os painéis obterem passados, eles têm diferentes endereços IP atribuídos para diferentes fins (consulte a secção seguinte). Verifique o ficheiro de anfitriões/etc. Em casos em que as unidades são adicionadas num inquilino existente, não espere que ter etc/anfitriões de novos sistemas implantados mantidos corretamente com os endereços IP dos sistemas de fornecido anteriormente. Por conseguinte, é com enquanto cliente para verificar as definições corretas para isso, que uma instância recentemente implementada, pode interagir e resolver os nomes de anteriormente implementados unidades no seu inquilino. 

## <a name="networking"></a>Redes
Partimos do princípio de que seguiu as recomendações no projeto as VNets do Azure e ligar esses VNets para as instâncias grandes do HANA, conforme descrito nestes documentos:

- [Descrição geral do SAP HANA (instância grande) e a arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infraestrutura de SAP HANA (instâncias grandes) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Existem alguns detalhes que vale a pena mencionar sobre o funcionamento em rede das unidades de únicos. Cada unidade de instância grande do HANA vem com dois ou três endereços IP que estão atribuídos a dois ou três portas NIC da unidade. Três endereços IP são utilizados nas configurações de escalamento horizontal do HANA e o cenário do HANA System Replication. Um dos endereços IP atribuídos ao NIC da unidade é para fora do conjunto de IP do servidor que foi descrito na [descrição geral do SAP HANA (instância grande) e a arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Consultar [cenários suportados de HLI](hana-supported-scenario.md) para obter detalhes de ethernet para a sua arquitetura.

## <a name="storage"></a>Armazenamento

O esquema de armazenamento para o SAP HANA no Azure (instâncias grandes) é configurado pelo SAP HANA no Azure Service Management através do SAP recomendado guia linhas, conforme documentado no [requisitos de armazenamento do SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) white paper. Os tamanhos aproximados dos volumes diferentes com SKUs diferentes instâncias grandes de HANA tem documentados em [descrição geral do SAP HANA (instância grande) e a arquitetura no Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

As convenções de nomenclatura dos volumes de armazenamento estão listadas na tabela a seguir:

| Utilização do armazenamento | Nome de montagem | Nome do volume | 
| --- | --- | ---|
| Dados do HANA | /Hana/data/SID/mnt0000<m> | Storage IP:/hana_data_SID_mnt00001_tenant_vol |
| Registo do HANA | /Hana/log/SID/mnt0000<m> | Storage IP:/hana_log_SID_mnt00001_tenant_vol |
| Cópia de segurança de registo HANA | /Hana/log/backups | Armazenamento de IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| HANA partilhado | /Hana/Shared/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/SAP/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Onde SID = a ID de sistema de instância HANA 

E de inquilino = uma enumeração interna de operações quando implementar um inquilino.

Como pode ver, usr/sap e HANA partilhados estão a partilhar o mesmo volume. A nomenclatura do mountpoints incluem o ID de sistema das instâncias do HANA, bem como o número de montagem. Em implementações de aumentar verticalmente só existe uma montagem, como mnt00001. Ao passo que a implementação de escalamento horizontal, verá tantos monta, como, tiver nós de trabalho e o mestre. Para o ambiente de escalamento horizontal, dados, registo, os volumes de cópia de segurança de registo são partilhados e anexados a cada nó na configuração de escalamento horizontal. Para configurações de executar várias instâncias do SAP, um conjunto diferente de volumes é criado e ligado à unidade HAN de instância grande. Consultar [cenários suportados de HLI](hana-supported-scenario.md) para obter detalhes de esquema de armazenamento para o seu cenário.

À medida que leia o documento e procurar uma unidade de instância grande do HANA, percebe que as unidades são fornecidos com o volume de disco bem generoso para HANA/dados e que temos um volume HANA/registo/cópia de segurança. O motivo por que estamos tão grandes dos HANA/dados o tamanho é que os instantâneos de armazenamento que oferecemos para como um cliente estiver a utilizar o mesmo volume de disco. Isso significa que o armazenamento mais instantâneos efetuar, mais espaço é consumido pelo instantâneos nos volumes de armazenamento atribuída. O volume HANA/registo/cópia de segurança não foi pensado para ser o volume de colocar as cópias de segurança da base de dados. Ele tem o tamanho a utilizar como volume de cópia de segurança para os backups de log de transação do HANA. Nas futuras versões do armazenamento do instantâneo self-service, que podemos destina-se a este volume específico para ter mais frequentes instantâneos. E com que mais frequente replicações para o site de recuperação de desastres se desejar a opção-in para a funcionalidade de recuperação de desastres fornecida pela infraestrutura de instância grande do HANA. Ver detalhes nos [SAP HANA (instâncias grandes) de elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Para além do armazenamento fornecido, pode comprar capacidade de armazenamento adicional em incrementos de 1 TB. Este armazenamento adicional pode ser adicionado como novos volumes para instâncias grandes do HANA.

Durante a integração com o SAP HANA no Azure Service Management, o cliente especifica um ID de utilizador (UID) e o ID de grupo (GID) para o grupo de utilizador e sapsys sidadm (ex: 1000,500) é necessário que durante a instalação do sistema SAP HANA, esses mesmos valores são utilizados. Como pretende implementar várias instâncias HANA numa unidade, tem vários conjuntos de volumes (um conjunto para cada instância). Como resultado, no momento da implementação terá de definir:

- O SID das diferentes instâncias HANA (sidadm é derivado fora dele).
- Tamanhos de memória das diferentes instâncias do HANA. Uma vez que o tamanho de memória por instâncias define o tamanho dos volumes em cada conjunto de individual volume.

Com base nas recomendações do fornecedor de armazenamento as seguintes opções de montagem estão configuradas para todos os volumes montados (exclui arranque LUN):

- rw de NFS, vers=3.0,username = 4, disco rígido, timeo = 600, rsize = 1048576, wsize = 1048576, intr, noatime, bloquear 0 0

Estes montagem pontos são configurados no/etc/fstab, como mostrado nos gráficos seguintes:

![fstab de volumes montados numa unidade de instância grande do HANA](./media/hana-installation/image1_fstab.PNG)

O resultado do comando df -h numa unidade de instância grande do HANA S72m teria o seguinte aspeto:

![fstab de volumes montados numa unidade de instância grande do HANA](./media/hana-installation/image2_df_output.PNG)


O controlador de armazenamento e nós nos carimbos de data / instância grande são sincronizadas com os servidores NTP. Consigo de sincronizar o SAP HANA nas unidades do Azure (instâncias grandes) e as VMs do Azure num servidor NTP, não deverá haver nenhum aconteça de desvios de tempo significativo entre a infraestrutura e as unidades de computação no Azure ou de instância grande carimbos de data /.

Para otimizar o SAP HANA para o armazenamento utilizado por baixo, também deve configurar os seguintes parâmetros de configuração de SAP HANA:

- max_parallel_io_requests 128
- async_read_submit no
- async_write_submit_active no
- async_write_submit_blocks todos os
 
Para versões de SAP HANA 1.0 até SPS12, esses parâmetros podem ser definidos durante a instalação da base de dados SAP HANA, conforme descrito em [2267798 no SAP Note # - configuração da base de dados SAP HANA](https://launchpad.support.sap.com/#/notes/2267798)

Também pode configurar os parâmetros após a instalação de banco de dados do SAP HANA com o framework hdbparam. 

Com o SAP HANA 2.0, a estrutura de hdbparam foi preterida. Como resultado os parâmetros devem ser definidos usando comandos SQL. Para obter detalhes, consulte [SAP Note #2399079: eliminação de hdbparam no HANA 2](https://launchpad.support.sap.com/#/notes/2399079).

Consultar [cenários suportados de HLI](hana-supported-scenario.md) para saber o esquema de armazenamento para a sua arquitetura.

## <a name="operating-system"></a>Sistema operativo

> [!IMPORTANT] 
> Unidades de tipo II apenas o SLES 12 SP2 sistema operacional de versão é suportada neste momento. 

Espaço de comutação da imagem do SO entregue está definido como 2 GB de acordo com o [1999997 de n. º de nota de suporte de SAP - FAQ: memória do SAP HANA](https://launchpad.support.sap.com/#/notes/1999997/E). Qualquer definição diferentes pretendido tem de ser definida por si como um cliente.

[SUSE Linux Enterprise Server 12 SP1 para aplicações SAP](https://www.suse.com/products/sles-for-sap/hana) é a distribuição de Linux instalado para o SAP HANA no Azure (instâncias grandes). Essa distribuição particular oferece recursos específicos da SAP &quot;prontos a utilizar&quot; (incluindo parâmetros predefinidos para executar o SAP no SLES efetivamente).

Ver [recurso de biblioteca/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) no site do SUSE e [SAP no SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) na rede de Comunidade do SAP (SCN) para vários recursos úteis relacionadas com a implementação de SAP HANA no SLES (incluindo a configuração de alta Disponibilidade, proteção de segurança específica para operações de SAP e muito mais).

SAP adicional e útil em ligações de SUSE:

- [SAP HANA no Site do SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Melhor prática para SAP: colocar em fila replicação – SAP NetWeaver no SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – proteção contra vírus de SLES para SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (incluindo 12 do SLES para SAP Applications).

Notas de suporte SAP aplicável à implementação de SAP HANA no SLES 12:

- [Suporte a nota SAP #1944799 – SAP HANA diretrizes para instalação do sistema operativo SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [Suporte a nota SAP #2205917 – SAP HANA DB recomendado configurações de SO para o SLES 12 para aplicações SAP](https://launchpad.support.sap.com/#/notes/2205917/E).
- [Nota de suporte do SAP #1984787 – SUSE Linux Enterprise Server 12: Observações de instalação](https://launchpad.support.sap.com/#/notes/1984787).
- [Suporte a nota SAP #171356 – Software SAP no Linux: informações gerais](https://launchpad.support.sap.com/#/notes/1984787).
- [Suporte a nota SAP #1391070 – soluções Linux UUID](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux para o SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) é outra oferta para a execução do SAP HANA nas instâncias grandes do HANA. Versões do RHEL 6.7 e 7.2 estão disponíveis. . Tenha em atenção em diferentes dos padrões convencionais de VMs do Azure nativo em que são suportadas apenas RHEL 7.2 e versões mais recentes, HANA nas instâncias grandes suporta RHEL 6.7 também. No entanto, recomendamos que utilize uma versão de 7.x RHEL.

SAP no links relacionados do Red Hat adicional e útil:
- [SAP HANA no Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

Notas de suporte SAP aplicável à implementação de SAP HANA em Red Hat:

- [Suporte a nota SAP #2009879 - SAP HANA diretrizes para o sistema operativo do Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879/E).
- [Nota de suporte do SAP #2292690 - SAP HANA DB: Configurações de SO recomendado para RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [Nota de suporte do SAP #2247020 - SAP HANA DB: Configurações de SO recomendado para RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [Suporte a nota SAP #1391070 – soluções Linux UUID](https://launchpad.support.sap.com/#/notes/1391070).
- [A nota SAP de suporte #2228351 - Linux: Revisão de 11 de SPS de base de dados do SAP HANA 110 (ou superior) em RHEL 6 ou SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [A nota SAP de suporte #2397039 - FAQ: SAP no RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [Suporte a nota SAP #1496410 - Red Hat Enterprise Linux 6.x: instalação e atualização](https://launchpad.support.sap.com/#/notes/1496410).
- [Suporte a nota SAP #2002167 - Red Hat Enterprise Linux 7.x: instalação e atualização](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Sincronização de hora

Aplicações de SAP criadas sobre a arquitetura do SAP NetWeaver são confidenciais nas diferenças de tempo para os vários componentes que compõem o sistema SAP. Informações do Estado da SAP ABAP curto com o título de erro do ZDATE\_grande\_tempo\_DIFF são familiar provável, uma vez que esses despejos curtos apresentado quando a hora do sistema de diferentes servidores ou VMs é também muito-drifting.

Para o SAP HANA no Azure (instâncias grandes), tempo de sincronização efetuada no Azure&#39;t aplicam-se para as unidades de computação no carimbos de data / instância grande. Esta sincronização não é aplicável a execução de aplicações SAP em VMs do Azure nativo, como o Azure assegura um sistema&#39;hora de s corretamente está a ser sincronizada. Como resultado, uma hora separada servidor tem de ser definido, que pode ser utilizada pelo SAP servidores de aplicações em execução em VMs do Azure e o SAP HANA de base de dados instâncias em execução nas instâncias grandes do HANA. A infraestrutura de armazenamento em carimbos de data / instância grande é a hora sincronizada com os servidores NTP.

## <a name="setting-up-smt-server-for-suse-linux"></a>Configurar o servidor SMT para SUSE Linux
Instâncias grandes do SAP HANA não tem conectividade direta à Internet. Por conseguinte, não é um processo simples para registar tal uma unidade com o fornecedor do sistema operacional e para transferir e aplicar patches. No caso do SUSE Linux, uma solução possível configurar um servidor SMT numa VM do Azure. Ao passo que a VM do Azure tem de ser hospedado numa VNet do Azure, que está ligado à instância grande do HANA. Com esse um servidor SMT, a unidade de instância grande do HANA poderia se registrar e baixar patches. 

SUSE fornece um guia maior sobre seus [ferramenta de gestão de subscrição para o SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Como pré-condição para a instalação de um servidor SMT que atenda a tarefa para a instância grande do HANA, seria necessário:

- Uma VNet do Azure que está ligada para o circuito de ER instância grande do HANA.
- Uma conta SUSE que está associada uma organização. Ao passo que a organização precisaria ter alguns subscrição SUSE válida.

### <a name="installation-of-smt-server-on-azure-vm"></a>Instalação do server SMT numa VM do Azure

Neste passo, instala o servidor SMT numa VM do Azure. A primeira medida é iniciar sessão para o [atendimento ao consumidor do SUSE](https://scc.suse.com/)

Como está conectado, vá para a organização--> credenciais da organização. Nesta secção, deve encontrar as credenciais que são necessárias para configurar o servidor SMT.

O terceiro passo é instalar uma VM do Linux SUSE na VNet do Azure. Para implementar a VM, pegar uma imagem da Galeria de SLES 12 SP2 do Azure. No processo de implantação, não defina um nome DNS e não utilize endereços IP estáticos como visto nesta captura de tela

![implementação da VM para o servidor SMT](./media/hana-installation/image3_vm_deployment.png)

A VM implementada era uma VM mais pequena e tem o endereço IP na VNet do Azure de 10.34.1.4. Nome da VM foi smtserver. Após a instalação, a conectividade com a unidade de instância grande do HANA (s) foi verificada. Depende de como organizado resolução de nomes poderá ter de configurar a resolução das unidades de instância grande do HANA no etc/anfitriões da VM do Azure. Adicione um disco adicional para a VM que vai ser utilizado para conter os patches. O disco de arranque em si pode ser demasiado pequeno. No caso demonstrado, o disco foi montado para /srv/www/htdocs conforme mostrado na captura de ecrã seguinte. Um disco de 100 GB deve ser suficientes.

![implementação da VM para o servidor SMT](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Inicie sessão para a instância grande do HANA unidade (s), manter /etc/hosts e verifique se pode chegar a VM do Azure que deve ser executado o servidor SMT através da rede.

Depois que essa verificação é feita com êxito, tem de iniciar sessão VM do Azure que deve executar o servidor SMT. Se estiver a utilizar o putty para iniciar sessão VM, terá de executar esta sequência de comandos na janela do bash:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Depois de executar estes comandos, reinicie o bash para ativar as definições. Em seguida, inicie YAST.

Na YAST, aceda a manutenção de Software e procurar smt. Selecione smt, que muda automaticamente para o yast2 smt, conforme mostrado abaixo

![SMT no yast](./media/hana-installation/image5_smt_in_yast.PNG)


Aceite a seleção para instalação no smtserver. Uma vez instalado, vá para a configuração do servidor SMT e introduza as credenciais organizacionais do Centro de SUSE obtido anteriormente. Também introduza o nome de anfitrião de VM do Azure como o URL do servidor SMT. Nesta demonstração, era https://smtserver tal como apresentado no gráfico seguinte.

![Configuração do servidor SMT](./media/hana-installation/image6_configuration_of_smtserver1.png)

Como passo seguinte, precisa testar se a ligação para o atendimento ao consumidor do SUSE funciona. Como pode ver nos gráficos seguintes, no caso de demonstração, ele funcionava.

![Teste ligar para o atendimento ao consumidor do SUSE](./media/hana-installation/image7_test_connect.png)

Uma vez iniciada a configuração SMT, tem de fornecer uma palavra-passe da base de dados. Uma vez que é uma nova instalação, terá de definir essa palavra-passe, como mostra o gráfico seguinte.

![Definir a palavra-passe para a base de dados](./media/hana-installation/image8_define_db_passwd.PNG)

A interação seguinte que tiver é quando é criado um certificado. Percorrer a caixa de diálogo, conforme mostrado a seguir e deve continuar a etapa.

![Criar certificado para o servidor SMT](./media/hana-installation/image9_certificate_creation.PNG)

Pode haver alguns minutos gastos no passo de "Verificação de sincronização de execução" no final da configuração. Após a instalação e configuração do servidor SMT, deve encontrar o repositório de diretório numa montagem ponto /srv/www/htdocs/além de alguns subdiretórios no repositório. 

Reinicie o servidor SMT e serviços relacionados com estes comandos.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Transferência de pacotes do SMT server

Depois de todos os serviços são reiniciados, selecione os pacotes apropriados no gerenciamento de SMT usando Yast. A seleção de pacote depende a imagem do SO do servidor instância grande do HANA e não a versão SLES ou a versão da VM a executar o servidor SMT. Um exemplo do ecrã de seleção é mostrado abaixo.

![Selecione pacotes](./media/hana-installation/image10_select_packages.PNG)

Quando tiver terminado com a seleção de pacote, precisa para começar a cópia inicial dos pacotes selecionadas para o servidor SMT que configurou. Esta cópia é acionada de shell usando o comando smt-espelhamento conforme mostrado abaixo


![Transfira pacotes para o servidor SMT](./media/hana-installation/image11_download_packages.PNG)

Como pode ver acima, os pacotes devem ser copiados em diretórios criados sob a montagem ponto /srv/www/htdocs. Este processo pode demorar algum tempo. Depende de quantos pacotes que selecionar, ele pode demorar até uma hora ou mais.
Como esse processo terminar, terá de mover para a configuração de cliente SMT. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Configurar o cliente SMT em unidades de instância grande do HANA

Neste caso, os clientes são as unidades de instância grande do HANA. A configuração de servidor SMT copiados clientSetup4SMT.sh o script na VM do Azure. Cópia do script sobre a unidade de instância grande do HANA pretende ligar ao seu servidor SMT. Iniciar o script com a opção -h e conceda-lhe como parâmetro o nome do seu servidor SMT. No smtserver neste exemplo.

![Configurar cliente SMT](./media/hana-installation/image12_configure_client.PNG)

Pode haver um cenário em que o carregamento do certificado do servidor pelo cliente foi concluída com êxito, mas o registo falha conforme mostrado abaixo.

![Registo de cliente falhar](./media/hana-installation/image13_registration_failed.PNG)

Se o registo falhou, leia isto [SUSE suporta documentos](https://www.suse.com/de-de/support/kb/doc/?id=7006024) e execute as etapas descritas aqui.

> [!IMPORTANT] 
> Como o nome do servidor tem de fornecer o nome da VM, neste smtserver maiúsculas, sem o nome de domínio completamente qualificado. Simplesmente, a funciona de nome VM. 

Depois destes passos tiverem sido executados, tem de executar o seguinte comando na unidade instância grande do HANA

```
SUSEConnect –cleanup
```

> [!Note] 
> No nossos testes tínhamos sempre de aguardar alguns minutos após esse passo. ClientSetup4SMT.sh execução imediata, depois das medidas corretivas descritas no artigo SUSE, terminou com mensagens que o certificado não seria válido ainda. A aguardar 5 a 10 minutos, normalmente e executar clientSetup4SMT.sh terminou numa configuração de cliente com êxito.

Se tiver executado para o problema que precisava corrigir com base nos passos do artigo SUSE, terá de reiniciar clientSetup4SMT.sh na unidade instância grande do HANA novamente. Agora ele deve ser concluído com êxito, conforme mostrado abaixo.

![Registo de cliente foi concluída com êxito](./media/hana-installation/image14_finish_client_config.PNG)

Neste passo, configurou o cliente SMT da unidade instância grande do HANA para ligar o servidor de SMT instalado na VM do Azure. Agora pode tirar 'zypper se' ou "zypper no" para instalar os patches de SO para instâncias grandes do HANA ou instalar pacotes adicionais. É compreendida que só pode obter patches que transferiu anteriormente no servidor SMT.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Exemplo de uma instalação de SAP HANA nas instâncias grandes do HANA
Esta secção ilustra como instalar o SAP HANA numa unidade de instância grande do HANA. O estado de início temos ter o seguinte aspeto:

- Proporcionou à Microsoft todos os dados para implementar uma instância grande do SAP HANA.
- A instância grande do SAP HANA que recebeu da Microsoft.
- Criou uma VNet do Azure que está ligada à sua rede no local.
- Ligado o circuito de ExpressRotue para instâncias grandes do HANA para a mesma VNet do Azure.
- Instalou uma VM do Azure a utilizar como uma jumpbox para instâncias grandes do HANA.
- Certificou-se de que pode ligar a partir da jumpbox em sua unidade de instância grande do HANA e vice-versa.
- Tiver selecionado se todos os pacotes necessários e os patches estão instalados.
- Leia as notas de SAP e documentação atualizados sobre a instalação do HANA no sistema operacional estiver a utilizar e certifique-se de que a versão do HANA à escolha é suportada no sistema operacional de versão.

O que é mostrado nas sequências seguintes é o download dos pacotes de instalação HANA à jumpbox VM, nesse caso, executada num sistema operacional Windows a cópia dos pacotes para a unidade de instância grande do HANA e a sequência da configuração.

### <a name="download-of-the-sap-hana-installation-bits"></a>Transferência de bits de instalação SAP HANA
Uma vez que as unidades de instância grande do HANA não tem conectividade direta à internet, não é possível baixar diretamente os pacotes de instalação do SAP para a VM de instância grande do HANA. Para superar a conectividade internet direta em falta, terá da jumpbox. Transfira os pacotes para a jumpbox VM.

Para transferir os pacotes de instalação do HANA, é necessário um utilizador de S SAP ou outro usuário, que permite-lhe aceder ao Marketplace de SAP. Execute esta sequência de ecrãs após iniciar sessão:

Aceda a [SAP Service Marketplace](https://support.sap.com/en/index.html) > clique em transferência de Software > instalações e atualização > pelo índice alfabética > em H – SAP HANA edição da plataforma > edição da plataforma SAP HANA 2.0 > instalação > Baixe o ficheiros seguintes

![Transferir a instalação do HANA](./media/hana-installation/image16_download_hana.PNG)

No caso de demonstração, iremos transferir pacotes de instalação do SAP HANA 2.0. Na caixa de rápida do Azure à VM, expanda os extração de arquivos no diretório conforme mostrado abaixo.

![Extrair a instalação do HANA](./media/hana-installation/image17_extract_hana.PNG)

Como os arquivos são extraídos, copie o diretório criado pela extração, no caso acima 51052030, para a unidade de instância grande do HANA para o volume de /hana/shared num diretório que criou.

> [!Important]
> Não copie os pacotes de instalação para a raiz ou o LUN de arranque, uma vez que o espaço é limitado e tem de ser utilizada por outros processos também.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Instalar o SAP HANA na unidade instância grande do HANA
Para instalar o SAP HANA, terá de iniciar sessão como raiz do utilizador. Raiz apenas tem permissões suficientes para instalar o SAP HANA.
A primeira coisa que precisa fazer é definir as permissões no diretório copiado através de no/hana/partilhado. As permissões têm de definir como

```
chmod –R 744 <Installation bits folder>
```

Se pretender instalar o SAP HANA com a configuração gráfica, o pacote de gtk2 tem de ser instalado nas instâncias grandes do HANA. Verifique se está instalada com o comando

```
rpm –qa | grep gtk2
```

Ainda mais passos, vamos são demonstrar a configuração de SAP HANA com a interface gráfica do usuário. Como passo seguinte, vá até o diretório de instalação e navegue para o diretório de sub-rotina HDB_LCM_LINUX_X86_64. Iniciar

```
./hdblcmgui 
```
fora do diretório. Agora introdução guiado através de uma sequência de telas em que precisa fornecer os dados para a instalação. No caso demonstrado, iremos estiver a instalar o servidor de base de dados do SAP HANA e os componentes de cliente do SAP HANA. Portanto nossa seleção é "Base de dados do SAP HANA", conforme mostrado abaixo

![Selecione o HANA na instalação](./media/hana-installation/image18_hana_selection.PNG)

No ecrã seguinte, escolha a opção "Instalar o novo sistema"

![Selecione a nova instalação do HANA](./media/hana-installation/image19_select_new.PNG)

Após este passo, tem de selecionar entre os vários componentes adicionais que podem ser instalados além para o servidor de base de dados do SAP HANA.

![Selecione os componentes adicionais do HANA](./media/hana-installation/image20_select_components.PNG)

Para efeitos desta documentação, escolhemos o cliente do SAP HANA e o SAP HANA Studio. Também está instalada a uma instância de aumentar verticalmente. Por conseguinte, no ecrã seguinte, tem de escolher "Sistema anfitrião único" 

![Selecione a instalação de aumentar verticalmente](./media/hana-installation/image21_single_host.PNG)

No ecrã seguinte, tem de fornecer alguns dados

![Fornecer o SID do SAP HANA](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Como ID de sistema HANA (SID), terá de fornecer o mesmo SID, à medida que proporcionou à Microsoft quando pediu a implementação de instância grande do HANA. Escolher um SID diferente faz com que a instalação falhar devido a problemas de permissão de acesso nos volumes diferentes

Como instalação o diretório que utiliza o/hana/partilhado diretório. No próximo passo, tem de fornecer as localizações para os ficheiros de dados HANA e os ficheiros de registo do HANA


![Fornecer a localização do registo do HANA](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Deve definir como dados de ficheiros de registo e os volumes que já vinham com os pontos de montagem que contêm o SID que escolheu na seleção de ecrã antes deste ecrã. Se o SID falta de correspondência com aquela que escreveu no, no ecrã de antes, volte atrás e ajustar o SID para o valor que nos pontos de montagem.

No próximo passo, reveja o nome de anfitrião e, eventualmente, corrigi-lo. 

![Nome de anfitrião de revisão](./media/hana-installation/image24_review_host_name.PNG)

No próximo passo, terá também de obter dados que deu à Microsoft quando pediu a implementação de instância grande do HANA. 

![Fornecer ao utilizador do sistema UID e GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Tem de fornecer o mesmo ID de utilizador do sistema e ID de grupo de utilizadores à medida que proporcionou à Microsoft como a ordem a implementação de unidade. Se não fornecer os IDs de muito mesmos, a instalação do SAP HANA na unidade instância grande do HANA falha.

Os dois ecrãs, não podemos nesta documentação, tem de fornecer a palavra-passe para o usuário do sistema de base de dados do SAP HANA e a palavra-passe para o utilizador sapadm, que é utilizado para o agente de anfitrião do SAP que é instalado como parte da datab SAP HANA instância do ase.

Depois de definir a palavra-passe, um ecrã de confirmação está a aparecer. Verifique todos os dados apresentados e prosseguir a instalação. Atingir uma tela de progresso que documenta o progresso da instalação, como a mostrada abaixo

![Verificar o progresso da instalação](./media/hana-installation/image27_show_progress.PNG)

Como a instalação estiver concluída, deve uma imagem semelhante ao seguinte

![Conclusão da instalação](./media/hana-installation/image28_install_finished.PNG)

Neste momento, a instância do SAP HANA deve ser até e em execução e está pronto para utilização. Deverá conseguir ligar à mesma a partir do SAP HANA Studio. Além disso, certifique-se de que verifique os patches mais recentes do SAP HANA e aplica esses patches.
























































 







 




