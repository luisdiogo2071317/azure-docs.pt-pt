---
title: Orientações de recuperação de desastre para o Azure Data Lake Store | Microsoft Docs
description: Orientações sobre a recuperação após desastre para o Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 9a2705a2372ea4f2fbea1e27bf675679dfda28f8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="disaster-recovery-guidance-for-data-in-data-lake-store"></a>Orientações de recuperação de desastre para os dados no Data Lake Store

O Azure Data Lake Store fornece armazenamento localmente redundante (LRS). Por conseguinte, os dados na sua conta do Azure Data Lake Store são sejam resilientes a falhas de hardware transitórias numa região através de réplicas automatizadas. Isto garante durabilidade e elevada disponibilidade, cumprindo o SLA do Azure Data Lake Store. Este artigo fornece orientação sobre como proteger ainda mais os seus dados de falhas de toda a região raras ou eliminações acidentais.

## <a name="disaster-recovery-guidance"></a>Documentação de orientação da recuperação após desastre
É fundamental para cada cliente preparar o seu próprio plano de recuperação após desastre. Ler as informações neste artigo para criar o plano de recuperação após desastre. Eis alguns recursos que o podem ajudar a criar o seu plano.

* [Recuperação após desastre e elevada disponibilidade para aplicações do Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Orientações técnicas sobre resiliência do Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Melhores práticas
Recomendamos que copie os dados críticos para outra conta do Data Lake Store noutra região com uma frequência alinhada com as necessidades do seu plano de recuperação após desastre. Existem vários métodos para copiar dados, incluindo [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) ou [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). O Azure Data Factory é um serviço útil para criar e implementar pipelines de movimento de dados periodicamente.

Se ocorrer uma indisponibilidade regional, pode aceder aos seus dados na região onde os dados foram copiados. Pode monitorizar o [Dashboard do Estado de Funcionamento do Serviço do Azure](https://azure.microsoft.com/status/) para determinar o estado do serviço do Azure em todo o mundo.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Orientações sobre a recuperação após danos em dados ou eliminação acidental
Embora o Azure Data Lake Store forneça resiliência de dados através de réplicas automatizadas, isto não impede que a aplicação (ou os programadores/utilizadores) danifique dados ou os elimine acidentalmente.

### <a name="best-practices"></a>Melhores práticas
Para impedir a eliminação acidental, recomendamos que defina primeiro as políticas de acesso corretas para a sua conta do Data Lake Store.  Isto inclui aplicar [bloqueios de recursos do Azure](../azure-resource-manager/resource-group-lock-resources.md) para bloquear recursos importantes, bem como aplicar controlo de acesso de nível de ficheiro e da conta, através de [funcionalidades de segurança do Data Lake Store](data-lake-store-security-overview.md) disponíveis. Também recomendamos que crie regularmente cópias dos dados críticos com [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) ou [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) noutra conta do Data Lake Store, pasta ou subscrição do Azure.  Esta ação pode ser utilizada para recuperar de um incidente de eliminação ou danos em dados. O Azure Data Factory é um serviço útil para criar e implementar pipelines de movimento de dados periodicamente.

As organizações também podem ativar o [registo de diagnósticos](data-lake-store-diagnostic-logs.md) na respetiva conta do Azure Data Lake Store para recolher registos de auditoria de acesso a dados que fornecem informações sobre quem poderá ter eliminado ou atualizado um ficheiro.

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Secure data in Data Lake Store (Proteger dados no Data Lake Store)](data-lake-store-secure-data.md)

