---
title: Inscrição self-service ou de avaliação no Azure Active Directory | Documentos da Microsoft
description: Utilizar a inscrição self-service num inquilino do Azure Active Directory (Azure AD)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 01/28/2018
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 99c5e99fa3bd33ef42e8df6ceba5be4be2cd1249
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872272"
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>O que é a inscrição self-service do Azure Active Directory?
Este artigo explica a inscrição self-service e como para suportá-la no Azure Active Directory (Azure AD). Se pretender assumir um nome de domínio a partir de um Azure AD não gerido de inquilinos, consulte [assumir um diretório não gerido como administrador](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Porquê utilizar a inscrição self-service?
* Leve os clientes para serviços que eles querem mais rapidamente
* Criar ofertas com base no e-mail para um serviço
* Criar fluxos de inscrição com base no e-mail que rapidamente permitem aos utilizadores criar identidades com seus aliases de e-mail de trabalho fácil de memorizar
* Um self-service criadas diretório do Azure AD pode ser transformado num diretório gerido que pode ser utilizado para outros serviços

## <a name="terms-and-definitions"></a>Termos e definições
* **Inscrição self-service**: Este é o método pelo qual um utilizador se inscreve num serviço em nuvem e tem uma identidade criada automaticamente para os mesmos no Azure AD com base no seu domínio de e-mail.
* **Não gerido diretório do Azure AD**: Este é o diretório onde essa identidade é criada. Um diretório não gerido é um diretório com nenhum administrador global.
* **Utilizador verificado por e-mail**: Este é um tipo de conta de utilizador no Azure AD. Um utilizador que têm uma identidade criada automaticamente depois de se inscrever para uma oferta de self-service é conhecido como um utilizador verificado por e-mail. Um utilizador verificado por e-mail é um membro regular de um diretório etiquetado com creationmethod = EmailVerified.

## <a name="how-do-i-control-self-service-settings"></a>Como posso controlar definições de gestão personalizada?
Os administradores têm dois controles de Self-serviços hoje mesmo. Poderá controlar se:

* Os utilizadores podem associar o diretório através de e-mail.
* Os utilizadores podem licenciar os próprios para aplicativos e serviços.

### <a name="how-can-i-control-these-capabilities"></a>Como posso controlar esses recursos?
Um administrador pode configurar estas capacidades utilizando os seguintes parâmetros de cmdlet Set-MsolCompanySettings do Azure AD:

* **AllowEmailVerifiedUsers** controla se um utilizador pode criar ou associar um diretório não gerido. Se definir esse parâmetro como $false, nenhum utilizador verificado por e-mail pode associar o diretório.
* **AllowAdHocSubscriptions** controla a capacidade dos utilizadores efetuar a inscrição self-service. Se definir esse parâmetro como $false, os utilizadores não podem efetuar a inscrição self-service. 
  
  > [!NOTE]
  > Flow e PowerApps inscrições de avaliação não são controladas mediante a **AllowAdHocSubscriptions** definição. Para obter mais informações, veja os artigos seguintes:
  > * [Como posso impedir os utilizadores existentes comecem a utilizar o Power BI?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
  > * [Flow na sua organização, as perguntas e respostas](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Como os controles funcionam em conjunto?
Esses dois parâmetros podem ser utilizados em conjunto para definir um controlo mais preciso sobre a inscrição self-service. Por exemplo, o seguinte comando irá permitir aos utilizadores efetuarem a inscrição self-service, mas apenas se os utilizadores que já tem uma conta no Azure AD (em outras palavras, os utilizadores que seriam necessário uma conta verificado por e-mail a ser criado pela primeira vez não é possível efetuar inscrição self-service):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
O fluxograma a seguir explica as diferentes combinações para estes parâmetros e as condições resultantes para o diretório e a inscrição self-service.

![][1]

Para obter mais informações e exemplos de como utilizar estes parâmetros, consulte [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Passos Seguintes
* [Adicionar um nome de domínio personalizado ao Azure AD](../fundamentals/add-custom-domain.md)
* [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referência de Cmdlet do Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/directory-self-service-signup/SelfServiceSignUpControls.png
