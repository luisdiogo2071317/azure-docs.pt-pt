---
title: Como - exigem geridos os dispositivos de acesso à aplicação de nuvem com o acesso condicional do Azure Active Directory | Microsoft Docs
description: Saiba como configurar políticas de acesso condicional baseado no dispositivo do Azure Active Directory (Azure AD) que necessitam de dispositivos geridos para acesso à aplicação de nuvem.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 066d25e8953a2be4bd64cdd1af79b7f2a25dd5f9
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036359"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Como: Exigir geridos os dispositivos de acesso à aplicação de nuvem com o acesso condicional

No mundo mobile-primeiro, primeiro de nuvem, o Azure Active Directory (Azure AD) permite início de sessão único para aplicações e serviços em qualquer lugar. Os utilizadores autorizados podem aceder as aplicações na nuvem de uma vasta gama de dispositivos, incluindo mobile bem como dispositivos pessoais. No entanto, muitos ambientes tem, pelo menos, algumas aplicações que só devem ser acedidas pelos dispositivos que cumprem as normas de segurança e conformidade. Estes dispositivos são também conhecidos como os dispositivos geridos. 

Este artigo explica como pode configurar políticas de acesso condicional que necessitam de dispositivos geridos para aceder a determinadas aplicações em nuvem no seu ambiente. 


## <a name="prerequisites"></a>Pré-requisitos

A necessidade de dispositivos geridos para ligações de acesso de aplicação de nuvem **acesso condicional do Azure AD** e **gestão de dispositivos do Azure AD** em conjunto. Se não estiver familiarizado com uma das seguintes áreas ainda, leia os tópicos seguintes, pela primeira vez:

- **[Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -este artigo fornece uma descrição geral conceptual do acesso condicional e a terminologia relacionada.

- **[Introdução à gestão de dispositivos no Azure Active Directory](device-management-introduction.md)**  -este artigo fornece uma descrição geral das várias opções de tem de inscrever os dispositivos sob controlo organizacional. 


## <a name="scenario-description"></a>Descrição do cenário

Envolver-se o equilíbrio entre produtividade e segurança é um desafio. A proliferação de dispositivos suportados para acederem aos recursos da nuvem ajuda a melhorar a produtividade dos utilizadores. No lado flip, provavelmente não quiser certos recursos no seu ambiente para ser acedido por dispositivos com um nível de proteção desconhecido. Para os recursos afetados, deve exigir que os utilizadores só podem aceder-lhes através de um dispositivo gerido. 

Com o acesso condicional do Azure AD, pode resolver este requisito com uma única política que concede acesso:

- Para aplicações em nuvem selecionado

- Para utilizadores e grupos selecionados

- A necessidade de um dispositivo gerido


## <a name="managed-devices"></a>Dispositivos geridos  

Em termos simples, os dispositivos geridos são dispositivos que estão sob *algumas ordenação* do controlo organizacional. No Azure AD, o pré-requisito para um dispositivo gerido é que foi registado com o Azure AD. Registar um dispositivo cria uma identidade para o dispositivo na forma de um objeto de dispositivo. Este objeto é utilizado pelo Azure para monitorizar informações de estado sobre um dispositivo. Como administrador do Azure AD, já pode utilizar este objeto para ativar/desativar (Ativar/desativar), o estado de um dispositivo.
  
![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Para obter um dispositivo registado com o Azure AD, tem três opções:

- **[Azure AD registado dispositivos](device-management-introduction.md#azure-ad-registered-devices)**  - para obter um dispositivo pessoal registado com o Azure AD

- **[Dispositivos associados ao Azure AD](device-management-introduction.md#azure-ad-joined-devices)**  - para obter um dispositivo organizacional do Windows 10 que não está associado a um local AD registado com o Azure AD. 

- **[Dispositivos associados ao Azure AD híbrido](device-management-introduction.md#hybrid-azure-ad-joined-devices)**  - para obter um dispositivo Windows 10 que está associado a um local AD registado com o Azure AD.

Para tornar-se um dispositivo gerido, um dispositivo registado tem de ser um **híbrida do Azure AD associada ao dispositivo** ou um **dispositivo que foi marcado como compatível**.  

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Exigir híbrida Azure AD dispositivos associados ao

Na sua política de acesso condicional, pode selecionar **requerem dispositivo associado ao Azure AD híbrido** para indicar que as aplicações em nuvem selecionado só podem ser acedidas através de um dispositivo gerido. 

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/10.png)

Esta definição aplica-se apenas a dispositivos Windows 10 associados a um local AD. Apenas pode registar estes dispositivos com o Azure AD utilizando uma associação do Azure AD híbrido, que é um [automatizada processo](device-management-hybrid-azuread-joined-devices-setup.md) para obter um dispositivo Windows 10 registado. 

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/45.png)

O que faz com que uma versão híbrida do Azure AD associada ao dispositivo um dispositivo gerido?  Para dispositivos que estão associados a um local AD, presume-se que o controlo sobre estes dispositivos é imposto utilizar soluções de gestão como **System Center Configuration Manager (SCCM)** ou **(GP)depolíticadegrupo** geri-los. Porque não existe nenhum método para o Azure AD determinar se qualquer um destes métodos tenha sido aplicado a um dispositivo, que requerem um dispositivo associado ao Azure AD híbrido é um mecanismo relativamente fraco para exigir um dispositivo gerido. É até que como um administrador para avaliar se os métodos que são aplicados ao seu local associados a um domínio dispositivos são fortes constituem um dispositivo gerido se esses um dispositivo também é um dispositivo associado ao Azure AD híbrido.


## <a name="require-device-to-be-marked-as-compliant"></a>Pedir que o dispositivo seja marcado como compatível

A opção de *requer um dispositivo ser assinalado como estando em conformidade* é a forma mais fortes para pedir um dispositivo gerido.

![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/11.png)

Esta opção requer um dispositivo ser registado com o Azure AD bem como para ser marcado como compatível por:
         
- Intune.
- Um sistema de gestão (MDM) de dispositivos móveis de terceiros que gere os dispositivos Windows 10 através da integração do Azure AD. Não são suportados sistemas MDM de terceiros para tipos de SO do dispositivo que não seja o Windows 10.
 
![Condições com base no dispositivo](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Para um dispositivo que está marcado como compatível, pode assumir que: 

- Os sua força de trabalho utiliza para aceder aos dados da empresa de dispositivos móveis são geridos
- Utiliza a sua força de trabalho de aplicações móveis são geridas
- Informações da sua empresa estão protegidas por ajudar a controlar a forma como a sua força de trabalho acede e partilhe o mesmo
- O dispositivo e as respetivas aplicações são compatíveis com os requisitos de segurança da empresa




## <a name="next-steps"></a>Passos Seguintes

Antes de configurar uma política de acesso condicional baseado no dispositivo no seu ambiente, deve ter uma vista de olhos a [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md).

