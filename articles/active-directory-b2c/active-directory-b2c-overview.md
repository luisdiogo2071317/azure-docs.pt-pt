---
title: O que é o Azure Active Directory B2C? | Microsoft Docs
description: Saiba mais sobre como pode criar e gerir a início de sessão experiência de aplicação utilizando o Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/05/2018
ms.author: davidmu
ms.openlocfilehash: ca9e45a214639da86cf8e0c4a39b3e3d6b6d6491
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="what-is-azure-active-directory-b2c"></a>O que é o Azure Active Directory B2C?

O Azure Active Directory (Azure AD) B2C é um serviço de gestão de identidades que lhe permite personalizar e controlar a forma como os clientes se inscrevem, iniciam sessão e gerem os respetivos perfis quando utilizam as suas aplicações. Isto inclui aplicações desenvolvidas para iOS, Android e .NET, entre outros. O Azure AD B2C permite que estas ações ao proteger as identidades de cliente em simultâneo.

Pode configurar uma aplicação, registada com o Azure AD B2C para efetuar uma variedade de ações de gestão de identidade. Alguns exemplos incluem:

- Ativar um cliente inscrever-se para utilizar a aplicação registada
- Ativar um cliente de concluir a inscrição iniciar sessão e começar a utilizar a aplicação
- Ativar um cliente de concluir a inscrição editar o respetivo perfil
- Ativar a autenticação multifator na sua aplicação
- Ativar o cliente inscrever-se e iniciar sessão com fornecedores de identidade específico
- Conceder acesso da sua aplicação para APIs que criar
- Personalizar o aspeto e funcionalidade de experiência de inscrição e o início de sessão
- Gerir sessões de início de sessão único para a sua aplicação

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>O que é necessário pensar sobre antes de utilizar o Azure AD B2C?

- Como pretende ao cliente para interagir com a minha aplicação?
- O que é a experiência de interface (IU) do utilizador que pretender fornecer aos clientes?
- Os fornecedores de identidade pretende informar os clientes de escolher entre a minha aplicação?
- A minha processo de início de sessão requer APIs adicionais para executar?

### <a name="customer-interaction"></a>Interação de cliente

Azure AD B2C suporta [OpenID Connect](https://openid.net/connect/) de cliente, todas as experiências. Na implementação do OpenID Connect do Azure AD B2C, a sua aplicação inicia journey este utilizador ao emitir pedidos de autenticação para o Azure AD B2C. O resultado do pedido é um `id_token`. Este token de segurança representa a identidade do cliente.

Todas as aplicações que utiliza o Azure AD B2C tem de ser registada um inquilino do Azure AD B2C no portal do Azure. O processo de registo recolhe e atribui valores a sua aplicação. Estes valores incluem um ID de aplicação que identifica exclusivamente a aplicação e um URI que pode ser utilizada para direcionar as respostas regressar ao mesmo de redirecionamento.

A interação de cada aplicação segue um padrão semelhante de alto nível:

1. A aplicação direciona o cliente para executar uma política.
2. O cliente conclui a política de acordo com a definição de política.
3. A aplicação recebe um token de segurança.
4. A aplicação utiliza o token de segurança para tentar aceder a recursos protegidos.
5. O servidor de recurso valida o token de segurança para verificar que o acesso pode ser concedido.
6. A aplicação atualiza periodicamente o token de segurança.

Estes passos podem diferir ligeiramente com base no tipo de aplicação que está a criar.

O Azure AD B2C interage com fornecedores de identidade, os clientes, outros sistemas e pelo diretório no local na sequência para concluir uma tarefa de identidade. Por exemplo, um cliente a iniciar sessão, registar um cliente novo ou repor uma palavra-passe. A plataforma subjacente que estabelece a fidedignidade de múltiplos intervenientes e concluir estes passos denomina-se a estrutura de experiência de identidade. Esta arquitetura e uma política (também denominada journey um utilizador ou uma política de confiança Framework) explicitamente define os atores, as ações, os protocolos e a sequência de passos para concluir.

O Azure AD B2C protege de recusa de serviço e a palavra-passe ataques contra as suas aplicações de várias formas. Azure AD B2C utiliza a deteção e a respetiva mitigação técnicas, como cookies SIN e limites de taxa e ligação para proteger os recursos contra ataques denial-of-service. Mitigação também está incluída na ataques de palavra-passe de dicionário e ataques de palavra-passe de força bruta.

#### <a name="built-in-policies"></a>Políticas incorporadas

Cada pedido enviado para o Azure AD B2C Especifica uma política. Uma política controla o comportamento de como a sua aplicação interage com o Azure AD B2C. Políticas incorporadas estão predefinidas para tarefas mais comuns de identidade, tais como inscrição, início de sessão e a edição do perfil.  Por exemplo, uma política de inscrição permite-lhe controlar comportamentos ao configurar as seguintes definições:

- Contas de redes sociais que o cliente pode utilizar para se inscrever para a aplicação
- Dados recolhidos a partir do cliente, como o nome próprio ou código postal
- Multi-Factor Authentication
- Aspeto e funcionalidade de todas as páginas de inscrição
- Informações devolvidas à aplicação

#### <a name="custom-policies"></a>Políticas personalizadas 

[As políticas personalizadas](active-directory-b2c-overview-custom.md) são ficheiros de configuração que definem o comportamento do Framework de experiência de identidade no seu inquilino do Azure AD B2C. As políticas personalizadas podem ser editadas totalmente executar muitas tarefas. Uma política personalizada é representada como um ou vários ficheiros de formatado em XML que se referenciar entre si numa cadeia hierárquica. 

Várias políticas personalizadas de diferentes tipos de podem ser utilizadas no seu inquilino do Azure AD B2C, conforme necessário e podem ser reutilizadas em aplicações. Esta flexibilidade permite-lhe definir e modificar experiências de identidade de cliente mínima com ou sem alterações para o seu código. Políticas podem ser utilizadas ao adicionar um parâmetro de consulta especiais para pedidos de autenticação HTTP.

Políticas personalizadas podem ser utilizadas para controlar os percursos de utilizador das seguintes maneiras:

- Definir a interação com APIs para capturar informações adicionais, certifique-se de afirmações de cliente fornecido, acionador ou processos externos.
- Alterar o comportamento com base em afirmações a partir de APIs ou a partir de afirmações no diretório, tais como *migrationStatus*.
- Qualquer fluxo de trabalho não abrangido por políticas incorporadas. Por exemplo, recolha informações mais de um cliente durante uma experiência de início de sessão e efetuar uma autorização verificar para aceder a um recurso.

### <a name="identity-providers"></a>Fornecedores de identidade

Um fornecedor de identidade é um serviço que efetua a autenticação de identidades de cliente e emite tokens de segurança. No Azure AD B2C, pode configurar um número de fornecedores de identidade no seu inquilino, como uma conta Microsoft, Facebook ou Amazon entre outros. 

Para configurar um fornecedor de identidade no seu inquilino do Azure AD B2C, tem de registar o identificador da aplicação ou o identificador de cliente e o segredo do cliente ou a palavra-passe da aplicação de fornecedor de identidade que criar. Este identificador e a palavra-passe, em seguida, são utilizados para configurar a sua aplicação.

### <a name="user-interface-experience"></a>Experiência de Interface de utilizador

A maioria do conteúdo HTML e CSS que é apresentada para os clientes pode ser controlada. Ao utilizar a funcionalidade da página IU personalização, personalizar o aspeto e funcionalidade de qualquer política. Também pode manter a marca e consistência visual entre a sua aplicação e o Azure AD B2C.

O Azure AD B2C executa o código no browser do cliente e utiliza uma abordagem moderna chamada de partilha de recursos de várias origens (CORS). Em primeiro lugar, especifique um URL de uma política com conteúdos HTML personalizados. O Azure AD B2C intercala elementos de IU com o conteúdo HTML que é carregado a partir do URL e, em seguida, apresenta a página para o cliente.

Pode enviar parâmetros para o Azure AD B2C numa cadeia de consulta. Transferindo o parâmetro para o ponto final de HTML, pode alterar dinamicamente o conteúdo da página. Por exemplo, pode alterar a imagem de fundo na página de inscrição ou início de sessão do Azure AD B2C, com base num parâmetro que passa a partir do seu web ou aplicação móvel.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Como começar a utilizar com o Azure AD B2C?

No Azure AD B2C, um inquilino representa a sua organização e é um diretório de utilizadores. Cada inquilino B2C do Azure AD é distinto e separado de outros inquilinos do Azure AD B2C. Um inquilino contém informações sobre os clientes que se ter inscrito para utilizar a aplicação. Por exemplo, as palavras-passe, dados de perfil e permissões.

Terá de ligar o seu inquilino do Azure AD B2C a sua subscrição do Azure para ativar todas as funcionalidades e paga custos de utilização. Para permitir que os clientes do Azure AD B2C iniciar sessão sua aplicação, tem de registar a aplicação num inquilino do Azure AD B2C.

Antes de configurar a sua aplicação para utilizar o Azure AD B2C, terá primeiro de criar um inquilino do Azure AD B2C e registar a sua aplicação. Para registar a sua aplicação, conclua os passos no [Tutorial: registar uma aplicação para ativar a inscrição e o início de sessão utilizando o Azure AD B2C](tutorial-register-applications.md).
  
Se for um programador de aplicações web do ASP.NET, configurar a sua aplicação para autenticar contas utilizando os passos no [Tutorial: Ativar a uma aplicação web para autenticar com as contas utilizando o Azure AD B2C](active-directory-b2c-tutorials-web-app.md).

Se o tem um programador de aplicações de ambiente de trabalho, configurar a aplicação para autenticar contas utilizando os passos no [Tutorial: Ativar a uma aplicação de ambiente de trabalho autenticar com as contas utilizando o Azure AD B2C](active-directory-b2c-tutorials-desktop-app.md).

Se for um programador de aplicações de página única com o Node.js, configurar a sua aplicação para autenticar contas utilizando os passos no [Tutorial: Ativar a uma aplicação de página única autenticar com as contas utilizando o Azure AD B2C](active-directory-b2c-tutorials-spa.md).

## <a name="next-steps"></a>Passos Seguintes

Começar a configurar a aplicação para a experiência de inscrição e o início de sessão ao continuar o tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Registar uma aplicação para ativar a inscrição e o início de sessão utilizando o Azure AD B2C](tutorial-register-applications.md)
