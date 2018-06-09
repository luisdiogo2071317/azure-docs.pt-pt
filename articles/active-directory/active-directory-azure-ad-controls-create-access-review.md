---
title: Criar uma revisão do acesso para os membros de um grupo ou os utilizadores com acesso a uma aplicação com o Azure AD | Microsoft Docs
description: Saiba como criar uma revisão do acesso para os membros de um grupo ou os utilizadores com acesso a uma aplicação.
services: active-directory
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: compliance-reports
ms.date: 05/14/2018
ms.author: rolyon
ms.openlocfilehash: 6a1d4a55074630cc20c98202d2e94ec682c323f4
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234078"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Criar uma revisão do acesso dos membros do grupo ou de acesso de aplicação com o Azure AD

Atribuições de acesso tornar-se "obsoletas" quando os utilizadores têm acesso não precisam de mais. Para reduzir os riscos associados atribuições de acesso obsoletos, os administradores podem utilizar o Azure Active Directory (Azure AD) para criar uma revisão do acesso para os membros do grupo ou os utilizadores atribuídos a uma aplicação. Criar periódica revisões de acesso pode ser tempo guardar. Se precisar de reveja regularmente os utilizadores que têm acesso a uma aplicação ou que são membros de um grupo, pode definir a frequência dessas revisões. Para obter mais informações sobre estes cenários, consulte [gerir o acesso de utilizador](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) e [gerir o acesso de convidado](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Criar uma revisão de acesso

1. Como administrador global, vá para o [acesso revê página](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)e selecione **programas**.

2. Seleccione o programa que contém o controlo de revisão de acesso que pretende criar. **Programa predefinido** sempre estiver presente, ou pode criar um programa diferente. Por exemplo, pode optar por ter um programa para cada iniciativa de compatibilidade ou o objetivo de negócio.

3. O programa, selecione **controlos**e, em seguida, selecione **adicionar** para adicionar um controlo.

4. Nome a revisão do acesso. Opcionalmente, forneça a revisão uma descrição. O nome e descrição são apresentadas para os revisores.

5. Defina a data de início. Por predefinição, uma revisão do acesso ocorre uma vez, inicia o mesmo tempo que é criado e -termina num mês. Pode alterar o início e fim de datas para ter acesso reveja início no futuro e pela última vez no entanto, o número de dias que pretende.

6. Para tornar o recorrente de revisão de acesso, altere a frequência de uma vez para semanais, mensais, trimestrais ou anual e utilize a caixa de texto ou controlo de deslize para definir quantos dias cada revisão da série periódica será aberta para entrada de revisores. Por exemplo, a duração máxima para pode ser definido para uma revisão mensal é 27 dias, para evitar a sobreposição de revisões. 

7.  A série de revisão periódica do acesso pode terminar formas 3: é executada continuamente para iniciar revisões indefinidamente, até uma data específica ou um determinado número de ocorrências tenha sido concluída. O utilizador ou outro administrador global pode parar a série após a criação, alterando a data nas definições, para que este termina a essa data.

8. Revisões de acesso podem ser membros de um grupo ou para os utilizadores que foram atribuídos a uma aplicação. Pode ainda mais definir o âmbito de acesso revisão para revisão apenas os utilizadores convidados que são membros (ou atribuídos à aplicação), em vez de rever todos os utilizadores que são membros ou que tenham acesso à aplicação.

9. Selecione um ou mais pessoas para rever todos os utilizadores no âmbito. Ou, pode optar por ter membros rever os suas próprias acesso. Se o recurso é um grupo, pode colocar os proprietários de grupo para rever. Também pode exigir que os revisores de fornecer um motivo quando estes aprovar acesso.

10. Se pretender aplicar manualmente os resultados ao concluir a revisão, clique em **iniciar**.  Caso contrário, a secção seguinte explica como configurar a revisão como auto se aplicam.

### <a name="configuring-an-access-review-with-auto-apply"></a>Configurar uma revisão do acesso com auto-apply

1.  Expanda o menu para após a conclusão definições e ativar automaticamente aplicam-se os resultados ao recurso. 

2.  Em casos onde os utilizadores não foram revistos pelo revisor dentro do período de revisão, pode ter a revisão do acesso de colocar recomendação do sistema (se estiver ativada) no negar/aprovar o acesso do utilizador contínuo, deixe o respetivo acesso inalterado ou remover os respetivos acesso. Isto não irá afetar os utilizadores que foram revistos, os revisores manualmente – esteja decisão do revisor final negar, em seguida, o acesso do utilizador será removido.

3.  Para ativar a opção para efetuar recomendações devem revisores não responder, expanda definições avançadas e ativar Mostrar recomendações.
 
4.  Por fim, clique em **iniciar**.

Com base nas suas seleções nas definições de conclusão,-aplicar automaticamente será executado após a data de fim da revisão ou quando parar manualmente a revisão. O estado da revisão deixará de concluído através de Estados intermédios como Applying e, finalmente, para o estado aplicada. Deverá ver negados utilizadores, se existir, a serem removidos da atribuição de aplicação ou de associação de grupo dentro de alguns minutos.


## <a name="manage-the-access-review"></a>Gerir a revisão do acesso

Por predefinição, o Azure AD envia um e-mail a revisores imediatamente após a revisão é iniciado. Se optar por não ter o Azure AD enviar o e-mail, lembre-se de que a informar os revisores uma revisão do acesso está a aguardar a sua conclusão. Pode mostrá-los as instruções sobre como [rever o acesso](active-directory-azure-ad-controls-perform-access-review.md). Se estiver a rever para convidados rever os suas próprias acesso, apresentar as instruções sobre como [rever o suas próprias acesso](active-directory-azure-ad-controls-perform-access-review.md).

Se alguns dos revisores tiverem os convidados, os convidados são notificados por e-mail apenas se tiver aceitaram já os respetivos convite.

Para gerir uma série de revisões de acesso, navegue para a revisão do acesso da **controlos**, e irá encontrar futuras ocorrências no revisões agendada e edite a data de fim ou adicionar/remover revisores em conformidade. 

Pode monitorizar o progresso conforme os revisores concluir as revisões no dashboard do Azure AD no **acesso revê** secção. Sem direitos de acesso são alterados no diretório até [concluída a revisão](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Passos Seguintes

Quando uma revisão do acesso é iniciado, o Azure AD envia automaticamente revisores uma mensagem de e-mail que lhe solicita informações para rever o acesso. Se um utilizador não receber uma mensagem de e-mail, pode enviar-lhes instruções sobre como [rever o acesso](active-directory-azure-ad-controls-perform-access-review.md). 

Se esta for uma revisão de uso individual, em seguida, após o período de revisão do acesso ou o administrador para a revisão do acesso, siga os passos no [concluir uma revisão do acesso](active-directory-azure-ad-controls-complete-access-review.md) para ver e aplicar os resultados.  

Se se tratar de uma série de revisão, em seguida, navegue para **rever histórico** na página de série de revisão do acesso, para selecionar uma revisão do acesso concluída.  Revisões futuros serão listadas sob **agendada revisão**, onde pode editar a duração e adicionar ou remover os revisores de revisões individuais.
