---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 248f2575e284ae456578b071013e1a5501329116
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843187"
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Utilizar o Microsoft Authentication Library (MSAL) para obter um token para o Microsoft Graph API

Abra `ViewController.swift` e substitua o código com:

```swift
import UIKit
import MSAL

/// 😃 A View Controller that will respond to the events of the Storyboard.
class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    // Update the below to your client ID you received in the portal. The below is for running the demo only
    let kClientID = "Your_Application_Id_Here"
    
    // These settings you don't need to edit unless you wish to attempt deeper scenarios with the app.
    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    let kAuthority = "https://login.microsoftonline.com/common"
    
    var accessToken = String()
    var applicationContext : MSALPublicClientApplication?

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

    /**
        Setup public client application in viewDidLoad
    */

    override func viewDidLoad() {

        super.viewDidLoad()

        do {

            /**
             Initialize a MSALPublicClientApplication with a given clientID and authority
             - clientId:            The clientID of your application, you should get this from the app portal.
             - authority:           A URL indicating a directory that MSAL can use to obtain tokens. In Azure AD
                                    it is of the form https://<instance/<tenant>, where <instance> is the
                                    directory host (e.g. https://login.microsoftonline.com) and <tenant> is a
                                    identifier within the directory itself (e.g. a domain associated to the
                                    tenant, such as contoso.onmicrosoft.com, or the GUID representing the
                                    TenantID property of the directory)
             - error                The error that occurred creating the application object, if any, if you're
                                    not interested in the specific error pass in nil.
             */

            guard let authorityURL = URL(string: kAuthority) else {
                self.loggingText.text = "Unable to create authority URL"
                return
            }

            let authority = try MSALAuthority(url: authorityURL)
            self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)

        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }

    override func viewWillAppear(_ animated: Bool) {

        super.viewWillAppear(animated)
        signoutButton.isEnabled = !self.accessToken.isEmpty
    }
    
    /**
     This button will invoke the authorization flow.
    */

    @IBAction func callGraphButton(_ sender: UIButton) {

        if self.currentAccount() == nil {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            self.acquireTokenInteractively()
        } else {
            self.acquireTokenSilently()
        }
    }

    func acquireTokenInteractively() {

        guard let applicationContext = self.applicationContext else { return }

        applicationContext.acquireToken(forScopes: kScopes) { (result, error) in

            if let error = error {

                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func acquireTokenSilently() {

        guard let applicationContext = self.applicationContext else { return }

        /**
         Acquire a token for an existing account silently
         - forScopes:           Permissions you want included in the access token received
                                in the result in the completionBlock. Not all scopes are
                                guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
                                authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
                                flow completes, or encounters an error.
         */

        applicationContext.acquireTokenSilent(forScopes: kScopes, account: self.currentAccount()) { (result, error) in

            if let error = error {

                let nsError = error as NSError

                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                if (nsError.domain == MSALErrorDomain
                    && nsError.code == MSALErrorCode.interactionRequired.rawValue) {

                    DispatchQueue.main.async {
                        self.acquireTokenInteractively()
                    }

                } else {
                    self.updateLogging(text: "Could not acquire token silently: \(error)")
                }

                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func currentAccount() -> MSALAccount? {

        guard let applicationContext = self.applicationContext else { return nil }

        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        do {

            let cachedAccounts = try applicationContext.allAccounts()

            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }

        } catch let error as NSError {

            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }

        return nil
    }
```

<!--start-collapse-->
### <a name="more-information"></a>Mais Informações
#### <a name="getting-a-user-token-interactively"></a>Obter um token de utilizador interativamente
Chamar o `acquireToken` método resulta numa janela do browser pedir ao utilizador para iniciar sessão. Aplicativos geralmente exigem um utilizador iniciar sessão interativamente na primeira vez que precisam acessar um recurso protegido, ou quando uma operação silenciosa para adquirir um token falha (por exemplo, a senha do usuário expirou).

#### <a name="getting-a-user-token-silently"></a>Obter um token de utilizador automaticamente
O `acquireTokenSilent` método processa a aquisições de token e a renovação sem qualquer interação do utilizador. Após `acquireToken` é executado pela primeira vez, `acquireTokenSilent` é o método normalmente usado para obter os tokens utilizados para aceder a recursos protegidos por chamadas subsequentes - como chamadas para pedir ou renovar os tokens são feitas automaticamente.

Eventualmente, `acquireTokenSilent` falhará – por exemplo, o utilizador tem sessão ou foi alterado a palavra-passe noutro dispositivo. Quando a MSAL Deteta que o problema pode ser resolvido, exigindo que uma ação interativa, ele é disparado um `MSALErrorCode.interactionRequired` exceção. Seu aplicativo pode manipular essa exceção de duas formas:

1.  Fazer uma chamada contra `acquireToken` imediatamente, o que resulta em pedir ao utilizador para iniciar sessão. Esse padrão é normalmente usado em aplicativos online onde não há nenhum conteúdo offline no aplicativo disponível para o utilizador. O exemplo de aplicação gerado por esta configuração assistida usa esse padrão: para ver isso em tempo de ação a primeira que executar o aplicativo. Uma vez que nenhum utilizador usou o aplicativo `applicationContext.allAccounts().first` irá conter um valor nulo e um ` MSALErrorCode.interactionRequired ` exceção será gerada. O código no exemplo, em seguida, processa a exceção ao chamar `acquireToken` resultando em pedir ao utilizador para iniciar sessão.

2.  Aplicativos também podem tornar uma indicação visual para o usuário que um interativo início de sessão é necessário, para que o usuário pode selecionar o momento certo para iniciar sessão ou a aplicação pode repetir `acquireTokenSilent` num momento posterior. Isto é normalmente utilizado quando o utilizador pode utilizar outras funcionalidades da aplicação sem a ser interrompida – por exemplo, não existe conteúdo offline no aplicativo. Neste caso, o usuário pode decidir quando pretende iniciar sessão para aceder ao recurso protegido ou para atualizar as informações Desatualizadas ou seu aplicativo pode optar por repetir `acquireTokenSilent` quando a rede é restaurada depois de ser temporariamente indisponível.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API com o token de que obteve apenas

Adicione o novo método abaixo para `ViewController.swift`. Este método é utilizado para efetuar uma `GET` solicitação com o Microsoft Graph API a utilizar um *cabeçalho de autorização de HTTP*:

```swift
 func getContentWithToken() {

        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
    
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in

            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }

            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {

                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            self.updateLogging(text: "Result from Graph: \(result))")

        }.resume()
    }
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Fazer uma chamada REST em relação a uma API protegida

Nesse aplicativo de exemplo, o `getContentWithToken()` método é utilizado para fazer um HTTP `GET` em relação a um recurso protegido que necessita de um token do pedido e, em seguida, devolver o conteúdo para o chamador. Este método adiciona o token obtido no *cabeçalho de autorização de HTTP*. Para este exemplo, o recurso é o Microsoft Graph API *me* ponto de extremidade – que exibe informações de perfil do usuário.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Configurar a fim de sessão

Adicione o seguinte método para `ViewController.swift` para terminar sessão do utilizador:

```swift 
 @IBAction func signoutButton(_ sender: UIButton) {

        guard let applicationContext = self.applicationContext else { return }

        guard let account = self.currentAccount() else { return }

        do {

            /**
             Removes all tokens from the cache for this application for the provided account
             - account:    The account to remove from the cache
             */

            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signoutButton.isEnabled = false

        } catch let error as NSError {

            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }

}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Obter mais informações sobre o fim de sessão

O `signoutButton` método Remove o utilizador da cache de utilizador MSAL – isso efetivamente instruirá MSAL esquecer o utilizador atual para que um pedido de futuro para adquirir um token só será bem sucedida se a que esteja a ser interativo.

Embora o aplicativo neste exemplo suporta um único utilizador, MSAL oferece suporte a cenários em que várias contas podem estar conectadas ao mesmo tempo – um exemplo é uma aplicação de e-mail em que um utilizador tiver várias contas.
<!--end-collapse-->

## <a name="register-the-callback"></a>Registre-se a chamada de retorno

Assim que o utilizador é autenticado, o navegador redirecionará o usuário para a aplicação. Siga os passos abaixo para se registrar esse retorno de chamada:

1.  Abra `AppDelegate.swift` e importar a MSAL:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Adicione o método seguinte à sua <code>AppDelegate</code> classe para manipular os retornos de chamada:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```
