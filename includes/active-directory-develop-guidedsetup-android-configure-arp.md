## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação à sua aplicação

Neste passo, terá de adicionar o ID de cliente ao seu projeto.

1.  Open `MainActivity` (sob `app`  >  `java`  >  *`{host}.{namespace}`*)
2.  Substitua a linha que começa com `final static String CLIENT_ID` com:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Abrir: `app` > `manifests` > `AndroidManifest.xml`
4. Adicionar a atividade seguinte seja `manifest\application` nó. Neste Registro um `BrowserTabActivity` para permitir que o sistema operacional retomar a sua aplicação depois de concluir a autenticação:

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

