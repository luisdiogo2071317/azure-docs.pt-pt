
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Configurar a sua aplicação Web do ASP.NET com as informações de registo da aplicação

Neste passo, irá configurar o seu projeto para utilizar SSL e, em seguida, utilize o URL de SSL para configurar as informações de registo da aplicação. Depois da primeira, adicionar a aplicação ' as informações de registo para a sua solução através de *Web. config*.

1.  No Explorador de soluções, selecione o projeto e observe o `Properties` janela (se não for apresentada uma janela de propriedades, prima F4)
2.  Alteração `SSL Enabled` para `True`
3.  Copie o valor de `SSL URL` acima e cole-a no `Redirect URL` campo na parte superior desta página, em seguida, clique em *atualização*:<br/><br/>![Propriedades do projeto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4.  Adicione o seguinte no `web.config` ficheiro localizado na pasta da raiz, na secção `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
