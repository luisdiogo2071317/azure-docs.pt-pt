---
title: Utilizar InSpec para a automatização de conformidade da sua infraestrutura do Azure
description: Saiba como utilizar InSpec para detetar problemas das implementações do Azure
keywords: Azure chef, devops, máquinas virtuais, a descrição geral, automatizar inspce
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4193b7fdb3932cbffa2b56b5d7eee6f3b573bd99
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260157"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Utilizar InSpec para a automatização de conformidade da sua infraestrutura do Azure
[InSpec](https://www.chef.io/inspec/) é uma arquitetura livre e open source para testar e auditoria as suas aplicações e infraestrutura. InSpec funciona através da comparação real com o estado do sistema com o estado pretendido que express no código InSpec fácil leitura e escrita fácil. InSpec Deteta violações e apresenta findings sob a forma de um relatório, mas coloca-o no controlo da remediação. Pode utilizar InSpec para validar o estado das máquinas virtuais em execução no Azure. Também pode utilizar InSpec para analisar e validar o estado de recursos e os grupos de recursos dentro de uma subscrição.

Este artigo descreve as vantagens de utilizar InSpec para facilitar a segurança e conformidade no Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Facilitar a compreender e avaliar a compatibilidade
Com InSpec, transformar os seus requisitos para o código de versão, executável, legível por humanos. Isto permite-lhe organizar os seus testes em perfis compostos onde definir e personalizar as exceções, conforme necessário.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Detetar problemas de toda a frota e atribuir prioridade a respetiva remediação
Modo de detetar InSpec sem agente permitem-lhe avaliar rapidamente - à escala - o nível de exposição. Os metadados incorporados para a classificação de gravidade/impacto ajuda a determinar que áreas concentrar-se para remediação.

## <a name="inspect-machines-data-and-new-saas-apis"></a>Inspecione máquinas, dados e novas APIs SaaS
As capacidades de conformidade de API de nuvem InSpec permitem-lhe efetuar continuamente asserções de bloqueio genérico e detalhadas sobre a compatibilidade de nuvem e o relatório no mesmo.

## <a name="satisfy-audits"></a>As auditorias de satisfazer
Com InSpec, pode responder a perguntas de auditoria em qualquer altura - não apenas em intervalos predeterminados como trimestral ou anual. InSpec permite-lhe introduzir um ciclo de auditoria saber a sua postura de conformidade exata, em vez de ser surpreendido por findings um auditor.

## <a name="reduce-ambiguity-and-miscommunication-regarding-rules"></a>Reduzir a ambiguidade e miscommunication sobre regras
Configurações de deixar de documentos e processos de abrir a interpretação. Código executável remove conversações sobre o que deve ser avaliado favor tangible testes com tentativa de limpar.

## <a name="keep-up-with-rapidly-changing-threat-and-compliance-landscapes"></a>Manter-se a mudar rapidamente landscapes ameaça e a conformidade
InSpec permite-lhe escrever e publicar o código de deteção mesmo dia e escrever novas regras em resposta rápida às normas de novo. Isto significa que as alterações no ameaças ou regulamentos já não é igual a emergencies.

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma máquina virtual do Windows no Azure utilizando Chef](/azure/virtual-machines/windows/chef-automation)