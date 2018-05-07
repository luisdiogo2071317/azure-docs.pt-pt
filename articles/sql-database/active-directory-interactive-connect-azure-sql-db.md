---
title: ActiveDirectoryInteractive estabelece ligação ao SQL Server | Microsoft Docs
description: Exemplo de código c#, com uma explicação, para ligar à SQL Database do Azure utilizando o modo de SqlAuthenticationMethod.ActiveDirectoryInteractive.
services: sql-database
author: GithubMirek
manager: jhubbard
ms.service: sql-database
ms.custom: active directory
ms.topic: article
ms.date: 04/06/2018
ms.author: MirekS
ms.reviewer: GeneMi
ms.openlocfilehash: 163b55e0f1727cc2ba9e3ee84eb58fe29f7a005d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>Utilize o modo de ActiveDirectoryInteractive para ligar à SQL Database do Azure

Este artigo fornece um passíveis de execução c# exemplo de código que liga à sua base de dados de SQL do Microsoft Azure. O programa c# utiliza o modo interativo de autenticação, que suporta a autenticação multifator (MFA) do Azure AD. Por exemplo, uma tentativa de ligação pode incluir um código de verificação a ser enviado para o seu telemóvel.

Para obter mais informações sobre o suporte MFA para as ferramentas do SQL Server, consulte [suporte do Azure Active Directory no SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. Valor de enumeração ActiveDirectoryInteractive

A partir do .NET Framework versão 4.7.2, a enumeração [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) tem um valor novo **. ActiveDirectoryInteractive**. Quando utilizado por um programa c# cliente, este valor de enumeração direciona o sistema para utilizar o modo interativo do Azure AD que suportam MFA para autenticar a SQL Database do Azure. O utilizador que executa o programa, em seguida, verá as caixas de diálogo seguintes:

1. Uma caixa de diálogo que mostra um nome de utilizador do Azure AD e que pede a palavra-passe do utilizador do Azure AD.
    - Esta caixa de diálogo não é apresentada não se é necessária nenhuma palavra-passe. Não é necessária nenhuma palavra-passe, se o domínio do utilizador está Federado com o Azure AD.

    Se o MFA é imposta no utilizador pela política definida no Azure AD, as caixas de diálogo seguintes são apresentadas junto.

2. Apenas muito pela primeira vez que o utilizador ocorrer no cenário de MFA, o sistema apresenta uma caixa de diálogo adicional. A caixa de diálogo pede-lhe um número de telemóvel para que serão enviadas mensagens de texto. Cada mensagem fornece o *código de verificação* que o utilizador tem de introduzir na caixa de diálogo seguinte.

3. Outra caixa de diálogo que pede-lhe o código de verificação de MFA, o qual o sistema foi enviado para um telemóvel.

Para obter informações sobre como configurar o Azure AD para exigir a MFA, consulte [introdução ao Azure multi-factor Authentication na nuvem](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Para capturas de ecrã destas caixas de diálogo, consulte [configurar a autenticação multifator para SQL Server Management Studio e o Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> A nossa página de pesquisa gerais para todos os tipos de APIs do .NET Framework está disponível na seguinte hiperligação para o nosso à mão **.NET API Browser** ferramenta:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Ao adicionar o nome do tipo anexado opcional para **? termo =** parâmetro, a página de pesquisa pode ter a nossa resultado pronto e aguardar para-na:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Preparativos para c#, utilizando o portal do Azure

Presumimos que já tem um [servidor da SQL Database do Azure criada](sql-database-get-started-portal.md) e disponível.


### <a name="a-create-an-app-registration"></a>A. Criar um registo de aplicação

Para utilizar a autenticação do Azure AD, o seu programa cliente c# tem de indicar um GUID como um *clientId* quando tenta ligar-se o seu programa. A concluir um registo de aplicação gera e apresenta o GUID no portal do Azure, identificadas como **ID da aplicação**. Os passos de navegação são os seguintes:

1. Portal do Azure &gt; **do Azure Active Directory** &gt; **registo de aplicação**

    ![Registo de aplicações](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. O **ID da aplicação** valor é gerado e apresentada.

    ![ID da aplicação apresentado](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Aplicação registada** &gt; **definições** &gt; **as permissões necessárias** &gt; **adicionar**

    ![Definições de permissões para a aplicação registada](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **As permissões necessárias** &gt; **acesso à API adicionar** &gt; **selecionar um API** &gt; **SQL Database do Azure**

    ![Adicione o acesso a API para a SQL Database do Azure](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **Acesso à API** &gt; **selecionar permissões** &gt; **permissões delegadas**

    ![Delegar permissões para API para a SQL Database do Azure](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. Definir o administrador do Azure AD no seu servidor de base de dados SQL

Cada servidor da SQL Database do Azure tem o seu próprio servidor lógico da SQL do Azure AD. Para o nosso cenário c#, tem de definir um administrador do Azure AD para o servidor de SQL do Azure.

1. **SQL Server** &gt; **administrador do Active Directory** &gt; **definir administrador**

    - Para mais informações sobre utilizadores e administradores do Azure AD para a SQL Database do Azure, consulte as capturas de ecrã em [configurar e gerir a autenticação do Azure Active Directory com a SQL Database](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), na respetiva secção **aprovisionar do Azure Administrador do Active Directory para o servidor de base de dados do Azure SQL**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Preparar um utilizador do Azure AD para ligar a uma base de dados específico

No Azure AD que é específico para o servidor da SQL Database do Azure, pode adicionar um utilizador que deverá ter acesso a uma base de dados específica.

Para obter mais informações, consulte [utilize Active Directory Authentication do Azure para a autenticação com base de dados do SQL Server, instância geridos ou SQL Data Warehouse](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Adicionar um utilizador não admin para o Azure AD

O administrador do Azure AD do servidor de base de dados SQL pode ser utilizado para ligar ao servidor da SQL Database. No entanto, num caso mais geral é adicionar um utilizador não admin para o Azure AD. Quando o utilizador não admin é utilizado para estabelecer a ligação, a sequência MFA é invocada se o MFA é imposta este utilizador pelo Azure AD.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory Authentication Library (ADAL)

O programa c# baseia-se no espaço de nomes **Microsoft.IdentityModel.Clients.ActiveDirectory**. As classes para este espaço de nomes estão na assemblagem com o mesmo nome.

- Utilize o NuGet para transferir e instalar a assemblagem ADAL.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- Adicione uma referência à assemblagem, para suportar uma compilação do programa c#.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod enum

É um espaços de nomes que depende de exemplo c# **SqlClient**. Do especial interesse é a enumeração **SqlAuthenticationMethod**. Esta enumeração tem os seguintes valores:

- **SqlAuthenticationMethod.ActiveDirectory*interativo ***:&nbsp; utilizá-lo com um nome de utilizador do Azure AD, para alcançar a autenticação multifator MFA.
    - Este valor é o foco do artigo presente. Produz uma experiência interativa, apresentando caixas de diálogo para a palavra-passe de utilizador e, em seguida, para validação de MFA se o MFA é imposta este utilizador.
    - Este valor está disponível a partir com o .NET Framework versão 4.7.2.

- **SqlAuthenticationMethod.ActiveDirectory*integrado ***:&nbsp; utilizá-lo para um *federado* conta. Para uma conta federada, o nome de utilizador é conhecido ao domínio Windows. Este método não suporta a MFA.

- **SqlAuthenticationMethod.ActiveDirectory*palavra-passe ***:&nbsp; utilizá-lo para a autenticação necessita de um utilizador do Azure AD e a palavra-passe do utilizador. Base de dados SQL do Azure efetua a autenticação. Este método não suporta a MFA.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>Preparar c# os valores dos parâmetros do portal do Azure

Para uma execução bem sucedida do programa c#, tem de atribuir os valores adequados para os seguintes campos estáticos. Estes campos estáticos atuam como parâmetros para o programa. Os campos são mostrados aqui com valores pretend. Também é apresentado se as localizações no portal do Azure a partir de onde pode obter os valores adequados:


| Nome do campo estático | Valor pretend | Onde no portal do Azure |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "os meus-favorito-sqldb-svr.database.windows.net" | **Servidores SQL** &gt; **filtrar por nome** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **utilizador** &gt; **novo utilizador convidado** |
| Initial_DatabaseName | "original" | **Servidores SQL** &gt; **bases de dados SQL** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** &gt; **registos de aplicação**<br /> &nbsp; &nbsp; &gt; **Pesquisar por nome** &gt; **ID da aplicação** |
| redirectUri | novo Uri ("https://bing.com/") | **Azure Active Directory** &gt; **registos de aplicação**<br /> &nbsp; &nbsp; &gt; **Pesquisar por nome** &gt; *[Your-App-regis]* &gt;<br /> &nbsp; &nbsp; **Definições** &gt; **RedirectURIs**<br /><br />Para este artigo, é adequado para RedirectUri qualquer valor válido. O valor não está realmente utilizado no nosso caso. |
| &nbsp; | &nbsp; | &nbsp; |


Dependendo do cenário em particular, não poderá ser necessário valores todos os parâmetros na tabela anterior.




## <a name="run-ssms-to-verify"></a>Execute o SSMS para verificar

É útil executar o SQL Server Management Studio (SSMS) antes de executar o programa c#. O SSMS executar verifica que diversas configurações estão corretas. Em seguida, qualquer falha do programa c# pode ser narrows para apenas o código de origem.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>Certifique-se de endereços IP de firewall de base de dados SQL

Execute o SSMS no mesmo computador, no mesmo edifício, que irá executar o programa c# mais tarde. Pode utilizar independentemente **autenticação** modo sentir é a mais fácil. Se não houver qualquer indicação de que a firewall do servidor de base de dados não está a aceitar o endereço IP, pode corrigir conforme mostrado no [regras de firewall ao nível do servidor e o nível de base de dados de SQL Database do Azure](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Certifique-se a autenticação multifator (MFA) para o Azure AD

Executar novamente o SSMS, desta vez com **autenticação** definido como **do Active Directory - Universal com suporte MFA**. Para esta opção tem de ter o SSMS versão 17.5 ou posterior.

Para obter mais informações, consulte [configurar a autenticação multifator para SSMS e o Azure AD](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>Exemplo de código c#

Para compilar neste exemplo c#, tem de adicionar uma referência à assemblagem DLL com o nome **Microsoft.IdentityModel.Clients.ActiveDirectory**.


#### <a name="reference-documentation"></a>Documentação de referência

- **SqlClient** espaço de nomes:
    - Pesquisa:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Direta:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.ActiveDirectory** espaço de nomes:
    - Pesquisa:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Direta:&nbsp; [Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


#### <a name="c-source-code-in-two-parts"></a>Origem código c#, em duas partes:

&nbsp;

```csharp

using System;    // C# ,  part 1 of 2.

// Add a reference to assembly:  Microsoft.IdentityModel.Clients.ActiveDirectory.DLL
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<YOUR VALUE HERE>";
        static public string AzureAD_UserID = "<YOUR VALUE HERE>";
        static public string Initial_DatabaseName = "master";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<YOUR VALUE HERE>";
        static public readonly Uri RedirectUri = new Uri("<YOUR VALUE HERE>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program .

```

&nbsp;

#### <a name="second-half-of-the-c-program"></a>Segunda metade do programa c#

Para melhor apresentação visual, o programa c# está dividido em duas blocos de código. Para executar o programa, cole os blocos de dois código em conjunto.

&nbsp;

```csharp

    // C# ,  part 2 of 2 ,  to concatenate below part 1.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2 .
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider .
} // EONamespace.  End of entire program source code.

```

&nbsp;

#### <a name="actual-test-output-from-c"></a>Resultado do teste real do c#

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

&nbsp;


## <a name="next-steps"></a>Passos Seguintes

- [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator)

