
## <a name="register-your-application"></a>Registar a sua aplicação
Pode registar a aplicação em qualquer uma das duas formas, conforme descrito nas dois secções.

### <a name="option-1-express-mode"></a>Opção 1: Modo de Express
Pode registar rapidamente a sua aplicação da seguinte forma:
1. Vá para o [Portal de registo de aplicações da Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  No **nome da aplicação** caixa, introduza um nome para a sua aplicação.

3. Certifique-se de que o **orientado configuração** caixa de verificação está selecionada e, em seguida, selecione **criar**.

4. Siga as instruções para obter o ID da aplicação e cole-o seu código.

### <a name="option-2-advanced-mode"></a>Opção 2: O modo avançado
Para registar a sua aplicação e adicione as informações de registo de aplicação à sua solução, efetue o seguinte:
1. Se ainda não registou a aplicação, vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. No **nome da aplicação** caixa, introduza um nome para a sua aplicação. 

3. Certifique-se de que o **orientado configuração** caixa de verificação está desmarcada e, em seguida, selecione **criar**.

4. Selecione **adicionar plataforma**, selecione **aplicação nativa**e, em seguida, selecione **guardar**.

5. Em **aplicação** > **java** > **{anfitrião}. { Namespace}**, abra `MainActivity`. 

6.  Substitua *[Introduza aqui a aplicação Id]* na linha seguinte com o ID da aplicação que acabou de registar:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Em **aplicação** > **manifestos**, abra o *AndroidManifest.xml* ficheiro.

8. No `manifest\application` nó, adicione a seguinte atividade. Se o fizer por isso, regista um `BrowserTabActivity` atividade que permite que o SO retomar a sua aplicação depois de concluir a autenticação:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
9. No `BrowserTabActivity` nó, substitua `[Enter the application Id here]` com o ID de aplicação.
