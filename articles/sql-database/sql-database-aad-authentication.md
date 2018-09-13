---
title: Autenticação do Active Directory do Azure - SQL do Azure (descrição geral) | Documentos da Microsoft
description: Saiba mais sobre como utilizar o Azure Active Directory para a autenticação com base de dados SQL, instância gerida e SQL Data Warehouse
services: sql-database
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.custom: security
ms.topic: conceptual
ms.date: 06/13/2018
ms.author: mireks
ms.reviewer: vanto
ms.openlocfilehash: 460e0dac81ef6c1452f83e53c813a93de849b9b2
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35955896"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-managed-instance-or-sql-data-warehouse"></a>Utilizar autenticação do Active Directory do Azure para a autenticação com base de dados SQL, instância gerida ou SQL Data Warehouse
Autenticação do Azure Active Directory é um mecanismo de ligar ao Azure [base de dados SQL](sql-database-technical-overview.md) e [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) usando identidades no Azure Active Directory (Azure AD). 

> [!NOTE]
> Este tópico aplica-se ao servidor SQL do Azure, bem como às bases de dados da Base de Dados SQL e do SQL Data Warehouse que são criadas no servidor SQL do Azure. Para simplificar, a Base de Dados SQL é utilizada para referenciar a Base de Dados SQL e o SQL Data Warehouse.

Com a autenticação do Azure AD, pode gerenciar centralmente as identidades dos utilizadores de base de dados e outros serviços da Microsoft num local central. Gerenciamento de ID central fornece um único local para gerir utilizadores de base de dados e simplifica a gestão de permissões. Os benefícios incluem o seguinte:

* Ele fornece uma alternativa à autenticação do SQL Server.
* Ajuda a parar a proliferação de identidades de utilizador em servidores de base de dados.
* Permite a rotação da palavra-passe num único local.
* Os clientes podem gerir permissões de base de dados através de grupos externos (Azure AD).
* Isso pode eliminar armazenar palavras-passe através da autenticação integrada do Windows e outras formas de autenticação suportado pelo Azure Active Directory.
* Autenticação do Azure AD utiliza utilizadores de base de dados contida para autenticar identidades ao nível da base de dados.
* O Azure AD suporta a autenticação baseada em tokens para as aplicações ligarem à base de dados SQL.
* Autenticação do Azure AD suporta o ADFS (Federação do domínio) ou autenticação de utilizador/palavra-passe nativo para um Azure Active Directory local sem sincronização de domínio.  
* O Azure AD suporta ligações do SQL Server Management Studio, que utilizam a autenticação do Active Directory Universal, que inclui o multi-factor Authentication (MFA).  MFA inclui uma autenticação segura com uma variedade de opções de verificação simples — telefonema, mensagem de texto, smart cards com pin ou notificação de aplicação móvel. Para obter mais informações, consulte [suporte do SSMS para o Azure AD MFA com base de dados SQL e SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Ligar ao SQL Server em execução numa VM do Azure não é suportada através de uma conta do Azure Active Directory. Utilize um conta do Active Directory do domínio em vez disso.  

Os passos de configuração incluem os seguintes procedimentos para configurar e utilizar a autenticação do Azure Active Directory.

1. Criar e preencher do Azure AD.
2. Opcional: Associar ou altere o active directory que está atualmente associado a sua subscrição do Azure.
3. Criar um administrador do Azure Active Directory para o servidor de base de dados do Azure SQL, a instância gerida, ou o [do Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Configure os computadores cliente.
5. Crie utilizadores de base de dados contidos na base de dados mapeado para identidades do Azure AD.
6. Ligar à base de dados com identidades do Azure AD.

> [!NOTE]
> Para saber como criar e preencher do Azure AD e, em seguida, configurar o Azure AD com a base de dados do Azure SQL, instância gerida e SQL Data Warehouse, veja [configurar o Azure AD com a base de dados do Azure SQL](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Arquitetura de confiança
O diagrama de alto nível seguinte resume a arquitetura da solução de como utilizar a autenticação do Azure AD com a base de dados do Azure SQL. Os mesmos conceitos aplicam-se ao SQL Data Warehouse. Para suportar a palavra-passe de utilizador nativa do Azure AD, é considerado apenas a parte da nuvem e a base de dados do Azure AD/Azure SQL. Para suportar a autenticação de federados (ou utilizador/palavra-passe para as credenciais do Windows), a comunicação com o bloco ADFS é necessária. As setas indicam os caminhos de comunicação.

![Diagrama de autenticação do aad][1]

O diagrama a seguir indica o Federação, confiança e relações de alojamento que permitem que um cliente ligar a uma base de dados ao enviar um token. O token é autenticado por um Azure AD e é considerada fidedigna pela base de dados. Cliente 1 pode representar um Azure Active Directory com utilizadores nativos ou um Azure AD com utilizadores federados. Cliente 2 representa uma solução possível, incluindo os utilizadores importados. Neste exemplo vem de um federados do Active Directory do Azure com o ADFS a ser sincronizada com o Azure Active Directory. É importante compreender que o acesso a uma base de dados utilizando autenticação do Azure AD requer que a subscrição de alojamento está associada ao Azure AD. Da mesma subscrição deve ser usada para criar o SQL Server que aloja a base de dados do Azure SQL ou SQL Data Warehouse.

![relação de subscrição][2]

## <a name="administrator-structure"></a>Estrutura de administrador
Ao utilizar a autenticação do Azure AD, existem duas contas de administrador para o servidor de base de dados SQL e a instância gerida; o administrador do SQL Server original e o administrador do Azure AD. Os mesmos conceitos aplicam-se ao SQL Data Warehouse. Apenas o administrador com base numa conta do Azure AD pode criar o primeiro utilizador de base de dados do Azure AD contido num banco de dados do utilizador. O início de sessão de administrador do Azure AD pode ser um utilizador ou um grupo do Azure AD. Quando o administrador é uma conta de grupo, ele pode ser usado por qualquer membro do grupo, permitindo que vários administradores do Azure AD para a instância do SQL Server. Com a conta de grupo como um administrador melhora a capacidade de gerenciamento, permitindo-lhe adicionar e remover membros do grupo no Azure AD sem alterar os utilizadores ou permissões na base de dados SQL de forma centralizada. Apenas um administrador do Azure AD (um utilizador ou grupo) pode ser configurado em qualquer altura.

![estrutura de administrador][3]

## <a name="permissions"></a>Permissões
Para criar novos utilizadores, tem de ter o `ALTER ANY USER` permissão na base de dados. O `ALTER ANY USER` permissão pode ser concedida a qualquer utilizador de base de dados. O `ALTER ANY USER` permissão também é mantida pelas contas de administrador do servidor e os utilizadores de base de dados com o `CONTROL ON DATABASE` ou `ALTER ON DATABASE` permissões para essa base de dados e por membros do `db_owner` função de base de dados.

Para criar um utilizador de base de dados contidos na base de dados do Azure SQL, instância gerida ou SQL Data Warehouse, tem de ligar a base de dados ou uma instância com uma identidade do Azure AD. Para criar o primeiro utilizador de base de dados contida, tem de ligar à base de dados com o administrador do Azure AD (que é o proprietário da base de dados). Isso é demonstrado na [configurar e gerir a autenticação do Azure Active Directory com a base de dados SQL ou SQL Data Warehouse](sql-database-aad-authentication-configure.md). Qualquer autenticação do Azure AD só é possível se o administrador do Azure AD foi criado para o servidor de base de dados do Azure SQL ou SQL Data Warehouse. Se o administrador do Azure Active Directory foi removido do servidor, os utilizadores do Azure Active Directory existentes criados anteriormente no SQL Server já não podem ligar à base de dados com as respetivas credenciais do Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>As funcionalidades do Azure AD e limitações
Os seguintes membros do Azure AD podem ser aprovisionados no servidor SQL do Azure ou SQL Data Warehouse:

- Membros nativos: membro criado no Azure AD no domínio gerido ou num domínio dos clientes. Para obter mais informações, consulte [adicionar seu próprio nome de domínio para o Azure AD](../active-directory/active-directory-domains-add-azure-portal.md).
- Federado membros do domínio: membro criado no Azure AD com um domínio federado. Para obter mais informações, consulte [Microsoft Azure suporta agora a Federação com o Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
- Importados os membros do outro Azure AD que sejam membros do domínio federado ou nativo.
- Grupos do Active Directory criados como grupos de segurança.

Limitações do AD Azure relacionadas com a instância gerida:
- Só o administrador do Azure AD pode criar bases de dados, utilizadores do Azure AD estão confinados a uma DB única e não têm esta permissão
- Propriedade de base de dados:
  - Principal do Azure AD não é possível alterar a propriedade da base de dados (ALTER autorização no banco de dados) e não pode ser definido como proprietário.
  - Para bases de dados criadas pelo administrador do Azure AD não está definida nenhuma propriedade (normalmente, owner_sid campo no sys.sysdatabases é 0x1).
- Agente do SQL não podem ser gerido quando iniciou sessão com as entidades de segurança do Azure AD. 
- Administrador do Azure AD não pode ser representado com EXECUTE AS
- Ligação DAC não é suportada com entidades do Azure AD. 

Estas funções de sistema devolvem valores nulos quando executada em entidades de segurança do Azure AD:
- `SUSER_ID()`
- `SUSER_NAME(<admin ID>)`
- `SUSER_SNAME(<admin SID>)`
- `SUSER_ID(<admin name>)`
- `SUSER_SID(<admin name>)`

## <a name="connecting-using-azure-ad-identities"></a>A ligar através de identidades do Azure AD

Autenticação do Azure Active Directory suporta os seguintes métodos de conexão a uma base de dados com identidades do Azure AD:

* Utilizando a autenticação integrada do Windows
* Utilizar um nome principal do Azure AD e uma palavra-passe
* Utilizar a autenticação de token de aplicação

### <a name="additional-considerations"></a>Considerações adicionais

* Para melhorar a capacidade de gestão, recomendamos que aprovisiona um dedicada do Azure AD grupo como um administrador.   
* Apenas um administrador do Azure AD (um utilizador ou grupo) pode ser configurado para um servidor de base de dados do Azure SQL, a instância gerida ou o Azure SQL Data Warehouse em qualquer altura.   
* Inicialmente, apenas um administrador do Azure AD para o SQL Server pode ligar para o servidor de base de dados do Azure SQL, a instância gerida ou o Azure SQL Data Warehouse através de uma conta do Azure Active Directory. Administrador do Active Directory, pode configurar subsequente do Azure AD os utilizadores de base de dados.   
* Recomendamos a definição de tempo limite da conexão para 30 segundos.   
* SQL Server 2016 Management Studio e SQL Server Data Tools para Visual Studio 2015 (versão 14.0.60311.1April 2016 ou posterior) suportam a autenticação do Azure Active Directory. (Autenticação do azure AD é suportada pelos **.NET Framework Data Provider Pro SqlServer**; pelo menos, versão .NET Framework 4.6). Por isso as versões mais recentes destas ferramentas e aplicações de camada de dados (DAC e. BACPAC) pode utilizar a autenticação do Azure AD.   
* [ODBC versão 13.1](https://www.microsoft.com/download/details.aspx?id=53339) suporta a autenticação do Azure Active Directory no entanto `bcp.exe` não é possível ligar utilizando a autenticação do Azure Active Directory, porque utiliza um fornecedor ODBC mais antigo.   
* `sqlcmd` suporta o início de autenticação do Azure Active Directory com a versão 13.1 disponível a partir da [Centro de transferências](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools para Visual Studio 2015 requer, pelo menos, a versão de Abril de 2016 das ferramentas de dados (versão 14.0.60311.1). Atualmente, os utilizadores do Azure AD não são apresentados no SSDT Object Explorer. Como solução, ver os usuários [database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [Microsoft JDBC Driver 6.0 para o SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) autenticação de suporte do Azure AD. Além disso, veja [definindo as propriedades de ligação](https://msdn.microsoft.com/library/ms378988.aspx).   
* O PolyBase não pode autenticar utilizando a autenticação do Azure AD.   
* Autenticação do Azure AD é suportada para a base de dados SQL através do portal do Azure **importar base de dados** e **exportar a base de dados** painéis. A importação e exportação através da autenticação do Azure AD também é suportado a partir do comando do PowerShell.   
* Autenticação do Azure AD é suportada para a base de dados SQL, instância gerida e SQL Data Warehouse através da utilização da CLI. Para obter mais informações, consulte [configurar e gerir a autenticação do Azure Active Directory com a base de dados SQL ou SQL Data Warehouse](sql-database-aad-authentication-configure.md) e [SQL Server - az o sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Passos Seguintes
- Para saber como criar e preencher do Azure AD e, em seguida, configurar o Azure AD com a base de dados do Azure SQL ou armazém de dados SQL do Azure, veja [configurar e gerir a autenticação do Azure Active Directory com a base de dados SQL, instância gerida ou SQL Data Warehouse ](sql-database-aad-authentication-configure.md).
- Para obter uma descrição geral do acesso e controlo na Base de Dados SQL, veja [Acesso e controlo da Base de Dados SQL](sql-database-control-access.md).
- Para obter uma descrição geral de inícios de sessão, utilizadores e funções de base de dados da Base de Dados SQL, veja [Inícios de sessão, utilizadores e funções de base de dados](sql-database-manage-logins.md).
- Para obter mais informações sobre os principais de bases de dados, veja [Principals (Principais)](https://msdn.microsoft.com/library/ms181127.aspx).
- Para obter mais informações sobre as funções de base de dados, veja [Database roles (Funções de base de dados)](https://msdn.microsoft.com/library/ms189121.aspx).
- Para obter mais informações sobre as regras de firewall na Base de Dados SQL, veja [Regras de firewall da Base de Dados SQL](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

