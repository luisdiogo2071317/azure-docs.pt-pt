---
title: 'Azure Active Directory dos serviços de domínio: Resolver problemas com alertas | Microsoft Docs'
description: Resolver problemas com alertas para os serviços de domínio do Azure AD
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 2a677d66ebd999d1bcaa4e46eb5b64fef0da006f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587433"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Serviços de domínio do Azure AD - resolver problemas com alertas
Este artigo fornece os guias de resolução de problemas de todos os alertas que pode deparar-se no seu domínio gerido.


Escolha os passos de resolução de problemas que correspondem para o ID ou a mensagem no alerta.

| **ID de alerta** | **Mensagem de alerta** | **Resolução** |
| --- | --- | :--- |
| AADDS001 | *LDAP seguro através da internet está ativada para o domínio gerido. No entanto, o acesso à porta 636 não está bloqueado para baixo a utilização de um grupo de segurança de rede. Isto poderá expor contas de utilizador no domínio gerido para ataques de força bruta de palavra-passe.* | [Configuração de LDAP segura incorreta](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *O diretório do Azure AD associado ao seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [Diretório em falta](#aadds100-missing-directory) |
| AADDS101 | *Serviços de domínio do Azure AD não pode ser ativados um diretório do Azure AD B2C.* | [O Azure AD B2C está em execução neste diretório](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Um Principal de serviço necessária para os serviços de domínio do Azure AD para funcionarem corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [Falta o Principal de serviço](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *O intervalo de endereços IP para a rede virtual em que tiver ativado os serviços de domínio do Azure AD é um intervalo de IP público. Serviços de domínio do AD do Azure tem de estar ativados numa rede virtual com um intervalo de endereços IP privado. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [O endereço é um intervalo de IP público](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *A Microsoft está não é possível aceder aos controladores de domínio para este domínio gerido. Isto pode acontecer se um grupo de segurança de rede (NSG) configurado na sua rede virtual bloqueia o acesso a domínio gerido. Outra razão possível é se houver uma rota definida pelo utilizador que tráfego de entrada de blocos da internet.* | [Erro de rede](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *O principal de serviço com o ID de aplicação "d87dcbc6-a371-462e-88e3-28ad15ec4e64" foi eliminado e, em seguida, recriado. A recriação deixa atrás inconsistentes permissões nos recursos de serviços de domínio do Azure AD necessários para atender o seu domínio gerido. Sincronização de palavras-passe no seu domínio gerido poderão ser afetada.* | [A aplicação de sincronização de palavra-passe está desatualizada](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *O domínio gerido última sincronização com o Azure AD no [date]. Os utilizadores poderão não conseguir iniciar sessão no domínio gerido ou associações a grupos podem não estar sincronizadas com o Azure AD.* | [Ainda não ocorreu sincronização no tempo](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *O domínio gerido última cópia de segurança no [date].* | [Uma cópia de segurança ainda não foram executada no tempo](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *O certificado LDAP seguro para o domínio gerido irá expirar no XX.* | [Certificado LDAP seguro prestes a expirar](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *O domínio gerido está suspensa porque a subscrição do Azure associada ao domínio não está ativa.* | [Suspensão devido a subscrição desativada](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *O domínio gerido está suspensa devido a uma configuração inválida. O serviço foi não é possível gerir, patches, ou atualizar os controladores de domínio para o seu domínio gerido durante muito tempo.* | [Suspensão devido a uma configuração inválida](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Falta o diretório
**Mensagem de alerta:**

*O diretório do Azure AD associado ao seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

**Resolução:**

Este erro é normalmente causado por incorretamente mover a sua subscrição do Azure para um novo Azure diretório AD e eliminar o antigo diretório do Azure AD que está associado a serviços de domínio do Azure AD.

Este erro é irrecuperável. Para resolver, terá [eliminar o seu domínio gerido existente](active-directory-ds-disable-aadds.md) -los e recriá-lo no seu novo diretório. Se estiver a ter problemas a eliminar, contacte a equipa de produto do Azure Active Directory Domain Services [para suporte](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: O Azure AD B2C está em execução neste diretório
**Mensagem de alerta:**

*Serviços de domínio do Azure AD não pode ser ativados um diretório do Azure AD B2C.*

**Resolução:**

>[!NOTE]
>Para poder continuar a utilizar os serviços de domínio do Azure AD, tem de recriar a instância de serviços de domínio do Azure AD num diretório não do Azure AD B2C.

Para restaurar o seu serviço, siga estes passos:

1. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do diretório do Azure AD existente.
2. Crie um novo diretório que não é um diretório do Azure AD B2C.
3. Siga o [introdução](active-directory-ds-getting-started.md) guia-o para recriar um domínio gerido.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: O endereço é um intervalo de IP público

**Mensagem de alerta:**

*O intervalo de endereços IP para a rede virtual em que tiver ativado os serviços de domínio do Azure AD é um intervalo de IP público. Serviços de domínio do AD do Azure tem de estar ativados numa rede virtual com um intervalo de endereços IP privado. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

**Resolução:**

> [!NOTE]
> Para resolver este problema, tem de eliminar o seu domínio gerido existente e recrie-a numa rede virtual com um intervalo de endereços IP privado. Este processo é acontece.

Antes de começar, leia o **privado espaço do endereço IP v4** secção [neste artigo](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Dentro da rede virtual, máquinas poderão fazer pedidos para recursos do Azure que se encontrem no mesmo intervalo de endereços IP como os que são configurados para a sub-rede. No entanto, uma vez que a rede virtual está configurada para este intervalo, esses pedidos serão encaminhados dentro da rede virtual e não serão atingir os recursos de web pretendido. Esta configuração pode resultar em erros imprevisíveis com os serviços de domínio do Azure AD.

**Se for o proprietário do intervalo de endereços IP na internet que está configurada na sua rede virtual, este alerta pode ser ignorado. No entanto, os serviços de domínio do Azure AD não é possível consolidar para o [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] com esta configuração, uma vez que o pode resultar em erros imprevisíveis.**


1. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do seu diretório.
2. Corrija o intervalo de endereços IP da sub-rede
  1. Navegue para o [página de redes virtuais no portal do Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecione a rede virtual que pretende utilizar para os serviços de domínio do Azure AD.
  3. Clique em **espaço de endereços** em definições
  4. Atualize o intervalo de endereços ao clicar no intervalo de endereços existente e editar ou adicionar um intervalo de endereços adicionais. Certifique-se que o novo intervalo de endereços é um intervalo de IP privado. Guarde as alterações.
  5. Clique em **sub-redes** no painel de navegação esquerdo.
  6. Clique na sub-rede que pretende editar na tabela.
  7. Atualizar o intervalo de endereços e guarde as alterações.
3. Siga [o guia de introdução iniciado através do Azure AD dos serviços de domínio](active-directory-ds-getting-started.md) para recriar o seu domínio gerido. Certifique-se de que escolha uma rede virtual com um intervalo de endereços IP privado.
4. A associação de domínio suas máquinas virtuais para o novo domínio, siga [neste guia](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Para garantir que o alerta é resolvido, verifique o estado de funcionamento do seu domínio nas duas horas.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: A sincronização não foi concluída no tempo

**Mensagem de alerta:**

*O domínio gerido última sincronização com o Azure AD no [date]. Os utilizadores poderão não conseguir iniciar sessão no domínio gerido ou associações a grupos podem não estar sincronizadas com o Azure AD.*

**Resolução:**

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na configuração do seu domínio gerido. Por vezes, com problemas de configuração podem bloquear a capacidade da Microsoft para sincronizar o seu domínio gerido. Se não for capaz de resolver todos os alertas, aguarde duas horas e verifique a fazer uma cópia para ver se a sincronização foi concluída.


## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Uma cópia de segurança não tiver sido colocada no tempo

**Mensagem de alerta:**

*O domínio gerido última cópia de segurança no [date].*

**Resolução:**

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na configuração do seu domínio gerido. Por vezes, com problemas de configuração podem bloquear a capacidade da Microsoft para sincronizar o seu domínio gerido. Se não for capaz de resolver todos os alertas, aguarde duas horas e verifique a fazer uma cópia para ver se a sincronização foi concluída.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Suspensão devido a subscrição desativada

**Mensagem de alerta:**

*O domínio gerido está suspensa porque a subscrição do Azure associada ao domínio não está ativa.*

**Resolução:**

Para restaurar o seu serviço, [renovar a sua subscrição do Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) associada do seu domínio gerido.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Suspensão devido a uma configuração inválida

**Mensagem de alerta:**

*O domínio gerido está suspensa devido a uma configuração inválida. O serviço foi não é possível gerir, patches, ou atualizar os controladores de domínio para o seu domínio gerido durante muito tempo.*

**Resolução:**

[Verificar estado de funcionamento do seu domínio](active-directory-ds-check-health.md) para todos os alertas que possam indicar problemas na configuração do seu domínio gerido. Se pode resolver qualquer um destes alertas, faça-o. Depois, contacte o suporte para reativar a sua subscrição.

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
