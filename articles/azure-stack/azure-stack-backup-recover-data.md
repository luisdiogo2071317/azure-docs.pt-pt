---
title: "Recuperar a partir de perda catastrófica de dados na pilha do Azure utilizando o serviço de cópia de segurança da infraestrutura | Microsoft Docs"
description: "Quando uma falha catastrófica faz com que a pilha do Azure falhar, pode restaurar dados da sua infraestrutura quando reestablishing a implementação de pilha do Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 859d31b554fcd1936ce555f6afb0f4631a3af7aa
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="recover-from-catastrophic-data-loss"></a>Recuperar a partir de perda catastrófica de dados

*Aplica-se a: Azure pilha integrado sistemas.*

Pilha do Azure é executado serviços do Azure no seu centro de dados. Pilha do Azure pode ser executados em ambientes tão pequenas como quatro nós instalado num bastidor único. Em contrapartida, o Azure é executado em mais de 40 regiões em vários datacenters e várias zonas em cada região. Recursos de utilizador podem abranger vários servidores, bastidores, centros de dados e regiões. Com a pilha do Azure, atualmente só tem a opção de implementar a sua nuvem toda a um bastidor único. Isto expõe a nuvem para o risco de eventos catastrófica no seu centro de dados ou falhas devido a erros de versão do produto. Quando um desastre strikes, a instância de pilha do Azure fica offline. Todos os dados é potencialmente irrecuperável.

Dependendo da causa raiz de perda de dados, poderá ter um única infraestrutura serviço de reparar ou restaurar toda a instância de pilha do Azure. Ainda poderá ter de restaurar para um hardware diferente na mesma localização ou numa localização diferente.

Este endereços de cenário recuperar a sua instalação completa em caso de falha do equipamento e a reimplementação da nuvem privada.

| Cenário                                                           | Perda de dados                            | Considerações                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Recuperar a partir de perda catastrófica de dados devido a erros de produto ou desastre | Todos os dados de infraestrutura e de utilizador e de aplicações | Aplicação do utilizador e os dados são protegidos separadamente a partir dos dados de infraestrutura |

## <a name="workflows"></a>Fluxos de trabalho

Journey Azure começar a proteger começa com a cópia de segurança da infraestrutura e de aplicação/inquilino separadamente. Este documento aborda como proteger a infraestrutura. 

![Implementação inicial de pilha do Azure](media\azure-stack-backup\azure-stack-backup-workflow1.png)

Em cenários de caso pior onde todos os dados são perdidas, a recuperação de pilha do Azure é o processo de restauro dos dados de infraestrutura exclusivos para essa implementação de pilha do Azure e todos os dados de utilizador. 

![Reimplementar a pilha do Azure](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Restauro

Se houver perda catastrófica de dados, mas o hardware é ainda utilizável, é necessária a reimplementação da pilha do Azure. Durante a reimplementação, pode especificar a localização de armazenamento e as credenciais necessárias para aceder a cópias de segurança. Neste modo, não é necessário especificar os serviços que têm de ser restaurados. Controlador de cópia de segurança da infraestrutura injects controlo plane estado como parte do fluxo de trabalho de implementação.

Se ocorrer um desastre que compõe o hardware não utilizável, reimplementação só é possível num novo hardware. Reimplementação pode demorar várias semanas hardware de substituição é ordenada e chega no Centro de dados. É possível restauro de dados do controlo plane em qualquer altura. No entanto, restauro não é suportado se a versão da instância reimplementada mais do que uma versão maior do que a versão utilizada na última cópia de segurança. 

| Modo de implementação | Ponto de partida | Ponto final                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Instalação de raiz   | Compilação de linha de base | OEM implementa a pilha do Azure e as atualizações para a versão mais recente suportada.                                                                                                                                          |
| Modo de recuperação   | Compilação de linha de base | OEM implementa a pilha do Azure no modo de recuperação e processa a versão correspondente requisitos com base na cópia de segurança mais recente disponível. OEM conclui a implementação através da atualização para a versão mais recente suportada. |

## <a name="data-in-backups"></a>Dados em cópias de segurança

Pilha do Azure suporta um tipo de implementação denominada modo de recuperação na nuvem. Este modo é utilizado apenas se optar por recuperar pilha do Azure depois de um desastre ou erros de produto composto a solução irrecuperável. Neste modo de implementação não recuperar os dados de utilizador armazenados na solução. O âmbito neste modo de implementação está limitado ao restaurar os dados seguintes:

 - Entradas de implementação
 - Sistemas de identidade interna
 - Federado identificar configuration (implementações desligadas)
 - Certificados de raiz utilizados pela autoridade de certificação interna
 - Dados do Azure Resource Manager configuração utilizador, tais como as subscrições, planos, ofertas e quotas de armazenamento, rede e recursos de computação
 - Os segredos do KeyVault e cofres
 - Atribuições de política do RBAC e atribuições de função 

Nenhum utilizador infraestrutura como serviço (IaaS) ou plataforma como um recursos de serviço (PaaS) são recuperados durante a implementação. Ou seja VMs de IaaS, contas de armazenamento, blobs, tabelas, configuração de rede e assim sucessivamente, serão perdidas. O objetivo de recuperação na nuvem é garantir que os operadores e os utilizadores podem iniciar novamente para o portal após a conclusão da implementação. Os utilizadores de registo novamente no não verão qualquer um dos respetivos recursos. Os utilizadores têm as suas subscrições restauradas e juntamente com que o original planos e oferece as políticas definidas pelo administrador. Utilizadores que iniciam sessão no sistema opera com as mesmas restrições impostas na solução original antes de desastre. Após a conclusão da recuperação de nuvem, o operador pode restaurar manualmente de valor acrescentado e RPs de terceiros e dados associados.

## <a name="next-steps"></a>Passos Seguintes

 - Saiba mais sobre as melhores práticas para [utilizando o serviço de cópia de segurança da infraestrutura](azure-stack-backup-best-practices.md).
