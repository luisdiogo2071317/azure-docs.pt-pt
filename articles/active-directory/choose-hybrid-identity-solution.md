---
title: Escolher uma solução de identidade híbrida do Azure | Documentos da Microsoft
description: Obtenha uma compreensão básica das soluções de identidade híbrida de disponibilidade e recomendações para que possa tomar a melhor decisão de governação de identidade para a sua organização.
keywords: ''
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: billmath
ms.date: 03/02/2018
ms.topic: article
ms.prod: ''
ms.service: azure
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 15a28cd5937be103aac888a5fc5485e39854a1b4
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319953"
---
# <a name="microsoft-hybrid-identity-solutions"></a>Soluções de identidade híbrida da Microsoft

[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) soluções de identidade híbrida permitem-lhe sincronizar objetos de diretório no local com o Azure AD, ainda, gerindo os utilizadores no local. A primeira decisão a tomar ao planear a sincronizar o seu local Windows Server Active Directory com o Azure AD é se pretende utilizar sincronizado identidade ou identidade federada. Identidades sincronizadas e, opcionalmente, hashes de palavra-passe, permitem que os utilizadores utilizem a mesma palavra-passe para aceder aos recursos organizacionais com base na cloud e no local. Para requisitos de cenários mais avançados, como o início de sessão único (SSO) ou o MFA no local, terá de implementar os serviços de Federação do Active Directory (AD FS) para federar identidades. 

Existem várias opções disponíveis para a configuração de identidade híbrida. Este artigo fornece informações para ajudar a escolher a melhor para sua organização com base na facilidade de implantação e necessidades de sua gestão de identidades e acessos específicos. Ao considerar qual modelo de identidade que melhor se adequa às necessidades da sua organização, terá também de pensar sobre o tempo, a infraestrutura existente, complexidade e custos. Esses fatores são diferentes para cada organização e podem ser alterados ao longo do tempo. No entanto, se seus requisitos se alteram, também tem a flexibilidade para mudar para um modelo de identidade diferente.

> [!TIP]
> Estas soluções são entregues por [do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="synchronized-identity"></a>Identidades sincronizadas 

Identidades sincronizadas é a forma mais simples para sincronizar objetos de diretório no local (utilizadores e grupos) com o Azure AD. 

![Identidade híbrida sincronizada](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Embora sincronizada de identidade é o método mais fácil e rápido, os utilizadores ainda precisam de manter uma palavra-passe separada para recursos baseados na nuvem. Para evitar isto, também pode (opcionalmente) [sincronizar um hash de palavras-passe do utilizador](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) ao seu diretório do Azure AD. Sincronização de hashes de palavra-passe permite que os utilizadores iniciem sessão aos recursos organizacionais com base na cloud com o mesmo nome de utilizador e palavra-passe que utilizam no local. O Azure AD Connect verifica periodicamente o seu diretório no local para as alterações e mantém sincronizado do diretório do Azure AD. Quando um atributo de utilizador ou palavra-passe é alterado no Active Directory local, é atualizado automaticamente no Azure AD. 

Na maioria das organizações que precisam apenas para permitir que os utilizadores iniciar sessão no Office 365 e outros recursos do Azure AD com base em aplicações SaaS, recomenda-se a opção de sincronização de palavra-passe predefinida. Se isso não funciona para, terá de decidir entre a autenticação pass-through e o AD FS.

> [!TIP]
> As palavras-passe do utilizador são armazenadas no local Windows Server Active Directory na forma de um valor de hash que representa a palavra-passe de utilizador reais. Um valor de hash é um resultado de uma função de matemática unidirecional (o algoritmo de hash). Não existe nenhum método para reverter o resultado de uma função unidirecional para a versão de texto sem formatação de uma palavra-passe. Não é possível utilizar um hash de palavra-passe para iniciar sessão na sua rede no local. Quando optar por sincronizar as palavras-passe, o Azure AD Connect extrai os hashes de palavra-passe do Active Directory no local e aplica segurança extra de processamento para o hash de palavra-passe antes dos dados foram sincronizados com o Azure AD. Sincronização de palavra-passe também pode ser utilizada em conjunto com a repetição de escrita de palavras-passe para ativar a gestão personalizada reposição palavra-passe no Azure AD. Além disso, pode ativar início de sessão único (SSO) para os usuários em computadores associados a um domínio que estejam ligados à rede empresarial. Com início de sessão único, utilizadores ativados apenas tem de introduzir um nome de utilizador para aceder de forma segura a recursos na cloud. 
>

## <a name="pass-through-authentication"></a>Autenticação pass-through

[Autenticação pass-through do Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) fornece uma solução de validação da palavra-passe simples para serviços com base no AD do Azure com o Active Directory no local. Se as políticas de segurança e conformidade para a sua organização que não permitem o envio de senhas dos usuários, mesmo num formulário com hash e só tem de suportar o SSO de ambiente de trabalho para dispositivos associados a um domínio, recomenda-se que avalie a utilizar a autenticação pass-through. Autenticação pass-through não requer qualquer implantação na rede de Perímetro, o que simplifica a infra-estrutura de implantação em comparação com o AD FS. Quando os utilizadores iniciam sessão com o Azure AD, este método de autenticação valida senhas diretamente no seu Active Directory no local.

![Autenticação pass-through](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Com a autenticação pass-through, não é necessário para uma infraestrutura de rede complexa e não precisa de armazenar palavras-passe no local na cloud. Combinado com o início de sessão único, autenticação pass-through fornece uma experiência verdadeiramente integrada ao iniciar sessão com o Azure AD ou outros serviços em nuvem.

Autenticação pass-through está configurada com o Azure AD Connect, que usa um agente de simples no local que está à escuta de pedidos de validação da palavra-passe. O agente pode ser facilmente implementado para várias máquinas para proporcionar elevada disponibilidade e balanceamento de carga. Uma vez que todas as comunicações são apenas saídas, não existe nenhum requisito para o conector ser instalado numa rede de Perímetro. Seguem-se os requisitos de computador do servidor para o conector:

- Windows Server 2012 R2 ou superior
- Associado a um domínio na floresta por meio do qual os utilizadores são validados

## <a name="federated-identity-ad-fs"></a>Identidades federadas (AD FS)

Para obter mais controlo sobre como os utilizadores acedem do Office 365 e outros serviços em nuvem, pode configurar a sincronização de diretórios com início de sessão único (SSO) usando [serviços de Federação do Active Directory (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server). Federar inícios dos seus utilizadores com o AD FS delega a autenticação para um servidor no local que valida as credenciais do utilizador. Nesse modelo, as credenciais do Active Directory no local nunca são transmitidas para o Azure AD.

![Identidades federadas](./media/choose-hybrid-identity-solution/federated-identity.png)

Este método de início de sessão também denominado Federação de identidades, garante que todos os autenticação de utilizador é controlado no local e permite aos administradores implementar níveis mais rigorosas de controlo de acesso. Federação de identidades com o AD FS é a opção mais complicada e exige a implementar servidores adicionais no seu ambiente no local. Federação de identidades também consolida a fornecer 24x7 suporte para a sua infraestrutura do Active Directory e o AD FS. Este nível elevado de suporte é necessário porque se aceder à Internet no local, controlador de domínio ou servidores do AD FS não estão disponíveis, os utilizadores não podem iniciar sessão para serviços cloud.

> [!TIP]
> Se decidir usar a Federação com os serviços de Federação do Active Directory (AD FS), pode, opcionalmente, configurar sincronização de palavra-passe como uma cópia de segurança no caso de falha de infraestrutura do AD FS.
>

## <a name="common-scenarios-and-recommendations"></a>Cenários comuns e recomendações

Seguem-se alguns cenários comuns de híbrida identidades e acessos gestão com recomendações sobre qual híbrida opção de identidade (ou opções) podem ser apropriadas para cada um.

|Eu preciso:|PWS e SSO<sup>1</sup>| PTA e SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Sincronize o novo utilizador, contato e contas de grupo criadas automaticamente no meu diretório de Active Directory no local para a cloud.|![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)| ![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png) |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Configurar o meu inquilino para cenários de híbrida do Office 365|![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)| ![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png) |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Ativar os meus utilizadores iniciar sessão e aceder aos serviços em nuvem com a palavra-passe no local|![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)| ![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png) |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Implementar o início de sessão único com credenciais da empresa|![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)| ![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png) |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Certifique-se de que nenhum hashes de palavra-passe são armazenados na cloud| |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Ativar soluções de autenticação multifator no local| | |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Autenticação de smart card de suporte para os meus utilizadores<sup>4</sup>| | |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|
|Apresentar as notificações de expiração de palavra-passe no Portal do Office e no ambiente de trabalho do Windows 10| | |![Recomendado](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> sincronização de palavra-passe com o início de sessão único.
>
> <sup>2</sup> autenticação pass-through e início de sessão único. 
>
> <sup>3</sup> único início de sessão Federado com o AD FS.
>
> <sup>4</sup> do AD FS pode ser integrado com o seu enterprise PKI para permitir o início de sessão através de certificados. Estes certificados podem ser implementados por meio de canais de aprovisionamento fidedignos como certificados MDM ou GPO ou smart card (incluindo cartões PIV/CAC) ou o Hello para empresas (certificado de confiança) de certificados de forma recuperável. Para obter mais informações sobre o suporte de autenticação de smart card, consulte [este blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).
>

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais num ambiente do Azure prova de conceito](https://aka.ms/aad-poc)
[instalar o Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)
[monitorizar a sincronização de identidade híbrida](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
