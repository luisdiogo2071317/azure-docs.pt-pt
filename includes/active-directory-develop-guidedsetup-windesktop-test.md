## <a name="test-your-code"></a>Testar o seu código

Para executar o projeto no Visual Studio, selecione **F5**. A aplicação **MainWindow** é apresentada, conforme mostrado aqui:

![Testar a aplicação](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Na primeira vez que executar a aplicação e selecione o **chamar Microsoft Graph API** no botão lhe for pedida a sessão. Utilize uma conta do Azure Active Directory (conta escolar ou profissional) ou uma conta Microsoft (live.com, outlook.com) para testá-lo.

![Iniciar sessão para a aplicação](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Fornecer consentimento de acesso à aplicação
Na primeira vez que iniciar sessão na sua aplicação, está também lhe para fornecer consentimento para permitir que a aplicação para aceder ao seu perfil e inicie sessão no, conforme mostrado aqui: 

![Forneça o seu consentimento para acesso de aplicação](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Ver os resultados da aplicação
Depois de iniciar sessão, deverá ver as informações de perfil de utilizador que são devolvidas pela chamada para a Microsoft Graph API. Os resultados são apresentados no **resultados de chamada de API** caixa. Informações básicas sobre o token que foi adquirido através de uma chamada para `AcquireTokenAsync` ou `AcquireTokenSilentAsync` devem ser visíveis na **informações do Token** caixa. Os resultados contêm as seguintes propriedades:

|Propriedade  |Formato  |Descrição |
|---------|---------|---------|
|**Nome** |Nome completo do utilizador |O utilizador do primeiro e último nome.|
|**Nome de Utilizador** |<span>user@domain.com</span> |O nome de utilizador que é utilizado para identificar o utilizador.|
|**Token expira** |DateTime |A hora em que o token expira. MSAL expande a data de expiração renovando o token, conforme necessário.|
|**Token de acesso** |Cadeia |A cadeia do token que é enviada para HTTP pedidos que necessitam de um *cabeçalho de autorização*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do *user.read* âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no Portal de registo de aplicação. Outras APIs para o Microsoft Graph, bem como as APIs personalizadas para o servidor de back-end, poderá necessitar de âmbitos adicionais. Requer o Microsoft Graph API do *Calendars.Read* âmbito lista de calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o *Calendars.Read* delegado permissão para as informações de registo de aplicação. Em seguida, adicione o *Calendars.Read* âmbito para o `acquireTokenSilent` chamada. 

>[!NOTE]
>O utilizador pode ser pedido para consents adicionais como aumentar o número de âmbitos.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
