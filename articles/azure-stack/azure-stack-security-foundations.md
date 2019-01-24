---
title: Compreender os controlos de segurança do Azure Stack
description: Enquanto administrador de serviços Saiba mais sobre os controlos de segurança aplicados ao Azure Stack
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: fba4020c5e947c135d9e17cfb1d0b2a79ad8a502
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819344"
---
# <a name="azure-stack-infrastructure-security-posture"></a>Postura de segurança de infraestrutura do Azure Stack

*Aplica-se a: Sistemas integrados do Azure Stack*

Considerações de segurança e os regulamentos de conformidade estão entre os controladores de principais para a utilização de clouds híbridas. O Azure Stack foi concebido para estes cenários. Este artigo explica os controlos de segurança no local para o Azure Stack.

Coexistam duas camadas de postura de segurança no Azure Stack. A primeira camada é a infraestrutura do Azure Stack, o que inclui os componentes de hardware até o Gerenciador de recursos do Azure. A primeira camada inclui o administrador e os portais de inquilino. A segunda camada consiste em cargas de trabalho criados, implementados e geridos pelos inquilinos. A segunda camada inclui itens como máquinas virtuais e serviços de aplicações web sites.

## <a name="security-approach"></a>Abordagem de segurança

A postura de segurança para o Azure Stack foi concebida para se Defender contra ameaças modernas e foi concebida para cumprir os requisitos das normas de conformidade principais. Como resultado, a postura de segurança da infraestrutura do Azure Stack baseia-se em dois pilares:

 - **Assume Breach**  
A partir do princípio de que o sistema já foi infringido, concentrar *detetar e limitar o impacto de violações de* versus apenas tentando impedir ataques. 
 - **Protegidos por predefinição**  
Uma vez que a infraestrutura é executado em hardware bem definido e software, Azure Stack *permite, configura e valida todos os recursos de segurança* por predefinição.

Porque o Azure Stack é entregue como um sistema integrado, a postura de segurança da infraestrutura do Azure Stack é definida pela Microsoft. Assim como no Azure, os inquilinos são responsáveis por definir a postura de segurança das suas cargas de trabalho de inquilino. Este documento fornece conhecimento básico sobre a postura de segurança da infraestrutura do Azure Stack.

## <a name="data-at-rest-encryption"></a>Dados de encriptação de Inativos
Todos os dados de inquilino e de infraestrutura de Azure Stack são encriptados em descanso ao utilizar o BitLocker. Esta encriptação protege contra perda física ou o roubo de componentes de armazenamento do Azure Stack. Para obter mais informações, consulte [dados de encriptação de Inativos no Azure Stack](azure-stack-security-bitlocker.md).

## <a name="data-in-transit-encryption"></a>Dados de encriptação de trânsito
Os componentes de infraestrutura do Azure Stack comunicam através de canais encriptados com TLS 1.2. Certificados de encriptação automática são geridos pela infraestrutura. 

Todos os pontos finais de infra-estrutura externo, como os pontos de extremidade REST ou o portal do Azure Stack, suportam a TLS 1.2 para proteger as comunicações. Certificados de encriptação, a partir de uma aplicação de terceiros ou sua empresa autoridade de certificação, tem de ser fornecidos para esses pontos de extremidade. 

Embora possam ser utilizados certificados autoassinados para estes pontos finais externos, o Microsoft recomenda enfaticamente contra a utilizá-los. 

## <a name="secret-management"></a>Gestão de segredos
Infraestrutura do Azure Stack utiliza uma infinidade de segredos, como palavras-passe, a funcionar. Muitas delas são rodadas automaticamente com frequência, pois são contas de serviço de Group-Managed que rodar a cada 24 horas.

Os segredos restantes que não são contas de serviço Group-Managed podem ser giradas manualmente, com um script, o ponto final com privilégios.

## <a name="code-integrity"></a>Integridade do código
O Azure Stack usa o Windows Server 2016 mais recentes recursos de segurança. Uma delas é o Windows Defender Device Guard, que proporciona permissões de aplicação e garante que apenas autorizados a execução do código dentro da infraestrutura do Azure Stack. 

Código autorizado está assinado pela Microsoft ou o parceiro OEM. O código de autorizados assinado está incluído na lista de software permitido especificado numa política definida pela Microsoft. Em outras palavras, apenas o software que foi aprovado para executar na infraestrutura do Azure Stack pode ser executado. Qualquer tentativa de executar código não autorizado é bloqueada e é gerada uma auditoria.

A política do Device Guard também impede que os agentes de terceiros ou software seja executado na infraestrutura do Azure Stack.

## <a name="credential-guard"></a>Credential Guard
Outro recurso de segurança do Windows Server 2016 no Azure Stack é o Windows Defender Credential Guard, que é utilizado para proteger as credenciais de infraestrutura do Azure Stack de Pass-the-Hash e ataques de passagem da permissão.

## <a name="antimalware"></a>Antimalware
Todos os componentes no Azure Stack (anfitriões Hyper-V e máquinas virtuais) é protegido com o antivírus do Windows Defender.

Em cenários de ligação, atualizações de definições e de motor antivírus são aplicadas várias vezes por dia. Em cenários desconectados, atualizações de antimalware são aplicadas como parte das atualizações mensais do Azure Stack. Para obter mais informações, consulte [atualizar o antivírus do Windows Defender no Azure Stack](azure-stack-security-av.md).

## <a name="constrained-administration-model"></a>Modelo de administração restrita
Administração no Azure Stack é controlada por meio de três pontos de entrada, cada um com uma finalidade específica: 
1. O [Portal de administrador](azure-stack-manage-portals.md) fornece uma experiência de apontar e clicar para operações de gestão diárias.
2. O Azure Resource Manager expõe todas as operações de gestão do Portal do administrador por meio de uma API REST, utilizado pelo PowerShell e CLI do Azure. 
3. Para operações específicas de nível baixo, por exemplo dados center integração ou oferecer suporte a cenários, Azure Stack expõe um ponto de extremidade do PowerShell chamado [ponto final com privilégios](azure-stack-privileged-endpoint.md). Este ponto final expõe apenas um conjunto na lista de permissões de cmdlets e ele é bastante auditado.

## <a name="network-controls"></a>Controlos de rede
Infraestrutura do Azure Stack é fornecido com várias camadas de rede lista de controlo de acesso (ACL). As ACLs de impedem o acesso não autorizado para os componentes da infra-estrutura e limitam as comunicações de infraestrutura para apenas os caminhos que são necessários para seu funcionamento. 

ACLs de rede são impostas em três camadas:
1.  Comutadores top of Rack
2.  Rede definida pelo software
3.  Firewalls de sistema operativo anfitrião e a VM

## <a name="regulatory-compliance"></a>Conformidade regulamentar

O Azure Stack tornou-se através de uma avaliação formal por uma empresa de auditoria de terceiros independentes. Como resultado, a documentação sobre como a infraestrutura do Azure Stack satisfaz os controlos aplicáveis a partir de várias normas de conformidade principais está disponível. A documentação não é uma certificação do Azure Stack devido às normas incluindo vários controles relacionados com o pessoal e relacionados com o processo. Em vez disso, os clientes podem utilizar esta documentação para iniciar seu processo de certificação.

As avaliações incluem as normas seguintes:

- [PCI-DSS](https://www.pcisecuritystandards.org/pci_security/) aborda a indústria de cartões de pagamento.
- [Matriz de controlo do CSA Cloud](https://cloudsecurityalliance.org/group/cloud-controls-matrix/#_overview) é um mapeamento abrangente em vários padrões, incluindo FedRAMP moderado, ISO27001, HIPAA, HITRUST, ITAR, NIST SP800-53 e outras pessoas.
- [FedRAMP elevado](https://www.fedramp.gov/fedramp-releases-high-baseline/) para clientes do Governo.

A documentação de conformidade pode ser encontrada no [Portal de confiança do serviço Microsoft](https://servicetrust.microsoft.com/ViewPage/Blueprint). Os guias de conformidade são um recurso protegido e tem de iniciar sessão com as credenciais do serviço cloud do Azure.

## <a name="next-steps"></a>Passos Seguintes

- [Saiba como rodar os segredos no Azure Stack](azure-stack-rotate-secrets.md)
- [PCI-DSS e os documentos de CCM da CSA para o Azure Stack](https://servicetrust.microsoft.com/ViewPage/TrustDocuments)
- [Documentos do DoD e do NIST para o Azure Stack](https://servicetrust.microsoft.com/ViewPage/Blueprint)
