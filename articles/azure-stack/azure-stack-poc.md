---
title: O que é o Azure Stack? | Microsoft Docs
description: Pilha do Azure permite-lhe executar serviços do Azure no seu centro de dados.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/04/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 848df59b01cc0c03b9a5f3dae2644d469c8651bb
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234891"
---
# <a name="what-is-azure-stack"></a>O que é o Azure Stack?

Pilha do Microsoft Azure é uma plataforma de nuvem híbrida que lhe permite fornecer serviços do Azure no seu centro de dados. Esta plataforma foi concebida para suportar necessidades comerciais em desenvolvimento. Pilha do Azure pode ativar cenários de novo para as suas aplicações modernas, tais como o limite e ambientes desligados, ou cumpre os requisitos específicos de segurança e conformidade.

Pilha do Azure é oferecida na duas opções de implementação para satisfazer as necessidades.

## <a name="azure-stack-integrated-systems"></a>Sistemas integrados do Azure Stack
Pilha do Azure integrados sistemas são fornecidos através de uma parceria da Microsoft e [parceiros de hardware](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), criar uma solução que oferece inovação paced de nuvem e computação simplicidade de gestão. Porque a pilha do Azure é fornecida como um integrada de hardware e sistema de software, tem a flexibilidade e o controlo que tiver, juntamente com a capacidade inovar da nuvem. Sistemas de pilha integrada do Azure no intervalo de tamanho de nós de 4-12 e jointly são suportados pela Microsoft e parceiros de hardware.  Utilize sistemas de pilha do Azure integrado para criar novos cenários e implementar novas soluções para as cargas de trabalho de produção.

## <a name="azure-stack-development-kit"></a>Development Kit do Azure Stack

Microsoft [Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-what-is.md) é uma implementação de nó único da pilha do Azure, que pode utilizar para avaliar e obter informações sobre a pilha do Azure.  Também pode utilizar ASDK como um ambiente de programação para criar aplicações com as APIs e as ferramentas que seja consistente com o Azure.

>[!Note]
>O ASDK não se destina a ser utilizado como um ambiente de produção.

O ASDK tem as seguintes limitações:

* ASDK está associado um único do Azure Active Directory (Azure AD) ou o fornecedor de identidade de serviços de Federação do Active Directory (AD FS). Pode criar vários utilizadores neste diretório e atribuir as subscrições para cada utilizador.
* Porque os componentes de pilha do Azure são implementados num computador anfitrião, estão limitados recursos físicos disponíveis para recursos de inquilino. Esta configuração não se destina à avaliação de escala ou desempenho.
* Cenários de redes estão limitados devido aos requisitos de implementação de NIC e de anfitrião único.

## <a name="next-steps"></a>Passos Seguintes

- [Funcionalidades e conceitos principais](azure-stack-key-features.md)
- [Pilha do Azure: Uma extensão do Azure (pdf)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
