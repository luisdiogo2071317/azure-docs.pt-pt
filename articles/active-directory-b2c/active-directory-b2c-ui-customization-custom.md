---
title: Personalizar uma interface do Usuário com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre como personalizar uma interface de utilizador (IU) ao utilizar políticas personalizadas no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9908a7cf96c56e414e0a8d7faea0352b60214ea4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446168"
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>O Azure Active Directory B2C: Configurar a personalização da interface do Usuário numa política personalizada

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Depois de concluir este artigo, terá uma política personalizada de inscrição e início de sessão com a marca e do aspeto. Com o Azure Active Directory B2C (Azure AD B2C), obtém controle quase total do conteúdo HTML e CSS que é apresentado aos utilizadores. Quando utilizar uma política personalizada, configurar personalização da interface do Usuário em XML em vez de usar controles no portal do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, conclua [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md). Deve ter uma política personalizada do trabalho para inscrição e início de sessão com contas locais.

## <a name="page-ui-customization"></a>Personalização da IU da página

Ao utilizar a funcionalidade de personalização da interface do Usuário de página, pode personalizar o aspeto e funcionalidade de qualquer política personalizada. Também pode manter a marca e a consistência visual entre a aplicação e do Azure AD B2C.

Eis como funciona: Azure AD B2C executa o código no browser do seu cliente e utiliza uma abordagem moderna chamada [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/). Em primeiro lugar, especifique um URL a política personalizada com conteúdo HTML personalizado. O Azure AD B2C mescla os elementos de interface do Usuário com o conteúdo HTML que são carregados a partir do URL e, em seguida, apresenta a página para o cliente.

## <a name="create-your-html5-content"></a>Criar o HTML5 conteúdo

Crie o HTML conteúdo com o nome da marca do seu produto no título.

1. Copie o seguinte fragmento HTML. É bem formado HTML5 com um elemento vazio denominado *\<div id = "api"\>\</div\>* localizada dentro do *\<corpo\>* etiquetas. Este elemento indica onde o conteúdo do Azure AD B2C está a ser inserido.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Por motivos de segurança, o uso de JavaScript está bloqueado para personalização.

2. Cole o fragmento copiado num editor de texto e, em seguida, guarde o ficheiro como *ui.html personalizar*.

## <a name="create-an-azure-blob-storage-account"></a>Criar uma conta de armazenamento de Blobs do Azure

>[!NOTE]
> Neste artigo, utilizamos o armazenamento de Blobs do Azure para alojar o nosso conteúdo. Pode optar por hospedar o seu conteúdo num servidor web, mas deve [ativar o CORS no seu servidor web](https://enable-cors.org/server.html).

Para alojar este conteúdo HTML no armazenamento de BLOBs, faça o seguinte:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Sobre o **Hub** menu, selecione **New** > **armazenamento** > **conta de armazenamento**.
3. Introduza uma única **nome** para a sua conta de armazenamento.
4. **Modelo de implementação** pode permanecer **do Resource Manager**.
5. Alteração **tipo de conta** ao **armazenamento de BLOBs**.
6. **Desempenho** pode permanecer **padrão**.
7. **A replicação** pode permanecer **RA-GRS**.
8. **Camada de acesso** pode permanecer **frequente**.
9. **Encriptação do serviço de armazenamento** pode permanecer **desativado**.
10. Selecione um **subscrição** para a sua conta de armazenamento.
11. Criar uma **grupo de recursos** ou selecione um existente.
12. Selecione o **localização geográfica** para a sua conta de armazenamento.
13. Clique em **Criar** para criar a conta do Storage.  
    Depois de concluída a implementação, o **conta de armazenamento** painel abre-se automaticamente.

## <a name="create-a-container"></a>Criar um contentor

Para criar um contentor público no armazenamento de BLOBs, faça o seguinte:

1. Clique nas **descrição geral** separador.
2. Clique em **contentor**.
3. Para **Name**, tipo **$root**.
4. Definir **tipo de acesso** ao **Blob**.
5. Clique em **$root** para abrir o contêiner do novo.
6. Clique em **Carregar**.
7. Clique no ícone de pasta junto a **selecionar um ficheiro**.
8. Aceda a **ui.html personalizar**, que criou anteriormente no [personalização da IU da página](#the-page-ui-customization-feature) secção.
9. Clique em **Carregar**.
10. Selecione o blob de personalizar ui.html que carregou.
11. Junto a **URL**, clique em **cópia**.
12. Num browser, cole o URL copiado e vá para o site. Se o site não estiver acessível, certifique-se de que o tipo de acesso do contentor está definido como **BLOBs**.

## <a name="configure-cors"></a>Configurar o CORS

Configure o armazenamento de BLOBs para a partilha de recursos de várias origens, fazendo o seguinte:

>[!NOTE]
>Quer experimentar a funcionalidade de personalização da interface do Usuário com o nosso exemplo HTML e conteúdo CSS? Fornecemos [uma ferramenta simples auxiliar](active-directory-b2c-reference-ui-customization-helper-tool.md) que carrega e configura o nosso conteúdo de exemplo na sua conta de armazenamento de Blobs. Se usar a ferramenta, avançar diretamente para [modificar a política de inscrição ou início de sessão personalizada](#modify-your-sign-up-or-sign-in-custom-policy).

1. Sobre o **armazenamento** painel, em **definições**, abra **CORS**.
2. Clique em **Adicionar**.
3. Para **permitido origens**, escreva um asterisco (\*).
4. Na **verbos permitidos** na lista pendente, selecione **obter** e **opções**.
5. Para **permitido cabeçalhos**, escreva um asterisco (\*).
6. Para **expostos cabeçalhos**, escreva um asterisco (\*).
7. Para **idade máxima (segundos)**, tipo **200**.
8. Clique em **Adicionar**.

## <a name="test-cors"></a>Testar o CORS

Valide a que está pronto, fazendo o seguinte:

1. Vá para o [www.test-cors.org](http://www.test-cors.org/) Web site e, em seguida, cole o URL na **URL remoto** caixa.
2. Clique em **enviar pedido**.  
    Se receber um erro, certifique-se de que sua [definições de CORS](#configure-cors) estão corretos. Também poderá ter de limpar a cache do browser ou abra uma sessão de navegação privada ao premir Ctrl + Shift + P.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Modificar a política de inscrição ou início de sessão personalizada

Sob o nível superior *\<TrustFrameworkPolicy\>* marcar, deve encontrar *\<BuildingBlocks\>* marca. Dentro do *\<BuildingBlocks\>* adicionar etiquetas, uma *\<ContentDefinitions\>* marca ao copiar o exemplo seguinte. Substitua *your_storage_account* com o nome da conta de armazenamento.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0</DataUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Carregar a política personalizada atualizada

1. Na [portal do Azure](https://portal.azure.com), [mudar para o contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e, em seguida, abra a **do Azure AD B2C** painel.
2. Clique em **todas as políticas**.
3. Clique em **carregar política**.
4. Carregue `SignUpOrSignin.xml` com o *\<ContentDefinitions\>* etiqueta que adicionou anteriormente.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testar a política personalizada com **executar agora**

1. Sobre o **do Azure AD B2C** painel, aceda à **todas as políticas**.
2. Selecione a política personalizada que carregou e clique nas **executar agora** botão.
3. Deverá conseguir inscrever-se utilizando um endereço de e-mail.

## <a name="reference"></a>Referência

Pode encontrar modelos de exemplo para a personalização da interface do Usuário aqui:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

A pasta de sample_templates/wingtip contém os seguintes ficheiros HTML:

| Modelo de HTML5 | Descrição |
|----------------|-------------|
| *phonefactor.html* | Utilize este ficheiro como um modelo para uma página de autenticação multifator. |
| *resetpassword.html* | Utilizar este ficheiro como um modelo para um Esqueceu-se a página de palavra-passe. |
| *selfasserted.html* | Utilize este ficheiro como um modelo para uma página de inscrição de conta de redes sociais, uma página de inscrição de conta local ou uma página de início de sessão da conta local. |
| *unified.html* | Utilize este ficheiro como um modelo para uma página de inscrição ou início de sessão unificada. |
| *updateprofile.html* | Utilize este ficheiro como um modelo para uma página de atualização de perfil. |

Na [modificar a secção de inscrição ou início de sessão de política personalizada](#modify-your-sign-up-or-sign-in-custom-policy), que configurou a definição de conteúdo para `api.idpselections`. O conjunto completo de conteúdo são de IDs de definição reconhecidas pelo framework de experiência de identidade do Azure AD B2C e suas descrições na seguinte tabela:

| ID de definição de conteúdo | Descrição | 
|-----------------------|-------------|
| *api.error* | **Página de erro**. Esta página é apresentada quando é encontrado uma exceção ou um erro. |
| *api.idpselections* | **Página de seleção do fornecedor de identidade**. Esta página contém uma lista de fornecedores de identidade que o utilizador pode escolher durante o início de sessão. Estas opções são a fornecedores de identidade empresarial, os fornecedores de identidade social como o Facebook e Google + ou contas locais. |
| *api.idpselections.signup* | **Seleção de fornecedor de identidade para inscrição**. Esta página contém uma lista de fornecedores de identidade que o utilizador pode escolher entre durante a inscrição. Estas opções são a fornecedores de identidade empresarial, os fornecedores de identidade social como o Facebook e Google + ou contas locais. |
| *api.localaccountpasswordreset* | **Esqueceu-se a página de palavra-passe**. Esta página contém um formulário que o utilizador deve realizar para iniciar uma reposição de palavra-passe.  |
| *api.localaccountsignin* | **Página de início de sessão de conta local**. Esta página contém um formulário de início de sessão para iniciar sessão com uma conta local que se baseia num endereço de e-mail ou um nome de utilizador. O formulário pode conter uma caixa de entrada de texto e a caixa de entrada de palavra-passe. |
| *api.localaccountsignup* | **Página de inscrição de conta local**. Esta página contém um formulário de inscrição para inscrever-se de uma conta local que se baseia num endereço de e-mail ou um nome de utilizador. O formulário pode conter vários controles de entrada, como uma caixa de entrada de texto, uma caixa de entrada de palavra-passe, um botão de rádio, caixas de lista pendente de seleção única e caixas de verificação de seleção múltipla. |
| *api.phonefactor* | **Página do multi-factor authentication**. Nesta página, os usuários podem verificar seus números de telefone (através da utilização de texto ou voz) durante a inscrição ou início de sessão. |
| *api.selfasserted* | **Página de inscrição de conta de redes sociais**. Esta página contém um formulário de inscrição tem de concluir os utilizadores quando se inscrevem, utilizando uma conta existente de um fornecedor de identidade de redes sociais como o Facebook ou o Google +. Esta página é semelhante ao anterior conta social página de inscrição, exceto os campos de entrada de palavra-passe. |
| *api.selfasserted.profileupdate* | **Página de atualização de perfil**. Esta página contém um formulário que os utilizadores podem utilizar para atualizar seu perfil. Esta página é semelhante à página de inscrição de conta de redes sociais, exceto os campos de entrada de palavra-passe. |
| *api.signuporsignin* | **Página de inscrição ou início de sessão unificada**. Esta página lida com a inscrição e início de sessão de utilizadores, que podem utilizar fornecedores de identidade empresarial, os fornecedores de identidade social como o Facebook ou Google + ou contas locais.  |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os elementos de interface do Usuário que pode ser personalizado, consulte [guia de referência para a personalização da interface do Usuário para as políticas incorporadas](active-directory-b2c-reference-ui-customization.md).
