---
title: "Proteger uma implementação de aplicação multicamada SAP NetWeaver utilizando o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como proteger o SAP NetWeaver implementações de aplicações utilizando o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: b6ab734186f23d51d60e51bd0946329d5209097b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
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

Neste artigo, utilizamos um cenário de recuperação de desastre do Azure para o Azure para demonstrar as capacidades de recuperação de desastre do SAP da recuperação do Site. Porque a replicação do Site Recovery não está específicas da aplicação, o processo que é descrito deverá também se aplicam a outros cenários.

### <a name="required-foundation-services"></a>Serviços necessários foundation
No cenário que vamos discutir neste artigo, são implementados os seguintes serviços de foundation:
* Gateway VPN do ExpressRoute do Azure ou do Azure
* Pelo menos um controlador de domínio do Active Directory e servidor DNS, em execução no Azure

Recomendamos que estabeleça esta infraestrutura antes de implementar a recuperação de Site.

## <a name="typical-sap-application-deployment"></a>Implementação de aplicação de SAP típica
Normalmente, implementar clientes SAP grande entre 6 e 20 aplicações SAP individuais. A maioria destas aplicações baseiam-se dos motores de SAP NetWeaver ABAP ou Java. Muitos motores de autónomo mais pequenos e específico não - NetWeaver SAP e, normalmente, algumas aplicações de SAP não suportam estas aplicações de NetWeaver core.  

É fundamental para inventariar todas as aplicações de SAP que estejam a executar no seu ambiente. Em seguida, determine o modo de implementação (camada de dois ou três camadas), versões, patches, tamanhos, taxas de volume de alterações e requisitos de persistência de disco.

![Diagrama de um padrão de implementação normal do SAP](./media/site-recovery-sap/sap-typical-deployment.png)

Proteger a camada de persistência de base de dados SAP utilizando ferramentas nativas de DBMS, tais como a replicação do sistema de AlwaysOn do SQL Server, Oracle Data Guard ou SAP HANA. Como a camada de base de dados SAP, a camada de cliente não está protegida pela recuperação de sites. É importante a ter em consideração os fatores que afetam esta camada. Fatores incluem o atraso de propagação de DNS, segurança e acesso remoto para o Centro de dados de recuperação após desastre.

Recuperação de site é a solução recomendada para a camada de aplicação, incluindo para SAP SCS e ASCS. Outras aplicações, tais como aplicações SAP não NetWeaver e aplicações não SAP, formam parte do ambiente geral da implementação SAP. Deve protegê-las com a recuperação de Site.

## <a name="replicate-virtual-machines"></a>Replicar máquinas virtuais
Para iniciar a replicação de todas as SAP aplicação máquinas virtuais para o Centro de dados de recuperação de desastre do Azure, siga as orientações no [replicar uma máquina virtual para o Azure](azure-to-azure-walkthrough-enable-replication.md).

Se utilizar um endereço IP estático, pode especificar o endereço IP que pretende que a máquina virtual a tomar. Para definir o endereço IP, aceda a **computação e rede definições** > **cartão de interface de rede**.

![Captura de ecrã que mostra como definir um endereço IP privado no painel de cartão de interface de rede de recuperação de Site](./media/site-recovery-sap/sap-static-ip.png)

## <a name="create-a-recovery-plan"></a>Criar um plano de recuperação
Um plano de recuperação suporta a sequenciação de várias camadas numa aplicação multicamada durante uma ativação pós-falha. Sequenciação ajuda a manter a consistência da aplicação. Quando cria um plano de recuperação para uma aplicação web de várias camadas, concluir os passos descritos na [criar um plano de recuperação utilizando a recuperação de Site](site-recovery-create-recovery-plans.md).

### <a name="add-scripts-to-the-recovery-plan"></a>Adicione scripts ao plano de recuperação
Para as suas aplicações funcionar corretamente, poderá ter de fazer algumas operações em máquinas virtuais do Azure após a ativação pós-falha ou durante uma ativação pós-falha de teste. Pode automatizar algumas operações de pós-falha. Por exemplo, pode atualizar a entrada DNS e alterar os vínculos e ligações adicionando scripts correspondentes para o plano de recuperação.

### <a name="dns-update"></a>Atualização de DNS
Se o DNS está configurado para a atualização dinâmicas DNS, máquinas virtuais atualizar, normalmente, o DNS com o novo endereço IP, quando são iniciados. Se pretender adicionar um passo explícito para atualizar o DNS com os novos endereços IP das máquinas virtuais, adicione um [script para atualizar o endereço IP no DNS](https://aka.ms/asr-dns-update) como uma ação de pós-falha nos grupos do plano de recuperação.  

## <a name="example-azure-to-azure-deployment"></a>Exemplo de implementação do Azure para o Azure
O diagrama seguinte mostra o cenário de recuperação de desastre de recuperação de sites do Azure para o Azure:

![Diagrama de um cenário de replicação do Azure para o Azure](./media/site-recovery-sap/sap-replication-scenario.png)

* O Centro de dados primário está em Singapura (Azure Sul-Oriental). O Centro de dados de recuperação de desastre é RAE de Hong Kong (Azure Oriental). Neste cenário, elevada disponibilidade local é fornecida por duas VMs que executam o AlwaysOn do SQL Server no modo síncrono no Singapura.
* A partilha de ficheiros SAP ASCS fornece elevada disponibilidade para os pontos únicos de SAP da falha. A partilha de ficheiros ASCS não necessita de um disco partilhado de cluster. Aplicações como SIOS não são necessárias.
* Proteção de recuperação de desastres para a camada DBMS é conseguida utilizando a replicação assíncrona.
* Este cenário mostra "recuperação de desastre simétrico". Esta duração descreve uma solução de recuperação de desastre é uma réplica exata de produção. A recuperação de desastre solução SQL Server tem elevada disponibilidade. Recuperação após desastre simétrico não é obrigatória para a camada de base de dados. Muitos clientes tirar partido da flexibilidade de implementações de nuvem para criar rapidamente um nó de elevada visibilidade local após um evento de recuperação após desastre.
* O diagrama ilustra o SAP NetWeaver ASCS e a camada de servidor de aplicação replicado pela recuperação de sites.

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
