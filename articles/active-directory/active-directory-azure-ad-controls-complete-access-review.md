---
title: Concluir uma revisão de acesso de membros de um grupo ou o acesso dos utilizadores a uma aplicação com o Azure AD | Documentos da Microsoft
description: Aprenda como concluir uma revisão de acesso para membros de um grupo ou os utilizadores com acesso a uma aplicação no Azure Active Directory.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 3b8fb474f163922844cf9b28568be58cf8cf62e1
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38307354"
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Concluir uma revisão de acesso de membros de um grupo ou o acesso dos utilizadores a uma aplicação no Azure AD

Os administradores podem utilizar o Azure Active Directory (Azure AD) para [criar uma revisão de acesso](active-directory-azure-ad-controls-create-access-review.md) para membros do grupo ou os utilizadores atribuídos a uma aplicação. Azure AD envia automaticamente os revisores, uma mensagem de e-mail que lhe pede para rever o acesso. Se um utilizador não recebeu uma mensagem de e-mail, pode enviar-lhes as instruções [rever o seu acesso](active-directory-azure-ad-controls-perform-access-review.md). (Tenha em atenção que os convidados que são atribuídos como revisores, mas não aceitou o convite não irão receber um e-mail de revisões de acesso, como o primeiro tem de ser aceite um convite, antes da revisão.) Depois do período de revisão de acesso ou se um administrador parar a revisão de acesso, siga os passos neste artigo para ver e aplicar os resultados.

## <a name="view-an-access-review-in-the-azure-portal"></a>Ver uma revisão de acesso no portal do Azure

1. Vá para o [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selecione **programas**e selecione o programa que contém o controlo de revisão de acesso.

2. Selecione **gerir**e selecione o controlo de revisão de acesso. Se existirem vários controlos no programa, pode filtrar os controlos de um tipo específico e ordená-los pelo respetivo estado. Também pode procurar pelo nome do controlo de revisão de acesso ou pelo nome a apresentar do proprietário que a criou. 

## <a name="stop-a-review-that-hasnt-finished"></a>Parar uma revisão que não foi concluída

Se a revisão ainda não atingiu a data de fim agendada, um administrador pode selecionar **parar** para terminar a revisão desde o início. Depois de parar a revisão, os utilizadores já não podem ser revistos. Não é possível reiniciar uma revisão depois está parado.

## <a name="apply-the-changes"></a>Aplicar as alterações 

Uma vez concluída uma revisão de acesso, seja porque atingiu a data de fim ou um administrador parado-la manualmente e aplicam-se automaticamente não foi configurado para a revisão, pode selecionar **aplicar** para aplicar as alterações manualmente. O resultado da revisão é implementado por atualizar o grupo ou aplicação. Se o acesso de um utilizador foi negado da revisão, quando um administrador seleciona esta opção, do Azure AD remove a respetiva atribuição de associação ou aplicação. 

Depois de uma revisão de acesso é concluída e automática foi configurado, em seguida, o estado da revisão deixará de concluído por meio de Estados intermediários e por fim, será alterado para o estado aplicado. Deve esperar para ver utilizadores sem permissão, se houver, que está a ser removido do recurso de grupo associação ou atribuições da aplicação em poucos minutos.

Um configurado automaticamente aplicar revisão ou por selecionar **aplicar** não tem um efeito num grupo que tem origem num diretório no local ou um grupo dinâmico. Se pretender alterar um grupo que tem origem no local, transferir os resultados e aplicar essas alterações para a representação do grupo nesse diretório.

## <a name="download-the-results-of-the-review"></a>Transferir os resultados da revisão

Para obter os resultados da revisão, selecione **aprovações** e, em seguida, selecione **transferir**. O ficheiro CSV resultante pode ser visualizado no Excel ou em outros programas que abrem o UTF-8 codificado ficheiros CSV.

## <a name="optional-delete-a-review"></a>Opcional: Eliminar uma revisão
Se já não estiver interessado na revisão, pode eliminá-la. Selecione **eliminar** para remover a revisão do Azure AD.

> [!IMPORTANT]
> Não há nenhum aviso antes de ocorre a eliminação, por isso, certifique-se de que pretende eliminar a revisão.
> 
> 

## <a name="next-steps"></a>Passos Seguintes

- [Gerir o acesso de utilizador com as revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Gerir o acesso de convidado com as revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Gerir programas e controlos de revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Criar uma revisão de acesso para os membros de um grupo ou o acesso a uma aplicação](active-directory-azure-ad-controls-create-access-review.md)
- [Criar uma revisão de acesso de utilizadores numa função administrativa do Azure AD](privileged-identity-management/pim-how-to-start-security-review.md)
