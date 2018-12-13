---
title: Como configurar dispositivos associados ao Azure Active Directory híbrido | Microsoft Docs
description: Saiba como configurar dispositivos associados ao Azure Active Directory híbrido.
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
ms.date: 11/01/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: e273568a04ec2a3758684025acf8034b8e788627
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871358"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Como planear a sua implementação híbrida do Azure Active Directory

Da forma semelhante ao utilizador, o dispositivo está a tornar-se outra identidade que deve proteger e também utilizar para proteger os seus recursos em qualquer momento e em qualquer lugar. Pode concretizar este objetivo ao trazer as identidades dos seus dispositivos para o Azure AD através de um dos seguintes métodos:

- Associação ao Azure AD
- Associação ao Azure AD Híbrido
- Registo do Azure AD

Ao trazer os seus dispositivos para o Azure AD, maximiza a produtividade dos seus utilizadores através do início de sessão único (SSO) nos seus recursos na cloud e no local. Ao mesmo tempo, pode proteger o acesso aos seus recursos na cloud e no local com o [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Se tiver um ambiente do Active Directory no local e quiser associar os seus dispositivos associados ao domínio ao Azure AD, pode fazê-lo ao configurar os dispositivos híbridos associados ao Azure AD. Este artigo fornece a com os passos relacionados para implementar um Azure AD híbrido Junte-se em seu ambiente. 


## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com o [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Planeie a sua implementação

Para planear a implementação do Azure AD híbrido, deve familiarizar-se com:

|   |   |
|---|---|
|![Marcar][1]|Rever suportado dispositivos|
|![Marcar][1]|Deve saber de coisas de revisão|
|![Marcar][1]|Rever como controlar a associação do Azure AD híbrido dos seus dispositivos|
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

Associação do Azure AD híbrido de dispositivos de nível inferior do Windows:

- **É** suportado nos ambientes de não federadas através de [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 

- **Não é** suportada ao utilizar a autenticação pass-through do Azure AD sem o início de sessão único totalmente integrado.

- **Não é** suportada ao utilizar a mobilidade de credenciais ou perfil de utilizador itinerantes ou ao utilizar a infraestrutura de ambiente de trabalho virtual (VDI).


O registo do Windows Server a executar a função de controlador de domínio (DC) não é suportado.

Se sua organização precisar de acesso à Internet através de um proxy de saída autenticado, tem de se certificar de que os seus computadores com Windows 10 podem ser autenticados com êxito no proxy de saída. Uma vez que os computadores com Windows 10 executam o registo de dispositivos utilizando o contexto da máquina, é necessário configurar a autenticação de proxy de saída utilizando o contexto da máquina.


Associação ao Azure AD híbrido é um processo para registrar automaticamente os seus dispositivos de associados a um domínio no local com o Azure AD. Há casos em que não pretende todos os seus dispositivos para registar automaticamente. Se isso é verdadeiro para, consulte [como controlar a associação do Azure AD híbrido dos seus dispositivos](hybrid-azuread-join-control.md).


## <a name="review-how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Rever como controlar a associação do Azure AD híbrido dos seus dispositivos

Associação ao Azure AD híbrido é um processo para registrar automaticamente os seus dispositivos de associados a um domínio no local com o Azure AD. Há casos em que não pretende todos os seus dispositivos para registar automaticamente. Isso é para true de exemplo, durante a implementação inicial para verificar que tudo funciona conforme esperado.

Para obter mais informações, consulte [como controlar a associação do Azure AD híbrido dos seus dispositivos](hybrid-azuread-join-control.md)

## <a name="select-your-scenario"></a>Selecione o seu cenário

Pode configurar a associação do Azure AD híbrido para os seguintes cenários:

- Domínios geridos
- Domínios Federados  



Se o seu ambiente tem domínios geridos, oferece suporte a associação do híbrida do Azure AD:

- Pass-Through Authentication (PTA)

- Sincronização de Hash de palavra-passe (PHS)

A partir da versão 1.1.819.0, o Azure AD Connect fornece um assistente para configurar a associação do Azure AD híbrido. O assistente permite-lhe simplificar significativamente o processo de configuração. Para obter mais informações, consulte:

- [Configurar a associação híbrida do Azure Active Directory para os domínios federados](hybrid-azuread-join-federated-domains.md)


- [Configurar a associação híbrida do Azure Active Directory para os domínios geridos](hybrid-azuread-join-managed-domains.md)


 Se instalar a versão necessária do Azure AD Connect não é uma opção para si, veja [como configurar manualmente o registo de dispositivos](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Associação ao Azure Active Directory configurar híbrido para domínios federados](hybrid-azuread-join-federated-domains.md)
> [associação ao Azure Active Directory configurar híbrido para domínios geridos](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
