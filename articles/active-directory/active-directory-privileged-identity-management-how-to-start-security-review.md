---
title: Como iniciar uma revisão de acesso | Documentos da Microsoft
description: Saiba como criar uma revisão de acesso para as identidades privilegiadas com a aplicação do Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 38386de86c83733e3539048de9e263ed6225eee5
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952165"
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Como iniciar uma revisão de acesso no Azure AD Privileged Identity Management
Atribuições de função tornam-se "obsoletas" quando os utilizadores tem acesso que não precisam mais privilegiado. Para reduzir o risco associado estas atribuições de função obsoleta, com função privilegiada administradores ou administradores globais regularmente devem criar as revisões de acesso para solicitar que os administradores de rever as funções que receberam os utilizadores. Este documento aborda os passos para iniciar uma revisão de acesso no Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Iniciar uma revisão de acesso
> [!NOTE]
> Se ainda não adicionou a aplicação de PIM ao dashboard no portal do Azure, veja os passos no [introdução ao Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Na página principal do aplicativo PIM, existem três formas de iniciar uma revisão de acesso:

* **As revisões de acesso** > **adicionar**
* **Funções** > **revisão** botão
* Selecione a função específica para ser revisadas na lista de funções > **revisão** botão

Quando clica no **reveja** botão, o **iniciar uma revisão de acesso** é apresentado o painel. Neste painel, vai configurar a revisão com um nome e o limite de tempo, escolher uma função para rever e decida quem as executará a revisão.

![Iniciar uma revisão de acesso - captura de ecrã](./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Configurar a revisão
Para criar uma revisão de acesso, é preciso um nome e defina uma data de início e de fim.

![Configurar examine - captura de ecrã](./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png)

Verifique o comprimento da revisão tempo suficiente para os utilizadores para concluí-la. Se concluir antes da data de fim, pode sempre pare a revisão desde o início.

### <a name="choose-a-role-to-review"></a>Escolher uma função para rever
Cada revisão se concentra numa única função. A menos que iniciou a revisão de acesso de um painel de função específica, terá de escolher uma função agora.

1. Navegue para **rever a associação de função**
   
    ![Rever a associação de função - captura de ecrã](./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png)
2. Escolha uma função a partir da lista.

### <a name="decide-who-will-perform-the-review"></a>Decida quem as executará a revisão
Existem três opções para a execução de uma revisão. Pode atribuir a revisão a alguém para concluir, pode fazê-lo mesmo ou pode ter cada utilizador rever o seu próprio acesso.

1. Navegue para **selecionar revisores**
   
    ![Selecionar revisores - captura de ecrã](./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png)
2. Escolha uma das opções:
   
   * **Selecione revisor**: Utilize esta opção se não sabe quem precisa de acesso. Com esta opção, pode atribuir a revisão a um proprietário do recurso ou o Gestor de grupo para concluir.
   * **-Me**: útil se quiser pré-visualizar como das revisões de acesso, ou se pretender rever em nome de pessoas que não é possível.
   * **Membros Rever próprios**: Utilize esta opção para que os utilizadores, reveja as suas próprias atribuições de funções.

### <a name="start-the-review"></a>Inicie a revisão
Finalmente, tem a opção para exigir que os utilizadores forneçam um motivo se eles aprovar o acesso. Adicione uma descrição da revisão se assim o desejar e selecione **iniciar**.

Certifique-se de que permitir que os utilizadores saber que existe uma revisão de acesso, esperando que eles e mostrar-lhes [como realizar uma revisão de acesso](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Gerir a revisão de acesso
Pode monitorizar o progresso conforme os revisores de concluir as revisões no dashboard do Azure AD PIM, na secção de revisões de acesso. Nenhum direito de acesso será alterado no diretório até [a revisão terminar](active-directory-privileged-identity-management-how-to-complete-review.md).

Até que o período de avaliação está acima, pode relembrar aos utilizadores para concluir a revisão ou pare a revisão desde o início da seção de revisões de acesso.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>Tabela PIM de conteúdos
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
