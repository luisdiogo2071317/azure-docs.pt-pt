---
title: Proteger uma implementação de aplicação multicamada SAP NetWeaver utilizando o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como proteger o SAP NetWeaver implementações de aplicações utilizando o Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: asgang
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2018
ms.author: asgang
ms.openlocfilehash: 27dfdec4e833a2f30963157ba2f4d95232e21270
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267337"
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-by-using-site-recovery"></a>Proteger uma implementação de aplicação multicamada SAP NetWeaver utilizando a recuperação de Site

A maioria das implementações de grande tamanho e tamanho médio SAP utilizar alguma forma de solução de recuperação após desastre. A importância das soluções de recuperação de desastre robusto e testable aumentou como processos de negócio mais núcleos são movidos para aplicações como o SAP. O Azure Site Recovery foi testado e integrado com aplicações SAP. Recuperação de site excede as capacidades da maioria das soluções de recuperação de desastre no local e num menor custo total de propriedade (TCO) que soluções concorrentes.

Com a recuperação de Site, pode:
* **Ativar a proteção do SAP NetWeaver e não NetWeaver aplicações de produção que são executadas no local** através da replicação de componentes para o Azure.
* **Ativar a proteção do SAP NetWeaver e não NetWeaver aplicações de produção que são executadas no Azure** através da replicação de componentes para outro datacenter do Azure.
* **Simplificar a migração para a nuvem** utilizando a recuperação de sites para migrar a implementação de SAP para o Azure.
* **Simplificar a atualizações de projeto SAP, teste e fazer o protótipo** criando uma produção a pedido para testar aplicações SAP do clone.

Este artigo descreve como proteger o SAP NetWeaver implementações de aplicações utilizando [do Azure Site Recovery](site-recovery-overview.md). O artigo aborda as melhores práticas para proteger uma implementação de SAP NetWeaver de três camadas no Azure através da replicação para outro datacenter do Azure utilizando a recuperação de Site. Descreve os cenários suportados e configurações e como executar as ativações pós-falha de teste (simulações de recuperação após desastre) e as ativações pós-falha real.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, certifique-se de que sabe como efetuar as seguintes tarefas:

* [Replicar uma máquina virtual para o Azure](azure-to-azure-walkthrough-enable-replication.md)
* [Estruturar uma rede de recuperação](site-recovery-azure-to-azure-networking-guidance.md)
* [Efetue uma ativação pós-falha de teste para o Azure](azure-to-azure-walkthrough-test-failover.md)
* [Efetue uma ativação pós-falha para o Azure](site-recovery-failover.md)
* [Replicar um controlador de domínio](site-recovery-active-directory.md)
* [Replicação do SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Cenários suportados
Pode utilizar a recuperação de Site para implementar uma solução de recuperação de desastres nos seguintes cenários:
* Sistemas SAP com um centro de dados do Azure que são replicados para outro datacenter do Azure (recuperação de desastres do Azure para o Azure). Para obter mais informações, consulte [arquitetura da replicação do Azure para o Azure](https://aka.ms/asr-a2a-architecture).
* Sistemas SAP em execução no VMware (ou físico) servidores no local que replicar para um site de recuperação após desastre num datacenter do Azure (recuperação de desastres do VMware para o Azure). Este cenário requer alguns componentes adicionais. Para obter mais informações, consulte [arquitetura de replicação de VMware para o Azure](https://aka.ms/asr-v2a-architecture).
* Sistemas SAP em execução no Hyper-V no local que replica para um site de recuperação após desastre num datacenter do Azure (recuperação de desastres do Hyper-V-para o Azure). Este cenário requer alguns componentes adicionais. Para obter mais informações, consulte [arquitetura de replicação de Hyper-V para Azure](https://aka.ms/asr-h2a-architecture).

Neste artigo, utilizamos uma **do Azure para o Azure** cenário de recuperação de desastres para demonstrar as capacidades de recuperação de desastre do SAP da recuperação do Site. Porque a replicação do Site Recovery não está específicas da aplicação, o processo que é descrito deverá também se aplicam a outros cenários.

### <a name="required-foundation-services"></a>Serviços necessários foundation
No cenário que vamos discutir neste artigo, são implementados os seguintes serviços de foundation:
* Gateway VPN do ExpressRoute do Azure ou do Azure
* Pelo menos um controlador de domínio do Active Directory e servidor DNS, em execução no Azure

Recomendamos que estabeleça esta infraestrutura antes de implementar a recuperação de Site.

## <a name="reference-sap-application-deployment"></a>Implementação de aplicação de SAP de referência

Esta arquitetura de referência mostra executado SAP NetWeaver num ambiente Windows no Azure com elevada disponibilidade.  Esta arquitetura é implementada com tamanhos de máquina virtual específica (VM) que podem ser alterados para acomodar as necessidades da sua organização.

![Diagrama de um padrão de implementação normal do SAP](./media/site-recovery-sap/reference_sap.png)

## <a name="disaster-recovery-considerations"></a>Considerações sobre a recuperação após desastre

Recuperação após desastre (DR), tem de ser capazes de efetuar a ativação pós-falha para uma região secundária. Cada camada utiliza uma estratégia diferente para fornecer proteção de recuperação após desastre (DR).

#### <a name="vms-running-sap-web-dispatcher-pool"></a>VMs com o conjunto de emissores de Web de SAP 
O componente de emissor Web é utilizado como um balanceador de carga para tráfego SAP entre os servidores de aplicações SAP. Para alcançar a elevada disponibilidade para o componente de emissor da Web, o Balanceador de carga do Azure é utilizado para implementar o programa de configuração Web Dispatcher paralelo numa configuração round robin para distribuição de tráfego HTTP (S) entre o Web Dispatchers disponível no agrupamento de Balanceador de. Isto será replicado utilizando o Azure Recovery (ASR) de Site e scripts de automatização serão utilizados para configurar o Balanceador de carga na região de recuperação após desastre. 

####<a name="vms-running-application-servers-pool"></a>VMs com o agrupamento de servidores de aplicação
Para gerir grupos de início de sessão ABAP para servidores de aplicações, a transação SMLG é utilizada. Utiliza a função no servidor dos serviços Central mensagem de balanceamento de carga para distribuir a carga de trabalho entre o agrupamento de servidores de aplicação de SAP para SAPGUIs e RFC tráfego. Isto será replicado utilizando o Azure Site Recovery 

####<a name="vms-running-sap-central-services-cluster"></a>VMs do cluster de serviços Central SAP em execução
Esta arquitetura de referência é executada serviços Central em VMs na camada de aplicação. Os serviços Central é um potencial ponto único de falha (SPOF) quando implementado numa única VM — implementação típica, quando a elevada disponibilidade não é um requisito.<br>

Para implementar uma solução de elevada disponibilidade, pode ser utilizado um cluster de disco partilhado ou de um cluster de partilha de ficheiros. Para configurar VMs para um cluster de disco partilhado, utilize o Cluster de ativação pós-falha do Windows Server. Testemunho de nuvem é recomendado como um testemunho de quórum. 
 > [!NOTE]
 > O Azure Site Recovery não replicar o testemunho de nuvem, por isso, recomenda-se para implementar o testemunho da nuvem na região de recuperação após desastre.

Para suportar o ambiente de cluster de ativação pós-falha, [SIOS DataKeeper Cluster Edition](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) executa a função de volume partilhado de cluster através da replicação de discos independentes pertencentes a nós de cluster. Azure não suporta nativamente discos partilhados e, por conseguinte, requer soluções fornecidas pelo SIOS. 

Outra forma de lidar com clustering é implementar um cluster de partilha de ficheiros. [SAP](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) alterou recentemente o padrão de implementação de serviços Central para acesso os diretórios globais /sapmnt através de um caminho UNC. Esta alteração remove a necessidade de SIOS ou outras soluções de disco partilhado nas VMs serviços Central. Ainda é recomendado para se certificar de que a partilha UNC /sapmnt está altamente disponível. Isto pode ser feito na instância serviços Central, utilizando o Cluster de ativação pós-falha do Windows Server com escala enviados ficheiros servidor (SOFS) e a funcionalidade espaços de armazenamento direto (S2D) no Windows Server 2016. 
 > [!NOTE]
 > Atualmente o suporte do Azure Site Recovery falhas apenas ponto consistente de replicação de máquinas virtuais utilizando direta de espaços de armazenamento 


## <a name="disaster-recovery-considerations"></a>Considerações sobre a recuperação após desastre

Pode utilizar o Azure Site Recovery para orquestrar a ativação através da implementação de SAP completa em regiões do Azure.
Seguem-se os passos para configurar a recuperação de desastre 

1. Replicar máquinas virtuais 
2. Estruturar uma rede de recuperação
3.  Replicar um controlador de domínio
4.  Replicar a camada de base de dados 
5.  Efetue uma ativação pós-falha de teste 
6.  Efetue uma ativação pós-falha 

Segue-se a recomendação para recuperação após desastre de cada camada utilizada neste exemplo. 

 **Camadas SAP** | **Recomendação**
 --- | ---
**Conjunto de emissores de Web SAP** |  Replicar com a recuperação de Site 
**Agrupamento de servidores de aplicações SAP** |  Replicar com a recuperação de Site 
**Cluster de serviços Central SAP** |  Replicar com a recuperação de Site 
**Máquinas virtuais do Active Directory** |  Replicação do Active Directory 
**Servidores de base de dados do SQL Server** |  SQL Server Always On nos replicação

##<a name="replicate-virtual-machines"></a>Replicar máquinas virtuais

Para iniciar a replicação de todas as SAP aplicação máquinas virtuais para o Centro de dados de recuperação de desastre do Azure, siga as orientações no [replicar uma máquina virtual para o Azure](azure-to-azure-walkthrough-enable-replication.md).


* Para obter orientações sobre a proteger o Active Directory e DNS, consulte [proteger o Active Directory e DNS](site-recovery-active-directory.md) documento.

* Para obter orientações sobre a proteção de camada de base de dados em execução no SQL server, consulte [proteger o SQL Server](site-recovery-active-directory.md) documento.

## <a name="networking-configuration"></a>Configuração de redes

Se utilizar um endereço IP estático, pode especificar o endereço IP que pretende que a máquina virtual a tomar. Para definir o endereço IP, aceda a **computação e rede definições** > **cartão de interface de rede**.

![Captura de ecrã que mostra como definir um endereço IP privado no painel de cartão de interface de rede de recuperação de Site](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Criar um plano de recuperação
Um plano de recuperação suporta a sequenciação de várias camadas numa aplicação multicamada durante uma ativação pós-falha. Sequenciação ajuda a manter a consistência da aplicação. Quando cria um plano de recuperação para uma aplicação web de várias camadas, concluir os passos descritos na [criar um plano de recuperação utilizando a recuperação de Site](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>A adicionar máquinas virtuais para grupos de ativação pós-falha

1.  Crie um plano de recuperação ao adicionar o servidor de aplicações, o emissor de web e a serviços SAP Central VMs.
2.  Clique em Personalizar para agrupar as VMs. Por predefinição, todas as VMs fazem parte do 'Grupo 1'.



### <a name="add-scripts-to-the-recovery-plan"></a>Adicione scripts ao plano de recuperação
Para as suas aplicações funcionar corretamente, poderá ter de fazer algumas operações em máquinas virtuais do Azure após a ativação pós-falha ou durante uma ativação pós-falha de teste. Pode automatizar algumas operações de pós-falha. Por exemplo, pode atualizar a entrada DNS e alterar os vínculos e ligações adicionando scripts correspondentes para o plano de recuperação.


Pode implementar os scripts de Azure Site Recovery frequentemente utilizados na sua conta de automatização clicando no botão 'Implementar no Azure' abaixo. Quando estiver a utilizar qualquer script publicado, certifique-se de que siga as orientações no script.

[![Implementar no Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Adicione um script de pré-ação para 'Grupo 1' ao grupo de disponibilidade de SQL de ativação pós-falha. Utilize o script de 'ASR-SQL-FailoverAG' publicado nos scripts de exemplo. Certifique-se de que siga as orientações no script e efetue as alterações necessárias no script adequadamente.
2. Adicionar um script de ação de post para anexar um balanceador de carga a falha na ativação pós-falha de máquinas virtuais da camada Web (grupo 1). Utilize o script de 'ASR AddSingleLoadBalancer' publicado nos scripts de exemplo. Certifique-se de que siga as orientações no script e efetue as alterações necessárias no script adequadamente.

![Plano de recuperação do SAP](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

1.  No portal do Azure, selecione o Cofre dos serviços de recuperação.
2.  Selecione o plano de recuperação que criou para aplicações SAP.
3.  Selecione **ativação pós-falha de teste**.
4.  Para iniciar o processo de ativação pós-falha de teste, selecione o ponto de recuperação e a rede virtual do Azure.
5.  Quando o ambiente secundário, realizar validações.
6.  Quando validações estiverem concluídas, para limpar o ambiente de ativação pós-falha, selecione **ativação pós-falha de teste de limpeza**.

Para obter mais informações, consulte [ativação pós-falha de teste para o Azure no Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Executar uma ativação pós-falha

1.  No portal do Azure, selecione o Cofre dos serviços de recuperação.
2.  Selecione o plano de recuperação que criou para aplicações SAP.
3.  Selecione **ativação pós-falha**.
4.  Para iniciar o processo de ativação pós-falha, selecione o ponto de recuperação.

Para obter mais informações, consulte [ativação pós-falha na recuperação de Site](site-recovery-failover.md).

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre como criar uma solução de recuperação após desastre para as implementações do SAP NetWeaver utilizando a recuperação de Site, consulte o documento técnico transferível [SAP NetWeaver: criar uma solução de recuperação após desastre com o Azure Site Recovery](http://aka.ms/asr-sap). O documento técnico aborda as recomendações para várias arquiteturas SAP, apresenta uma lista de aplicações suportadas e tipos VM para SAP no Azure e descreve as opções do plano de teste para a sua solução de recuperação após desastre.
* Saiba mais sobre [replicar outras cargas de trabalho](site-recovery-workload.md) utilizando a recuperação de Site.
