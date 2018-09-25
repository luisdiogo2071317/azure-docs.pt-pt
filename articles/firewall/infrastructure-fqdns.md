---
title: Infraestrutura FQDN para o Firewall do Azure
description: Saiba mais sobre a infraestrutura de FQDNs no Firewall do Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 1369a82829b2c80768d746ba92daf2482c1fd7f8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994144"
---
# <a name="infrastructure-fqdns"></a>FQDNs de infraestrutura

O Azure Firewall inclui uma coleção de regras incorporadas para os FQDNs de infraestrutura que são permitidos por predefinição. Estes FQDNs são específicos da plataforma e não podem ser utilizados para outros fins. 

Os serviços seguintes estão incluídos na coleção de regra interna:

- Acesso ao repositório de imagem de plataforma (PIR) de armazenamento de computação
- Acesso de armazenamento de estado de discos geridos
- Diagnóstico do Azure e o registo (MDS)
- Azure Active Directory

## <a name="overriding"></a>Substituir 

Pode substituir esta coleção de regras de infra-estrutura interna através da criação de negação de todos os coleção de regras de aplicação é processada pela última vez. Será sempre processada antes da coleção de regras de infraestrutura. Qualquer item que não esteja na coleção de regras de infraestrutura é negado por predefinição.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [implementar e configurar uma Firewall de Azure](tutorial-firewall-deploy-portal.md).