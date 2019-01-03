---
title: 'Sincronização do Azure AD Connect: Ativar a Reciclagem do AD | Documentos da Microsoft'
description: Este tópico recomenda a utilização da funcionalidade de reciclagem do AD com o Azure AD Connect.
services: active-directory
keywords: Reciclagem do AD, a eliminação acidental, âncora de origem
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/17/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d1a09c077adbc3c9176740bce69bf051a912bc22
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555579"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Sincronização do Azure AD Connect: Ativar a reciclagem do AD
Recomenda-se que ative a funcionalidade de reciclagem do AD para os diretórios de Active Directory no local, que são sincronizados com o Azure AD. 

Caso tenha eliminado acidentalmente uma local do objeto de utilizador do AD e o restauro-lo a utilizar a funcionalidade, os do Azure AD que restaura o objeto de utilizador do Azure AD correspondente.  Para obter informações sobre a funcionalidade de reciclagem do AD, consulte o artigo [descrição geral do cenário para restaurar eliminar objetos do Active Directory](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Benefícios da ativação da Reciclagem do AD
Esta funcionalidade ajuda com o restauro de objetos de utilizador do Azure AD, fazendo o seguinte:

* Caso tenha eliminado acidentalmente uma local do objeto de utilizador do AD, o objeto de utilizador do Azure AD correspondente será eliminado no próximo ciclo de sincronização. Por predefinição, o Azure AD mantém o eliminado objeto de utilizador do Azure AD num Estado eliminado de forma recuperável durante 30 dias.

* Se tiver no local AD reciclar a funcionalidade de reciclagem ativada, pode restaurar a eliminada no local o objeto de utilizador do AD sem alterar o valor de âncora de origem. Quando a recuperada no local o objeto de utilizador do AD está sincronizado com o Azure AD, o Azure AD será objeto de utilizador de restauro a correspondente eliminados de forma recuperável do Azure AD. Para obter informações sobre o atributo de âncora de origem, consulte o artigo [do Azure AD Connect: Conceitos de design](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Se não tiver no local a funcionalidade de reciclagem do AD ativada, poderá ser necessária para criar um objeto de utilizador do AD para substituir o objeto excluído. Se o serviço do Azure AD Connect sincronização estiver configurado para utilizar o atributo de AD gerados pelo sistema (por exemplo, ObjectGuid) para o atributo de âncora de origem, o objeto de utilizador do AD criado recentemente não terão o mesmo valor de âncora de origem como o objeto de utilizador foi eliminado do AD. Quando o objeto de utilizador do AD criado recentemente é sincronizado com o Azure AD, o AD do Azure cria um novo objeto de utilizador do Azure AD em vez de restaurar o eliminados de forma recuperável objeto de utilizador do Azure AD.

> [!NOTE]
> Por predefinição, o Azure AD mantém eliminados objetos de utilizador do Azure AD num Estado eliminado de forma recuperável durante 30 dias antes de serem eliminados permanentemente. No entanto, os administradores podem acelerar a eliminação de tais objetos. Depois dos objetos são eliminados permanentemente, já não podem ser recuperados, mesmo no local a funcionalidade de reciclagem do AD está ativada.

>[!NOTE]
>Anteriormente quando um utilizador foi removido do âmbito de sincronização no local e eliminado na cloud, o estado de DirSyncEnabled da conta foi incorretamente definido como "False". Se, posteriormente, esse utilizador manualmente foi restaurado da Reciclagem do Azure AD, este era um Estado incorreto da conta de "Apenas na Cloud". Isso agora foi corrigido e o valor do Estado DirSyncEnabled é sempre mantido "True" quando um utilizador é removido do âmbito de sincronização, em seguida, eliminados de forma recuperável e manualmente restaurada da Reciclagem do Azure AD.

## <a name="next-steps"></a>Passos Seguintes
**Tópicos de descrição geral**

* [Sincronização do Azure AD Connect: Compreender e personalizar a sincronização](how-to-connect-sync-whatis.md)

* [Integrar as identidades no local ao Azure Active Directory](whatis-hybrid-identity.md)
