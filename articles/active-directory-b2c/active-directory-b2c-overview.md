---
title: O que é o Azure Active Directory B2C? | Microsoft Docs
description: Saiba como criar e gerir a sua experiência de início de sessão em aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 10/23/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fdb856f92bf790df0065cfc74ce5896f1d10c47b
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954111"
---
# <a name="what-is-azure-active-directory-b2c"></a>O que é o Azure Active Directory B2C?

O Azure Active Directory (Azure AD) B2C é um serviço de gestão de identidades que lhe permite personalizar e controlar a forma como os clientes interagem com a sua aplicação. Esta interação inclui a inscrição, o início de sessão e a gestão dos respetivos perfis quando os clientes utilizam as suas aplicações. Isto inclui aplicações para iOS, Android e .NET, entre outros. O Azure AD B2C permite estas ações ao mesmo tempo que protege as identidades dos seus clientes.

Pode configurar uma aplicação registada no Azure AD B2C para realizar inúmeras tarefas de gestão de identidades. Alguns exemplos incluem:

- Permitir que um cliente se inscreva para utilizar a sua aplicação registada
- Permitir que um cliente inscrito inicie sessão e comece a utilizar a sua aplicação
- Permitir que um cliente inscrito edite o respetivo perfil
- Ativar a autenticação multifator na sua aplicação
- Permitir que o cliente se inscreva e inicie sessão com fornecedores de identidade específicos
- Conceder acesso a partir da sua aplicação a APIs criadas por si
- Personalizar o aspeto e a funcionalidade da experiência de inscrição e de início de sessão
- Gerir sessões de início de sessão único para a sua aplicação

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>O que é preciso ter em conta antes de utilizar o Azure AD B2C?

- Como pretendo que o cliente interaja com a minha aplicação?
- Qual é a experiência de interface de utilizador (IU) que pretendo proporcionar aos clientes?
- Que fornecedores de identidade pretendo permitir que os clientes escolham na minha aplicação?
- O meu processo de início de sessão requer APIs adicionais para ser executado?

### <a name="customer-interaction"></a>Interação do cliente

O Azure AD B2C suporta o [OpenID Connect](https://openid.net/connect/) para todas as experiências de cliente. Na implementação do OpenID Connect do Azure AD B2C, a sua aplicação começa o percurso do utilizador através da emissão de pedidos de autenticação para o Azure AD B2C. O resultado do pedido é um `id_token`. Este token de segurança define a identidade do cliente.

Cada aplicação que utiliza o Azure AD B2C tem de ser registada num inquilino do Azure AD B2C com o portal do Azure. O processo de registo recolhe e atribui valores à sua aplicação. Estes valores incluem um ID de aplicação que identifica exclusivamente a aplicação. É definido um URI de redirecionamento utilizado para direcionar as respostas de volta para a aplicação.

A interação de cada aplicação segue um padrão de alto nível semelhante:

1. A aplicação direciona o cliente para executar uma política.
2. O cliente conclui a política de acordo com a definição de política.
3. A aplicação recebe um token de segurança.
4. A aplicação utiliza o token de segurança para tentar aceder a um recurso protegido.
5. O servidor de recurso valida o token de segurança para verificar se o acesso pode ser concedido.
6. A aplicação atualiza periodicamente o token de segurança.

Estes passos diferem ligeiramente consoante o tipo de aplicação que está a criar.

O Azure AD B2C interage com fornecedores de identidade, clientes, outros sistemas e com o diretório local sequencialmente para concluir uma tarefa de identidade. Por exemplo, iniciar sessão de um cliente, registar um novo cliente ou repor uma palavra-passe. A plataforma subjacente que estabelece a confiança de múltiplos intervenientes e conclui estes passos é denominada Framework de Experiência de Identidade. Esta arquitetura e uma política (também denominada um percurso do utilizador ou uma política de Arquitetura Fidedigna) define explicitamente os atores, as ações, os protocolos e a sequência de passos a concluir.

O Azure AD B2C protege de ataques denial of service e de palavra-passe lançados contra as suas aplicações. O Azure AD B2C utiliza técnicas de deteção e mitigação, como cookies SYN e limites de velocidade e de ligação para proteger os recursos contra ataques denial of service. A mitigação também é incluída para ataques de força bruta de palavra-passe e ataques de palavra-passe de dicionário.

#### <a name="built-in-policies"></a>Políticas incorporadas

Cada pedido enviado para o Azure AD B2C especifica uma política. Uma política controla o comportamento de interação da sua aplicação com o Azure AD B2C. As políticas incorporadas estão predefinidas para as tarefas de identidade mais comuns, tais como inscrição, início de sessão e edição de perfil.  Por exemplo, uma política de inscrição permite-lhe controlar comportamentos através da configuração das seguintes definições:

- Contas de redes sociais que o cliente utiliza para se inscrever na aplicação
- Dados recolhidos do cliente, como o nome próprio ou o código postal
- Multi-Factor Authentication
- Aspeto e funcionalidade de todas as páginas de inscrição
- Informações devolvidas à aplicação

#### <a name="custom-policies"></a>Políticas personalizadas 

[Políticas personalizadas](active-directory-b2c-overview-custom.md) são ficheiros de configuração que definem o comportamento do [Framework de Experiência de Identidade](trustframeworkpolicy.md) no seu inquilino do Azure AD B2C. As políticas personalizadas podem ser alteradas para realizar muitas tarefas. Uma política personalizada é representada como um ou vários ficheiros com formatação XML que fazem referência entre si numa cadeia hierárquica. 

São utilizadas políticas personalizadas de diferentes tipos no seu inquilino do Azure AD B2C, conforme necessário, e podem ser reutilizadas em todas as aplicações. Esta flexibilidade permite-lhe definir e modificar experiências de identidade de cliente com alterações mínimas ou sem alterações ao seu código. As políticas são utilizadas ao adicionar um parâmetro de consulta especial a pedidos de autenticação HTTP.

As políticas personalizadas são utilizadas para controlar os percursos do utilizador das seguintes formas:

- Definindo a interação com APIs para capturar informações adicionais, verificar afirmações fornecidas pelo cliente ou acionar processos externos.
- Alterando o comportamento com base nas afirmações das APIs ou de afirmações no diretório, como *migrationStatus*.
- Qualquer fluxo de trabalho não abrangido por políticas incorporadas. Por exemplo, recolhendo mais informações de um cliente durante uma experiência de início de sessão e realizando uma verificação de autorização para aceder a um recurso.

### <a name="identity-providers"></a>Fornecedores de identidade

Um fornecedor de identidade é um serviço que efetua a autenticação de identidades de clientes e emite tokens de segurança. No Azure AD B2C, pode configurar vários fornecedores de identidade no seu inquilino, como uma [conta Microsoft](active-directory-b2c-setup-msa-app.md), [Facebook](active-directory-b2c-setup-fb-app.md) ou [Amazon](active-directory-b2c-setup-amzn-app.md), entre outros. 

Para configurar um fornecedor de identidade no seu inquilino do Azure AD B2C, tem de registar o identificador da aplicação ou o identificador do cliente e a palavra-passe ou o segredo do cliente a partir da aplicação de fornecedor de identidade que criar. Em seguida, este identificador e palavra-passe são utilizados para configurar a sua aplicação.

### <a name="user-interface-experience"></a>Experiência de Interface de Utilizador

A maior parte do conteúdo HTML e CSS que é apresentado aos clientes é controlável. Ao utilizar a funcionalidade de personalização da IU de página, personaliza o aspeto e a funcionalidade de qualquer política. Pode manter a consistência visual e de marca entre a sua aplicação e o Azure AD B2C através desta funcionalidade de personalização.

O Azure AD B2C executa código no browser do cliente e utiliza uma abordagem moderna chamada Partilha de Recursos Transversais à Origem (CORS). Em primeiro lugar, especifique um URL numa política com conteúdo HTML personalizado. O Azure AD B2C une os elementos de IU com o conteúdo HTML que é carregado a partir do URL e, em seguida, apresenta a página ao cliente.

Pode enviar parâmetros para o Azure AD B2C numa cadeia de consulta. Ao transmitir o parâmetro para o ponto final HTML, o conteúdo da página é alterado dinamicamente. Por exemplo, pode alterar a imagem de fundo na página de inscrição ou de início de sessão do Azure AD B2C, com base num parâmetro que transmite a partir da sua aplicação Web ou móvel.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Como posso começar a utilizar o Azure AD B2C?

No Azure AD B2C, um inquilino representa a sua organização e é um diretório de utilizadores. Cada inquilino do Azure AD B2C é distinto e independente dos outros inquilinos do Azure AD B2C. Um inquilino contém informações sobre os clientes que se inscreveram para utilizar a sua aplicação. Por exemplo, palavras-passe, dados de perfil e permissões.

Pode associar o seu inquilino do Azure AD B2C à sua subscrição do Azure para ativar todas as funcionalidades e pagar os custos de utilização. Para permitir aos clientes iniciar sessão na sua aplicação, registe-a num inquilino do Azure AD B2C.

Antes de configurar a sua aplicação para utilizar o Azure AD B2C, primeiro tem de criar um inquilino do Azure AD B2C e registar a sua aplicação. Para registar a sua aplicação, execute os passos em [Tutorial: Registar uma aplicação para ativar a inscrição e o início de sessão com o Azure AD B2C](tutorial-register-applications.md).
  
Se for programador de aplicações Web ASP.NET, configure a sua aplicação para autenticar contas com os passos em [Tutorial: Ativar uma aplicação Web para fazer a autenticação com contas através do Azure AD B2C](active-directory-b2c-tutorials-web-app.md).

Se for programador de aplicações de ambiente de trabalho, configure a sua aplicação para autenticar contas com os passos em [Tutorial: Ativar uma aplicação de ambiente de trabalho para fazer a autenticação com contas através do Azure AD B2C](active-directory-b2c-tutorials-desktop-app.md).

Se for programador de aplicações de página única com Node.js, configure a sua aplicação para autenticar contas com os passos em [Tutorial: Ativar uma aplicação de página única para fazer a autenticação com contas através do Azure AD B2C](active-directory-b2c-tutorials-spa.md).

## <a name="next-steps"></a>Passos seguintes

Inicie a configuração da sua aplicação para a experiência de inscrição e de início de sessão ao avançar para o tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Criar um inquilino do Azure Active Directory B2C](tutorial-create-tenant.md)
