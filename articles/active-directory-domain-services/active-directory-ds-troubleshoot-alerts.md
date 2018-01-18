---
title: "Azure Active Directory dos serviços de domínio: Resolver problemas com alertas | Microsoft Docs"
description: "Resolver problemas com alertas para os serviços de domínio do Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Serviços de domínio do Azure AD - resolver problemas com alertas
Este artigo fornece os guias de resolução de problemas de todos os alertas que pode deparar-se no seu domínio gerido.


Escolha os passos de resolução de problemas que correspondem aos ou alerta ID ou mensagem que tiver.

| **ID de alerta** | **Mensagem de alerta** | **Resolução** |
| --- | --- | :--- |
| AADDS001 | *LDAP seguro através da internet está ativada para o domínio gerido. No entanto, o acesso à porta 636 não está bloqueado para baixo a utilização de um grupo de segurança de rede. Isto poderá expor contas de utilizador no domínio gerido para ataques de força bruta de palavra-passe.* | [Configuração de LDAP segura incorreta](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *O diretório do Azure AD associado ao seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [Diretório em falta](#aadds100-missing-directory) |
| AADDS101 | *Serviços de domínio do Azure AD não pode ser ativados um diretório do Azure AD B2C.* | [O Azure AD B2C está em execução neste diretório](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Um Principal de serviço necessária para os serviços de domínio do Azure AD para funcionarem corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [Falta o Principal de serviço](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *O intervalo de endereços IP para a rede virtual em que tiver ativado os serviços de domínio do Azure AD é um intervalo de IP público. Serviços de domínio do AD do Azure tem de estar ativados numa rede virtual com um intervalo de endereços IP privado. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.* | [O endereço é um intervalo de IP público](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *A Microsoft está não é possível aceder aos controladores de domínio para este domínio gerido. Isto pode acontecer se um grupo de segurança de rede (NSG) configurado na sua rede virtual bloqueia o acesso a domínio gerido. Outra razão possível é se houver uma rota definida pelo utilizador que tráfego de entrada de blocos da internet.* | [Erro de rede](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Falta o diretório
**Mensagem de alerta:**

*O diretório do Azure AD associado ao seu domínio gerido poderá ter sido eliminado. O domínio gerido já não está a ser uma configuração suportada. Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

**Remediação:**

Este erro é normalmente causado por incorretamente mover a sua subscrição do Azure para um novo Azure diretório AD e eliminar o antigo diretório do Azure AD que está associado a serviços de domínio do Azure AD.

Este erro é irrecuperável. Para resolver, terá [eliminar o seu domínio gerido existente](active-directory-ds-disable-aadds.md) -los e recriá-lo no seu novo diretório. Se estiver a ter problemas a eliminar, contacte a equipa de produto do Azure Active Directory Domain Services [para suporte](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: O Azure AD B2C está em execução neste diretório
**Mensagem de alerta:**

*Serviços de domínio do Azure AD não pode ser ativados um diretório do Azure AD B2C.*

**Remediação:**

>[!NOTE]
>Para poder continuar a utilizar os serviços de domínio do Azure AD, tem de recriar a instância de serviços de domínio do Azure AD num diretório não do Azure AD B2C.

Para restaurar o seu serviço, siga estes passos:

1. [Eliminar o seu domínio gerido](active-directory-ds-disable-aadds.md) do diretório do Azure AD existente.
2. Crie um novo diretório que não é um diretório do Azure AD B2C.
3. Siga o [introdução](active-directory-ds-getting-started.md) guia-o para recriar um domínio gerido.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: O endereço é um intervalo de IP público

**Mensagem de alerta:**

*O intervalo de endereços IP para a rede virtual em que tiver ativado os serviços de domínio do Azure AD é um intervalo de IP público. Serviços de domínio do AD do Azure tem de estar ativados numa rede virtual com um intervalo de endereços IP privado. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

**Remediação:**

> [!NOTE]
> Para resolver este problema, tem de eliminar o seu domínio gerido existente e recrie-a numa rede virtual com um intervalo de endereços IP privado. Este processo é acontece.

Antes de começar, leia o **privado espaço do endereço IP v4** secção [neste artigo](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

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
8. Verifique o estado de funcionamento do seu domínio nas duas horas para se certificar de que concluiu os passos corretamente.


## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
