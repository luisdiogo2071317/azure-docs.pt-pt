## <a name="test-your-code"></a>Testar o seu código

Para testar a aplicação no Visual Studio, prima **F5** para executar o projeto. O browser abre o http://<span></span>localhost: localização {porta} e ver o **iniciar sessão com a Microsoft** botão. Selecione o botão para iniciar o processo de início de sessão.

Quando estiver pronto para executar o teste, utilize uma conta do Microsoft Azure Active Directory (Azure AD) (conta escolar ou profissional) ou uma conta Microsoft pessoal (<span>em direto.</span> com ou <span>outlook.</span> com) para iniciar sessão.

![Inicie sessão com a Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Inicie sessão na sua conta Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Ver os resultados da aplicação
Depois de iniciar sessão, o utilizador é redirecionado para a home page do seu site. A página inicial é o URL HTTPS que é especificado nas suas informações de registo de aplicação no Portal de registo de aplicações do Microsoft. A home page inclui uma mensagem de boas-vindas "Hello \<utilizador >," uma ligação para terminar sessão e uma ligação para ver as afirmações do utilizador. A ligação para afirmações do utilizador navega até à **autorizar** controlador que criou anteriormente.

### <a name="browse-to-see-the-users-claims"></a>Procurar para ver as afirmações do utilizador
Para ver as afirmações do utilizador, selecione a ligação para procurar a vista de controlador que só está disponível para utilizadores autenticados.

#### <a name="view-the-claims-results"></a>Ver os resultados de afirmações
Depois de procura para a vista de controlador, deverá ver uma tabela que contém as propriedades básicas para o utilizador:

|Propriedade |Valor |Descrição |
|---|---|---|
|**Nome** |Nome completo do utilizador | O utilizador do primeiro e último nome.
|**Nome de Utilizador** |utilizador<span>@domain.com</span> | O nome de utilizador que é utilizado para identificar o utilizador.
|**Assunto** |Assunto |Uma cadeia que identifica exclusivamente o utilizador em web.|
|**ID do inquilino** |GUID | A **guid** que exclusivamente representa a organização do utilizador do Azure AD.|

Além disso, deverá ver uma tabela de todas as afirmações que estão no pedido de autenticação. Para obter mais informações, consulte o [lista de afirmações são um Token de ID do Azure AD de](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Teste o acesso a um método que tem um atributo de autorizar (opcional)
Para testar o acesso a **autorizar** controlador para afirmações do utilizador como um utilizador anónimo, siga estes passos:
1. Selecione a ligação para terminar sessão de utilizador e concluir o processo de início de sessão.
2. No seu browser, escreva http://<span></span>localhost: {porta} / autenticado para aceder ao seu controlador de que está protegido com o **autorizar** atributo.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Resultados esperados depois de acesso a um controlador protegido
Lhe for pedida a autenticação para utilizar a vista de controlador protegido.

## <a name="additional-information"></a>Informações adicionais

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Proteger o seu Web site completo
Para proteger o seu Web site todo, do **global. asax** ficheiro, adicione o **AuthorizeAttribute** atributo para o **GlobalFilters** filtrar no **aplicação Abrir** método:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>Restringir o acesso de início de sessão à sua aplicação
Por predefinição, contas pessoais, como o outlook.com, live.com e outros podem iniciar sessão sua aplicação. Contas profissionais ou escolares nas organizações que estão integradas com o Azure AD também podem iniciar sessão, por predefinição.

Para restringir o início de sessão de acesso de utilizadores para a sua aplicação, estão disponíveis várias opções.

#### <a name="restrict-access-to-a-single-organization"></a>Restringir o acesso a uma única organização
Pode restringir o acesso de início de sessão para a sua aplicação para apenas as contas de utilizador que estão numa única organização do Azure AD:
1. No **Web. config** de ficheiros, altere o valor para o **inquilino** parâmetro. Altere o valor de **comuns** para o nome do inquilino da organização, tais como **contoso.onmicrosoft.com**.
2. No seu **OWIN arranque** classe, defina o **ValidateIssuer** argumento **verdadeiro**.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Restringir o acesso a uma lista das organizações
Pode restringir o acesso de início de sessão para contas de utilizador único que estão numa organização do Azure AD que se encontra na lista das organizações permitidas:
1. No **Web. config** ficheiro, na sua **OWIN arranque** classe, defina o **ValidateIssuer** argumento para **verdadeiro**.
2. Defina o valor da **ValidIssuers** parâmetro à lista de permitidos organizações.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Utilize um método personalizado para validar emissores
Pode implementar um método personalizado para validar os emissores utilizando o **IssuerValidator** parâmetro. Para obter mais informações sobre como utilizar este parâmetro, leia sobre o [TokenValidationParameters classe](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) no MSDN.

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
