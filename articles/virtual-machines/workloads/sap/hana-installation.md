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
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dad088138fea2dd4fadc0cc9eed71245c32a8e0b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162674"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Como instalar e configurar o SAP HANA (instâncias grandes) no Azure

Antes de ler este artigo, familiarize-se com [termos comuns de instâncias grandes do HANA](hana-know-terms.md) e o [SKUs de instâncias grandes do HANA](hana-available-skus.md).

A instalação do SAP HANA é sua responsabilidade e pode começar a atividade após a entrega de um SAP HANA de novo no servidor do Azure (instâncias grandes). E, depois a conectividade entre a vnet ou Vnets do Azure e a instância grande do HANA unidade (s) foi estabelecida. 

> [!Note]
> Por diretiva SAP, a instalação do SAP HANA deve ser executada por uma pessoa certificada para efetuar instalações do SAP HANA. Uma pessoa, o que tiver passado o exame de certificação associar de tecnologia de SAP, o exame de certificação de instalação do SAP HANA, ou por um certificado integrador de sistemas (SI).

Verificar novamente, especialmente quando instalar HANA 2.0, a planear [2235581 de n. º de nota de suporte de SAP - SAP HANA: sistemas operativos suportados](https://launchpad.support.sap.com/#/notes/2235581/E) para certificar-se de que o sistema operacional é suportado com o SAP HANA de versão que optar por instalar. Tenha em atenção que o SO suportado para HANA 2.0 é mais restrito do que o sistema operativo suportado para HANA 1.0. 

> [!IMPORTANT] 
> Unidades de tipo II apenas o SLES 12 SP2 sistema operacional de versão é suportada neste momento. 

É necessário validar o seguinte antes de iniciar a instalação do HANA:
- [Validar o HLI unidade (s)](#validate-the-hana-large-instance-units)
- [Configuração do sistema operativo](#operating-system)
- [Configuração da rede](#networking)
- [Configuração do armazenamento](#storage)


## <a name="validate-the-hana-large-instance-units"></a>Validar a unidade de instância grande do HANA

Depois de receber a unidade de instância grande do HANA da Microsoft Valide as seguintes definições e ajustar conforme necessário.

**Primeiro passo** depois de receber a instância grande do HANA e ter acesso estabelecido e conectividade para as instâncias, é registrar o sistema operacional da instância do seu fornecedor do sistema operacional. Este passo inclui a registar o seu SO do SUSE Linux numa instância do SUSE SMT que tem de ter implementado numa VM no Azure. A unidade de instância grande do HANA, pode ligar a esta instância SMT (consulte [como servidor SMT de configuração para o SUSE Linux](hana-setup-smt.md)). Ou seu SO do Red Hat tem de ser registado com o Gestor de subscrições do Red Hat precisam de ligar a. Observações também de ver neste [documento](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Este passo também é necessário para conseguir corrigir o sistema operacional. Uma tarefa que é responsabilidade do cliente. Para o SUSE, encontre a documentação para instalar e configurar SMT [aqui](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

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

### <a name="time-synchronization"></a>Sincronização de hora

Aplicações de SAP criadas sobre a arquitetura do SAP NetWeaver são confidenciais nas diferenças de tempo para os vários componentes que compõem o sistema SAP. Informações do Estado da SAP ABAP curto com o título de erro do ZDATE\_grande\_tempo\_DIFF são familiar provável, uma vez que esses despejos curtos apresentado quando a hora do sistema de diferentes servidores ou VMs é também muito-drifting.

Para o SAP HANA no Azure (instâncias grandes), tempo de sincronização efetuada no Azure&#39;t aplicam-se para as unidades de computação no carimbos de data / instância grande. Esta sincronização não é aplicável a execução de aplicações SAP em VMs do Azure nativo, como o Azure assegura um sistema&#39;hora de s corretamente está a ser sincronizada. Como resultado, uma hora separada servidor tem de ser definido, que pode ser utilizada pelo SAP servidores de aplicações em execução em VMs do Azure e o SAP HANA de base de dados instâncias em execução nas instâncias grandes do HANA. A infraestrutura de armazenamento em carimbos de data / instância grande é a hora sincronizada com os servidores NTP.


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

À medida que leia o documento e procurar uma unidade de instância grande do HANA, percebe que as unidades são fornecidos com o volume de disco bem generoso para HANA/dados e que temos um volume HANA/registo/cópia de segurança. O motivo por que estamos tão grandes dos HANA/dados o tamanho é que os instantâneos de armazenamento que oferecemos para como um cliente estiver a utilizar o mesmo volume de disco. Isso significa que o armazenamento mais instantâneos efetuar, mais espaço é consumido pelo instantâneos nos volumes de armazenamento atribuída. O volume HANA/registo/cópia de segurança não foi pensado para ser o volume de colocar as cópias de segurança da base de dados. Ele tem o tamanho a utilizar como volume de cópia de segurança para os backups de log de transação do HANA. Ver detalhes nos [SAP HANA (instâncias grandes) de elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

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


**Passos seguintes?**

- Consulte [instalação de HANA HLI](hana-example-installation.md)










































 







 




