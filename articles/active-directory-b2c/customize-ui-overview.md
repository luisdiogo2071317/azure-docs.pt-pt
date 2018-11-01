---
title: Sobre a personalização da interface de utilizador no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre como personalizar a interface do usuário para as suas aplicações que utilizam o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: cec9e5bfea16e5ba11f8cd7324acf15188fb907a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634330"
---
# <a name="about-user-interface-customization-in-azure-active-directory-b2c"></a>Sobre a personalização da interface de utilizador no Azure Active Directory B2C

A capacidade de sua marca e personalizar a interface de utilizador (IU) do Azure Active Directory (Azure AD) B2C atende às suas aplicações é importante para fornecer uma experiência totalmente integrada para o seu cliente. Estas experiências incluem o perfil de inscrição, início de sessão, edição e a reposição de palavra-passe. Este artigo fornece informações para ajudar a personalizar a interface do Usuário de seus aplicativos.

Dependendo das suas necessidades quando se trata estas experiências, personalizar a interface do Usuário do seu aplicativo de maneiras diferentes. Por exemplo:

- Se estiver a utilizar [políticas incorporadas](active-directory-b2c-reference-policies.md) para fornecer a palavra-passe de inscrição ou início de sessão, repor ou edição de perfil de experiências na sua aplicação, utilize o [portal do Azure para personalizar a interface do Usuário](tutorial-customize-ui.md).
- Se está a dar início de sessão só, a página de reposição a respetiva palavra-passe que acompanha este artigo e a verificação envia um e-mail, utilizar os mesmos passos de personalização, que são utilizados para um [página de início de sessão no Azure AD](../active-directory/fundamentals/customize-branding.md).
- Se os clientes tentarem editar o respetivo perfil antes de iniciar sessão, eles são redirecionados para uma página que personalizar com os mesmos passos que são utilizados para personalizar a página de início de sessão do Azure AD.
- Se estiver a utilizar [políticas personalizadas](active-directory-b2c-overview-custom.md) fornecer a palavra-passe de inscrição ou início de sessão, reposição ou Editar perfil em seu aplicativo, usar [ficheiros de política para personalizar a interface do Usuário](active-directory-b2c-ui-customization-custom.md).
- Se precisar de fornecer conteúdo dinâmico com base na decisão de um cliente, utilize [conteúdo de página de políticas personalizadas que podem ser alterados](active-directory-b2c-ui-customization-custom-dynamic.md) dependendo de um parâmetro que é enviado numa cadeia de consulta. Por exemplo, a imagem de fundo da página de inscrição ou início de sessão do Azure AD B2C altera, com base num parâmetro que passar de seu aplicativo web ou móvel.

O Azure AD B2C executa o código no browser do seu cliente e utiliza uma abordagem moderna chamada [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/). Em tempo de execução, o conteúdo é carregado a partir de uma URL que especificar numa política. Especificar URLs diferentes para diferentes páginas. Depois de conteúdo é carregado a partir do URL, é mesclado com um fragmento HTML inserido a partir do Azure AD B2C e, em seguida, é apresentada para o seu cliente.

Antes de começar, reveja as seguintes orientações:

- O Azure AD B2C mescla o conteúdo HTML em suas páginas. Não copie e tentar alterar o conteúdo predefinido que fornece o Azure AD B2C. É melhor criar o seu conteúdo HTML do zero e utilize o conteúdo predefinido como referência.
- Por motivos de segurança, não são permitidos para incluir o JavaScript nos seus conteúdos.
- Versões de browser suportado são: 
    - Internet Explorer 11, 10 e o Edge
    - Suporte limitado para o Internet Explorer 9 e 8
    - Google Chrome 42.0 e acima
    - Mozilla Firefox 38.0 e acima
- Certifique-se de que não incluem marcas de formulário em seu HTML porque ele interfere com as operações POST geradas pelo HTML injetado do Azure AD B2C.

## <a name="where-do-i-store-ui-content"></a>Onde posso armazenar o conteúdo de interface do Usuário?

Pode alojar o seu conteúdo em qualquer lugar, tal como da interface do Usuário [armazenamento de Blobs do Azure](../storage/blobs/storage-blobs-introduction.md), servidores, as CDNs, AWS S3, web ou sistemas de partilha de ficheiros. O ponto importante é que alojam o conteúdo num ponto final HTTPS publicamente disponível com a CORS ativada. Tem de utilizar um URL absoluto ao especificá-lo no seu conteúdo.

## <a name="how-do-i-get-started"></a>Como posso começar?

Faça o seguinte procedimento para personalizar a interface do Usuário:

- Criar o HTML bem formado conteúdo com um vazio `<div id="api"></div>` elemento localizados em algum lugar no `<body>`. Este marcas de elemento, qual é inserido o conteúdo do Azure AD B2C. O exemplo seguinte mostra uma página mínima:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- Aloje o seu conteúdo num ponto final HTTPS (com CORS permitido). Ambos OBTERÃO e métodos de pedido de opções tem de estar ativados ao configurar a CORS.
- Use as CSS para os elementos de IU do Azure AD B2C insere na sua página de estilo. O exemplo seguinte mostra um arquivo CSS simple, que também inclui definições para os elementos HTML injetados Inscreva-se:

    ```css 
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Criar ou editar uma política a utilizar os conteúdos que criou.

A tabela seguinte lista os fragmentos HTML do Azure AD B2C intercala o `<div id="api"></div>` elemento localizado no seu conteúdo.

| Página inserida | Descrição de HTML |
| ------------- | ------------------- |
| Seleção de fornecedor de identidade | Contém uma lista de botões para fornecedores de identidade que o cliente pode escolher de entre durante a inscrição ou início de sessão. Esses botões incluem fornecedores de identidade de redes sociais, como o Facebook, Google ou contas locais (com base no nome de utilizador ou endereço de e-mail). |
| Inscrição de conta local | Contém um formulário de inscrição de conta local com base num endereço de e-mail ou um nome de utilizador. O formulário pode conter controles de entrada diferentes, como a caixa de entrada de texto, caixa de entrada de palavra-passe, o botão de rádio, caixas de lista pendente de seleção única e caixas de verificação de seleção múltipla. |
| Inscrição de conta social | Podem ser exibidas ao inscrever-se com uma conta existente de um fornecedor de identidade de redes sociais como o Facebook ou do Google. É utilizado quando informações adicionais que devem ser recolhidas do cliente através de um formulário de inscrição. |
| A unificação de inscrição ou início de sessão | Identificadores de inscrição e início de sessão de clientes que podem utilizar fornecedores de identidade de redes sociais, como o Facebook, Google ou contas locais. |
| Multi-Factor Authentication | Os clientes podem verificar seus números de telefone (usando o Editor de texto ou de voz) durante a inscrição ou início de sessão. |
| Erro | Fornece informações de erro para o cliente. |


## <a name="how-do-i-localize-content"></a>Como localizar o conteúdo?

Localizar o seu conteúdo HTML, permitindo [personalização de idioma](active-directory-b2c-reference-language-customization.md) no seu inquilino do Azure AD B2C. Ativar esta funcionalidade permite que o Azure AD B2C para reencaminhar o parâmetro abrir ID Connect `ui-locales` para o ponto final. O servidor de conteúdo pode utilizar este parâmetro para fornecer páginas HTML de idioma específico.

Conteúdo pode ser obtido a partir de locais diferentes com base na localidade que é utilizada. O ponto final CORS ativado, vai configurar uma estrutura de pastas para alojar conteúdo para idiomas específicos. Chamará o um certo se usar o valor de caráter universal {cultura: RFC5646}. Por exemplo, sua página personalizada como URI pode ser `https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html`. Pode carregar a página em francês ao extrair o conteúdo a partir de `https://contoso.blob.core.windows.net/fr/myHTML/unified.html`

## <a name="examples"></a>Exemplos

Para obter exemplos de personalização, transfira e reveja estes [arquivos de modelo de exemplo](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).

## <a name="next-steps"></a>Passos Seguintes

- Se estiver a utilizar as políticas incorporadas, pode começar a personalizar a interface do Usuário com o tutorial: [personalizar a interface do usuário das suas aplicações no Azure Active Directory B2C](tutorial-customize-ui.md).
- Se estiver a utilizar políticas personalizadas, pode começar a personalizar a interface do Usuário com o artigo: [personalizar a interface de utilizador da sua aplicação utilizando uma política personalizada no Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).

