---
title: Conclua uma revisão do acesso dos membros de um grupo ou o acesso dos utilizadores a uma aplicação com o Azure AD | Microsoft Docs
description: Saiba como realizar uma revisão do acesso para os membros de um grupo ou os utilizadores com acesso a uma aplicação no Azure Active Directory.
services: active-directory
documentationcenter: ''
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: compliance-reports
ms.date: 05/02/2018
ms.author: rolyon
ms.openlocfilehash: f34435e018e6cb30d51866cdd3182d86b148ba94
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235027"
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Conclua uma revisão do acesso dos membros de um grupo ou o acesso dos utilizadores a uma aplicação no Azure AD

Os administradores podem utilizar o Azure Active Directory (Azure AD) para [criar uma revisão de acesso](active-directory-azure-ad-controls-create-access-review.md) para membros do grupo ou os utilizadores atribuídos a uma aplicação. Azure AD envia automaticamente revisores uma mensagem de e-mail que lhe solicita informações para rever o acesso. Se um utilizador não receber uma mensagem de e-mail, pode enviar-lhes as instruções [rever o seu acesso](active-directory-azure-ad-controls-perform-access-review.md). (Tenha em atenção que os convidados que são atribuídos como revisores, mas não tem aceite o convite não irão receber um e-mail de revisões de acesso, como, primeiro tem de aceitar um convite antes de revisão.) O período de revisão do acesso ou se um administrador deixa a revisão do acesso, siga os passos neste artigo para ver e aplicar os resultados.

## <a name="view-an-access-review-in-the-azure-portal"></a>Ver uma revisão do acesso no portal do Azure

1. Vá para o [acesso revê página](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selecione **programas**e selecione o programa que contém o controlo de revisão do acesso.

2. Selecione **gerir**e selecione o controlo de revisão do acesso. Se existirem vários controlos no programa, pode filtrar os controlos de um tipo específico e ordená-los pelo respetivo estado. Também pode procurar pelo nome do controlo de revisão de acesso ou pelo nome a apresentar do proprietário que a criou. 

## <a name="stop-a-review-that-hasnt-finished"></a>Parar uma revisão de que não foi concluída

Se a revisão ainda não atingiu a data de fim agendada, um administrador pode selecionar **parar** para terminar a revisão antecipadamente. Depois de interromper a revisão, os utilizadores já não podem ser revistos. Não é possível reiniciar uma revisão depois está parado.

## <a name="apply-the-changes"></a>Aplicar as alterações 

Após a conclusão, uma revisão do acesso a vez que atingiu a data de fim ou um administrador parado-la manualmente e aplicam-se automaticamente não foi configurado para a análise, pode selecionar **aplicar** para aplicar as alterações manualmente. O resultado de revisão é implementado atualizando o grupo ou aplicação. Se acesso um utilizador foi negado na revisão, quando um administrador seleciona esta opção, o Azure AD remove a atribuição de associação ou aplicação. 

Depois de concluído uma revisão do acesso e aplicam-se automaticamente foi configurado, em seguida, o estado do Consulte deixará de concluído através de Estados intermédios e por fim, irá alterar estado aplicada. Deverá ver negados utilizadores, se aplicável, que está a ser removido do recurso do grupo de atribuição de associação ou aplicação dentro de alguns minutos.

Um configurado automaticamente aplicar revisão ou selecionar **aplicar** não tem um efeito de um grupo que origina um diretório no local ou um grupo dinâmico. Se pretender alterar um grupo que tem origem no local, transfira os resultados e aplicar essas alterações para a representação do grupo no diretório.

## <a name="download-the-results-of-the-review"></a>Transferir os resultados de revisão

Para obter os resultados de revisão, selecione **aprovações** e, em seguida, selecione **transferir**. O ficheiro CSV resultante pode ser visualizado no Excel ou em outros programas que abra UTF-8 codificado ficheiros CSV.

## <a name="optional-delete-a-review"></a>Opcional: Eliminar uma revisão
Se já não estiver interessado em de revisão, podem eliminá-lo. Selecione **eliminar** para remover a revisão do Azure AD.

> [!IMPORTANT]
> Não há nenhum aviso antes de ocorre a eliminação, por isso, lembre-se de que pretende eliminar a revisão.
> 
> 

## <a name="next-steps"></a>Passos Seguintes

- [Gerir o acesso de utilizador com as revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Gerir o acesso de convidado com as revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Gerir programas e controlos de revisões de acesso do Azure AD](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Criar uma revisão de acesso para os membros de um grupo ou o acesso a uma aplicação](active-directory-azure-ad-controls-create-access-review.md)
- [Criar uma revisão de acesso de utilizadores numa função administrativa do Azure AD](active-directory-privileged-identity-management-how-to-start-security-review.md)
