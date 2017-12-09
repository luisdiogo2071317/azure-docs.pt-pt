
## <a name="create-an-application-express"></a>Criar uma aplicação (rápida)
Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:
1. Registar a sua aplicação através do [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Introduza um nome para a aplicação e o seu correio eletrónico
3.  Certifique-se que a opção para a configuração orientado está marcada
4.  Siga as instruções para obter o ID de aplicação e cole-o para o seu código

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Adicione as informações de registo de aplicação à sua solução (avançado)
Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:
1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação
2. Introduza um nome para a aplicação e o seu correio eletrónico 
3. Certifique-se que a opção para a configuração orientado está desmarcada
4. Clique em `Add Platform`, em seguida, selecione `Native Application` e clique em Guardar
5. Copie o GUID no ID da aplicação, volte atrás para Visual Studio, abra `App.xaml.cs` e substitua `your_client_id_here` com o ID de aplicação que acabou de ser registado:

```csharp
private static string ClientId = "your_application_id_here";
```
