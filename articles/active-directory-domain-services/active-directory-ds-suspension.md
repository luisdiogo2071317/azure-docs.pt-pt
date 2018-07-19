---
title: 'O Azure Active Directory Domain Services: Suspenso domínios | Documentos da Microsoft'
description: A suspensão do domínio gerido e eliminação
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: ergreenl
ms.openlocfilehash: 99896b0f618151cd368a21c4a212b7d8e28b7a6f
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126098"
---
# <a name="suspended-domains"></a>Domínios suspensos
Quando o Azure AD Domain Services não consegue atender a um domínio gerido por um longo período de tempo, o domínio gerido é colocado num estado suspenso. Este artigo explica por que os domínios geridos são suspensas e para saber como corrigir um domínio suspenso.


## <a name="states-your-managed-domain-can-be-in"></a>Podem ter Estados de seu domínio gerido

![Linha cronológica de domínio suspenso](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

O gráfico anterior descreve os possíveis Estados de um domínio gerido do Azure AD Domain Services pode estar em.

### <a name="running-state"></a>Estado "Em execução"
Pertença a um domínio gerido, que está configurado corretamente e operacionais regularmente os **em execução** estado.

**O que pode esperar:**
* Microsoft regularmente pode monitorizar o estado de funcionamento do seu domínio gerido.
* Controladores de domínio para o seu domínio gerido estão corrigidos e atualizados regularmente.
* Alterações do Azure Active Directory regularmente são sincronizadas com o seu domínio gerido.
* Cópias de segurança regulares direcionadas para o seu domínio gerido.


### <a name="needs-attention-state"></a>Estado de "Necessita de atenção"
Um domínio gerido está na **requer atenção** de estado, se um ou mais problemas exigirem um administrador agir. A página de estado de funcionamento do seu domínio gerido vai listar um ou mais alertas neste estado. Por exemplo, se tiver configurado um NSG restritivo da rede virtual, pode ser Microsoft não é possível atualizar e monitorizar o seu domínio gerido. Esta configuração inválida resulta num alerta a ser gerado e o domínio gerido é colocado no estado "Requer atenção".

Cada alerta tem um conjunto de passos de resolução. Alguns alertas são passageiros e irão obter automaticamente resolvidas pelo serviço. Pode resolver alguns outras alertas ao seguir as instruções nos passos de resolução correspondente para esse alerta. Para resolver alguns alertas críticos, terá de contactar o suporte da Microsoft.

Para obter mais informações, consulte [como resolver problemas de alertas num domínio gerido](active-directory-ds-troubleshoot-alerts.md).

**O que pode esperar:**

Em alguns casos (por exemplo, se tiver uma configuração de rede inválido), os controladores de domínio para o seu domínio gerido poderão estar inacessíveis. A Microsoft não garante a seu domínio gerido é monitorizado, corrigido, atualizado ou uma cópia de segurança em intervalos regulares neste estado.

* O domínio gerido está em mau estado de funcionamento e monitorização de estado de funcionamento em curso pode parar, até que o alerta seja resolvido.
* Controladores de domínio para o seu domínio gerido não podem ser corrigidos ou atualizados.
* Alterações do Azure Active Directory poderão não ser sincronizadas com o seu domínio gerido.
* Podem ser demoradas cópias de segurança para o seu domínio gerido, se possível.
* Se resolver os alertas que afetam o seu domínio gerido, pode ser possível restaurar o seu domínio gerido para o estado "Em execução".
* Alertas críticos são acionados para os problemas de configuração em que a Microsoft é não é possível alcançar os controladores de domínio. Se tais alertas não são resolvidos no prazo de 15 dias, seu domínio gerido será colocado no estado "Suspenso".


### <a name="suspended-state"></a>Estado "Suspenso"
Um domínio gerido é colocado **suspenso** estado pelos seguintes motivos:
* Um ou mais alertas críticos ainda não foi resolvidos em 15 dias. Alertas críticos podem ser causados por uma configuração incorreta que bloqueia o acesso aos recursos necessários para o Azure AD Domain Services.
    * Por exemplo, se o domínio gerido tem o alerta [AADDS104: erro de rede](active-directory-ds-troubleshoot-nsg.md) durante mais de 15 dias.
* Existe um problema de faturação com a sua subscrição do Azure ou a sua subscrição do Azure expirou.

Domínios geridos são suspensas quando a Microsoft não consegue gerir, monitorizar, aplicar um patch ou o domínio de forma contínua de cópia de segurança.

**O que pode esperar:**
* Controladores de domínio para o seu domínio gerido são desconfiguradas e não estão acessíveis dentro da rede virtual.
* Acesso de Secure LDAP para o domínio gerido através da internet (se ativada) deixa de funcionar.
* Observe a falhas na autenticação para o domínio gerido, fazendo logon em máquinas de virtuais associados a um domínio e ligar através de LDAP/LDAPS.
* As cópias de segurança para o seu domínio gerido já não são feitas.
* Interrompe a sincronização com o Azure AD.
* Resolva o alerta que faz com que o seu domínio gerido estar no estado "Suspenso" e, em seguida, contacte o suporte.
* Suporte poderá restaurar o seu domínio gerido, apenas se existe uma cópia de segurança que é inferior a 30 dias de antiguidade.

O domínio gerido apenas permanecerá num estado suspenso durante 15 dias. Para recuperar o seu domínio gerido, a Microsoft recomenda a que resolver alertas críticos imediatamente.


### <a name="deleted-state"></a>Estado "Eliminado"
É um domínio gerido, que permanece no estado "Suspenso" durante 15 dias **Deleted**.

**O que pode esperar:**
* Todas as cópias de segurança para o domínio gerido e de recursos são eliminadas.
* Não é possível restaurar o domínio gerido e tem de criar um novo domínio gerido para utilizar o Azure AD Domain Services.
* Depois de eliminado, não são faturadas para o domínio gerido.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Como sabe se o seu domínio gerido está suspenso?
Verá uma [alerta](active-directory-ds-troubleshoot-alerts.md) na página de estado de funcionamento de serviços de domínio do Azure AD no portal do Azure, que declara o domínio suspenso. O estado do domínio também mostra "Suspenso".


## <a name="restore-a-suspended-domain"></a>Restaurar um domínio suspenso
Para restaurar um domínio no estado "Suspenso", conclua os seguintes passos:

1. Navegue para o [página do Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) no portal do Azure
2. Clique no domínio gerido.
3. No painel de navegação esquerdo, clique em **estado de funcionamento**.
4. Clique no alerta. O ID do alerta será AADDS503 ou AADDS504, dependendo da causa de suspensão.
5. Clique na ligação resolução fornecida no alerta e siga os passos para resolver o alerta.

Só pode ser restaurado o seu domínio gerido para a data da última cópia de segurança. A data da última cópia de segurança é apresentada na página de estado de funcionamento do seu domínio gerido. Quaisquer alterações que ocorreram após a não ser restaurada a última cópia de segurança. As cópias de segurança para um domínio gerido são armazenadas durante 30 dias. As cópias de segurança com mais de 30 dias são eliminadas.


## <a name="next-steps"></a>Passos Seguintes
- [Resolver alertas no seu domínio gerido](active-directory-ds-troubleshoot-alerts.md)
- [Leia mais sobre o Azure AD Domain Services](active-directory-ds-overview.md)
- [Contacte a equipa de produto](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
