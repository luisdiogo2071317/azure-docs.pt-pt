---
title: "Migrar políticas clássicas no portal do Azure | Microsoft Docs"
description: "Saiba o que precisa de saber para migrar políticas clássicas no portal do Azure."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 16628bd4fa41d2e7697e1c2501f2ccd31dbd0496
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migrar políticas clássicas no portal do Azure 


[Acesso condicional](active-directory-conditional-access-azure-portal.md) é uma funcionalidade do Azure Active directory (Azure AD), que lhe permite controlar como autorizado os utilizadores acedam as aplicações na nuvem. Enquanto o objetivo ainda for o mesmo, a versão do portal do Azure tem introduzidas melhorias significativas para condicional como funciona o acesso.

Deve considerar a migrar as políticas que não tiver criado no portal do Azure porque:

- Agora que pode abordar cenários que não foi possível processar antes.

- Pode reduzir o número de políticas, que tem de gerir por consolidá-los.   

- Pode gerir todas as políticas de acesso condicional numa localização central.

- Portal clássico do Azure será descontinuado.   

Este artigo explica o que precisa de saber para migrar as políticas de acesso condicional existentes para a estrutura de novo.
 
## <a name="classic-policies"></a>Políticas clássicas

No [portal do Azure](https://portal.azure.com), a [acesso condicional - políticas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) página é o ponto de entrada para o acesso condicional políticas. No entanto, no seu ambiente, poderá também ter políticas de acesso condicional que não tiver criado utilizando esta página. Estas políticas são conhecidas como *políticas clássicas*. As políticas de clássicas são políticas de acesso condicional, que criou:

- portal clássico do Azure
- Portal clássico do Intune
- O portal de proteção de aplicação do Intune


No **acesso condicional** página, pode aceder as políticas clássicas clicando [ **políticas clássico (pré-visualização)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) no **gerir** secção. 


![Azure Active Directory](./media/active-directory-conditional-access-migration/71.png)


O **políticas clássico** vista disponibiliza uma opção para:

- Filtre as políticas clássicas.
 
    ![Azure Active Directory](./media/active-directory-conditional-access-migration/72.png)

- Desative políticas clássicas.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/73.png)
   
- Reveja as definições de políticas de clássico (e desativá-los).

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/74.png)


Se tiver desativado a uma política clássica, não é possível reverter este passo já. Esta é a razão pode modificar a associação a grupos numa política clássica através de **detalhes** vista. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/75.png)

Alterando os grupos selecionados ou excluindo grupos específicos, pode testar o efeito de uma política desativada clássico para alguns utilizadores de teste antes de desativar a política para incluídos todos os utilizadores e grupos. 



## <a name="azure-ad-conditional-access-policies"></a>Políticas de acesso condicional do Azure AD

Com o acesso condicional no portal do Azure, pode gerir todas as políticas numa única localização central. Porque a implementação de acesso condicional como significativamente foi alterada, deverá familiarizar-se com os conceitos básicos antes de migrar as suas políticas clássicas.

Consulte:

- [Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md) para saber mais sobre os conceitos básicos e a terminologia.

- [Melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md) para obter algumas orientações sobre como implementar o acesso condicional na sua organização.

- [Introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) para se familiarizar com a interface de utilizador no portal do Azure.


 
## <a name="migration-considerations"></a>Considerações sobre a migração

Neste artigo, políticas de acesso condicional do Azure AD são também conhecidas como *novas políticas*.
As políticas clássicas continuam a trabalhar side by side with as novas políticas até que desative ou elimine-os. 

Os seguintes aspetos são importantes no contexto de uma consolidação de política:

- Enquanto clássicas políticas estão associadas a uma aplicação de nuvem específicas, pode selecionar como muitas aplicações na nuvem que precisar para numa política de novo.

- Controlos de uma política clássica e uma nova política para uma aplicação de nuvem necessitam de todos os controlos (*e*) para ser concluído. 


- Uma nova política, pode:
 
    - Combine várias condições se requerida pelo seu cenário. 

    - Selecione vários conceder requisitos de acesso controlarem e combinam-os com uma lógica *ou* (exigir um dos controlos selecionados) ou com uma lógica *e* (exigir todos os controlos selecionados).

        ![Azure Active Directory](./media/active-directory-conditional-access-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange online

Se pretender migrar políticas clássicas para **Exchange online do Office 365** que incluam **Exchange Active Sync** como condição de aplicações de cliente, poderá não conseguir consolidá-los para uma nova política. 

Isto é, por exemplo, caso se pretender suportar todos os tipos de aplicação de cliente. Numa política nova que tenha **Exchange Active Sync** como condição de aplicações de cliente, não é possível selecionar outras aplicações de cliente.

![Azure Active Directory](./media/active-directory-conditional-access-migration/64.png)

Uma consolidação para uma nova política também não é possível se as políticas clássicas contêm várias condições. Uma nova política tem **Exchange Active Sync** como aplicações de cliente condição configurada não suporta a outras condições:   

![Azure Active Directory](./media/active-directory-conditional-access-migration/08.png)

Se tiver uma política nova que tenha **Exchange Active Sync** como aplicações de cliente condição configurada, terá de certificar-se de que todas as outras condições não estão configuradas. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/16.png)
 

[Com base na aplicação](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) clássicas políticas para o Exchange Online do Office 365 que inclua **Exchange Active Sync** como condição de aplicações de cliente permitem **suportado** e **não suportado** [plataformas de dispositivos](active-directory-conditional-access-technical-reference.md#device-platform-condition). Embora não é possível configurar plataformas de dispositivos individuais numa política nova relacionada, pode limitar o suporte para [plataformas de dispositivos suportadas](active-directory-conditional-access-technical-reference.md#device-platform-condition) apenas. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/65.png)

Pode consolidar várias políticas clássicas que incluem **Exchange Active Sync** como condição de aplicações de cliente se tiverem:

- Apenas **Exchange Active Sync** como condição 

- Vários requisitos para a concessão de acesso configurado

Um cenário comum é a consolidação de:

- Uma política clássica baseado nos dispositivos do portal clássico do Azure 
- Uma política clássica com base na aplicação no portal de proteção da aplicação do Intune 
 
Neste caso, pode consolidar as políticas do clássicas para uma nova política, que tem ambos os requisitos selecionados.

![Azure Active Directory](./media/active-directory-conditional-access-migration/62.png)



### <a name="device-platforms"></a>Plataformas de dispositivos

Políticas clássicas com [controlos de aplicação](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) são previamente configurada com o iOS e Android, como o [condição de plataforma de dispositivo](active-directory-conditional-access-technical-reference.md#device-platform-condition). 

Uma nova política, terá de selecionar o [plataformas de dispositivos](active-directory-conditional-access-technical-reference.md#device-platform-condition) que pretende suportar individualmente.

![Azure Active Directory](./media/active-directory-conditional-access-migration/41.png)



 
 


## <a name="next-steps"></a>Passos seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 
