---
title: Ferramenta de programa auxiliar de personalização da IU da página no Azure Active Directory B2C | Documentos da Microsoft
description: Uma ferramenta de programa auxiliar usada para demonstrar o recurso de personalização da interface do Usuário de página no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 18f921fb718aeb7ae4add2836fbb6ffabd66668f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445063"
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>O Azure Active Directory B2C: Uma auxiliar ferramenta usada para demonstrar a funcionalidade de personalização de interface (IU) de utilizador de página
Este artigo é um complemento para o [artigo de personalização da interface do Usuário principal](active-directory-b2c-reference-ui-customization.md) no Azure Active Directory (Azure AD) B2C. Os passos seguintes descrevem como testar a funcionalidade de personalização da interface do Usuário de página com o conteúdo HTML e CSS de exemplo que fornecemos.

## <a name="get-an-azure-ad-b2c-tenant"></a>Obter um inquilino do Azure AD B2C
Antes de pode personalizar qualquer coisa, precisará [obter um inquilino do Azure AD B2C](active-directory-b2c-get-started.md) se ainda não tiver uma.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Criar uma política de inscrição ou início de sessão
O conteúdo de exemplo que fornecemos pode ser usado para personalizar as duas páginas num [política de inscrição ou início de sessão](active-directory-b2c-reference-policies.md): a [página de início de sessão unificada](active-directory-b2c-reference-ui-customization.md) e o [página atributos de declaração própria](active-directory-b2c-reference-ui-customization.md). Quando [criar a sua política de inscrição ou início de sessão](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), adicione a conta Local (endereço de e-mail), o Facebook, Google e Microsoft como **fornecedores de identidade**. Estes são os IDPs únicas que aceitará nosso conteúdo HTML de exemplo.  Também pode adicionar um subconjunto destes IDPs se desejar.

## <a name="register-an-application"></a>Registar uma aplicação
Precisará [registar uma aplicação](active-directory-b2c-app-registration.md) no seu inquilino do B2C que pode ser usado para executar a sua política. Depois de registar a sua aplicação, tem algumas opções que pode utilizar para executar, na verdade, a política de inscrição:

* Criar um guia rápido de aplicativos listados na seção "Introdução" o Azure AD B2C [Inscreva-se e iniciar sessão em consumidores nas suas aplicações](active-directory-b2c-overview.md).
* Utilizar previamente criados [recreio do Azure AD B2C](https://aadb2cplayground.azurewebsites.net) aplicação. Se optar por utilizar o recreio, tem de registar uma aplicação no seu inquilino do B2C com a **URI de redirecionamento** `https://aadb2cplayground.azurewebsites.net/`.
* Utilize o **executar agora** botão na sua política no [portal do Azure](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Personalizar a política
Para personalizar o aspeto e funcionalidade da sua política, terá de criar primeiro os ficheiros HTML e CSS usando as convenções específicas do Azure AD B2C. Em seguida, pode carregar seu conteúdo estático numa localização disponível ao público para que possa aceder a Azure AD B2C. Isto pode ser o seu próprio servidor web dedicado, armazenamento de Blobs do Azure, rede de entrega de conteúdos do Azure ou qualquer outro estático que aloja o recurso de fornecedor. Os únicos requisitos são que o seu conteúdo está disponível através de HTTPS e pode ser acedido utilizando a CORS. Assim que mostrei seu conteúdo estático na web, pode editar a política para apontar para esta localização e apresentar esse conteúdo aos seus clientes. O [artigo de personalização da interface do Usuário principal](active-directory-b2c-reference-ui-customization.md) descreve detalhadamente como funciona o recurso de personalização do Azure AD B2C.

Para efeitos deste tutorial, iremos já criou alguns conteúdos de exemplo e -lo alojado no armazenamento de Blobs do Azure. O conteúdo de exemplo é uma personalização muito básica no tema da nossa empresa fictícia, "Wingtip Toys". Para experimentar em sua própria diretiva, siga estes passos:

1. Inicie sessão com o seu inquilino no [portal do Azure](https://portal.azure.com/) e navegue para o painel de funcionalidades do B2C.
2. Clique em **políticas de inscrição ou início de sessão**, clique em sua política e clique em Editar (por exemplo, "b2c\_1\_início de sessão\_se\_início de sessão\_em").
3. Clique em **personalização da IU da página** e, em seguida **unificado a página de inscrição ou início de sessão**.
4. Ativar/desativar a **página personalizada de utilização** mudar para **Sim**. Na **URI da página personalizada** , insira `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Clique em **OK**.
5. Clique em **página de inscrição de conta Local**. Ativar/desativar a **modelo personalizado de utilização** mudar para **Sim**. Na **URI da página personalizada** , insira `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Repita o passo mesmo para o **página de inscrição de conta de redes sociais**.
   Clique em **OK** duas vezes para fechar os painéis de personalização da interface do Usuário.
7. Clique em **Guardar**.

Agora pode testar a política personalizada. Pode utilizar o seu próprio aplicativo ou o recreio do Azure AD B2C se desejar, mas pode também simplesmente clicar o **executar agora** comando no painel da política. Selecione a sua aplicação na caixa de lista pendente e escolha o URI de redirecionamento adequado. Clique nas **executar agora** botão. É aberto um novo separador do browser e pode percorrer a experiência de utilizador de inscrever-se para a sua aplicação com o novo conteúdo no local!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Carregar o conteúdo de exemplo para o armazenamento de Blobs do Azure
Se gostaria de utilizar o armazenamento de Blobs do Azure para alojar o seu conteúdo da página, pode criar sua própria conta de armazenamento e utilizar a nossa ferramenta de programa auxiliar de B2C para carregar os ficheiros.

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Clique em **+ novo** > **dados + armazenamento** > **conta de armazenamento**. Precisará de uma subscrição do Azure para criar uma conta de armazenamento de Blobs do Azure. Pode inscrever-se numa avaliação gratuita no [Web site do Azure](https://azure.microsoft.com/pricing/free-trial/).
3. Fornecer um **Name** para o armazenamento de conta (por exemplo, "contoso") e escolha as seleções adequadas para **escalão de preço**, **grupo de recursos** e  **Subscrição**. Certifique-se de que tem o **afixar ao Startboard** opção marcada. Clique em **Criar**.
4. Volte ao Startboard e clique na conta de armazenamento que acabou de criar.
5. Na **resumo** secção, clique em **contentores**e, em seguida, clique em **+ adicionar**.
6. Fornecer um **Name** para o contentor (por exemplo, "b2c") e selecione **Blob** como o **acessar tipo**. Clique em **OK**.
7. O contentor que criou será apresentada na lista no **Blobs** painel. Anote o URL do contentor; Por exemplo, deve ser semelhante à `https://contoso.blob.core.windows.net/b2c`. Fechar o **Blobs** painel.
8. No painel da conta de armazenamento, clique em **chaves** indique os valores da **nome da conta de armazenamento** e **chave de acesso primária** campos.

> [!NOTE]
> **Chave de acesso primária** é uma credencial de segurança importantes.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>Transferir os ficheiros de exemplo e da ferramenta programa auxiliar
Pode baixar o [armazenamento de Blobs do Azure exemplo de ferramenta e arquivos auxiliares como um ficheiro. zip](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) ou clonar a partir do GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Este repositório contém um `sample_templates\wingtip` diretório, que contém o exemplo HTML, CSS e imagens. Para estes modelos fazer referência a sua própria conta de armazenamento de Blobs do Azure, terá de editar os ficheiros HTML. Open `unified.html` e `selfasserted.html` e substitua todas as ocorrências das `https://localhost` com o URL do seu próprio contentor que escrevi, nos passos anteriores. Deve usar o caminho absoluto dos ficheiros HTML, porque nesse caso, o HTML será fornecido pelo Azure AD, sob o domínio `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Carregar os ficheiros de exemplo
No mesmo repositório, deszipe `B2CAzureStorageClient.zip` e execute o `B2CAzureStorageClient.exe` dentro do ficheiro. Este programa simplesmente irá carregar todos os ficheiros no diretório que especificar para a sua conta de armazenamento e ativar o acesso CORS para esses ficheiros. Se seguiu os passos acima, os ficheiros HTML e CSS agora apontar para a conta de armazenamento. Tenha em atenção que o nome da conta de armazenamento é a parte que precede `blob.core.windows.net`; por exemplo, `contoso`. Pode verificar que o conteúdo tiver sido carregado corretamente ao tentar aceder `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` num browser. Também usar [ http://test-cors.org/ ](http://test-cors.org/) para se certificar de que o conteúdo está agora CORS ativada. (Procure "Estado XHR: 200" no resultado.)

### <a name="customize-your-policy-again"></a>Personalizar a política novamente
Agora que carregar o conteúdo de exemplo para sua própria conta de armazenamento, tem de Editar política de inscrição para fazer referência a ela. Repita os passos a ["Personalizar a sua política de"](#customize-your-policy) secção acima, desta vez usando URLs de sua própria conta de armazenamento. Por exemplo, a localização da sua `unified.html` ficheiro seria `<url-of-your-container>/wingtip/unified.html`.

Agora, pode utilizar o **executar agora** botão ou o seu próprio aplicativo para executar novamente a sua política. O resultado deve ser quase exatamente o mesmo, o que é utilizado o mesmo exemplo HTML e CSS em ambos os casos. No entanto, as políticas estão agora a referenciar a sua própria instância do Blob Storage do Azure e está livres editar e carregar os ficheiros novamente como. Para obter mais informações sobre como personalizar o HTML e CSS, consulte a [artigo de personalização da interface do Usuário principal](active-directory-b2c-reference-ui-customization.md).

