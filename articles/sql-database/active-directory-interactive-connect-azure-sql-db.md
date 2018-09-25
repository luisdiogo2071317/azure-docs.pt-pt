---
title: ActiveDirectoryInteractive se conecta ao SQL | Documentos da Microsoft
description: Exemplo de código c#, com explicações, para ligar à base de dados do Azure SQL com o modo de SqlAuthenticationMethod.ActiveDirectoryInteractive.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi
ms.date: 04/06/2018
manager: craigg
ms.openlocfilehash: 80944e73f21d75943d4fa71c7ac9500e47bab250
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055531"
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>Utilizar o modo de ActiveDirectoryInteractive para ligar à base de dados do Azure SQL

Este artigo fornece um executável c# exemplo de código que se liga a sua base de dados de SQL do Microsoft Azure. O programa c# usa o modo interativo da autenticação, que suporte do Azure AD a autenticação multifator (MFA). Por exemplo, uma tentativa de ligação pode incluir um código de verificação a ser enviado para o telemóvel.

Para obter mais informações sobre o suporte MFA para ferramentas SQL, consulte [suporte do Azure Active Directory no SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. Valor de enumeração de ActiveDirectoryInteractive

A partir do .NET Framework versão 4.7.2, a enumeração [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) tem um novo valor **. ActiveDirectoryInteractive**. Quando utilizado por um programa c# cliente, este valor de enum direciona o sistema para utilizar o modo interativo do Azure AD MFA de suporte para autenticar no Azure SQL Database. O utilizador que executa o programa, em seguida, verá as caixas de diálogo seguintes:

1. Uma caixa de diálogo que apresenta um nome de utilizador do Azure AD e que pede-lhe a palavra-passe do utilizador do Azure AD.
    - Esta caixa de diálogo não é apresentada se nenhuma palavra-passe é necessária. Nenhuma palavra-passe é necessária se o domínio do utilizador está Federado com o Azure AD.

    Se a MFA é imposta no utilizador, a política definida no Azure AD, as caixas de diálogo seguintes são apresentadas a seguir.

2. Apenas na primeira vez que o utilizador ocorrer no cenário de MFA, o sistema apresenta uma caixa de diálogo adicional. A caixa de diálogo pede-lhe um número de telefone celular para o qual serão enviadas mensagens de texto. Cada mensagem fornece a *código de verificação* que o usuário deve digitar na caixa de diálogo seguinte.

3. Outra caixa de diálogo que solicita o código de verificação de MFA, o que o sistema tiver enviada para um telefone celular.

Para obter informações sobre como configurar o Azure AD para exigir a MFA, consulte [introdução ao multi-factor Authentication na cloud](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Para capturas de ecrã dessas caixas de diálogo, consulte [configurar a autenticação multifator para o SQL Server Management Studio e o Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Nossa página de pesquisa geral de todos os tipos de APIs do .NET Framework está disponível na seguinte ligação para a nossa útil **Browser de API do .NET** ferramenta:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Ao adicionar o nome do tipo para opcional acrescentado **? termo =** parâmetro, a página de pesquisa pode ter resultado da nossa pronto e aguardando para nós:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Preparativos para c#, utilizando o portal do Azure

Partimos do princípio que já tem um [servidor de base de dados do Azure SQL criada](sql-database-get-started-portal.md) e está disponível.


### <a name="a-create-an-app-registration"></a>R. Criar um registo de aplicações

Para utilizar autenticação do Azure AD, seu programa de cliente do c# tem de fornecer um GUID como um *clientId* quando seu programa tenta se conectar. Concluir o registo de uma aplicação gera e apresenta o GUID no portal do Azure, como o nome **ID da aplicação**. Os passos de navegação são os seguintes:

1. Portal do Azure &gt; **do Azure Active Directory** &gt; **registo de aplicações**

    ![Registo de aplicações](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. O **ID da aplicação** valor é gerado e apresentado.

    ![ID da aplicação apresentado](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Aplicação registada** &gt; **definições** &gt; **permissões obrigatórias** &gt; **adicionar**

    ![Definições de permissões para a aplicação registada](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Permissões obrigatórias** &gt; **adicionar acesso à API** &gt; **selecionar uma API** &gt; **SQL Database do Azure**

    ![Adicionar acesso à API para a base de dados do Azure SQL](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **Acesso à API** &gt; **selecionar permissões** &gt; **permissões delegadas**

    ![Permissões de delegado à API para a base de dados do Azure SQL](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. Definir o administrador do Azure AD no seu servidor de base de dados SQL

Cada servidor de base de dados do Azure SQL tem seu próprio servidor lógico da SQL do Azure AD. Para o nosso c# cenário, tem de definir o administrador do Azure AD para o seu servidor SQL do Azure.

1. **SQL Server** &gt; **administrador do Active Directory** &gt; **definir administrador**

    - Para obter mais informações sobre os administradores do Azure AD e os utilizadores para a base de dados do Azure SQL, consulte as capturas de ecrã [configurar e gerir a autenticação do Azure Active Directory com a base de dados SQL](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), na respetiva secção **aprovisionar do Azure Administrador do Active Directory para o seu servidor de base de dados do Azure SQL**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Preparar um utilizador para ligar a uma base de dados específico

No Azure AD que é específico do seu servidor de base de dados do Azure SQL, pode adicionar um utilizador que deve ter acesso a uma base de dados específico.

Para obter mais informações, consulte [utilizar do Azure Active Directory a autenticação para a autenticação com base de dados SQL, instância gerida ou SQL Data Warehouse](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Adicionar um utilizador de não-administrador com o Azure AD

O administrador do Azure AD do servidor de base de dados SQL pode ser utilizado para ligar ao seu servidor de base de dados SQL. No entanto, um caso mais geral é adicionar um utilizador de não-administrador com o Azure AD. Quando o utilizador não administrador é utilizado para ligar, a sequência MFA é invocada se a MFA é imposta este utilizador pelo Azure AD.




## <a name="azure-active-directory-authentication-library-adal"></a>O Azure Active Directory Authentication Library (ADAL)

O programa c# baseia-se no espaço de nomes **ActiveDirectory**. As classes para este espaço de nomes estão no assembly com o mesmo nome.

- Utilize NuGet para transferir e instalar o assembly da ADAL.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- Adicione uma referência ao assembly, para oferecer suporte a uma compilação do programa em C#.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod enum

É um espaços de nomes que depende de exemplo de c# **SqlClient**. De especial interesse é o enum **SqlAuthenticationMethod**. Essa enumeração possui os seguintes valores:

- **SqlAuthenticationMethod.ActiveDirectory * Interactive ***:&nbsp; utilizá-lo com um nome de utilizador do Azure AD para alcançar a autenticação multifator MFA.
    - Este valor é o foco deste artigo presente. Produz uma experiência interativa ao apresentar caixas de diálogo para a palavra-passe de utilizador e, em seguida, para a validação de MFA, se a MFA é imposta este utilizador.
    - Este valor está disponível a partir do .NET Framework versão 4.7.2.

- **SqlAuthenticationMethod.ActiveDirectory * integrado ***:&nbsp; utilizá-lo para um *federado* conta. Para uma conta federada, o nome de utilizador é conhecido para o domínio do Windows. Este método não suporta a MFA.

- **SqlAuthenticationMethod.ActiveDirectory * palavra-passe ***:&nbsp; utilizá-lo para a autenticação que requer um utilizador e palavra-passe do utilizador. Base de dados SQL do Azure realiza a autenticação. Este método não suporta a MFA.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>Preparar o c# valores de parâmetro do portal do Azure

Para uma execução com êxito do programa em C#, tem de atribuir os valores adequados para os seguintes campos estáticos. Estes campos estáticos atuam como parâmetros para o programa. Os campos são mostrados aqui com valores pretend. Mostra também são os locais no portal do Azure a partir de onde pode obter os valores adequados:


| Nome do campo estático | Fingir valor | Onde no portal do Azure |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "meu-favorito-sqldb-svr.database.windows.net" | **Servidores SQL** &gt; **filtrar por nome** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **O Azure Active Directory** &gt; **utilizador** &gt; **novo utilizador convidado** |
| Initial_DatabaseName | "principal" | **Servidores SQL** &gt; **bases de dados SQL** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **O Azure Active Directory** &gt; **registos das aplicações**<br /> &nbsp; &nbsp; &gt; **Procurar por nome** &gt; **ID da aplicação** |
| RedirectUri | novo Uri ("https://bing.com/") | **O Azure Active Directory** &gt; **registos das aplicações**<br /> &nbsp; &nbsp; &gt; **Procurar por nome** &gt; *[Your-App-regis]* &gt;<br /> &nbsp; &nbsp; **As definições** &gt; **RedirectURIs**<br /><br />Neste artigo, qualquer valor válido é adequado para RedirectUri. Na verdade, não é utilizado o valor em nosso caso. |
| &nbsp; | &nbsp; | &nbsp; |


Dependendo do seu cenário específico, poderá não ter valores de todos os parâmetros na tabela anterior.




## <a name="run-ssms-to-verify"></a>Execute o SSMS para verificar

É útil executar o SQL Server Management Studio (SSMS) antes de executar o programa em c#. O SSMS executar verifica se a várias configurações estão corretas. Em seguida, qualquer falha do programa em C# pode ser estreita para apenas seu código-fonte.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>Certifique-se de endereços IP de firewall de base de dados SQL

Execute o SSMS no mesmo computador, no mesmo prédio, que irá executar o programa c# mais tarde. Pode utilizar o que ocorrer **autenticação** modo achar que é o mais fácil. Se há qualquer indicação de que a firewall do servidor de base de dados não está a aceitar seu endereço IP, pode corrigir isso conforme mostrado na [regras de firewall ao nível do servidor e ao nível da base de dados de base de dados do Azure SQL](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Certifique-se a autenticação multifator (MFA) para o Azure AD

Executar novamente o SSMS, desta vez com **autenticação** definida como **Active Directory - Universal com o suporte MFA**. Para esta opção tem de ter o SSMS versão 17,5 ou posterior.

Para obter mais informações, consulte [configurar a autenticação multifator para o SSMS e o Azure AD](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>Exemplo de código do c#

Para compilar este exemplo do c#, tem de adicionar uma referência ao assembly DLL denominado **ActiveDirectory**.


#### <a name="reference-documentation"></a>Documentação de referência

- **SqlClient** espaço de nomes:
    - Pesquisa:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Direct:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **ActiveDirectory** espaço de nomes:
    - Pesquisa:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Direct:&nbsp; [ActiveDirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


#### <a name="c-source-code-in-two-parts"></a>C# código-fonte, em duas partes

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

#### <a name="second-half-of-the-c-program"></a>Segunda metade do programa em C#

Para melhor visualização do elemento visual, o programa em c# é dividido em dois blocos de código. Para executar o programa, cole os blocos de código de dois em conjunto.

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

#### <a name="actual-test-output-from-c"></a>Resultado do teste real da linguagem c#

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

