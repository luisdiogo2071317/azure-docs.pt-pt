---
title: 'Azure Active Directory Domain Services: Alertas de resolução de problemas | Documentos da Microsoft'
description: Resolver alertas para o Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: ergreenl
ms.openlocfilehash: a6928b5a849f35456a6fb7699acd7720f686c2aa
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243066"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Serviços de domínio do Azure AD - alertas de resolução de problemas
Este artigo fornece os guias de resolução de problemas para todos os alertas que poderá notar no seu domínio gerido.


Escolha os passos de resolução de problemas que correspondem para o ID ou uma mensagem no alerta.

| **ID do alerta** | **Mensagem de alerta** | **Resolução** |
| --- | --- | :--- |
| AADDS001 | *Secure LDAP via internet está ativada para o domínio gerido. No entanto, o acesso à porta 636 não está bloqueado através de um grupo de segurança de rede. Isso pode expor as contas de utilizador no domínio gerido a ataques de força bruta de palavra-passe.* | [Configuração incorreta do LDAP seguro](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Diretório do Azure AD associado com o seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, corrigir e sincronizar o domínio gerido.* | [Diretório em falta](#aadds100-missing-directory) |
| AADDS101 | *Não é possível ativar o Azure AD Domain Services num diretório do Azure AD B2C.* | [O Azure AD B2C está em execução neste diretório](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Um Principal de serviço necessárias para o Azure AD Domain Services funcionar corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, corrigir e sincronizar o domínio gerido.* | [Principal de serviço em falta](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *O intervalo de endereços IP para a rede virtual em que tiver ativado o Azure AD Domain Services está num intervalo IP público. O Azure AD Domain Services tem de estar ativados numa rede virtual com um intervalo de endereços IP privados. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o domínio gerido.* | [O endereço é um intervalo de IP público](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *A Microsoft é não é possível alcançar os controladores de domínio para este domínio gerido. Isto pode acontecer se um grupo de segurança de rede (NSG) configurado na sua rede virtual bloqueia o acesso ao domínio gerido. Outra razão possível é se há uma rota definida pelo utilizador esse tráfego de entrada de blocos da internet.* | [Erro de rede](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *O principal de serviço com o ID da aplicação "d87dcbc6-a371-462e-88e3-28ad15ec4e64" foi eliminado e, em seguida, recriado. A recriação de deixa para trás inconsistentes permissões em recursos de serviços de domínio do Azure AD necessários para atender a seu domínio gerido. Sincronização de palavras-passe no seu domínio gerido poderá ser afetada.* | [A aplicação de sincronização de palavra-passe está desatualizada](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS106 | *A subscrição do Azure associada a seu domínio gerido foi eliminada.  O Azure AD Domain Services requer uma subscrição ativa para continuar a funcionar corretamente.* | [Subscrição do Azure não foi encontrada](#aadds106-your-azure-subscription-is-not-found) |
| AADDS107 | *A subscrição do Azure associada a seu domínio gerido não está ativa.  O Azure AD Domain Services requer uma subscrição ativa para continuar a funcionar corretamente.* | [Subscrição do Azure está desativada](#aadds107-your-azure-subscription-is-disabled) |
| AADDS108 | *Um recurso que é utilizado para o seu domínio gerido foi eliminado. Este recurso é necessária para o Azure AD Domain Services funcionar corretamente.* | [Um recurso foi eliminado](#aadds108-resources-for-your-managed-domain-cannot-be-found) |
| AADDS109 | *A sub-rede selecionada para a implementação do Azure AD Domain Services está cheio e não tem espaço para o controlador de domínio adicional que tem de ser criada.* | [Sub-rede está cheio](#aadds109-the-subnet-associated-with-your-managed-domain-is-full) |
| AADDS110 | *Identificámos que a sub-rede da rede virtual neste domínio poderá não ter endereços IP suficientes. O Azure AD Domain Services precisa de um mínimo dois disponíveis endereços IP na sub-rede está ativada no. Recomendamos um mínimo de ter 3 a 5 endereços IP livres na sub-rede. Isto pode ter ocorrido se outras máquinas virtuais são implementadas na sub-rede, portanto esgotar o número de endereços IP disponíveis ou se existe uma restrição no número de endereços IP disponíveis na sub-rede.* | [Endereços IP não suficientes](#aadds110-not-enough-ip-address-in-the-managed-domain) |
| AADDS111 | *Um ou mais dos recursos de rede utilizados pelo domínio gerido não podem ser manipulado como o âmbito de destino foi bloqueado.* | [Recursos estão bloqueados](#aadds111-resources-are-locked) |
| AADDS112 | *Um ou mais dos recursos de rede utilizados pelo domínio gerido não podem ser manipulado devido à política restriction(s).* | [Recursos não são utilizáveis](#aadds112-resources-are-unusable) |
| AADDS113 | *Os recursos utilizados pelo Azure AD Domain Services foram detetados num Estado inesperado e não podem ser recuperados.* | [Os recursos são irrecuperáveis](#aadds113-resources-are-unrecoverable) |
| AADDS114 | * O azure AD Domain Services, controladores de domínio não são possível aceder a porta 443. É necessário para o serviço, gerir e atualizar o seu domínio gerido. * | [Porta 442 bloqueado](#aadds114-port-443-blocked) |
| AADDS500 | *O domínio gerido última sincronização com o Azure AD no [date]. Os utilizadores podem não ser possível iniciar sessão no domínio gerido ou associações de grupo podem não ser sincronizadas com o Azure AD.* | [Sincronização ainda não ocorreu há algum tempo](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *O domínio gerido última cópia de segurança em [date].* | [Ainda não foi feita uma cópia de segurança há algum tempo](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *O certificado de secure LDAP para o domínio gerido irá expirar em [date].* | [Prazo de expiração de certificado de secure LDAP](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *O domínio gerido está suspenso porque a subscrição do Azure associada ao domínio não está ativa.* | [Suspensão devido à subscrição desativada](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *O domínio gerido está suspensa devido a uma configuração inválida. O serviço foi não é possível gerir, patch, ou atualizar os controladores de domínio para o seu domínio gerido por muito tempo.* | [Suspensão devido a uma configuração inválida](#aadds504-suspension-due-to-an-invalid-configuration) |



## <a name="aadds100-missing-directory"></a>AADDS100: Diretório de falta
**Mensagem de alerta:**

*Diretório do Azure AD associado com o seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, corrigir e sincronizar o domínio gerido.*

**Resolução:**

Este erro é habitualmente provocado por incorretamente mover a sua subscrição do Azure para um novo Azure diretório AD e a eliminar o antigo diretório do Azure AD que continua associado ao Azure AD Domain Services.

Este erro é irrecuperável. Para resolver, tem [eliminar o domínio gerido existente](active-directory-ds-disable-aadds.md) e recriá-lo no diretório novo. Se estiver a ter problemas ao eliminar, contacte a equipa de produto do Azure Active Directory Domain Services [para obter suporte](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: O Azure AD B2C está em execução neste diretório
**Mensagem de alerta:**

*Não é possível ativar o Azure AD Domain Services num diretório do Azure AD B2C.*

**Resolução:**

>[!NOTE]
>Para poder continuar a utilizar o Azure AD Domain Services, tem de recriar a sua instância do Azure AD Domain Services num diretório não - Azure AD B2C.

Para restaurar o seu serviço, siga estes passos:

1. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do diretório do Azure AD existente.
2. Crie um novo diretório que não é um diretório do Azure AD B2C.
3. Siga os [introdução ao](active-directory-ds-getting-started.md) guia para recriar um domínio gerido.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: O endereço é um intervalo de IP público

**Mensagem de alerta:**

*O intervalo de endereços IP para a rede virtual em que tiver ativado o Azure AD Domain Services está num intervalo IP público. O Azure AD Domain Services tem de estar ativados numa rede virtual com um intervalo de endereços IP privados. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o domínio gerido.*

**Resolução:**

> [!NOTE]
> Para resolver este problema, tem de eliminar o domínio gerido existente e voltar a criá-lo numa rede virtual com um intervalo de endereços IP privados. Este processo é problemático.

Antes de começar, leia os **privado espaço de endereços IP v4** secção [este artigo](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Dentro da rede virtual, as máquinas podem efetuar pedidos aos recursos do Azure que estão no mesmo intervalo de endereços IP que estão configuradas para a sub-rede. No entanto, uma vez que a rede virtual está configurada para este intervalo, essas solicitações serão encaminhadas dentro da rede virtual e não irão aceder os recursos da web pretendido. Esta configuração pode levar a erros imprevisíveis com os serviços de domínio do Azure AD.

**Se for o proprietário o intervalo de endereços IP na internet que está configurado na sua rede virtual, este alerta pode ser ignorado. No entanto, o Azure AD Domain Services não é possível consolidar para o [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] com esta configuração, uma vez que ele pode levar a erros imprevisíveis.**


1. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do diretório.
2. Corrigir o intervalo de endereços IP para a sub-rede
  1. Navegue para o [página de redes virtuais no portal do Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecione a rede virtual que pretende utilizar para o Azure AD Domain Services.
  3. Clique em **espaço de endereços** em definições
  4. Atualize o intervalo de endereços ao clicar no intervalo de endereços existente e editá-lo ou adicionar um intervalo de endereços adicionais. Certificar-se de que o novo intervalo de endereços está num intervalo de IP privados. Guarde as alterações.
  5. Clique em **sub-redes** no painel de navegação esquerdo.
  6. Clique na sub-rede que pretende editar na tabela.
  7. Atualizar o intervalo de endereços e guarde as alterações.
3. Siga [guia de introdução ao utilizar o Azure AD Domain Services](active-directory-ds-getting-started.md) para recriar o seu domínio gerido. Certifique-se de que escolha uma rede virtual com um intervalo de endereços IP privados.
4. A associação ao domínio suas máquinas virtuais para o novo domínio, siga [este guia](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Para garantir que o alerta é resolvido, verifique o estado de funcionamento do seu domínio nas duas horas.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Não foi encontrada a subscrição do Azure

**Mensagem de alerta:**

*A subscrição do Azure associada a seu domínio gerido foi eliminada.  O Azure AD Domain Services requer uma subscrição ativa para continuar a funcionar corretamente.*

**Resolução:**

O Azure AD Domain Services requer uma subscrição para a função e não pode ser movido para uma subscrição diferente. Porque a subscrição do Azure que foi associado a seu domínio gerido foi eliminada, terá de recriar uma subscrição do Azure e Azure AD Domain Services.

1. Criar uma subscrição do Azure
2. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do diretório do Azure AD existente.
3. Siga os [introdução ao](active-directory-ds-getting-started.md) guia para recriar um domínio gerido.

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: A sua subscrição do Azure está desativada

**Mensagem de alerta:**

*A subscrição do Azure associada a seu domínio gerido não está ativa.  O Azure AD Domain Services requer uma subscrição ativa para continuar a funcionar corretamente.*

**Resolução:**


1. [Renovar a sua subscrição do Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable).
2. Assim que a subscrição for renovada, serviços de domínio do Azure AD irão receber uma notificação do Azure para reativar o seu domínio gerido.

## <a name="aadds108-resources-for-your-managed-domain-cannot-be-found"></a>AADDS108: Não não possível localizar recursos para o seu domínio gerido

**Mensagem de alerta:**

*Um recurso que é utilizado para o seu domínio gerido foi eliminado. Este recurso é necessária para o Azure AD Domain Services funcionar corretamente.*

**Resolução:**

O Azure AD Domain Services cria recursos específicos ao implementar para funcionar corretamente, incluindo endereços IP públicos, NICs e um balanceador de carga. Se qualquer um das com nome são eliminados, isso faz com que o seu domínio gerido estar num Estado não suportado e impede que o seu domínio a ser gerido. Este alerta é encontrado quando alguém que consiga editar os recursos do Azure AD Domain Services elimina um recurso necessário. Os passos seguintes descrevem como restaurar o seu domínio gerido.

1.  Navegue para a página de estado de funcionamento do Azure AD Domain Services
  1.    Viajar para o [página do Azure AD Domain Services]() no portal do Azure.
  2.    No painel de navegação esquerdo, clique em **estado de funcionamento**
2.  Verificar se o alerta é menos de 4 horas
  1.    Na página de estado de funcionamento, clique no alerta com o ID de **AADDS108**
  2.    O alerta irá ter um carimbo para quando primeiro foi encontrado. Se esse timestamp há menos de 4 horas, é provável que o Azure AD Domain Services pode recriar o recurso foi eliminado.
3.  Se o alerta é a mais de 4 horas, o domínio gerido está num Estado irrecuperável. Tem de eliminar e recriar o Azure AD Domain Services.


## <a name="aadds109-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS109: A sub-rede associada do seu domínio gerido está cheio

**Mensagem de alerta:**

*A sub-rede selecionada para a implementação do Azure AD Domain Services está cheio e não tem espaço para o controlador de domínio adicional que tem de ser criada.*

**Resolução:**

Este erro é irrecuperável. Para resolver, tem [eliminar o domínio gerido existente](active-directory-ds-disable-aadds.md) e [recriar o seu domínio gerido](active-directory-ds-getting-started.md)


## <a name="aadds110-not-enough-ip-address-in-the-managed-domain"></a>AADDS110: Insuficiente endereço IP no domínio gerido

**Mensagem de alerta:**

*Identificámos que a sub-rede da rede virtual neste domínio poderá não ter endereços IP suficientes. O Azure AD Domain Services precisa de um mínimo dois disponíveis endereços IP na sub-rede está ativada no. Recomendamos um mínimo de ter 3 a 5 endereços IP livres na sub-rede. Isto pode ter ocorrido se outras máquinas virtuais são implementadas na sub-rede, portanto esgotar o número de endereços IP disponíveis ou se existe uma restrição no número de endereços IP disponíveis na sub-rede.*

**Resolução:**

1. [Eliminar o seu domínio gerido](#active-directory-ds-disable-aadds.md) do seu inquilino.
2. Corrigir o intervalo de endereços IP para a sub-rede
  1. Navegue para o [página de redes virtuais no portal do Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecione a rede virtual que pretende utilizar para o Azure AD Domain Services.
  3. Clique em **espaço de endereços** em definições
  4. Atualize o intervalo de endereços ao clicar no intervalo de endereços existente e editá-lo ou adicionar um intervalo de endereços adicionais. Guarde as alterações.
  5. Clique em **sub-redes** no painel de navegação esquerdo.
  6. Clique na sub-rede que pretende editar na tabela.
  7. Atualizar o intervalo de endereços e guarde as alterações.
3. Siga [guia de introdução ao utilizar o Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) para recriar o seu domínio gerido. Certifique-se de que escolha uma rede virtual com um intervalo de endereços IP privados.
4. A associação ao domínio suas máquinas virtuais para o novo domínio, siga [este guia](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).
5. Verifique o estado de funcionamento do seu domínio nas duas horas para se certificar de que concluiu os passos corretamente.

## <a name="aadds111-resources-are-locked"></a>AADDS111: Recursos estão bloqueados

**Mensagem de alerta:**

*Um ou mais dos recursos de rede utilizados pelo domínio gerido não podem ser manipulado como o âmbito de destino foi bloqueado.*

**Resolução:**

1.  Operação do Gestor de recursos de revisão de logon os recursos de rede (isso deve lhe dar informações no qual bloqueio está a impedir a modificação).
2.  Remova os bloqueios nos recursos para que o principal de serviço do Azure AD Domain Services pode operar nos mesmos.


## <a name="aadds112-resources-are-unusable"></a>AADDS112: Recursos estiverem inutilizáveis

**Mensagem de alerta:**

*Um ou mais dos recursos de rede utilizados pelo domínio gerido não podem ser manipulado devido à política restriction(s).*

**Resolução:**

1.  Operação do Gestor de recursos de revisão de logon os recursos de rede para o seu domínio gerido
2.  Enfraquecem as restrições de política nos recursos para que o principal de serviço do AAD-DS pode operar nos mesmos.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Os recursos são irrecuperáveis

**Mensagem de alerta:**

*Os recursos utilizados pelo Azure AD Domain Services foram detetados num Estado inesperado e não podem ser recuperados.*

**Resolução:**

Este erro é irrecuperável. Para resolver, tem [eliminar o domínio gerido existente](active-directory-ds-disable-aadds.md) e [recriar o seu domínio gerido](active-directory-ds-getting-started.md)

## <a name="aadds114-port-443-blocked"></a>AADDS114: Porta 443 bloqueado

**Mensagem de alerta:**

*Azure AD Domain Services, controladores de domínio não são possível aceder a porta 443. É necessário para o serviço, gerir e atualizar o seu domínio gerido.*

**Resolução:**

Permita acesso de entrada através da porta 443 no seu grupo de segurança de rede para serviços de domínio do Azure AD.


## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: A sincronização não foi concluída no tempo

**Mensagem de alerta:**

*O domínio gerido última sincronização com o Azure AD no [date]. Os utilizadores podem não ser possível iniciar sessão no domínio gerido ou associações de grupo podem não ser sincronizadas com o Azure AD.*

**Resolução:**

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na sua configuração do seu domínio gerido. Às vezes, com problemas de configuração podem bloquear a capacidade da Microsoft para sincronizar o domínio gerido. Se for capaz de resolver todos os alertas, aguarde duas horas e verificação de volta para ver se a sincronização foi concluída.

Aqui estão algumas razões comuns, por que deixa de sincronização em domínios geridos:
- Ligação de rede é bloqueada no domínio gerido. Para saber mais sobre a verificação da sua rede para problemas, leia como a [resolver problemas dos grupos de segurança de rede](active-directory-ds-troubleshoot-nsg.md) e [requisitos de rede do Azure AD Domain Services](active-directory-ds-networking.md).
-  Sincronização de palavra-passe nunca foi configurada ou foi concluída. Para configurar a sincronização de palavra-passe, leia [este artigo](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Uma cópia de segurança não foi tomada há algum tempo

**Mensagem de alerta:**

*O domínio gerido última cópia de segurança em [date].*

**Resolução:**

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na sua configuração do seu domínio gerido. Às vezes, com problemas de configuração podem bloquear a capacidade da Microsoft para sincronizar o domínio gerido. Se for capaz de resolver todos os alertas, aguarde duas horas e verificação de volta para ver se a sincronização foi concluída.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Suspensão devido à subscrição desativada

**Mensagem de alerta:**

*O domínio gerido está suspenso porque a subscrição do Azure associada ao domínio não está ativa.*

**Resolução:**

> [!WARNING]
> Se o seu domínio gerido está suspenso durante um período prolongado de tempo, é em risco de que está a ser eliminado. É melhor abordar a suspensão mais depressa possível. Para obter mais informações, visite [este artigo](active-directory-ds-suspension.md).

Para restaurar o seu serviço, [renovar a sua subscrição do Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) associados com o seu domínio gerido.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Suspensão devido a uma configuração inválida

**Mensagem de alerta:**

*O domínio gerido está suspensa devido a uma configuração inválida. O serviço foi não é possível gerir, patch, ou atualizar os controladores de domínio para o seu domínio gerido por muito tempo.*

**Resolução:**

> [!WARNING]
> Se o seu domínio gerido está suspenso durante um período prolongado de tempo, é em risco de que está a ser eliminado. É melhor abordar a suspensão mais depressa possível. Para obter mais informações, visite [este artigo](active-directory-ds-suspension.md).

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na sua configuração do seu domínio gerido. Se pode resolver qualquer um destes alertas, fazê-lo. Depois, contacte o suporte para reativar a subscrição.


## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
