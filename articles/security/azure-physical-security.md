---
title: Instalações do Azure, no local e a segurança física | Microsoft Docs
description: O artigo descreve os centros de dados do Azure, incluindo a infraestrutura física, segurança e conformidade ofertas.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: a6a9b1d6e12dabb09cde684c34481b4b3442c1b8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102469"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Instalações do Azure, no local e a segurança física
Nuvem da Microsoft é composta por um [infraestrutura de centro de dados globalmente distribuída](https://azure.microsoft.com/global-infrastructure/) suportar milhares de serviços online e a expansão de mais do que 100 altamente seguro instalações em todo o mundo.

A infraestrutura concebida para colocar as aplicações próximo utilizadores em todo o mundo, preservando residency de dados e opções de conformidade e a resiliência abrangentes para os clientes da oferta. Azure possui 52 regiões em todo o mundo e está disponível no 140 países.

Uma região é um conjunto de centros de dados que estão interligados através de uma rede de grande e resiliente. A rede incluir a distribuição de conteúdo, o balanceamento de carga, redundância e encriptação por predefinição. Com regiões mais globais do que qualquer outro fornecedor da cloud, o Azure oferece aos clientes flexibilidade para implementar aplicações onde precisam.

As regiões do Azure estão organizadas em geografias. Uma geografia do Azure garante que os requisitos de residência, de soberania, de conformidade e de resiliência dos dados são honrados dentro dos limites geográficos.

As geografias permitem aos clientes com necessidades de residência de dados específicos e conformidade aproximarem os seus dados e aplicações. Localizações geográficas são com tolerância a falhas para conseguir a falha de região completa através da respetiva ligação para a infraestrutura de rede dedicada de elevada capacidade.

As Zonas de Disponibilidade são localizações fisicamente separadas dentro de uma região do Azure. Cada Zona de Disponibilidade é composta por um ou mais datacenters equipados com energia, refrigeração e rede independentes. As Zonas de Disponibilidade permitem aos clientes executarem aplicações críticas com elevada disponibilidade e replicação de baixa latência.

A figura seguinte mostra como a infraestrutura global do Azure pares região e zonas de disponibilidade no mesmo limite de residency de dados e de elevada disponibilidade, recuperação após desastre, cópia de segurança.

![Limites de residency de dados][1]

Um grande geográfico distribuído requisitos de espaço dos centros de dados permite que a Microsoft para ser próximo de clientes para reduzir a latência de rede e permitir a cópia de segurança georredundante e ativação pós-falha.

## <a name="physical-security"></a>Segurança física
Microsoft designs, cria e funciona centros de dados de uma forma que estritamente controla o acesso físico a áreas onde os dados de cliente estão armazenados. A Microsoft compreende a importância de proteção de dados de cliente e empenhados em ajudar a proteger dos centros de dados que contêm os dados. Temos uma divisão de toda na Microsoft dedicado ao estruturação, construção e funcionamento de instalações de físicas que suportam o Azure. Este agrupamento é investido no manter a segurança física de-de-última geração.

Microsoft assume uma abordagem em camadas para a segurança física para reduzir o risco dos utilizadores não autorizados, obtendo acesso físico aos dados e os recursos de centro de dados. Gerida pela Microsoft de centros de dados têm um vasto conjunto camadas de proteção: aceder aprovação no perímetro a instalações, no perímetro a criação, dentro de criação e o piso de centro de dados. Camadas de segurança física são:

- Pedido de acesso e aprovação – tem de solicitar acesso antes de chegar ao centro de dados. Está a necessário para fornecer uma justificação de negócio válido para a sua visita, tal como a conformidade ou a fins de auditoria. Todos os pedidos são aprovados numa base necessidade para acesso por funcionários da Microsoft. Numa base necessidade para acesso ajuda a manter o número de pessoas necessárias para concluir uma tarefa nos centros de dados para o mínimo bare. Depois de permissão é concedida, um indivíduo só tem acesso à área discreta do Centro de dados com base na justificação de negócio aprovados. As permissões estão limitadas a um determinado período de tempo e expirarem após o período de tempo permitido.

- Perímetro da instalação - quando chegar à Centro de dados, tiver necessário passar por um ponto de acesso bem definidos. Normalmente, altura fences compostas steel e concreto abranger cada polegada de perímetro. Existem câmaras em torno dos centros de dados, com uma equipa de segurança os seus vídeos 24x7 e 365 dias do ano de monitorização.

- Entrada de como criar - entrada de como o Centro de dados é staffed com officers profissionais de segurança que têm sofrido rigorosas verificações de formação e em segundo plano. Estes officers segurança patrol também regularmente o Centro de dados enquanto também a monitorização de vídeos de câmaras dentro do datacenter 24x7 e 365 dias um ano.

- Dentro de edifício - depois de introduzir a criação, tem de passar autenticação de dois fatores com biometria para continuar a mover através do Centro de dados. Se a sua identidade estiver validada, pode introduzir a parte do Centro de dados que aprovou acesso. Pode ficar lá apenas para a duração de tempo aprovado.

- Piso de centro de dados –, só é permitidos para o piso que está a ser aprovados para introduzir. É necessário transmitir uma e filtragens deteção metal de corpo completa. Para reduzir o risco de dados não autorizado introduzir ou abandonar o fileparser o Centro de dados sem nossos dados de conhecimento, apenas os dispositivos aprovados podem tornar as respetivas forma para o piso de centro de dados. Além disso, o monitor de vídeo câmaras início e de cada servidor em bastidor. E filtragens de deteção metal completa do corpo é repetida ao sair de piso de centro de dados. Para deixar o Centro de dados, está a necessário para passar uma análise de segurança adicional.

Visitantes são necessários para surrender destaques após partidas das qualquer instalação da Microsoft.

## <a name="physical-security-reviews"></a>Analisa a segurança física
Revisões de segurança física das instalações de são executados periodicamente para garantir que os datacenters corretamente requisitos de segurança do Microsoft Azure de endereço. O técnico de fornecedor de alojamento de centro de dados não fornece gestão de serviço do Microsoft Azure. Técnico não dispõe de acesso de início de sessão para os sistemas do Azure ou o acesso físico para o espaço de colocalização do Azure e cages.

## <a name="data-bearing-devices"></a>Dispositivos de efeito de dados
A Microsoft utiliza os procedimentos do melhor prática e uma solução wiping que [NIST 800-88 compatíveis](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). Para unidades de disco rígido que não não possível limpar, é utilizado um processo de destruição que destroys-lo e apresenta-os a recuperação de informações impossível. Processo de destruição pode ser disintegrate, shred, pulverize ou incinerate. O meio adequado disposal é determinado pelo tipo de recurso. Registos de destruição são retidos.  

## <a name="equipment-disposal"></a>Disposal equipamento
Microsoft Azure implementa este princípio em nome dos clientes. Após fim-de-vida um sistema, técnico operacional Microsoft siga processar dados rigorosas e procedimentos de disposal de hardware para garantir que o hardware que contém dados de cliente não é disponibilizado para entidades não fidedignas. Uma abordagem Secure apagar é seguida (através de firmware da unidade de disco rígido) para unidades de que o suportam. Para unidades de disco rígido que não não possível limpar, é utilizado um processo de destruição que destroys-lo e apresenta-os a recuperação de informações impossível. Processo de destruição pode ser disintegrate, shred, pulverize ou incinerate. O meio adequado disposal é determinado pelo tipo de recurso. Registos de destruição são retidos. Todos os serviços do Microsoft Azure utilizam os serviços de gestão de armazenamento e disposal aprovados suporte de dados.

## <a name="compliance"></a>Conformidade
A infraestrutura do Azure é concebida e gerida para satisfazer um conjunto amplo de normas de conformidade internacional e específicos da indústria, tais como ISO 27001, HIPAA, FedRAMP, SOC 1 e certificados SOC 2. Normas específico do país também são cumpridas, incluindo IRAP da Austrália, RU G a nuvem e Singapura MTCS. As exigentes auditorias levadas a cabo por entidades independentes, como o British Standards Institute, comprovam que o Azure cumpre os controlos de segurança rigorosos ditados por estas normas.

Consulte o [ofertas de conformidade](https://www.microsoft.com/trustcenter/compliance/complianceofferings) para uma lista completa das normas de conformidade aderiu pelo Azure.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
