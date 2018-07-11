---
title: 'Sincronização do Azure AD Connect: entender e personalizar a sincronização | Documentos da Microsoft'
description: Explica como o Azure AD Connect sincronizar funciona e como personalizar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 247b7bb5b5dbce94c8f8339a6f06c8ae5dab75d9
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919372"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Sincronização do Azure AD Connect: entender e personalizar a sincronização
Os serviços de sincronização do Azure Active Directory Connect (sincronização do Azure AD Connect) é um componente principal do Azure AD Connect. Ele cuida de todas as operações relacionadas com para sincronizar dados de identidade entre o seu ambiente no local e o Azure AD. Sincronização do Azure AD Connect é o sucessor do DirSync, Azure AD Sync e Forefront Identity Manager com o conector Azure Active Directory configurado.

Este tópico é o lar dos **do Azure AD Connect** (também denominado **motor de sincronização**) e apresenta uma lista de links para todos os outros tópicos relacionados a ele. Para obter ligações para o Azure AD Connect, consulte [integrar as identidades no local com o Azure Active Directory](active-directory-aadconnect.md).

O serviço de sincronização é composta por dois componentes, no local **do Azure AD Connect** chamado de componente e do lado do serviço no Azure AD **serviço de sincronização do Azure AD Connect**.

## <a name="azure-ad-connect-sync-topics"></a>Tópicos de sincronização do Azure AD Connect
| Tópico | O que abrange e quando ler |
| --- | --- |
| **Noções básicas de sincronização do Azure AD Connect** | |
| [Compreendendo a arquitetura](active-directory-aadconnectsync-understanding-architecture.md) |Para aqueles que quem estiver familiarizado com o motor de sincronização e quiser saber mais sobre a arquitetura e os termos utilizados. |
| [Conceitos técnicos](active-directory-aadconnectsync-technical-concepts.md) |Uma versão abreviada do tópico de arquitetura e resumidamente explica os termos utilizados. |
| [Topologias do Azure AD Connect](active-directory-aadconnect-topologies.md) |Descreve os diferentes topologias e os cenários que suporta o motor de sincronização. |
| **Configuração personalizada** | |
| [Executar novamente o Assistente de instalação](active-directory-aadconnectsync-installation-wizard.md) |Explica o que opções tem disponíveis ao executar novamente o Assistente de instalação do Azure AD Connect. |
| [Noções básicas sobre o aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Descreve o modelo de configuração chamado aprovisionamento declarativo. |
| [Entender as expressões do aprovisionamento declarativas](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Descreve a sintaxe para a linguagem de expressão usada no aprovisionamento declarativo. |
| [Entender a configuração predefinida](active-directory-aadconnectsync-understanding-default-configuration.md) |Descreve as regras de out-of-box e a configuração predefinida. Também descreve como as regras funcionam em conjunto para os cenários de out-of-box trabalhar. |
| [Entender utilizadores e contactos](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Continua no tópico anterior e descreve como a configuração para utilizadores e contactos funciona em conjunto, em particular num ambiente de várias floresta. |
| [Como fazer uma alteração para a configuração predefinida](active-directory-aadconnectsync-change-the-configuration.md) |Descreve como fazer uma alteração para fluxos de atributos de configuração comuns. |
| [Melhores práticas para alterar a configuração predefinida](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Limitações do suporte e para fazer alterações à configuração de out-of-box. |
| [Configurar a Filtragem](active-directory-aadconnectsync-configure-filtering.md) |Descreve as diferentes opções para como limitar quais os objetos que estão a ser sincronizados com o Azure AD e passo a passo como configurar estas opções. |
| **Recursos e cenários** | |
| [Impedir eliminações acidentais](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Descreve os *impedir eliminações acidentais* recurso e como configurá-lo. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Descreve o agendador interno, o que está a importar, a sincronização e exportação de dados. |
| [Implementar a sincronização de hash de palavra-passe](active-directory-aadconnectsync-implement-password-hash-synchronization.md) |Descreve como funciona a sincronização de palavra-passe, como implementar e como operar e solucionar problemas. |
| [Repetição de escrita do dispositivo](active-directory-aadconnect-feature-device-writeback.md) |Descreve como funciona a repetição de escrita do dispositivo no Azure AD Connect. |
| [Extensões de diretórios](active-directory-aadconnectsync-feature-directory-extensions.md) |Descreve como expandir o esquema do Azure AD com seus próprios atributos personalizados. |
| [PreferredDataLocation do Office 365](active-directory-aadconnectsync-feature-preferreddatalocation.md) |Descreve como colocar recursos de Office 365 do utilizador na mesma região que o utilizador. |
| **Serviço de sincronização** | |
| [Funcionalidades de serviço de sincronização do Azure AD Connect](active-directory-aadconnectsyncservice-features.md) |Descreve o lado do serviço de sincronização e como alterar as definições de sincronização no Azure AD. |
| [Resiliência de atributos duplicados](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Descreve como ativar e utilizar **userPrincipalName** e **proxyAddresses** resiliência de valores de atributos duplicados. |
| **Operações e a interface do Usuário** | |
| [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) |Descreve a interface do Usuário do Synchronization Service Manager, incluindo [Operations](active-directory-aadconnectsync-service-manager-ui-operations.md), [conectores](active-directory-aadconnectsync-service-manager-ui-connectors.md), [estruturador de Metaversos](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), e [depesquisadeMetaversos](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) separadores. |
| [Considerações e tarefas operacionais](active-directory-aadconnectsync-operations.md) |Descreve as preocupações operacionais, como a recuperação após desastre. |
| **Como...** | |
| [Repor a conta do Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) |Como repor as credenciais da conta de serviço utilizada para ligar a partir do Azure AD Connect para o Azure AD. |
| **Mais informações e referências** | |
| [Portas](active-directory-aadconnect-ports.md) |Lista as portas em que precisa abrir entre o motor de sincronização e seus diretórios no local e o Azure AD. |
| [Atributos sincronizados com o Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Apresenta uma lista de todos os atributos que está a ser sincronizados entre locais AD e o Azure AD. |
| [Referência das Funções](active-directory-aadconnectsync-functions-reference.md) |Apresenta uma lista de todas as funções disponíveis no aprovisionamento declarativo. |

## <a name="additional-resources"></a>Recursos Adicionais
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
