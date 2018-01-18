
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Utilizar MSAL para obter um token para a Microsoft Graph API

1.  Em **aplicação** > **java** > **{domínio}. { appname}**, abra `MainActivity`. 
2.  Adicione as seguintes importações:

    ```java
    import android.app.Activity;
    import android.content.Intent;
    import android.util.Log;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    import android.widget.Toast;
    import com.android.volley.*;
    import com.android.volley.toolbox.JsonObjectRequest;
    import com.android.volley.toolbox.Volley;
    import org.json.JSONObject;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import com.microsoft.identity.client.*;
    ```

3. Substitua o `MainActivity` classe com o seguinte código:

    ```java
    public class MainActivity extends AppCompatActivity {

        final static String CLIENT_ID = "[Enter the application Id here]";
        final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
        final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

        /* UI & Debugging Variables */
        private static final String TAG = MainActivity.class.getSimpleName();
        Button callGraphButton;
        Button signOutButton;

        /* Azure AD Variables */
        private PublicClientApplication sampleApp;
        private AuthenticationResult authResult;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            callGraphButton = (Button) findViewById(R.id.callGraph);
            signOutButton = (Button) findViewById(R.id.clearCache);

            callGraphButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onCallGraphClicked();
                }
            });

            signOutButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onSignOutClicked();
                }
            });

    /* Configure your sample app and save state for this activity */
            sampleApp = null;
            if (sampleApp == null) {
                sampleApp = new PublicClientApplication(
                        this.getApplicationContext(),
                        CLIENT_ID);
            }

    /* Attempt to get a user and acquireTokenSilent
    * If this fails we do an interactive request
    */
            List<User> users = null;

            try {
                users = sampleApp.getUsers();

                if (users != null && users.size() == 1) {
            /* We have 1 user */

                    sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
                } else {
            /* We have no user */

            /* Let's do an interactive request */
                    sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
                }
            } catch (MsalClientException e) {
                Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

            } catch (IndexOutOfBoundsException e) {
                Log.d(TAG, "User at this position does not exist: " + e.toString());
            }

        }

    //
    // App callbacks for MSAL
    // ======================
    // getActivity() - returns activity so we can acquireToken within a callback
    // getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
    // getAuthInteractiveCallback() - callback defined to handle acquireToken() case
    //

        public Activity getActivity() {
            return this;
        }

        /* Callback method for acquireTokenSilent calls 
        * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
        * else errors that we need to do an interactive request.
        */
        private AuthenticationCallback getAuthSilentCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call Graph now */
                    Log.d(TAG, "Successfully authenticated");

                /* Store the authResult */
                    authResult = authenticationResult;

                /* call graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    } else if (exception instanceof MsalUiRequiredException) {
                    /* Tokens expired or no session, retry with interactive */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Callback used for interactive request.  If succeeds we use the access
            * token to call the Microsoft Graph. Does not check cache
            */
        private AuthenticationCallback getAuthInteractiveCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call graph now */
                    Log.d(TAG, "Successfully authenticated");
                    Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

                /* Store the auth result */
                    authResult = authenticationResult;

                /* call Graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Set the UI for successful token acquisition data */
        private void updateSuccessUI() {
            callGraphButton.setVisibility(View.INVISIBLE);
            signOutButton.setVisibility(View.VISIBLE);
            findViewById(R.id.welcome).setVisibility(View.VISIBLE);
            ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                    authResult.getUser().getName());
            findViewById(R.id.graphData).setVisibility(View.VISIBLE);
        }

        /* Use MSAL to acquireToken for the end-user
        * Callback will call Graph api w/ access token & update UI
        */
        private void onCallGraphClicked() {
            sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
        }

        /* Handles the redirect from the System Browser */
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
        }

    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Mais informações
#### <a name="get-a-user-token-interactively"></a>Obter um utilizador token interativamente
Chamar o `AcquireTokenAsync` método resulta numa janela que solicita aos utilizadores iniciar sessão. As aplicações necessitam normalmente aos utilizadores iniciar sessão interativamente na primeira vez que necessitam para aceder a recursos protegidos. Também poderá ter de iniciar sessão quando uma operação automática para obter um token de falha (por exemplo, quando palavra-passe um utilizador está expirado).

#### <a name="get-a-user-token-silently"></a>Obter um utilizador token automaticamente
O `AcquireTokenSilentAsync` método processa aquisições token e renovações sem qualquer interação do utilizador. Depois de `AcquireTokenAsync` é executado pela primeira vez, `AcquireTokenSilentAsync` é o habitual método a utilizar para obter os tokens que acedem a recursos protegidos para as chamadas subsequentes, porque as chamadas para pedir ou renovar tokens são efetuadas automaticamente.

Eventualmente, o `AcquireTokenSilentAsync` método irá falhar. Motivos da falha podem ser que o utilizador tem sessão ou alterar a palavra-passe noutro dispositivo. Quando MSAL Deteta que é possível resolver o problema, exigindo que uma ação interativa, é acionado um `MsalUiRequiredException` exceção. A aplicação pode processar esta exceção de duas formas:

* Pode efetuar uma chamada contra `AcquireTokenAsync` imediatamente. Esta chamada resulta numa pedir ao utilizador para iniciar sessão. Este padrão é normalmente utilizado nas aplicações online em que não existe conteúdo disponível offline para o utilizador. O exemplo gerado por esta configuração orientada segue neste padrão, que pode ver no tempo de ação primeiro que executar o exemplo. 
    * Porque nenhum utilizador tiver utilizado a aplicação, `PublicClientApp.Users.FirstOrDefault()` contém um valor nulo e um `MsalUiRequiredException` exceção é emitida. 
    * O código no exemplo, em seguida, processa a exceção ao chamar `AcquireTokenAsync`, que resulta numa pedir ao utilizador para iniciar sessão. 

* Em vez disso, pode apresentar uma indicação de visual aos utilizadores um interativa início de sessão é necessário, para que o utilizador podem selecionar no momento certo para iniciar sessão. Ou pode repetir a aplicação `AcquireTokenSilentAsync` mais tarde. Este padrão é frequentemente utilizado quando os utilizadores podem utilizar outras funcionalidades de aplicação sem interrupção – por exemplo, quando o conteúdo offline está disponível na aplicação. Neste caso, os utilizadores podem decidir quando pretendem iniciar sessão para aceder ao recurso protegido ou Atualize as informações Desatualizadas. Em alternativa, a aplicação pode optar por repita `AcquireTokenSilentAsync` quando a rede for restaurada após ter sido temporariamente indisponível. 
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API utilizando o token que apenas obtido
Adicione os métodos seguintes para o `MainActivity` classe:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Mais informações sobre como efetuar uma chamada REST em relação a uma API protegida

Nesta aplicação de exemplo, `callGraphAPI` chamadas `getAccessToken` e, em seguida, faz com que um HTTP `GET` pedido contra um recurso que necessita de um token e devolve o conteúdo. Este método adiciona o token adquirido no cabeçalho de autorização de HTTP. Para este exemplo, o recurso é o Microsoft Graph API *-me* ponto final, que apresenta as informações do perfil do utilizador.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Configurar a fim de sessão

Adicione os métodos seguintes para o `MainActivity` classe:

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Mais informações sobre a fim de sessão do utilizador

O `onSignOutClicked` método no código anterior remove os utilizadores da cache de utilizador MSAL, que indica eficazmente MSAL para se esqueça do utilizador atual para que um pedido futuro para adquirir um token apenas terá êxito se é efetuada para ser interativo.

Apesar da aplicação neste exemplo suporta utilizadores único, MSAL suporta cenários em que várias contas podem ter sessão iniciadas ao mesmo tempo. Um exemplo é uma aplicação de e-mail em que um utilizador tem várias contas.
<!--end-collapse-->
