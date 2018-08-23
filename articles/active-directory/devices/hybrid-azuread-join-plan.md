---
title: Como configurar híbrida do Azure Active Directory dispositivos associados ao | Documentos da Microsoft
description: Saiba como configurar híbrida do Azure Active Directory associados a um dispositivos.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: fbc40d0768a7cf1cb83a3e78dd524fac26254bea
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060313"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Como planear a sua implementação híbrida do Azure Active Directory

De forma semelhante a um utilizador, um dispositivo está se tornando outra identidade que pretende proteger e também utilizar para proteger os seus recursos em qualquer momento e local. Pode concretizar este objetivo ao trazer as identidades dos seus dispositivos com o Azure AD através de um dos seguintes métodos:

- Associação do Azure AD
- Associação ao Azure AD Híbrido
- Registo do Azure AD

Ao trazer os seus dispositivos com o Azure AD, maximize a produtividade dos seus utilizadores através do início de sessão único (SSO) todos os seus recursos na cloud e no local. Ao mesmo tempo, pode proteger o acesso aos seus recursos na cloud e no local com o [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Se tiver um ambiente do Active Directory no local e pretender associar os dispositivos associados a um domínio ao Azure AD, isso pode ser feito através da configuração de dispositivos de associados ao Azure AD híbrido. Este artigo fornece a com os passos relacionados para implementar um Azure AD híbrido Junte-se em seu ambiente. 


## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com o [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Planeie a sua implementação

Para planear a implementação do Azure AD híbrido, deve familiarizar-se com:

|   |   |
|---|---|
|![Marcar][1]|Rever suportado dispositivos|
|![Marcar][1]|Deve saber de coisas de revisão|
|![Marcar][1]|Selecione o seu cenário|


 


## <a name="review-supported-devices"></a>Rever suportado dispositivos 

Associação do híbrida do Azure AD suporta uma ampla gama de Windows de dispositivos. Como a configuração para dispositivos que executam versões mais antigas do Windows requer passos adicionais ou diferentes, os dispositivos suportados são agrupados em duas categorias:

**Dispositivos atuais do Windows**

- Windows 10
    
- Windows Server 2016


Para dispositivos com o sistema de operativo ambiente de trabalho do Windows, a versão suportada é a atualização de aniversário do Windows 10 (versão 1607) ou posterior. Como melhor prática, atualize para a versão mais recente do Windows 10.



 **Dispositivos de nível inferior do Windows**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


Como uma primeira etapa de planeamento, deve rever o seu ambiente e determinar se é necessário suportar dispositivos de nível inferior do Windows.



## <a name="review-things-you-should-know"></a>Deve saber de coisas de revisão

Se o seu ambiente consiste numa única floresta sincronizada dados de identidade para mais do que um inquilino do Azure AD, é possível utilizar uma associação do Azure AD híbrido.

Se está contando com a ferramenta de preparação do sistema (Sysprep), certifique-se de que criar imagens de uma instalação do Windows que não tem sido configurada para associação do híbrida do Azure AD.

Se está contando com um instantâneo da Máquina Virtual (VM) para criar VMs adicionais, certifique-se de que utiliza um instantâneo VM que não tenha sido configurado para associação do híbrida do Azure AD.

O registo de dispositivos de nível inferior do Windows não é suportado para dispositivos configurados para o perfil de utilizador itinerantes ou mobilidade de credenciais. Se está a depender de perfis ou as definições de roaming, utilizam o Windows 10.

- O registo de dispositivos de nível inferior do Windows **é** suportado nos ambientes de não federadas por meio de sessão único totalmente integrado [Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 
 
- O registo de dispositivos de nível inferior do Windows **není** suportada ao utilizar a autenticação pass-through do Azure AD sem o início de sessão único totalmente integrado.

- O registo de dispositivos de nível inferior do Windows **není** suportada para dispositivos através de perfis de roaming. Se está a depender de perfis ou as definições de roaming, utilizam o Windows 10.


O registo do Windows Server a executar a função de controlador de domínio (DC) não é suportado.

Se sua organização necessita de acesso à Internet através de um proxy autenticado de saída, tem de certificar-se de que os seus computadores Windows 10 podem autenticar com êxito para o proxy de saída. Como computadores Windows 10 executarem o registo de dispositivos com o contexto da máquina, é necessário configurar a autenticação de proxy de saída usando o contexto da máquina.


Associação ao Azure AD híbrido é um processo para registrar automaticamente os seus dispositivos de associados a um domínio no local com o Azure AD. Há casos em que não pretende todos os seus dispositivos para registar automaticamente. Se isso é verdadeiro para, consulte [como controlar a associação do Azure AD híbrido dos seus dispositivos](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Selecione o seu cenário

Pode configurar a associação do Azure AD híbrido para os seguintes cenários:

- Domínios geridos
- Domínios federados  



Se o seu ambiente tem domínios geridos, oferece suporte a associação do híbrida do Azure AD:

- Passar através da autenticação (PTA) com início de sessão único totalmente integrado (SSO) 

- Palavra-passe tiver sincronização (PHS) com início de sessão único totalmente integrado (SSO) 

A partir da versão 1.1.819.0, o Azure AD Connect fornece um Assistente para configurar a associação do Azure AD híbrido. O assistente permite-lhe simplificar significativamente o processo de configuração. Para obter mais informações, consulte:

- [Configurar a associação híbrida do Azure Active Directory para os domínios federados](hybrid-azuread-join-federated-domains.md)

- [Configurar a associação híbrida do Azure Active Directory para os domínios geridos](hybrid-azuread-join-managed-domains.md)


 Se instalar a versão necessária do Azure AD Connect não é uma opção para si, veja [como configurar manualmente o registo de dispositivos](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Associação ao Azure Active Directory configurar híbrido para domínios federados](hybrid-azuread-join-federated-domains.md)
> [associação ao Azure Active Directory configurar híbrido para domínios geridos](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
