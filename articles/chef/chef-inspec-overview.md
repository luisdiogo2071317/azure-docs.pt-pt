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
ms.openlocfilehash: a5de2ca04a193f97a2a65a043f744abb8e0ea758
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359226"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Usar da InSpec para automação de conformidade da sua infraestrutura do Azure
[InSpec](https://www.chef.io/inspec/) é a linguagem de código aberto do Chef para descrever as regras de segurança e conformidade que podem ser compartilhadas entre os engenheiros de software, as operações e engenheiros de segurança. Funciona da InSpec, comparando o estado real da sua infraestrutura com o estado pretendido que são expressos em fácil de ler e fácil de escrever código InSpec. InSpec Deteta violações e apresenta as descobertas na forma de um relatório, mas coloca no controle de remediação.

Pode usar da InSpec para validar o estado dos recursos e grupos de recursos dentro de uma subscrição, incluindo máquinas virtuais, configurações de rede, definições do Azure Active Directory e muito mais.

Este artigo descreve as vantagens da utilização da InSpec para facilitar a segurança e conformidade no Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Tornar fácil de compreender e avaliar a conformidade
Documentação de conformidade escrita em folhas de cálculo ou documentos do Word deixe requisitos aberto para interpretações. Com o da InSpec, transforme os seus requisitos em código com versão do executável, legível por humanos. Código substitui conversas sobre o que deve ser avaliado em favor tangíveis testes com a intenção clara.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Detete problemas de toda a frota e atribuir prioridades a respetiva remediação
InSpec sem agente detetar modo permitem-lhe avaliar rapidamente - em escala - seu nível de exposição. Metadados incorporados para a classificação de gravidade/impacto ajuda a determinar quais áreas de foco de remediação. Também pode escrever regras rapidamente em resposta a novas vulnerabilidades ou regulamentos e distribuam-las imediatamente.

## <a name="satisfy-audits"></a>Satisfazer auditorias
Com o da InSpec, possa responder a perguntas de auditoria em qualquer altura - não apenas em intervalos de predeterminado como trimestral ou anuais. Ao executar continuamente testes InSpec, introduz um ciclo de auditorias saber a sua postura de conformidade exata e o histórico, em vez de ter ficado surpreso com as descobertas de um auditor.

## <a name="next-steps"></a>Passos Seguintes

* Tente InSpec no Azure Cloud Shell [ ![iniciar Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "iniciar Cloud Shell")](https://shell.azure.com)
