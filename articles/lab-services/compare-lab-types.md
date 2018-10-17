---
title: Comparar diferentes tipos de laboratórios no Azure Lab Services | Microsoft Docs
description: Explica e compara os diferentes tipos de laboratórios que pode criar com o Azure Lab Services
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 8cf779f203850ca03942ba2395baf07412712610
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092974"
---
# <a name="compare-managed-labs-in-azure-lab-services-and-devtest-labs"></a>Comparar laboratórios geridos no Azure Lab Services e DevTest Labs
Pode criar dois tipos de laboratórios, **laboratórios geridos** com o Azure Lab Services, e **laboratórios personalizados** com o Azure DevTest Labs. Se apenas quiser introduzir o que precisa num laboratório e permitir que o serviço configure e faça a gestão da infraestrutura necessária para o laboratório, escolha um dos **laboratórios geridos**. Atualmente, o **laboratório de sala de aula** é o único tipo de laboratório gerido que pode criar com o Azure Lab Services. Se quiser gerir a sua própria infraestrutura, crie um laboratório com o Azure DevTest Labs.

As seguintes secções fornecem mais detalhes sobre estes laboratórios. 

## <a name="managed-lab-types"></a>Tipos de laboratório geridos
O Azure Lab Services permite-lhe criar laboratórios cuja infraestrutura é gerida pelo Azure. Este artigo refere-se a eles como laboratórios geridos. Os laboratórios geridos oferecem diferentes tipos de laboratórios que se adequam às suas necessidades específicas. Atualmente, o único tipo de laboratório gerido suportado é o **laboratório de sala de aula**. 

Os laboratórios geridos permitem-lhe começar imediatamente, com configuração mínima. O próprio serviço processa toda a gestão da infraestrutura do laboratório, desde gerar as VMs até processar os erros e dimensionar a infraestrutura. Para criar um laboratório gerido, por exemplo, um laboratório de sala de aula, tem de criar uma conta de laboratório para a sua organização. A conta de laboratório serve como a conta central em que são geridos todos os laboratórios na organização. 

Quando cria e utiliza recursos do Azure nestes laboratórios geridos, o serviço cria e gere recursos nas subscrições internas da Microsoft. Não são criados na sua própria subscrição do Azure. O serviço mantém um registo da utilização destes recursos nas subscrições internas da Microsoft. A utilização é faturada na sua subscrição do Azure que contém a conta de laboratório.   

Eis alguns **casos de utilização de laboratórios geridos**: 

- Forneça aos estudantes um laboratório com máquinas virtuais configuradas com o que é necessário para uma aula. Dê a cada estudante um número limitado de horas para utilizar as VMs para trabalhos de casa ou projetos pessoais.
- Configure um conjunto de VMs de computação de alto desempenho para realizar investigações repletas de gráficos e computação. Execute as VMs necessárias e limpe as máquinas quando terminar. 
- Mova o laboratório de computadores físico da escola para a cloud. Dimensione automaticamente o número de VMs apenas para o limite máximo de custo e utilização definido no laboratório.  
- Aprovisione rapidamente um laboratório de máquinas virtuais para alojar um hackathon. Elimine o laboratório com um único clique quando terminar. 


## <a name="devtest-labs"></a>DevTest Labs
Poderá ter cenários em que pretende gerir toda a infraestrutura e configuração, na sua própria subscrição. Para o fazer, pode criar um laboratório com o Azure DevTest Labs no portal do Azure. Para estes laboratórios, não tem de criar uma conta de laboratório. Estes laboratórios não são apresentados na conta de laboratório (que existe para os laboratórios geridos).  

Veja a seguir alguns **casos de utilização do DevTest Labs**: 

- Aprovisione rapidamente um laboratório de máquinas virtuais para alojar um hackathon ou uma sessão prática numa conferência. Elimine o laboratório com um único clique quando terminar. 
- Crie um conjunto de VMs configuradas com a sua aplicação e permita que a sua equipa utilize facilmente uma máquina virtual para deteção de erros.  
- Forneça aos programadores máquinas virtuais configuradas com todas as ferramentas necessárias. Agende o início e encerramento automático para minimizar os custos. 
- Crie repetidamente um laboratório de máquinas de teste como parte da sua implementação. Teste os bits mais recentes e limpe as máquinas de teste quando terminar. 
- Defina uma variedade de agentes de teste e máquinas virtuais configuradas de forma diferente para testar o desempenho e dimensionamento. 
- Ofereça sessões de formação para os seus clientes através de um laboratório configurado com a versão mais recente do seu produto. Dê a cada cliente um número limitado de horas para utilizar o laboratório. 


## <a name="managed-lab-types-vs-devtest-labs"></a>Tipos de laboratório geridos vs. DevTest Labs
A seguinte tabela compara dois tipos de laboratórios suportados pelo Azure Lab Services: 

| Funcionalidades | Laboratórios geridos | DevTest Labs |
| -------- | ----------------- | ---------- |
| Gestão da infraestrutura do Azure no laboratório. |  Gerido automaticamente pelo serviço | Gerido por conta própria  |
| Resiliência incorporada para problemas de infraestrutura | Processado automaticamente pelo serviço | Gerido por conta própria  |
| Gestão de subscrições | O serviço processa a alocação de recursos nas subscrições da Microsoft que suportam o serviço. O dimensionamento é processado automaticamente pelo serviço. | Pode gerir por conta própria na sua subscrição do Azure. Sem dimensionamento automático de subscrições. |
| Implementação do Azure Resource Manager no laboratório | Não disponível | Disponível |

## <a name="next-steps"></a>Passos seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](classroom-labs/tutorial-setup-classroom-lab.md)
- [Configurar um laboratório](tutorial-create-custom-lab.md)
