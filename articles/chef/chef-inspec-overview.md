---
title: Usar da InSpec para automação de conformidade da sua infraestrutura do Azure
description: Saiba como utilizar da InSpec para detetar problemas na suas implementações do Azure
keywords: Azure, chef, devops, máquinas virtuais, visão geral, automatizar, inspec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: e854b140c32773fc5d64e828e7dd40fab1f6ca8d
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332375"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Usar da InSpec para automação de conformidade da sua infraestrutura do Azure
[InSpec](https://www.chef.io/inspec/) é uma estrutura gratuita e de código-fonte aberto para teste e a auditoria de suas aplicações e infraestrutura. Funciona da InSpec, comparando o estado real do seu sistema com o estado pretendido que são expressos em fácil de ler e fácil de escrever código InSpec. InSpec Deteta violações e apresenta as descobertas na forma de um relatório, mas coloca no controle de remediação. Pode usar da InSpec para validar o estado das suas máquinas virtuais em execução no Azure. Também pode utilizar da InSpec para analisar e validar o estado dos recursos e grupos de recursos dentro de uma subscrição.

Este artigo descreve as vantagens da utilização da InSpec para facilitar a segurança e conformidade no Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Tornar fácil de compreender e avaliar a conformidade
Com o da InSpec, transforme os seus requisitos em código com versão do executável, legível por humanos. Isto permite-lhe organizar seus testes em perfis compostos, onde define e personalizar exceções conforme necessário.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Detete problemas de toda a frota e atribuir prioridades a respetiva remediação
InSpec sem agente detetar modo permitem-lhe avaliar rapidamente - em escala - seu nível de exposição. Metadados incorporados para a classificação de gravidade/impacto ajuda a determinar quais áreas de foco de remediação.

## <a name="inspect-machines-data-and-new-saas-apis"></a>Inspecionar máquinas, dados e novas APIs de SaaS
As capacidades de conformidade da API de InSpec cloud permitem-lhe fazer asserções genérico e detalhadas sobre a sua conformidade na cloud e o relatório no mesmo continuamente.

## <a name="satisfy-audits"></a>Satisfazer auditorias
Com o da InSpec, possa responder a perguntas de auditoria em qualquer altura - não apenas em intervalos de predeterminado como trimestral ou anuais. InSpec permite-lhe introduzir um ciclo de auditorias saber a sua postura de conformidade exata, em vez de ter ficado surpreso com as descobertas de um auditor.

## <a name="reduce-ambiguity-and-miscommunication-regarding-rules"></a>Reduzir a ambiguidade e desvios de comunicação sobre regras
Configurações de deixar de documentos e processos aberto para interpretações. Código executável remove as conversações sobre o que deve ser avaliado em favor tangíveis testes com a intenção clara.

## <a name="keep-up-with-rapidly-changing-threat-and-compliance-landscapes"></a>Manter-se mudando rapidamente panoramas de ameaças e conformidade
InSpec permite-lhe escrever e publicar o código de detecção no mesmo dia e escrever novas regras em rápida resposta a novas regulamentações. Isso significa que as alterações no ameaças ou regulamentos já não é igual a emergências.

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma máquina virtual do Windows no Azure utilizando o Chef](/azure/virtual-machines/windows/chef-automation)
