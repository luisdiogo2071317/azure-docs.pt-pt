## <a name="prerequisites"></a>Pré-requisitos
Antes de escrever o código de gestão de CDN, é necessário efetuar alguma preparação para ativar o código para interagir com o Azure Resource Manager. Para fazer desta preparação, tem de:

* Criar um grupo de recursos que contém o perfil CDN criado neste tutorial
* Configurar o Azure Active Directory para fornecer autenticação para a aplicação
* Aplicar permissões ao grupo de recursos, de modo a que apenas utilizadores autorizados do inquilino do Azure AD podem interagir com o perfil CDN

### <a name="creating-the-resource-group"></a>Criar o grupo de recursos
1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
2. Clique em **crie um recurso**.
3. Procurar **grupo de recursos** e no painel do grupo de recursos, clique em **criar**.

    ![Criar um novo grupo de recursos](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. O grupo de recursos de nome *CdnConsoleTutorial*.  Selecione a sua subscrição e escolha uma localização perto de si.  Se assim o desejar, pode clicar no **afixar ao dashboard** caixa de verificação para afixar o grupo de recursos para o dashboard no portal.  Afixação torna mais fácil localizar mais tarde.  Depois de efetuadas as suas seleções, clique em **criar**.

    ![O grupo de recursos de atribuição de nomes](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Depois de criar o grupo de recursos, se não tiver afixá-lo ao dashboard, pode encontrá-lo ao clicar em **procurar**, em seguida, **grupos de recursos**.  Para o abrir, clique no grupo de recursos.  Tome nota do seu **ID de subscrição**. Vamos precisar dele mais tarde.

    ![O grupo de recursos de atribuição de nomes](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Criar a aplicação do Azure AD e aplicar permissões
Existem duas abordagens para autenticação da aplicação no Azure Active Directory: utilizadores individuais ou de um principal de serviço. Um principal de serviço é semelhante a uma conta de serviço no Windows.  Em vez de conceder permissões para interagir com os perfis da CDN de um utilizador específico, em vez disso, são concedidas permissões para o principal de serviço.  Principais de serviço são tipicamente usadas para os processos automatizados, não interativo.  Apesar deste tutorial é escrever uma aplicação de consola interativa, iremos irá focar-se na abordagem de principal de serviço.

Criar um principal de serviço é composta por vários passos, incluindo a criação de uma aplicação do Azure Active Directory.  Para criá-la, vamos [siga este tutorial](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Lembre-se de que siga todos os passos a [tutorial ligado](../articles/resource-group-create-service-principal-portal.md).  É *importante* que conclua-lo tal como descrito.  Certifique-se de que tenha em atenção o **ID de inquilino**, **nome de domínio do inquilino** (normalmente uma *. onmicrosoft.com* domínio, exceto se tiver especificado um domínio personalizado), **ID de cliente** , e **chave de autenticação de cliente**, uma vez que temos esta informação mais tarde.  Tenha o cuidado de proteger o **ID de cliente** e **chave de autenticação de cliente**, uma vez que estas credenciais podem ser utilizadas por qualquer pessoa para executar operações como o principal de serviço.
>
> Quando obtiver para o passo com o nome de aplicação de multi-inquilino de configurar, selecione **não**.
>
> Quando obtiver para o passo [atribuir a aplicação a função](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), utilize o grupo de recursos que criou anteriormente, *CdnConsoleTutorial*, mas em vez do **leitor** função, atribuir os  **Contribuinte de perfil de CDN** função.  Depois de atribuir a aplicação a **contribuinte de perfil de CDN** função no seu grupo de recursos, volte neste tutorial. 
>
>

Assim que tiver criado o principal de serviço e atribuídos a **contribuinte de perfil de CDN** função, o **utilizadores** painel para o grupo de recursos deve ter um aspeto semelhante a imagem seguinte.

![Painel de utilizadores](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Autenticação de utilizador interativa
Se, em vez de um principal de serviço, em vez disso, deverá dispor autenticação interativa de utilizador individuais, o processo é semelhante de um principal de serviço.  Na verdade, terá de seguir o mesmo procedimento, mas fazer algumas alterações secundárias.

> [!IMPORTANT]
> Apenas siga estes passos, se escolher utilizar a autenticação de utilizador individuais em vez de um principal de serviço.
>
>

1. Ao criar a sua aplicação, em vez de **aplicação Web**, escolha **aplicação nativa**.

    ![Aplicação nativa](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Na página seguinte, é-lhe pedida uma **URI de redirecionamento**.  O URI não serão validados, mas não se esqueça de que introduziu. Precisar mais tarde.
3. Não é necessário para criar um **chave de autenticação de cliente**.
4. Em vez de atribuir um principal de serviço para o **contribuinte de perfil de CDN** função, vamos atribuir utilizadores individuais ou grupos.  Neste exemplo, pode ver que posso tiver atribuída *CDN demonstração de utilizador* para o **contribuinte de perfil de CDN** função.  

    ![Acesso de utilizador individuais](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
