---
title: Criar uma revisão de acesso para membros de um grupo ou os utilizadores com acesso a uma aplicação com o Azure AD | Documentos da Microsoft
description: Saiba como criar uma revisão de acesso para membros de um grupo ou os utilizadores com acesso a uma aplicação.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: e52686a8cfd691a7f82c6e64968cd94634fd596e
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607795"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Criar uma revisão de acesso de membros do grupo ou o acesso de aplicação com o Azure AD

As atribuições de acesso se tornar "obsoletas" quando os utilizadores têm acesso não precisam mais. Para reduzir o risco associado atribuições de acesso obsoletos, os administradores podem utilizar o Azure Active Directory (Azure AD) para criar uma revisão de acesso para membros do grupo ou os utilizadores atribuídos a uma aplicação. A criação periódica revisões de acesso pode ser economia de tempo. Se precisar de reveja regularmente os utilizadores que têm acesso a uma aplicação ou são membros de um grupo, pode definir a frequência dessas revisões. Para obter mais informações sobre estes cenários, consulte [gerir o acesso de utilizador](manage-user-access-with-access-reviews.md) e [gerir o acesso de convidado](manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Criar uma revisão de acesso

1. Como um administrador global ou administrador de conta de utilizador, vá para o [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)e selecione **programas**.

2. Selecione o programa que mantém o controlo de revisão de acesso que pretende criar. **Programa predefinido** sempre esteja presente, ou pode criar um programa diferente. Por exemplo, pode optar por ter um programa para cada iniciativa de conformidade ou de suas metas de negócios.

3. Dentro do programa, selecione **controles**e, em seguida, selecione **Add** para adicionar um controlo.

4. Nome da revisão de acesso. Opcionalmente, forneça a revisão de uma descrição. O nome e descrição são apresentadas aos revisores.

5. Defina a data de início. Por predefinição, uma revisão de acesso ocorre uma vez, é iniciada ao mesmo tempo, que é criado e termina dentro de um mês. Pode alterar o início e datas de término para ter um acesso rever início no futuro e pela última vez no entanto, o número de dias que pretende.

6. Para tornar a revisão de acesso recorrente, alterar a frequência de uma vez para semanalmente, mensalmente, trimestralmente ou anualmente e utilize a caixa de texto ou de controlo de deslize para definir o número de dias cada revisão da série recorrente será aberta para entrada de revisores. Por exemplo, a duração máxima para pode definir para uma revisão mensal é 27 dias, para evitar a sobreposição de revisões. 

7.  A série de revisão de acesso recorrentes pode ser encerrada de 3 formas: ela seja executada continuamente para iniciar as revisões indefinidamente, até uma data específica ou após um determinado número de ocorrências de conclusão. , Outro administrador de conta de utilizador ou outro administrador global pode parar a série após a criação, alterando a data nas definições, para que ele termina a essa data.

8. As revisões de acesso podem ser para os membros de um grupo ou para os utilizadores que foram atribuídos a uma aplicação. Pode ainda mais definir o âmbito de acesso apenas de revisão para rever os utilizadores convidados que são membros (ou atribuídos à aplicação), em vez de examinar todos os utilizadores que são membros ou que têm acesso à aplicação.

9. Selecione um ou mais pessoas para rever todos os utilizadores no âmbito. Ou pode selecionar para que os membros, reveja o seu próprio acesso. Se o recurso é um grupo, é possível pedir os proprietários do grupo para rever. Também pode exigir que os revisores fornecem um motivo quando eles aprovar o acesso.

10. Se desejar aplicar manualmente os resultados quando a revisão terminar, clique em **iniciar**.  Caso contrário, a seção seguinte explica como configurar a revisão automaticamente aplicar.

### <a name="configuring-an-access-review-with-auto-apply"></a>Configurar uma revisão de acesso com auto-apply

1.  Expanda o menu para após as definições de conclusão e ativar automaticamente aplicar resultados ao recurso. 

2.  Em casos em que os utilizadores não foram revistos pelo revisor dentro do período de revisão, pode ter a revisão de acesso por levar a recomendação do sistema (se ativada) ao negar/aprovar acesso contínuo do utilizador, deixe o respetivo acesso inalterado ou remover seus acesso. Isto não irá afetar os utilizadores que foram revistos, os revisores manualmente – se decisão do revisor final é negar, em seguida, o acesso do utilizador será removido.

3.  Para ativar a opção efetuar recomendações devem os revisores não responder, expanda as definições avançadas e ativar as recomendações de mostrar.
 
4.  Por fim, clique em **iniciar**.

Com base nas suas seleções após as definições de conclusão,-aplicar automaticamente irá ser executado após a data de fim a revisão ou quando manualmente para a revisão. O estado da revisão deixará de concluído por meio de Estados intermediários como aplicar e, finalmente, para o estado aplicado. Deve esperar para ver utilizadores sem permissão, caso haja algum, que está a ser removido a atribuição de aplicação ou associação de grupo dentro de alguns minutos.


## <a name="manage-the-access-review"></a>Gerir a revisão de acesso

Por predefinição, o Azure AD envia um e-mail para os revisores logo após a revisão é iniciado. Se optar por não ter o Azure AD para enviar o e-mail, certifique-se de que informe os revisores de que uma revisão de acesso está a aguardar a sua conclusão. Pode mostrar-lhes as instruções sobre como a [rever acesso](perform-access-review.md). Se sua revisão for para convidados rever o seu próprio acesso, mostrar-lhes as instruções sobre como a [rever o seu próprio acesso](perform-access-review.md).

Se alguns dos revisores são convidados, convidados são notificados por e-mail apenas se eles já já aceitou o convite.

Para gerir uma série de revisões de acesso, navegue para a revisão de acesso de **controles**, e será encontrar ocorrências futuras nas revisões de agendada e edite a data de fim ou adicionar/remover os revisores da mesma forma. 

Pode monitorizar o progresso conforme os revisores de concluir as revisões no dashboard do Azure AD no **revisões de acesso** secção. Nenhum direito de acesso forem alterado no diretório até [a revisão é concluída](complete-access-review.md).

## <a name="next-steps"></a>Passos Seguintes

Quando tiver iniciado uma revisão de acesso, o Azure AD envia automaticamente os revisores de uma mensagem de e-mail que lhe pede para rever o acesso. Se um utilizador não recebeu uma mensagem de e-mail, pode enviar-lhes as instruções sobre como a [rever acesso](perform-access-review.md). 

Se se tratar de uma revisão de uso individual, em seguida, após o período de revisão de acesso ou o administrador para a revisão de acesso, siga os passos em [concluir uma revisão de acesso](complete-access-review.md) para ver e aplicar os resultados.  

Se se tratar de uma série de revisão, em seguida, navegue até **rever histórico** na página de série de revisão de acesso, para selecionar uma revisão de acesso concluída.  Revisões futuras serão listadas sob **agendada revisão**, onde pode editar o período de tempo e adicionar ou remover os revisores de revisões individuais.
