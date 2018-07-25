---
title: Definições de MDM e de política de grupo | Documentos da Microsoft
description: Fornece informações sobre a política de grupo e dispositivos móveis definições de gestão (MDM), que devem ser utilizadas em dispositivos pertencentes à empresa. Estas políticas são aplicadas para o dispositivo do utilizador inteiro.
services: active-directory
keywords: o que são o grupo de política e definições de MDM para o Roaming de estado empresarial, Roaming de estado empresarial, cloud do windows
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.openlocfilehash: 9db0fa29f6af0053d45f9f0238b52ac34fdb464a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223266"
---
# <a name="group-policy-and-mdm-settings"></a>Definições de política de grupo e MDM
Use esses diretiva de grupo e definições de gestão (MDM) de dispositivos móveis apenas em dispositivos pertencentes à empresa uma vez que estas políticas são aplicadas para o dispositivo do utilizador inteiro. Aplicar uma política MDM para desativar a sincronização de definições para um pessoal, dispositivos pertencentes ao utilizador irão afetar negativamente a utilização desse dispositivo. Além disso, outras contas de utilizador no dispositivo também serão afetadas pela política.

As empresas que desejam gerenciar roaming para os dispositivos pessoais (não geridos) podem utilizar o portal do Azure para ativar ou desativar o roaming, em vez de utilizar a política de grupo ou a MDM.
As tabelas seguintes descrevem as definições de política disponíveis.

## <a name="mdm-settings"></a>Definições de MDM
Aplicam as definições de política MDM para Windows 10 e Windows 10 Mobile.  Existe um suporte de Windows 10 Mobile apenas para a conta Microsoft com base em roaming através de conta do OneDrive do utilizador.  Consulte a secção "Dispositivos e os pontos finais" para obter detalhes sobre os dispositivos são suportados para sincronização do Azure AD com base.

| Nome | Descrição |
| --- | --- |
| Permitir a ligação de conta Microsoft |Permite que os usuários se autentiquem com uma conta Microsoft no dispositivo |
| Permitir Sincronização de minhas configurações |Permite que os usuários sejam acedidas remotamente as definições do Windows e dados de aplicação. A desativar esta política irá desativar a sincronização, bem como as cópias de segurança em dispositivos móveis |

## <a name="group-policy-settings"></a>Definições de política de grupo
Aplicam as definições de política de grupo para dispositivos Windows 10 que estão associados a um domínio do Active Directory. A tabela também inclui herdadas definições que pode parecer que gerir definições de sincronização, mas isso não funciona para o Enterprise Estado Roaming para Windows 10, que são indicadas com "Não utilize" na descrição.

| Nome | Descrição |
| --- | --- |
| Contas: Bloquear contas da Microsoft |Esta definição de política impede que os utilizadores adicionem novas contas Microsoft neste computador |
| Não sincronizar |Impede que os usuários sejam acedidas remotamente as definições do Windows e dados de aplicação |
| Não sincronizar personalizar |Desativa a sincronização do grupo de temas |
| Não sincronizar as definições do browser |Desativa a sincronização do grupo do Internet Explorer |
| Não sincronizar as palavras-passe |Desativa a sincronização do grupo de palavras-passe |
| Não sincronizar outras definições do Windows |Desativa a sincronização do grupo de definições do Windows outros |
| Não são sincronizados a personalização do ambiente de trabalho |Não utilize; não tem qualquer efeito |
| Não são sincronizados em ligações com tráfego limitado |Desativa está em roaming numa limitados de ligações, como 3G via rede móvel |
| Não sincronizar as aplicações |Não utilize; não tem qualquer efeito |
| Não sincronizar as definições da aplicação |Desativa o roaming de dados de aplicação |
| Não sincronizar as definições de início |Não utilize; não tem qualquer efeito |

## <a name="related-topics"></a>Tópicos relacionados
* [Descrição geral do Roaming de estado empresarial](active-directory-windows-enterprise-state-roaming-overview.md)
* [Ativar roaming no Azure Active Directory de estado empresarial](active-directory-windows-enterprise-state-roaming-enable.md)
* [FAQ de roaming de dados e definições](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Referência de configurações de roaming com o Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Resolução de problemas](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

