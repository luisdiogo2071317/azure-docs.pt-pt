## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testar a consultar o Microsoft Graph API da sua aplicação iOS

Para executar o código no simulador do, prima **comando** + **R**.

![Testar a aplicação no simulador do](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Quando estiver pronto para testar, selecione **chamar Microsoft Graph API**. Quando lhe for pedido, introduza o seu nome de utilizador e palavra-passe.

### <a name="provide-consent-for-application-access"></a>Fornecer consentimento de acesso à aplicação
Na primeira vez que iniciar sessão na sua aplicação, é-lhe pedido que forneça o seu consentimento para permitir que a aplicação para aceder ao seu perfil e iniciar sessão no:

![Forneça o seu consentimento para acesso de aplicação](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Ver os resultados da aplicação
Depois de iniciar sessão, deverá ver as informações do perfil de utilizador devolvidas pela chamada Microsoft Graph API no **registo** secção. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do **user.read** âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no portal de registo. Outras APIs para o Microsoft Graph, bem como as APIs personalizadas para o servidor de back-end, poderá necessitar de âmbitos adicionais. Requer o Microsoft Graph API do **Calendars.Read** âmbito lista de calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o **Calendars.Read** delegado permissão para as informações de registo de aplicação. Em seguida, adicione o **Calendars.Read** âmbito para o **acquireTokenSilent** chamada. 

>[!NOTE]
>O utilizador pode ser pedido para consents adicionais como aumentar o número de âmbitos.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
