---
title: Recuperar da perda catastrófica de dados no Azure Stack com o serviço de cópia de segurança da infraestrutura | Documentos da Microsoft
description: Quando uma falha catastrófica faz com que o Azure Stack para falhar, pode restaurar os dados de infraestrutura, quando restabelecendo sua implementação do Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: ed89468d65f35d0a8cda90da365e8625f46e3f92
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059539"
---
# <a name="recover-from-catastrophic-data-loss"></a>Recuperar da perda catastrófica de dados

*Aplica-se a: sistemas integrados do Azure Stack.*

O Azure Stack executa serviços do Azure no seu datacenter. O Azure Stack pode executar em ambientes tão pequenas como instalado num bastidor único de quatro nós. Por outro lado, o Azure é executado em mais de 40 regiões em vários datacenters e várias zonas em cada região. Recursos de utilizador podem abranger vários servidores, racks, datacenters e regiões. Com o Azure Stack, atualmente tem apenas a opção de implementar a sua nuvem inteira num bastidor único. Isso expõe sua cloud ao risco de eventos catastróficos no seu datacenter ou falhas devido a erros de produtos principais. Quando acontece um desastre, a instância do Azure Stack fica offline. Todos os dados é potencialmente irrecuperável.

Dependendo da causa raiz da perda de dados, terá de reparar um única infraestrutura serviço ou restaurar toda a instância do Azure Stack. Ainda poderá restaurar para um hardware diferente na mesma localização ou numa localização diferente.

Isso resolve de cenário recuperar a sua instalação inteira em caso de falha dos equipamentos e a reimplementação da nuvem privada.

| Cenário                                                           | Perda de dados                            | Considerações                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Recuperar da perda catastrófica de dados devido a erro de desastre ou produto | Todos os dados de infraestrutura e de utilizador e de aplicação | Aplicação de utilizador e os dados são protegidos separadamente dos dados de infraestrutura |

## <a name="workflows"></a>Fluxos de trabalho

A jornada de proteger o início Azure começa com o backup de dados de aplicação/inquilino e de infraestrutura em separado. Este documento aborda como proteger a infraestrutura. 

![Implementação inicial do Azure Stack](media\azure-stack-backup\azure-stack-backup-workflow1.png)

No pior cenários de casos em que todos os dados são perdidos, a recuperação do Azure Stack é o processo de restauro dos dados de infraestrutura exclusivos para essa implementação do Azure Stack e todos os dados de utilizador. 

![Implementar o Azure Stack](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Restauro

Se houver perda catastrófica de dados, mas o hardware ainda será possível usar, é necessária a reimplementação do Azure Stack. Durante a implementação, pode especificar a localização de armazenamento e as credenciais necessárias para aceder às cópias de segurança. Neste modo, não é necessário especificar os serviços que precisam ser restaurados. Controlador de cópia de segurança de infra-estrutura injeta o estado do plano de controle como parte do fluxo de trabalho de implantação.

Se houver um desastre que processa o hardware inutilizável, reimplementação só é possível no novo hardware. A reimplementação pode demorar várias semanas, enquanto o hardware de substituição é ordenada e chega no Centro de dados. Restauro de dados do plano de controlo é possível em qualquer altura. No entanto, restauro não é suportado se a versão da instância reimplantada for mais de uma versão superior à versão utilizada na última cópia de segurança. 

| Modo de implementação | Ponto de partida | Ponto final                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Instalação de raiz   | Compilação de linha de base | OEM implementa o Azure Stack e as atualizações para a versão mais recente suportada.                                                                                                                                          |
| Modo de recuperação   | Compilação de linha de base | OEM implementa o Azure Stack no modo de recuperação e lida com a versão correspondente requisitos com base na cópia de segurança mais recente disponível. O OEM é concluída a implementação através da atualização para a versão mais recente suportada. |

## <a name="data-in-backups"></a>Dados em cópias de segurança

O Azure Stack oferece suporte a um tipo de implementação chamado modo de recuperação na cloud. Este modo é usado somente se optar por recuperar o Azure Stack após um desastre ou bugs do produto processado a solução irrecuperável. Neste modo de implementação não recupera os dados de utilizador armazenados na solução. O escopo neste modo de implementação é limitado para restaurar os dados seguintes:

 - Entradas de implementação
 - Sistemas de identidades interno
 - Federado identificar configuração (implementações desligadas)
 - Certificados de raiz utilizados pela autoridade de certificação interna
 - Dados do Azure Resource Manager configuração utilizador, como as subscrições, planos, ofertas e quotas para o armazenamento, rede e recursos de computação
 - Segredos do Cofre de chaves e os cofres
 - Atribuições de políticas RBAC e as atribuições de funções 

Nenhum utilizador infraestrutura como serviço (IaaS) ou plataforma como um recursos de serviço (PaaS) são recuperados durante a implementação. Isto é IaaS VMs, contas de armazenamento, blobs, tabelas, configuração de rede e assim por diante, são perdidos. O objetivo de recuperação na cloud é garantir que os operadores e os utilizadores podem iniciar novamente para o portal depois de concluída a implementação. Voltar a iniciar sessão de utilizadores não verão qualquer um dos seus recursos. Os utilizadores têm suas assinaturas restauradas e juntamente com que o original planos e oferece as políticas definidas pelo administrador. Os utilizadores que iniciem novamente para o sistema para operar com as mesmo restrições impostas pela solução original antes do desastre. Após a conclusão da recuperação de nuvem, o operador pode restaurar manualmente valor agregado e RPs de terceiros e dados associados.

## <a name="next-steps"></a>Passos Seguintes

 - Saiba mais sobre as melhores práticas para [com o serviço de cópia de segurança da infraestrutura](azure-stack-backup-best-practices.md).
