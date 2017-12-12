## <a name="test-your-code"></a>Testar o seu código

### <a name="test-with-visual-studio"></a>Testar com o Visual Studio
Se estiver a utilizar o Visual Studio, prima **F5** para executar o projeto. O browser abre o http://<span></span>localhost: localização {porta} e ver o **chamar Microsoft Graph API** botão.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Teste com o Python ou outro servidor web
Se não estiver a utilizar o Visual Studio, certifique-se de que o servidor web é iniciado. Configurar o servidor para escutar a uma porta TCP com base na localização do seu **index.html** ficheiro. Para o Python, começar a escutar a porta ao executar a terminal da linha de comandos da pasta de aplicação:
 
```bash
python -m http.server 8080
```
Abra o browser e escreva http://<span></span>localhost:8080 ou http://<span></span>localhost: {porta} onde **porta** é a porta que o servidor web está a escutar. Deverá ver o conteúdo do ficheiro index.html e **chamar Microsoft Graph API** botão.

## <a name="test-your-application"></a>Testar a aplicação

Depois do browser carrega o ficheiro index.html, selecione **chamar Microsoft Graph API**. Na primeira vez que executar a sua aplicação, o browser redireciona-o para o ponto final v 2.0 de Microsoft Azure Active Directory (Azure AD) e é-lhe pedido que inicie sessão no:
 
![Inicie sessão na sua conta SPA de JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Fornecer consentimento de acesso à aplicação

Na primeira vez que iniciar sessão na sua aplicação, é-lhe pedido que forneça o seu consentimento para permitir que a aplicação para aceder ao seu perfil e iniciar sessão no:

![Forneça o seu consentimento para acesso de aplicação](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Ver os resultados da aplicação
Depois de iniciar sessão, deverá ver as informações do perfil de utilizador no **resposta de chamada de API do gráfico** caixa.
 
![Resultados esperados da chamada de Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

Também deverá ver informações básicas sobre o token que foi adquirido no **Token de acesso** e **afirmações Token ID** caixas.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do **user.read** âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no portal de registo. Outras APIs para o Microsoft Graph, bem como as APIs personalizadas para o servidor de back-end, poderá necessitar de âmbitos adicionais. Requer o Microsoft Graph API do **Calendars.Read** âmbito lista de calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o **Calendars.Read** delegado permissão para as informações de registo de aplicação. Em seguida, adicione o **Calendars.Read** âmbito para o **acquireTokenSilent** chamada. 

>[!NOTE]
>O utilizador pode ser pedido para consents adicionais como aumentar o número de âmbitos.

Se uma API de back-end não necessita de um âmbito (não recomendado), pode utilizar o **clientId** como o âmbito no **acquireTokenSilent** e **acquireTokenRedirect** chamadas.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
