---
title: Resolver problemas de vistas de custo de enterprise do Azure | Documentos da Microsoft
description: Saiba como resolver quaisquer problemas que poderá ter com exibições de custo organizacional no portal do Azure.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2017
ms.author: cwatson
ms.openlocfilehash: 853307f24574e6cea5841bcace815fea2bbcf2c8
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47390669"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Resolver problemas de vistas de custos empresariais 

Dentro de inscrições de empresas, existem várias definições que podem fazer com que os utilizadores dentro da inscrição não conseguir ver os custos.  Estas definições são geridas pelo administrador de inscrição ou pelo parceiro, se a inscrição não está a ser adquirida diretamente com a Microsoft.  Este artigo ajuda-o a compreender quais são as definições e seu impacto a inscrição. Estas definições são independentes da [funções de RBAC do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 


## <a name="enabling-access-to-costs"></a>Ativar o acesso aos custos

Está a vê uma mensagem não autorizado, ou *"as vistas de custo estão desativadas na sua inscrição".* Quando está à procura de informações de custo? ![não autorizado](media/billing-enterprise-mgmt-groups/unauthorized.png)

Tal poderá dever-se um dos seguintes motivos:

1. Comprou o Azure através de um parceiro de enterprise e o parceiro ainda não lançadas preços ainda. Para disponibilizar os preços, contacte o seu parceiro para atualizar a definição dentro de [Enterprise portal](https://ea.azure.com).
2. Em alternativa, se for um cliente com contrato EA direto, existem duas possibilidades:
    * For um proprietário da conta e o administrador de inscrição desativou as "AO modo de exibição cobranças" definição.  
    * É um administrador do departamento e o administrador de inscrição desativou as "DA vista cobranças" definição.
    * Contacte o administrador de inscrição para obter acesso. O administrador de inscrição pode visitar o [Enterprise portal](https://ea.azure.com/manage/enrollment) e atualizar a definição, como visto aqui:

![Definições do Portal Enterprise](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Recurso não está disponível? 
Se estiver a receber uma mensagem de erro "este recurso está indisponível" quando tentar aceder a um grupo de gestão ou de subscrição, em seguida, não tem a função correta para ver este item.  

![recurso não encontrado](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Entre em contato com a administrar dos grupos de gestão ou de subscrição para obter acesso.  
* Para subscrições, veja [controlo de acesso de controlo (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) documento para obter ajuda no qual a função é necessária.
