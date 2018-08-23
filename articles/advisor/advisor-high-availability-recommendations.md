---
title: Recomendações de elevada disponibilidade do Assistente do Azure | Documentos da Microsoft
description: Utilize o Assistente do Azure para melhorar a elevada disponibilidade das suas implementações do Azure.
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: makohli
ms.openlocfilehash: 7bd0737e7fb26af95eed63696d1ac07c88a9dec4
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054132"
---
# <a name="advisor-high-availability-recommendations"></a>Recomendações de elevada disponibilidade do Assistente

O Assistente do Azure ajuda-Certifique-se e melhorar a continuidade das suas aplicações críticas para a empresa. É possível obter recomendações de alta disponibilidade por Advisor a partir da **elevada disponibilidade** separador do dashboard do Advisor.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Certifique-se a tolerância a falhas de máquina virtual

Para fornecer redundância à aplicação, é recomendável agrupar duas ou mais máquinas virtuais num conjunto de disponibilidade. Advisor identifica as máquinas virtuais que não fazem parte de um conjunto de disponibilidade e recomenda movendo-as para um conjunto de disponibilidade. Esta configuração garante que, durante a um evento de manutenção planeada ou, pelo menos uma máquina virtual está disponível e cumpre o SLA de máquinas virtuais do Azure. Pode escolher para criar o conjunto de disponibilidade para a máquina virtual ou para adicionar a máquina virtual a um conjunto de disponibilidade existente.

> [!NOTE]
> Se optar por criar um conjunto de disponibilidade, tem de adicionar mais, pelo menos, uma máquina virtual na mesma. Recomendamos que agrupa dois ou mais máquinas virtuais num disponibilidade definida para se certificar de que a máquina, pelo menos, um está disponível durante um período de indisponibilidade.

## <a name="ensure-availability-set-fault-tolerance"></a>Certifique-se de que a tolerância a falhas de conjunto de disponibilidade 

Para fornecer redundância à aplicação, é recomendável agrupar duas ou mais máquinas virtuais num conjunto de disponibilidade. O assistente identifica os conjuntos de disponibilidade que contenham uma única máquina virtual e recomenda adicionar uma ou mais máquinas virtuais ao mesmo. Esta configuração garante que, durante a um evento de manutenção planeada ou, pelo menos uma máquina virtual está disponível e cumpre o SLA de máquinas virtuais do Azure. Pode escolher para criar uma máquina virtual ou para adicionar uma máquina virtual existente para o conjunto de disponibilidade.  

## <a name="ensure-application-gateway-fault-tolerance"></a>Certifique-se a tolerância a falhas de gateway de aplicação
Para garantir a continuidade de negócio de aplicativos de missão crítica que têm a tecnologia gateways de aplicação, o Advisor identifica instâncias de gateway de aplicação que não estão configuradas para tolerância a falhas e sugere ações de remediação que pode efetuar. Advisor identifica os gateways de aplicação de instância única de médias ou grandes, e ele recomenda adicionar pelo menos uma instância mais. Ele também identifica instance único ou vários gateways de aplicação pequenos e recomenda a migração para médias ou grandes SKUs. Advisor recomenda estas ações para garantir que as instâncias de gateway de aplicação estão configuradas para satisfazer os requisitos de SLA atuais para estes recursos.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Melhorar o desempenho e fiabilidade dos discos da máquina virtual

Advisor identifica as máquinas virtuais com discos standard e recomenda a atualização para os discos premium.
 
Armazenamento Premium do Azure fornece suporte de discos de elevado desempenho e de baixa latência para máquinas virtuais que executam cargas de trabalho de e/S intensivas. Discos da máquina virtual que utilizem contas de armazenamento premium armazenam dados em unidades de estado sólido (SSDs). Para obter o melhor desempenho para a sua aplicação, recomendamos que migre qualquer discos de máquinas virtuais que requerem elevado IOPS para o armazenamento premium. 

Se os seus discos não necessitam de IOPS elevado, pode limitar os custos mantendo-os no armazenamento standard. Armazenamento Standard armazena dados de disco de máquina virtual em unidades de disco rígido (HDDs) em vez do SSDs. Pode optar por migrar discos da máquina virtual para discos premium. Os discos Premium são suportados na máquina de virtual a maioria dos SKUs. No entanto, em alguns casos, se pretender utilizar os discos premium, precisará atualizar a sua máquina virtual SKUs também.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Proteger os dados de máquina virtual de eliminação acidental

Configurar a cópia de segurança da máquina virtual garante a disponibilidade dos seus dados críticos e oferece proteção contra eliminação ou corrupção acidental.  Advisor identifica as máquinas virtuais em que a cópia de segurança não está ativada, e ele recomenda a ativação de cópia de segurança. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Certifique-se de que tem acesso a especialistas de cloud do Azure, sempre que precisar

Ao executar uma carga de trabalho críticas para a empresa, é importante ter acesso ao suporte técnico quando necessário. Advisor identifica potenciais subscrições empresariais críticas que não têm suporte técnico incluído no seu plano de suporte e recomenda a atualização para uma opção que inclui suporte técnico.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Criar alertas do Azure Service Health para ser notificado quando é afetado por problemas do Azure

Recomendamos a configuração de alertas do Azure Service Health para ser notificado quando é afetado por problemas de serviço do Azure. [O Azure Service Health](https://azure.microsoft.com/features/service-health/) é um serviço gratuito que fornece orientação e suporte personalizados quando são afetados por um problema de serviço do Azure. O assistente identifica as subscrições que não tem alertas configurados e recomenda a criação de um.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Configurar pontos finais do Gestor de tráfego para resiliência

Perfis do Gestor de tráfego com mais de um ponto final experiência maior disponibilidade, se falhar de qualquer ponto de extremidade. Colocar os pontos finais em diferentes regiões ainda mais melhora a fiabilidade do serviço. Advisor identifica os perfis de tráfego Manager onde há apenas um ponto final e recomenda adicionar pelo menos um ponto de extremidade mais noutra região.

Se todos os pontos finais num perfil do Gestor de tráfego que está configurado para o encaminhamento de proximidade estiverem na mesma região, os utilizadores a partir de outras regiões podem ocorrem atrasos de ligação. Adicionar ou mover um ponto de extremidade para outra região irá melhorar o desempenho geral e proporcionar melhor disponibilidade se falharem de todos os pontos finais numa única região. Advisor identifica os perfis do Gestor de tráfego configurados para o encaminhamento de proximidade onde todos os pontos finais estão na mesma região e recomenda adicionar ou mover um ponto de extremidade para outra região do Azure.

Se um perfil do Gestor de tráfego estiver configurado para encaminhamento geográfico, em seguida, o tráfego é encaminhado para pontos finais com base em regiões definidas. Se uma região falhar, não há nenhum predefinida de ativação pós-falha. Ter um ponto de extremidade em que o agrupamento Regional está configurado para "Todos (World)", evitar o tráfego a ser removido e melhorar a disponibilidade do serviço. Advisor identifica configurados para encaminhamento geográfico onde não existe nenhum ponto final configurado para ter o agrupamento Regional como "Todos (World)" e recomenda fazer essa alteração de configuração de perfis do Gestor de tráfego.

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Como acessar as recomendações de elevada disponibilidade no Advisor

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2.  No dashboard do assistente, clique nas **elevada disponibilidade** separador.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as recomendações do assistente, consulte:
* [Introdução ao Assistente do Azure](advisor-overview.md)
* [Introdução ao Assistente](advisor-get-started.md)
* [Recomendações de custos do Assistente](advisor-performance-recommendations.md)
* [Recomendações de desempenho do Assistente](advisor-performance-recommendations.md)
* [Recomendações de segurança do Advisor](advisor-security-recommendations.md)

