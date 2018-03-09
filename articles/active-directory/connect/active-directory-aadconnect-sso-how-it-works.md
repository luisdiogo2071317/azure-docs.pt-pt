---
title: 'O Azure AD Connect: Totalmente integrada Single Sign-On - como funciona | Microsoft Docs'
description: Este artigo descreve como funciona a funcionalidade do Azure Active Directory totalmente integrada Single Sign-On.
services: active-directory
keywords: "o que é o Azure AD Connect, a instalação do Active Directory, os componentes necessários para o Azure AD, SSO, o início de sessão único"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: 9d17a4038f2171b74c8ba1dbc21e8335e6893691
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory totalmente integrada Single Sign-On: detalhada da Technical

Este artigo fornece detalhes técnicos em como funciona a funcionalidade do Azure Active Directory totalmente integrada Single Sign-On (SSO totalmente integrado).

## <a name="how-does-seamless-sso-work"></a>Como funciona o SSO totalmente integrado?

Esta secção tem três partes para o mesmo:
1. A configuração da funcionalidade SSO totalmente integrada.
2. Como uma transação de início de sessão do utilizador único num browser da web funciona com o SSO totalmente integrada.
3. Como uma transação de início de sessão do utilizador único num cliente nativo funciona com o SSO totalmente integrada.

### <a name="how-does-set-up-work"></a>Como configurar o trabalho?

SSO totalmente integrada é ativada através do Azure AD Connect, conforme mostrado [aqui](active-directory-aadconnect-sso-quick-start.md). Ao ativar a funcionalidade, ocorrem os seguintes passos:
- Uma conta de computador com o nome `AZUREADSSOACC` (que representa o Azure AD) é criado no local do Active Directory (AD).
- Chave de desencriptação de Kerberos a conta de computador é partilhado de forma segura com o Azure AD.
- Além disso, são criados dois Kerberos nomes principais de serviço (SPNs) para representar os dois URLs que são utilizados durante o início de sessão do Azure AD.

>[!NOTE]
> A conta de computador e os SPNs Kerberos são criados em cada floresta do AD sincronizar com o Azure AD (utilizando o Azure AD Connect) e cujos utilizadores pretende SSO totalmente integrada. Mover o `AZUREADSSOACC` conta de computador para uma unidade organizacional (UO) onde outras contas de computador são armazenadas para garantir que é gerido da mesma forma e não é eliminado.

>[!IMPORTANT]
>Recomendamos vivamente que lhe [rollover de chave de desencriptação de Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) do `AZUREADSSOACC` conta de computador, pelo menos, a cada 30 dias.

Assim que a configuração estiver concluída, o SSO totalmente integrado funciona da mesma forma que qualquer outro início de sessão que utiliza autenticação integrada de Windows (IWA).

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Como iniciar sessão num web browser com o trabalho de SSO totalmente integrado?

O fluxo de início de sessão num browser da web é o seguinte:

1. O utilizador tenta aceder a aplicações web (por exemplo, o Outlook Web App - https://outlook.office365.com/owa/) de um dispositivo associado a domínio empresarial no interior da rede empresarial.
2. Se o utilizador não está já iniciou sessão, o utilizador é redirecionado para a página de início de sessão do Azure AD.
3. Os tipos de utilizador no seu nome de utilizador para a página de início de sessão do Azure AD.

  >[!NOTE]
  >Para [determinadas aplicações](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), os passos 2 e 3 são ignorados.

4. Utilizar JavaScript em segundo plano, o Azure AD desafia o browser, através de uma resposta 401 não autorizado, para fornecer uma permissão Kerberos.
5. O browser, por sua vez, pedidos de um pedido de suporte do Active Directory para o `AZUREADSSOACC` conta de computador (o que representa o Azure AD).
6. Active Directory localiza a conta de computador e devolve uma permissão Kerberos para o browser encriptado com o segredo da conta de computador.
7. O browser reencaminha o pedido de suporte do Kerberos que adquiriu do Active Directory para o Azure AD.
8. Azure AD desencripta a permissão de Kerberos, que inclui a identidade do utilizador com sessão iniciada no dispositivo da empresa, utilizando a chave partilhada anteriormente.
9. Após a avaliação, Azure AD devolve um token de volta para a aplicação ou pede ao utilizador para efetuar proofs adicionais, tais como o multi-factor Authentication.
10. Se o utilizador inicie sessão com êxito, o utilizador é capaz de aceder à aplicação.

O diagrama seguinte ilustra a todos os componentes e os passos envolvidos.

![Início de sessão único totalmente integrado no - Web fluxo da aplicação](./media/active-directory-aadconnect-sso/sso2.png)

SSO totalmente integrada é oportunistas medida planeado, o que significa que se falhar, a experiência de início de sessão retrocede para o respetivo comportamento normal - revertidos, o utilizador tem de introduzir a palavra-passe para iniciar sessão.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Como iniciar sessão num cliente nativo com o trabalho de SSO totalmente integrado?

O fluxo de início de sessão num cliente nativo é o seguinte:

1. O utilizador tenta aceder a uma aplicação nativa (por exemplo, o cliente do Outlook) de um dispositivo associado a domínio empresarial no interior da rede empresarial.
2. Se o utilizador não está já iniciou sessão, a aplicação nativa obtém o nome de utilizador do utilizador da sessão do Windows do dispositivo.
3. A aplicação envia o nome de utilizador para o Azure AD e obtém o ponto final de WS-Trust MEX do seu inquilino.
4. A aplicação, em seguida, consulta o ponto final de WS-Trust MEX para ver se integrado ponto final de autenticação está disponível.
5. Se tiver êxito passo 4, é emitido um desafio de Kerberos.
6. Se a aplicação é capaz de obter a permissão de Kerberos, reencaminha-lo até o ponto final de autenticação integrada do Azure AD.
7. Azure AD desencripta a permissão de Kerberos e validá-lo.
8. Azure AD inicia o utilizador no e emite um token SAML para a aplicação.
9. A aplicação envia o token SAML para o ponto final de tokens de OAuth2 do Azure AD.
10. Azure AD valida o token SAML e problemas na aplicação, um token de acesso e um token de atualização para o recurso especificado e um token de id.
11. O utilizador obtém acesso aos recursos da aplicação.

O diagrama seguinte ilustra a todos os componentes e os passos envolvidos.

![Início de sessão único totalmente integrado no - fluxo de aplicação nativa](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>Passos Seguintes

- [**Início Rápido** ](active-directory-aadconnect-sso-quick-start.md) - obter cópias de segurança e executar o SSO totalmente integrada de AD do Azure.
- [**Perguntas mais frequentes** ](active-directory-aadconnect-sso-faq.md) -respostas para as perguntas mais frequentes.
- [**Resolver problemas** ](active-directory-aadconnect-troubleshoot-sso.md) -Saiba como resolver problemas comuns com a funcionalidade.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - para apresentação de pedidos de funcionalidades de novo.
