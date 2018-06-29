---
title: Segurança da infraestrutura do Azure | Microsoft Docs
description: O artigo descreve como a Microsoft garante a segurança dos nossos centros de dados do Azure.
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
ms.openlocfilehash: 313fbc0fea317e8888bf64e7f7817ab0e5c9f049
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102532"
---
# <a name="security-of-azure-infrastructure"></a>Segurança da infraestrutura do Azure
Microsoft Azure é executada nos centros de dados gerida e operada por Microsoft. Estes geograficamente centros de dados está em conformidade com as normas de chave da indústria, tais como 27001: 2013 de norma ISO/IEC e SP de NIST 800-53, para segurança e fiabilidade. Os centros de dados são geridos, monitorizados e administrados pelos funcionários de operações do Microsoft. A equipa de operações tem anos de experiência na entrega maior do online services do mundo com 24x7 continuidade.

Esta série de artigos fornece informações sobre o que faz Microsoft para proteger a infraestrutura do Azure. O endereço de artigos:

- [Segurança física](azure-physical-security.md)
- [Disponibilidade](azure-infrastructure-availability.md)
- [Os componentes e limites](azure-infrastructure-components.md)
- [Arquitetura de rede](azure-infrastructure-network.md)
- [Rede de produção](azure-production-network.md)
- [Base de Dados SQL](azure-infrastructure-sql.md)
- [Operações](azure-infrastructure-operations.md)
- [Monitorização](azure-infrastructure-monitoring.md)
- [Integridade](azure-infrastructure-integrity.md)
- [Proteção de dados](azure-protection-of-customer-data.md)

## <a name="shared-responsibility-model"></a>Modelo de responsabilidade partilhado
É importante compreender a divisão de responsabilidade entre si e a Microsoft. No local, que é proprietário da pilha de toda, mas como mover para a nuvem algumas responsabilidades transferir para o Microsoft. A matriz de responsabilidade seguinte mostra as áreas da pilha de num software como serviço (SaaS), plataforma como um serviço (PaaS) e a infraestrutura como serviço (IaaS) implementação que é responsável pela Microsoft é responsável por.

![Responsabilidade partilhada][1]

Responsabilidades são sempre mantidas por si, independentemente do tipo de implementação, são:

- Dados
- Pontos Finais
- Conta
- Gestão de acesso

Lembre-se de que compreende a divisão de responsabilidade entre si e a Microsoft numa implementação de SaaS, IaaS e PaaS. Consulte [partilhado responsabilidades de informática em nuvem](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) para obter mais detalhes.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
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
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
