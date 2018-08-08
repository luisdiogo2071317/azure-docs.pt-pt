---
title: O Azure AD iOS, introdução | Documentos da Microsoft
description: Como criar uma aplicação iOS que se integra com o Azure AD para início de sessão e as chamadas do Azure AD protegido APIs com o OAuth.
services: active-directory
documentationcenter: ios
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: jmprieur
ms.openlocfilehash: c370a90cf050a88e66ea0417f018429f7815b7c9
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592242"
---
# <a name="azure-ad-ios-getting-started"></a>O Azure AD iOS, introdução
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Azure Active Directory (Azure AD) fornece a biblioteca de autenticação do Active Directory ou a ADAL, para clientes de iOS que precisam de aceder a recursos protegidos. ADAL simplifica o processo que a aplicação utiliza para obter os tokens de acesso. Para demonstrar como é fácil, neste artigo vamos criar uma aplicação de lista de tarefas do objetivo C que:

* Obtém acesso tokens para chamar o Azure AD Graph API utilizando o [protocolo de autenticação OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Procura um diretório para os utilizadores com um alias de determinado.

Para criar a aplicação de trabalho completo, terá de:

1. Registe a sua aplicação com o Azure AD.
2. Instale e configure a ADAL.
3. Utilize a ADAL para obter os tokens do Azure AD.

Para começar, [transferir a estrutura de aplicação](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) ou [transfira o exemplo concluído](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Também precisa de um inquilino do Azure AD em que pode criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino [Saiba como obter um](quickstart-create-new-tenant.md).


> [!TIP]
> Experimente a pré-visualização do nosso novo [portal do programador](https://identity.microsoft.com/Docs/iOS) que ajuda-o a começar a trabalhar com o Azure AD em apenas alguns minutos. O portal do programador orienta-o ao longo do processo de registar uma aplicação e integrar o Azure AD para o seu código. Quando tiver terminado, terá uma aplicação simples que pode autenticar utilizadores no seu inquilino e back-end, que pode aceitar tokens e executar a validação. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Determinar quais seu redirecionamento URI é para iOS
Para começar com segurança os seus aplicativos em determinados cenários SSO, tem de criar uma *URI de redirecionamento* num formato específico. Um URI de redirecionamento é utilizado para garantir que os tokens de retornam para a aplicação correta que solicitou-los.


O formato de iOS para um redirecionamento URI é:

```
<app-scheme>://<bundle-id>
```

* **esquema de aplicação** -isto é registado no projeto XCode. É como as outras aplicações podem ligar-lhe. Pode encontrar este procedimento em Info. plist -> tipos de URL -> URL de identificador. Deve criar um, se ainda não tiver uma ou mais configurado.
* **id do pacote** -este é o identificador de pacote encontra-se em "identity" nas definições do projeto XCode.

Um exemplo para esse código de início rápido: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. Registar a aplicação de DirectorySearcher
Para definir a sua aplicação para obter os tokens, tem primeiro de registá-lo no seu inquilino do Azure AD e conceder permissão para aceder à API do Azure AD Graph:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Na barra superior, clique na sua conta. Sob o **Directory** lista, escolha o inquilino do Active Directory onde pretende registar a sua aplicação.
3. Clique em **todos os serviços** no painel de navegação à esquerda e, em seguida, selecione **Azure Active Directory**.
4. Clique em **registos de aplicações**e, em seguida, selecione **Add**.
5. Siga as instruções para criar um novo **aplicação cliente nativa**.
  * O **nome** do aplicativo descreve a aplicação aos utilizadores finais.
  * O **Uri de redirecionamento** é uma combinação de esquema e a cadeia de caracteres que o Azure AD utiliza para devolver respostas token. Introduza um valor que é específica para seu aplicativo e baseia-se nas informações de URI de redirecionamento anteriores.
6. Depois de concluir o registo, o Azure AD atribui a aplicação um ID de aplicação único. Precisará desse valor nas próximas seções, por isso, copie-o do separador de aplicação.
7. Do **configurações** página, selecione **permissões obrigatórias** e, em seguida, selecione **Add**. Selecione **Microsoft Graph** como a API e, em seguida, adicione o **Read Directory Data** permissão sob **permissões delegadas**. Esta ação configura a sua aplicação para consultar o Azure AD Graph API para os utilizadores.

## <a name="3-install-and-configure-adal"></a>3. Instale e configure a ADAL
Agora que tem uma aplicação no Azure AD, pode instalar o ADAL e escrever seu código relacionadas com a identidade. Para a ADAL comunicar com o Azure AD, terá de fornecê-lo com algumas informações sobre o registo de aplicação.

1. Comece adicionando a ADAL para o projeto de DirectorySearcher utilizando o CocoaPods.

    ```
    $ vi Podfile
    ```
2. Adicione o seguinte a este podfile:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Agora, carregue o podfile com o CocoaPods. Este passo cria uma área de trabalho XCode nova que carrega.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. No projeto de início rápido, abra o ficheiro plist `settings.plist`. Substitua os valores dos elementos na secção para refletir os valores que introduziu no portal do Azure. O código faz referência a esses valores sempre que utilizar a ADAL.
  * O `tenant` é o domínio de inquilino do Azure AD, por exemplo, contoso.onmicrosoft.com.
  * O `clientId` é o ID de cliente da sua aplicação que copiou do portal.
  * O `redirectUri` é o URL de redirecionamento que registou no portal.

## <a name="4-use-adal-to-get-tokens-from-azure-ad"></a>4. Utilizar a ADAL para obter os tokens do Azure AD
O princípio básico por trás da ADAL é que sempre que a sua aplicação precisa de um token de acesso, ele simplesmente chama uma completionBlock `+(void) getToken : `, e a ADAL faz o resto. 

1. Na `QuickStart` projeto, abra `GraphAPICaller.m` e localize o `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comentário perto da parte superior. Isso é onde passar a ADAL as coordenadas por meio de um CompletionBlock, para comunicar com o Azure AD e dizer a ele como colocar em cache tokens.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Agora, é necessário utilizar este token para procurar utilizadores no gráfico. Encontrar o `// TODO: implement SearchUsersList` comentário. Este método faz um pedido GET para o Azure AD Graph API para consulta para os utilizadores cujo UPN começa com o termo de pesquisa especificada. Para consultar o Azure AD Graph API, precisa incluir um access_token no `Authorization` cabeçalho do pedido. É onde entra ADAL.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```


3. Quando a aplicação solicita um token ao chamar `getToken(...)`, ADAL tenta devolver um token sem pedir ao utilizador as credenciais. Se ADAL determina que o utilizador tem de iniciar sessão obter um token, ele será exibir uma caixa de diálogo de início de sessão, recolher as credenciais do usuário e, em seguida, devolver um token após a autenticação com êxito. Se ADAL não conseguir devolver um token por qualquer motivo, deve ser lançado um `AdalException`.

> [!Note] 
> O `AuthenticationResult` objeto contém um `tokenCacheStoreItem` objeto que pode ser utilizado para recolher as informações que poderão necessitar de seu aplicativo. Guia de introdução, `tokenCacheStoreItem` é utilizado para determinar se a autenticação já foi feita.
>
>

## <a name="5-build-and-run-the-application"></a>5. Compilar e executar a aplicação
Parabéns! Tem agora uma aplicação iOS de trabalho que pode autenticar utilizadores, com segurança chamar as APIs da Web ao utilizar o OAuth 2.0 e obter informações básicas sobre o utilizador. Se ainda não o tiver feito, agora é o tempo para preencher o seu inquilino com alguns usuários. Iniciar a sua aplicação de início rápido e, em seguida, inicie sessão com um desses usuários. Procurar por outros utilizadores com base no respetivo UPN. Feche a aplicação e, em seguida, inicie-o novamente. Tenha em atenção que a sessão do utilizador permanece intacta.

ADAL torna mais fácil incorporar todas estas funcionalidades de identidade comum em seu aplicativo. Ele cuida de todo o trabalho sujo para, como gestão de cache, suporte de protocolo de OAuth, apresentando ao usuário uma interface do Usuário para iniciar sessão, e atualizar a expirou tokens. Tudo o que realmente precisa saber é uma única chamada à API, `getToken`.

Para referência, o exemplo concluído (sem os valores de configuração) é fornecido numa [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). 

## <a name="next-steps"></a>Passos Seguintes
Agora pode passar para cenários adicionais. Poderá querer experimentar:

* [Proteger uma API com o Azure AD da Web de node. js](quickstart-v1-nodejs-webapi.md)
* Saiba [como ativar o SSO em vária aplicações em dispositivos iOS através da ADAL](howto-v1-enable-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

