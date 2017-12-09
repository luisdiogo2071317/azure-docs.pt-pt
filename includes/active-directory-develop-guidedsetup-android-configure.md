
## <a name="create-an-application-express"></a>Criar uma aplicação (rápida)
Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:
1. Registar a sua aplicação através do [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Introduza um nome para a aplicação e o seu correio eletrónico
3.  Certifique-se que a opção para a configuração orientado está marcada
4.  Siga as instruções para obter o ID de aplicação e cole-o para o seu código

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Adicione as informações de registo de aplicação à sua solução (avançado)
Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:
1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação
2. Introduza um nome para a aplicação e o seu correio eletrónico 
3. Certifique-se que a opção para a configuração orientado está desmarcada
4. Clique em `Add Platform`, em seguida, selecione `Native Application` e clique em Guardar
5.  Abra `MainActivity` (em `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Substitua o *[Introduza aqui a aplicação Id]* na linha que começa por `final static String CLIENT_ID` com o ID de aplicação que acabou de ser registado:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Abra `AndroidManifest.xml` (em `app`  >  `manifests`) adicione a seguinte atividade para `manifest\application` nós. Este procedimento regista um `BrowserTabActivity` para permitir que o SO retomar a sua aplicação depois de concluir a autenticação:
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
No `BrowserTabActivity`, substitua `[Enter the application Id here]` com o ID de aplicação.
</li>
</ol>
