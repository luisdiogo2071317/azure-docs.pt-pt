---
title: Problemas ao iniciar sessão numa aplicação de galeria configurada para federado início de sessão único | Documentos da Microsoft
description: Documentação de orientação para os erros específicos ao iniciar sessão numa aplicação que configurou para baseado em SAML início de sessão único Federado com o Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 8a21f1ac0839a37455fe06537242edc6e43731a4
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477305"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Problemas ao iniciar sessão numa aplicação de galeria configurada para início de sessão único federado

Para resolver seu problema, terá de verificar a configuração de aplicação no Azure AD, o seguinte:

-   Seguiu todos os passos de configuração para a aplicação da galeria do Azure AD.

-   O identificador e o URL de resposta configurado no AAD corresponder estes valores esperados no aplicativo

-   Se foi atribuído aos utilizadores para a aplicação

## <a name="application-not-found-in-directory"></a>Não foi encontrada no diretório de aplicação

*Error AADSTS70001: Aplicação com o identificador 'https://contoso.com"não foi encontrado no diretório*.

**Causa possível**

O emissor atributo envia da aplicação para o Azure AD no pedido de SAML não corresponde ao valor de identificador configurado na aplicação do Azure AD.

**Resolução**

Certifique-se de que o atributo de emissor no pedido de SAML correspondência o identificador de valor configurado no Azure AD:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único

7.  Assim que o aplicativo é carregado, clique nas **início de sessão único** no menu de navegação do lado esquerdo da aplicação.

8.  Aceda a **domínio e URLs** secção. Certifique-se de que o valor na caixa de texto identificador correspondência o valor para o valor do identificador apresentado o erro.

Depois de atualizar o valor do identificador no Azure AD e ele correspondência o envia de valor pelo aplicativo no pedido de SAML, deverá conseguir iniciar sessão na aplicação.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>O endereço de resposta não corresponde ao endereço de resposta configurado para a aplicação.

*AADSTS50011 de erro: O endereço de resposta https://contoso.com"não coincide com o endereço de resposta configurado para a aplicação*

**Causa possível**

O valor de AssertionConsumerServiceURL no pedido de SAML não corresponde a valor de URL de resposta ou o padrão configurado no Azure AD. O valor de AssertionConsumerServiceURL no pedido de SAML é o URL que vê no erro.

**Resolução**

Certifique-se de que o valor de AssertionConsumerServiceURL no pedido de SAML correspondência o URL de resposta valor configurado no Azure AD.

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único

7.  Assim que o aplicativo é carregado, clique nas **início de sessão único** no menu de navegação do lado esquerdo da aplicação.

8.  Aceda a **domínio e URLs** secção. Certifique-se ou atualizar o valor na caixa de texto URL de resposta para corresponder ao valor AssertionConsumerServiceURL no pedido de SAML.  
    * Se não vir a caixa de texto do URL de resposta, selecione o **Mostrar definições de URL avançadas** caixa de verificação.

Depois de atualizar o valor de URL de resposta no Azure AD e ele correspondência o envia de valor pelo aplicativo no pedido de SAML, deverá conseguir iniciar sessão na aplicação.

## <a name="user-not-assigned-a-role"></a>Não atribuído uma função de utilizador

*Error AADSTS50105: O utilizador com sessão iniciada "brian@contoso.com" não está atribuído a uma função para a aplicação*.

**Causa possível**

O utilizador não foi concedido acesso à aplicação no Azure AD.

**Resolução**

Para atribuir diretamente um ou mais utilizadores a uma aplicação, siga os passos abaixo:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende atribuir um utilizador a partir da lista.

7.  Assim que o aplicativo é carregado, clique em **utilizadores e grupos** no menu de navegação do lado esquerdo da aplicação.

8.  Clique nas **Add** botão na parte superior do **utilizadores e grupos** lista para abrir o **adicionar atribuição** painel.

9.  Clique nas **utilizadores e grupos** Seletor da **adicionar atribuição** painel.

10. Escreva o **nome completo** ou **endereço de e-mail** do utilizador estiver interessado em atribuir para o **procurar por nome ou endereço de e-mail** caixa de pesquisa.

11. Paire o rato sobre o **usuário** na lista para revelar uma **caixa de verificação**. Clique na caixa de verificação junto a fotografia do perfil do usuário ou a logótipo para adicionar o utilizador para o **selecionados** lista.

12. **Opcional:** Se quiser **adicionar mais de um usuário**, tipo em outro **nome completo** ou **endereço de e-mail** para o **procurar por nome ou endereço de e-mail** caixa de pesquisa e clique na caixa de verificação para adicionar este utilizador para o **selecionados** lista.

13. Quando tiver terminado de selecionar utilizadores, clique nas **selecione** botão para adicioná-los à lista de utilizadores e grupos que devem ser atribuídos à aplicação.

14. **Opcional:** clique a **selecionar função** Seletor no **adicionar atribuição** painel para selecionar uma função para atribuir aos utilizadores que selecionou.

15. Clique nas **atribuir** botão para atribuir a aplicação aos utilizadores selecionados.

Após um curto período de tempo, os utilizadores que selecionou ser capaz de iniciar esses aplicativos usando os métodos descritos na secção de descrição de solução.

## <a name="not-a-valid-saml-request"></a>Não um pedido de SAML válida

*Error AADSTS75005: O pedido não é uma mensagem de protocolo Saml2 válida.*

**Causa possível**

O Azure AD não suporta o Pedido SAML enviado pela aplicação para Início de Sessão Único. Alguns problemas comuns são:

-   Campos necessários no pedido de SAML em falta

-   Método de codificação do pedido SAML

**Resolução**

1.  Captura de pedido SAML. Siga o tutorial [como depurar baseado em SAML início de sessão único para aplicações no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) para saber como capturar o pedido SAML.

2.  Contacte o fornecedor do aplicativo e a partilha:

   -   Pedido SAML

   -   [Requisitos de protocolo do Azure AD único início de sessão SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Eles devem validar que suportam a implementação de SAML do Azure AD para início de sessão único.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Nenhum recurso no requiredResourceAccess lista

*Erro AADSTS65005: A aplicação de cliente pediu acesso ao recurso ' 00000002-0000-0000-c000-000000000000'. Este pedido falhou porque o cliente não especificada este recurso em sua lista de requiredResourceAccess*.

**Causa possível**

O objeto de aplicativo está danificado.

**Resolução: opção 1**

Para resolver o problema, adicione o valor de identificador exclusivo na configuração do Azure AD. Para adicionar um valor de identificador, siga os passos abaixo:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação tiver configurado o início de sessão único.

7.  Assim que o aplicativo é carregado, clique nas **início de sessão único** no menu de navegação do lado esquerdo da aplicação

8.  Sob o **domínio e o URL** secção, para verificar a **Mostrar definições de URL avançadas**.

9.  na **identificador** um identificador exclusivo para a aplicação do tipo de caixa de texto.

10. **Guardar** a configuração.


**Opção de resolução 2**

Se a opção 1 acima não funcionava para, tente remover a aplicação a partir do diretório. Em seguida, adicione e reconfigurar o aplicativo, siga os passos abaixo:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único

7.  Clique em **elimine** no canto superior esquerdo da aplicação **descrição geral** painel.

8.  Atualize o Azure AD e adicionar a aplicação a partir da galeria do Azure AD. Em seguida, Configure a aplicação

<span id="_Hlk477190176" class="anchor"></span>Depois de reconfigurar o aplicativo, deverá conseguir iniciar sessão na aplicação.

## <a name="certificate-or-key-not-configured"></a>Certificado ou chave não configurado

*Error AADSTS50003: Nenhuma chave de assinatura configurado.*

**Causa possível**

O objeto de aplicativo está danificado e do Azure AD não reconhece o certificado configurado para a aplicação.

**Resolução**

Para eliminar e criar um novo certificado, siga os passos abaixo:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

 * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único

7.  Assim que o aplicativo é carregado, clique nas **início de sessão único** no menu de navegação do lado esquerdo da aplicação.

8.  Clique em **criar novo certificado** sob a **certificado de assinatura de SAML** secção.

9.  Selecione a data de expiração. Em seguida, clique em **guardar.**

10. Verifique **ativar o novo certificado** para substituir o certificado do Active Directory. Em seguida, clique em **guardar** na parte superior do painel e aceite para ativar o certificado de rollover.

11. Sob o **certificado de assinatura SAML** secção, clique em **remover** para remover o **não utilizados** certificado.

## <a name="saml-request-not-present-in-the-request"></a>Pedido de SAML não está presente no pedido

*Error AADSTS750054: SAMLRequest ou SAMLResponse tem de estar presente como parâmetros de cadeia de caracteres no pedido HTTP para redirecionamento de SAML enlace de consulta.*

**Causa possível**

O Azure AD não foi possível identificar o pedido SAML dentro dos parâmetros de URL na solicitação HTTP. Isto pode acontecer se a aplicação não está a utilizar redirecionar enlace HTTP para enviar o pedido SAML para o Azure AD.

**Resolução**

O aplicativo precisa enviar o pedido SAML codificado no cabeçalho de localização usando a ligação de redirecionamento de HTTP. Para obter mais informações sobre como implementá-la, leia a secção de redirecionamento de enlace HTTP na [documento de especificação de protocolo SAML](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf).


## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Problema ao personalizar as afirmações SAML enviadas para uma aplicação

Para saber como personalizar as afirmações de atributo SAML enviadas para a sua aplicação, veja [afirmações mapeamento no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes
[Como depurar baseado em SAML início de sessão único para aplicações no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
