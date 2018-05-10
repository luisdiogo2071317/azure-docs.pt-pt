---
title: Configurar políticas de acesso condicional baseado no dispositivo do Azure Active Directory | Microsoft Docs
description: Saiba como configurar políticas de acesso condicional baseado no dispositivo do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 07957d5ec843c414813d69b7084915bcd70a5a61
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Configurar políticas de acesso condicional baseado no dispositivo do Azure Active Directory

Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar os utilizadores autorizados como pode aceder aos recursos. Por exemplo, pode limitar o acesso a alguns recursos para dispositivos geridos. Uma política de acesso condicional que requer um dispositivo gerido também é conhecido como política de acesso condicional baseado no dispositivo.

Este tópico explica como pode configurar políticas de acesso condicional baseado no dispositivo para aplicações do Azure AD ligados. 


## <a name="before-you-begin"></a>Antes de começar

Ligações de acesso condicional baseado no dispositivo **acesso condicional do Azure AD** e **em conjunto da gestão de dispositivos do Azure AD**. Se não estiver familiarizado com uma das seguintes áreas ainda, leia os tópicos seguintes, pela primeira vez:

- **[Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -este tópico fornece uma descrição geral conceptual do acesso condicional e a terminologia relacionada.

- **[Introdução à gestão de dispositivos no Azure Active Directory](device-management-introduction.md)**  -este tópico fornece uma descrição geral das várias opções de tem de ligar os dispositivos com o Azure AD. 



## <a name="managed-devices"></a>Dispositivos geridos  

No mundo mobile-primeiro, primeiro de nuvem, o Azure Active Directory permite início de sessão único para dispositivos, aplicações e serviços em qualquer lugar. Alguns recursos no seu ambiente, conceder acesso para os utilizadores certos poderão não ser suficientemente boas. Para além dos utilizadores certos, também poderá necessitar de que as tentativas de acesso só podem ser efetuadas através de um dispositivo gerido.

Um dispositivo gerido é um dispositivo que cumpra as normas de segurança e conformidade. Em termos simples, os dispositivos geridos são dispositivos que estão sob *algumas ordenação* do controlo organizacional. No Azure AD, o pré-requisito para um dispositivo gerido é que foi registado com o Azure AD. Registar um dispositivo cria uma identidade para o dispositivo na forma de um objeto de dispositivo. Este objeto é utilizado pelo Azure para monitorizar informações de estado sobre um dispositivo. Como administrador do Azure AD, já pode utilizar este objeto para ativar/desativar (Ativar/desativar), o estado de um dispositivo.
  
![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Para obter um dispositivo registado com o Azure AD, tem três opções:

- **[Azure AD registado dispositivos](device-management-introduction.md#azure-ad-registered-devices)**  - para obter um dispositivo pessoal registado com o Azure AD

- **[Dispositivos associados ao Azure AD](device-management-introduction.md#azure-ad-joined-devices)**  - para obter um dispositivo organizacional do Windows 10 que não está associado a um local registado AD registado com o Azure AD. 

- **[Dispositivos associados ao Azure AD híbrido](device-management-introduction.md#hybrid-azure-ad-joined-devices)**  - para obter um dispositivo Windows 10 que está associado a um local AD registado com o Azure AD.

Para se tornar um dispositivo gerido, registou um dispositivo pode ser uma versão híbrida do Azure AD associados a um dispositivo ou num dispositivo que foi marcado como compatível.  

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/47.png)


 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Exigir híbrida Azure AD dispositivos associados ao

Na sua política de acesso condicional, pode selecionar **requerem dispositivo associado ao Azure AD híbrido** para indicar que as aplicações em nuvem selecionado só podem ser acedidas através de um dispositivo gerido. 

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/10.png)

Esta definição aplica-se apenas a dispositivos Windows 10 associados a um AD no local do Azure. Apenas pode registar estes dispositivos com o Azure AD utilizando uma associação do Azure AD híbrido, que é um [automatizada processo](device-management-hybrid-azuread-joined-devices-setup.md) para obter um dispositivo Windows 10 registado. 

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/45.png)

O que faz com que uma versão híbrida do Azure AD associada ao dispositivo um dispositivo gerido?  Para dispositivos que estão associados a um local AD, presume-se que o controlo sobre estes dispositivos é imposto utilizar soluções de gestão como **System Center Configuration Manager (SCCM)** ou **(GP)depolíticadegrupo** geri-los. Porque não existe nenhum método para o Azure AD determinar se qualquer um destes métodos tenha sido aplicado a um dispositivo, que requerem um dispositivo associado ao Azure AD híbrido é um mecanismo relativamente fraco para exigir um dispositivo gerido. É até que como um administrador para avaliar se os métodos que são aplicados ao seu local associados a um domínio dispositivos são fortes constituem um dispositivo gerido se esses um dispositivo também é um dispositivo associado ao Azure AD híbrido.


## <a name="require-device-to-be-marked-as-compliant"></a>Pedir que o dispositivo seja marcado como compatível

A opção de *requer um dispositivo ser assinalado como estando em conformidade* é a forma mais fortes para pedir um dispositivo gerido.

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/11.png)

Esta opção requer um dispositivo ser registado com o Azure AD bem como para ser marcado como compatível por:
         
- Intune 
- Um dispositivo móvel de terceiros geridos sistema que gere os dispositivos Windows 10 através da integração do Azure AD 
 
![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Para um dispositivo que está marcado como compatível, pode assumir que: 

- Os sua força de trabalho utiliza para aceder aos dados da empresa de dispositivos móveis são geridos
- Utiliza a sua força de trabalho de aplicações móveis são geridas
- Informações da sua empresa estão protegidas por ajudar a controlar a forma como a sua força de trabalho acede e partilhe o mesmo
- O dispositivo e as respetivas aplicações são compatíveis com os requisitos de segurança da empresa




## <a name="next-steps"></a>Passos Seguintes

Antes de configurar uma política de acesso condicional baseado no dispositivo no seu ambiente, deve ter uma vista de olhos a [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md).

