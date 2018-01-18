## <a name="test-your-code"></a>Testar o seu código

1. Implemente o código para o dispositivo/emulador.

2. Quando estiver pronto para testar a aplicação, utilize uma conta do Azure Active Directory (conta escolar ou profissional) ou uma conta Microsoft (live.com, outlook.com) para iniciar sessão. 

    ![Testar a aplicação](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Introduza o nome de utilizador e palavra-passe](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Fornecer consentimento de acesso à aplicação
Na primeira vez que iniciar sessão na sua aplicação, está também lhe para fornecer consentimento para permitir que a aplicação para aceder ao seu perfil e inicie sessão no, conforme mostrado aqui: 

![Forneça o seu consentimento para acesso de aplicação](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Ver os resultados da aplicação
Depois de iniciar sessão, deverá ver os resultados que são devolvidos pela chamada para a Microsoft Graph API. A chamada para a Microsoft Graph API **-me** endpoint devolve o [perfil de utilizador](https://graph.microsoft.com/v1.0/me). Para obter uma lista de pontos finais de Microsoft Graph comuns, consulte [documentação de programador do Microsoft Graph API](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do *user.read* âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no Portal de registo de aplicação. Outras APIs para o Microsoft Graph, bem como as APIs personalizadas para o servidor de back-end, poderá necessitar de âmbitos adicionais. Requer o Microsoft Graph API do *Calendars.Read* âmbito lista de calendários do utilizador. 

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o *Calendars.Read* delegado permissão para as informações de registo de aplicação. Em seguida, adicione o *Calendars.Read* âmbito para o `acquireTokenSilent` chamada. 

>[!NOTE]
>O utilizador pode ser pedido para consents adicionais como aumentar o número de âmbitos.

<!--end-collapse-->

[!INCLUDE  [Help and support](active-directory-develop-help-support-include.md)]
