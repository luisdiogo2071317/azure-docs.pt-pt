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
ms.date: 07/16/2018
ms.author: ergreenl
ms.openlocfilehash: 5f350bacdc21ec4e5077c93ecc9d97f2fe6c39a5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090914"
---
# <a name="suspended-domains"></a>Domínios suspensos
Quando o Azure AD Domain Services não consegue atender a um domínio gerido por um longo período de tempo, o domínio gerido é colocado num estado suspenso. Este artigo explicará por que os domínios geridos são suspensas, o comprimento de suspensão e para saber como corrigir um domínio suspenso.


## <a name="states-your-managed-domain-can-be-in"></a>Podem ter Estados de seu domínio gerido

![Linha cronológica de domínio suspenso](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

O gráfico anterior descreve como um domínio é suspenso, o tempo em que ela será suspensa e, por fim, a eliminação de um domínio gerido. As secções seguintes detalham os motivos por que um domínio pode ser suspenso e como anular a suspensão de um domínio gerido.

### <a name="running-state"></a>Estado "Em execução"
Pertença a um domínio gerido, que está configurado corretamente e operacionais regularmente os **em execução** estado.

**O que pode esperar:**
* A Microsoft é capaz de monitorar regularmente o estado de funcionamento do seu domínio gerido.
* Controladores de domínio para o seu domínio gerido estão corrigidos e atualizados regularmente.
* Alterações do Azure Active Directory regularmente são sincronizadas com o seu domínio gerido.
* Cópias de segurança regulares direcionadas para o seu domínio gerido.


### <a name="needs-attention-state"></a>Estado de "Necessita de atenção"
Um domínio gerido está na **requer atenção** de estado, se um ou mais problemas exigirem um administrador agir. A página de estado de funcionamento do seu domínio gerido vai listar um ou mais alertas neste estado. Por exemplo, se tiver configurado um NSG restritivo da rede virtual, pode ser Microsoft não é possível atualizar e monitorizar o seu domínio gerido. Esta configuração inválida resulta num alerta a ser gerado e o domínio gerido é colocado no estado "Requer atenção".

Cada alerta tem um conjunto de passos de resolução. Alguns alertas são passageiros e irão obter automaticamente resolvidas pelo serviço. Pode resolver alguns outras alertas ao seguir as instruções nos passos de resolução correspondente para esse alerta. Para resolver alguns alertas críticos, terá de contactar o suporte da Microsoft.

Para obter mais informações, consulte [como resolver problemas de alertas num domínio gerido](active-directory-ds-troubleshoot-alerts.md).

**O que pode esperar:**

Em alguns casos (por exemplo, se tiver uma configuração de rede inválido), os controladores de domínio para o seu domínio gerido poderão estar inacessíveis. Por conseguinte, a Microsoft não garante a seu domínio gerido é monitorizado, corrigido, atualizado ou uma cópia de segurança em intervalos regulares neste estado.

* O domínio gerido está em mau estado de funcionamento e monitorização de estado de funcionamento em curso poderá ser afetado, até que o alerta seja resolvido.
* Controladores de domínio para o seu domínio gerido não podem ser corrigidos ou atualizados.
* Alterações do Azure Active Directory poderão não ser sincronizadas com o seu domínio gerido.
* Podem ser demoradas cópias de segurança para o seu domínio gerido, se possível.
* Se resolver os alertas que afetam o seu domínio gerido, pode ser possível restaurar o seu domínio gerido para o estado "Em execução".
* Alertas críticos são acionados para os problemas de configuração em que a Microsoft é não é possível alcançar os controladores de domínio. Se esses alertas não estão resolvidas no prazo de 15 dias, seu domínio gerido será colocado no estado "Suspenso".


### <a name="suspended-state"></a>Estado "Suspenso"
Um domínio gerido é colocado **suspenso** estado pelos seguintes motivos:
* Um ou mais alertas críticos não tenham sido resolvidos em 15 dias. Isto pode ser causado por uma configuração incorreta que bloqueia o acesso aos recursos necessários para o Azure AD Domain Services.
    * Por exemplo, se o domínio gerido tem o alerta [AADDS104: erro de rede](active-directory-ds-troubleshoot-nsg.md) durante mais de 15 dias.
* Existe um problema de faturação com a sua subscrição do Azure ou se a subscrição do Azure expirou.

Domínios geridos são suspensas quando a Microsoft não consegue gerir, monitorizar, patches ou para o domínio de forma contínua de cópia de segurança.

**O que pode esperar:**
* Controladores de domínio para o seu domínio gerido são desconfiguradas e não sejam encontram acessíveis dentro da rede virtual.
* Acesso de Secure LDAP para o domínio gerido através da internet (se ativada) deixa de funcionar.
* Irá ver falhas na autenticação para o domínio gerido, início de sessão domínio associado a um máquinas virtuais, ligar através de LDAP/LDAPS etc.
* As cópias de segurança para o seu domínio gerido já não são feitas.
* Tem de resolver o alerta que faz com que o seu domínio gerido estar no estado "Suspenso" e, em seguida, contacte o suporte.
* O suporte pode ser capaz de restaurar o seu domínio gerido, apenas se existir uma cópia de segurança existente que é inferior a 30 dias de antiguidade.


### <a name="deleted-state"></a>Estado "Eliminado"
É um domínio gerido, que permanece no estado "Suspenso" durante 15 dias **Deleted**.

**O que pode esperar:**
* Todas as cópias de segurança para o domínio gerido e de recursos são eliminadas.
* Não é possível restaurar o domínio gerido e será necessário criar um novo domínio gerido para utilizar o Azure AD Domain Services.
* Não vai ser Taxado para o domínio gerido.


## <a name="what-happens-when-a-managed-domain-is-suspended"></a>O que acontece quando um domínio gerido está suspenso?
Quando um domínio é suspenso, o Azure AD Domain Services interrompe e anular Aprovisiona os controladores de domínio para o seu domínio gerido. Como resultado, as cópias de segurança já não são direcionadas, os utilizadores não conseguem iniciar sessão no seu domínio e deixa de sincronização com o Azure AD.

O domínio gerido apenas permanecerá num estado suspenso durante um máximo de 15 dias. Para garantir uma recuperação em tempo hábil, recomenda-se que a suspensão de endereços assim que possível.


## <a name="how-do-i-know-if-my-managed-domain-is-suspended"></a>Como posso saber se meu domínio gerido está suspenso?
Verá uma [alerta](active-directory-ds-troubleshoot-alerts.md) na página de estado de funcionamento de serviços de domínio do Azure AD no portal do Azure, que declara o domínio suspenso. Além disso, o estado do domínio mostra "Suspenso".


## <a name="how-do-i-restore-a-suspended-domain"></a>Como faço para restaurar um domínio suspenso?
Para restaurar um domínio no estado "Suspenso", conclua os seguintes passos:

1. Navegue para o [página do Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) no portal do Azure
2. Clique no domínio que pretende anular a suspensão
3. No painel de navegação esquerdo, clique em **estado de funcionamento**
4. Clique no alerta. O ID do alerta será AADDS503 ou AADDS504, dependendo da causa de suspensão.
5. Clique na ligação resolução fornecida no alerta e siga os passos para resolver o alerta.

O domínio só pode ser restaurado para a data da última cópia de segurança. A data da última cópia de segurança é apresentada na página de estado de funcionamento do seu domínio gerido. Quaisquer alterações que ocorreram após a última cópia de segurança não vai ser restaurada. As cópias de segurança para um domínio gerido são armazenadas durante 30 dias. As cópias de segurança com mais de 30 dias são eliminadas.


## <a name="deleting-domains"></a>A eliminação de domínios
Se o domínio é suspenso durante mais de 15 dias, o Azure AD Domain Services elimina o domínio gerido devido a inatividade e a incapacidade de domínio do serviço. Não será cobrado para serviços de domínio do Azure AD. Neste momento, não é possível restaurar o seu domínio gerido e precisar de recriá-lo.


## <a name="next-steps"></a>Passos Seguintes
- [Resolver alertas no seu domínio gerido](active-directory-ds-troubleshoot-alerts.md)
- [Leia mais sobre o Azure AD Domain Services](active-directory-ds-overview.md)
- [Contacte a equipa de produto](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
