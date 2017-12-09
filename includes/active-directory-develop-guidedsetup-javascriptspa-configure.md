
## <a name="register-your-application"></a>Registar a sua aplicação

Existem várias formas para criar uma aplicação, selecione um dos atributos:

### <a name="option-1-register-your-application-express-mode"></a>Opção 1: Registar a sua aplicação (modo Express)
Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:

1.  Registar a sua aplicação através do [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Introduza um nome para a aplicação e o seu correio eletrónico
3.  Certifique-se a opção *orientado configuração* está marcada
4.  Siga as instruções para obter o ID de aplicação e cole-o para o seu código

### <a name="option-2-register-your-application-advanced-mode"></a>Opção 2: Registar a sua aplicação (modo avançado)

1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação
2. Introduza um nome para a aplicação e o seu correio eletrónico 
3. Certifique-se a opção *orientado configuração* está desmarcada
4.  Clique em `Add Platform`, em seguida, selecione`Web`
5. Adicionar o `Redirect URL` que correspondem ao URL da aplicação com base no seu servidor web. Consulte as secções abaixo para obter instruções sobre como definir / obter o URL de redirecionamento no Visual Studio e Python.
6. Clique em *guardar*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio instruções para obter o URL de redirecionamento
> Siga as instruções para obter o seu URL de redirecionamento:
> 1.    No *Explorador de soluções*, selecione o projeto e observe o `Properties` janela (se não for apresentada uma janela de propriedades, prima `F4`)
> 2.    Copie o valor de `URL` para a área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Voltar ao painel de *Portal de registo de aplicação* e cole o valor como um `Redirect URL` e clique em 'Guardar'

<p/>

> #### <a name="setting-redirect-url-for-python"></a>URL de redirecionamento de definição para Python
> Para o Python, pode definir web porta do servidor através de linha de comandos. Esta configuração orientada utiliza a porta 8080 para referência, mas quanto utilizar qualquer porta disponível. Em qualquer caso, siga as instruções abaixo para configurar um URL de redirecionamento, as informações de registo de aplicação:<br/>
> - Voltar ao painel de *Portal de registo de aplicação* e defina `http://localhost:8080/` como um `Redirect URL`, ou utilize `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (onde *[porta]* é a porta TCP personalizada número) e clique em 'Guardar'


#### <a name="configure-your-javascript-spa"></a>Configurar o JavaScript SPA

1.  Crie um ficheiro denominado `msalconfig.js` que contém as informações de registo de aplicação. Se estiver a utilizar o Visual Studio, selecione o projeto (pasta raiz do projeto), rato e selecione: `Add`  >  `New Item`  >  `JavaScript File`. Nome`msalconfig.js`
2.  Adicione o seguinte código no seu `msalconfig.js` ficheiro:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Substitua <code>Enter_the_Application_Id_here</code> com o Id de aplicação que acabou de ser registado
</li>
</ol>
