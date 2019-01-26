---
title: Proteção avançada contra ameaças - base de dados do Azure para PostgreSQL
description: Proteção avançada contra ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: b01238b0ebfe07b605824a75301165e477bd05e9
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910717"
---
# <a name="azure-database-for-postgresql-advanced-threat-protection"></a>Base de dados do Azure para PostgreSQL proteção avançada contra ameaças

A Proteção Avançada Contra Ameaças para Base de Dados do Azure para PostgreSQL deteta atividades anómalas que indicam tentativas potencialmente perigosas e invulgares para aceder ou explorar as suas bases de dados.

Proteção contra ameaças faz parte da oferta de proteção contra ameaças avançadas (ATP), que é um pacote unificado para os recursos de segurança avançada. Proteção avançada contra ameaças podem ser acessada e gerenciada através da [portal do Azure](https://portal.azure.com) e está atualmente em pré-visualização.

> [!NOTE]
> A funcionalidade de proteção avançada contra ameaças está **não** disponíveis nas regiões de cloud soberana e seguir o Azure government: US Gov Texas, EUA Gov Arizona, US Gov Iowa, US, Gov Virginia, US DoD East, US DoD Central, Alemanha Central, Alemanha norte, leste da China, leste da China 2. Visite [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/) para disponibilidade geral do produto.
>

> [!NOTE]
> Esta funcionalidade está disponível em todas as regiões do Azure em que a base de dados do Azure para PostgreSQL é implementada para os servidores de fins gerais e memória otimizada.

## <a name="what-is-advanced-threat-protection"></a>O que é a proteção avançada contra ameaças?

Proteção avançada contra ameaças para a base de dados do Azure para PostgreSQL fornece uma nova camada de segurança, o que permite aos clientes detetar e responder a potenciais ameaças à medida que ocorrem ao fornecer alertas de segurança relativamente a atividades anómalas. Os utilizadores recebem um alerta após a atividades suspeitas da base de dados e potenciais vulnerabilidades, bem como os padrões de acesso e consultas de base de dados anómalas. Proteção avançada contra ameaças para a base de dados do Azure para PostgreSQL integra alertas no [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), que inclui detalhes de atividade suspeita e recomenda ação sobre como investigar e mitigar a ameaça. Proteção avançada contra ameaças para a base de dados do Azure para PostgreSQL facilita lidar com potenciais ameaças à base de dados sem a necessidade de ser um especialista em segurança ou gerir sistemas de monitorização de segurança avançada. 

![Conceito de proteção avançada contra ameaças](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Alertas de proteção contra ameaças avançadas 
Proteção avançada contra ameaças para a base de dados do Azure para PostgreSQL Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados e ele pode acionar os seguintes alertas:
- **Acesso a partir de uma localização invulgar**: Este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor PostgreSQL, em que alguém iniciou sessão para a base de dados do Azure para o servidor PostgreSQL a partir de uma localização geográfica invulgar. Em alguns casos, o alerta deteta uma ação legítima (uma nova manutenção de programador ou aplicação). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso a partir do Centro de dados do Azure invulgar**: Este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor PostgreSQL, em que alguém iniciou sessão para o servidor de um centro de dados do Azure invulgar que foi visto neste servidor durante o período recente. Em alguns casos, o alerta Deteta uma ação legítima (sua nova aplicação no Azure, Power BI, base de dados para o Editor de consultas do PostgreSQL). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
- **Acesso a partir de principal invulgar**: Este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor PostgreSQL, em que alguém iniciou sessão no servidor com um principal invulgar (base de dados Azure para PostgreSQL utilizador). Em alguns casos, o alerta deteta uma ação legítima (nova aplicação, manutenção de programador). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso a partir de uma aplicação potencialmente prejudicial**: Este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder a base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
- **Base de dados do Azure de força bruta para credenciais de PostgreSQL**: Este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com credenciais diferentes. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque de força bruta.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Para obter mais informações sobre os preços, consulte o [base de dados do Azure para a página de preços do PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/) 
* Configurar [base de dados do Azure para PostgreSQL Advanced Threat Protection](howto-database-threat-protection-portal.md) no portal do Azure  
