---
title: Protegendo bancos de dados de PaaS no Azure | Documentos da Microsoft
description: " Saiba mais sobre segurança de base de dados do Azure SQL e SQL Data Warehouse melhores práticas para proteger as suas aplicações móveis e web de PaaS. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 00b2b249f5889888f34d57fd1577ccfea776d00c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347975"
---
# <a name="securing-paas-databases-in-azure"></a>Protegendo bancos de dados de PaaS no Azure

Neste artigo, discutimos a uma coleção de [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) e [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) melhores práticas de segurança para proteger as suas aplicações móveis e web de PaaS. Essas práticas recomendadas são derivadas da nossa experiência com o Azure e as experiências dos clientes, como.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Base de dados SQL do Azure e SQL Data Warehouse
[Base de dados SQL do Azure](../sql-database/sql-database-technical-overview.md) e [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) fornecer um serviço de base de dados relacional para seus aplicativos baseados na Internet. Vamos examinar os serviços que ajudam a proteger os seus dados e aplicações ao utilizar a base de dados do Azure SQL e SQL Data Warehouse numa implementação de PaaS:

- Autenticação do Active Directory do Azure (em vez da autenticação do SQL Server)
- Firewall do SQL do Azure
- Encriptação de dados transparente (TDE)

## <a name="best-practices"></a>Melhores práticas

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Utilizar um repositório centralizado de identidades para autenticação e autorização

Bases de dados SQL do Azure podem ser configurados para utilizar um dos dois tipos de autenticação:

- **Autenticação do SQL** utiliza um nome de utilizador e palavra-passe. Quando criou o servidor lógico para a sua base de dados, especificou um início de sessão "administrador do servidor" com um nome de utilizador e palavra-passe. Com estas credenciais, pode autenticar qualquer base de dados nesse servidor como o proprietário da base de dados.

- **Autenticação do Active Directory Azure** utiliza identidades geridas pelo Azure Active Directory e é suportada para domínios geridos e integrados. Para utilizar autenticação do Active Directory do Azure, tem de criar outro administrador de servidor chamado o "administrador do Azure AD," o que tem permissão para administrar grupos e utilizadores do Azure AD. Este administrador também pode fazer todas as operações que um administrador de servidor normal faz.

[Autenticação do Azure Active Directory](../active-directory/develop/authentication-scenarios.md) é um mecanismo de ligação a base de dados do Azure SQL e SQL Data Warehouse, utilizando as identidades no Azure Active Directory (AD). O Azure AD fornece uma alternativa à autenticação do SQL Server para que pode parar a proliferação de identidades de utilizador em servidores de base de dados. Autenticação do Azure AD permite-lhe gerir centralmente as identidades dos utilizadores de base de dados e outros serviços da Microsoft num local central. Gerenciamento de ID central fornece um único local para gerir utilizadores de base de dados e simplifica a gestão de permissões.  

Vantagens de utilizar a autenticação do Azure AD em vez da autenticação do SQL incluem:

- Permite a rotação da palavra-passe num único local.
- Gere as permissões de base de dados a utilizar o Azure externo grupos do AD.
- Elimina a armazenar palavras-passe através da autenticação integrada do Windows e outras formas de autenticação suportado pelo Azure AD.
- Utilizadores de base de dados utiliza contido para autenticar identidades ao nível da base de dados.
- Suporta a autenticação baseada em tokens para as aplicações ligarem à base de dados SQL.
- Suporta o ADFS (Federação do domínio) ou autenticação de utilizador/palavra-passe nativo para um local do Azure AD sem sincronização de domínio.
- Oferece suporte a conexões do SQL Server Management Studio, que utilizam a autenticação do Active Directory Universal, que inclui [multi-factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md). MFA inclui uma autenticação segura com uma variedade de opções de verificação simples — telefonema, mensagem de texto, smart cards com pin ou notificação de aplicação móvel. Para obter mais informações, consulte [suporte do SSMS para o Azure AD MFA com base de dados SQL e SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Para saber mais sobre a autenticação do Azure AD, veja:

- [Ligar à base de dados SQL ou SQL Data Warehouse, utilizando a autenticação do Azure Active Directory](../sql-database/sql-database-aad-authentication.md)
- [Authentication to Azure SQL Data Warehouse (Autenticação no Azure SQL Data Warehouse)](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Suporte de autenticação baseada em tokens para o Azure SQL DB utilizando a autenticação do Azure AD](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Para garantir que o Azure Active Directory é uma boa opção para o seu ambiente, veja [funcionalidades do Azure AD e limitações](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), especificamente as considerações adicionais.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Restringir o acesso com base no endereço de IP
Pode criar regras de firewall que especificam intervalos de endereços IP aceitáveis. Estas regras podem ser visadas nos níveis do servidor e a base de dados. Recomendamos que utilize regras de firewall ao nível da base de dados sempre que possível para melhorar a segurança e tornar a sua base de dados mais portátil. Regras de firewall ao nível do servidor são melhor utilizadas para os administradores e quando tiver muitas bases de dados que têm os mesmos requisitos de acesso, mas não quer perder tempo a configurar individualmente a cada base de dados.

Restrições de endereço IP de origem predefinido da base de dados de SQL permitem o acesso de qualquer endereço do Azure (incluindo outras subscrições e inquilinos). Pode restringir a esta opção para permitir apenas os endereços IP acessar a instância. Mesmo com a firewall do SQL e restrições de endereço IP, a autenticação forte é ainda necessária. Ver as recomendações feitas anteriormente no artigo.

Para saber mais sobre as restrições de IP e Firewall do SQL Azure, veja:

- [Controlo de acesso de base de dados SQL do Azure](../sql-database/sql-database-control-access.md)
- [Configurar regras de firewall da SQL Database do Azure - descrição geral](../sql-database/sql-database-firewall-configure.md)
- [Configurar uma regra de firewall ao nível do servidor de base de dados do Azure SQL com o portal do Azure](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Encriptação de dados inativos
[Encriptação de dados transparente (TDE)](https://msdn.microsoft.com/library/azure/bb934049) está ativada por predefinição. TDE transparente encripta os ficheiros de registo e de dados do SQL Server, SQL Database do Azure e Azure SQL Data Warehouse. TDE protege contra um comprometimento de acesso direto à sua cópia de segurança ou os ficheiros. Isto permite-lhe encriptar dados Inativos sem alterar os aplicativos existentes. TDE deve sempre ficar ativado; No entanto, isto não irá parar um atacante a utilizar o caminho de acesso normal. TDE fornece a capacidade para estar em conformidade com várias leis, regulamentações e diretrizes estabelecidas em vários setores.

SQL do Azure gere problemas relacionados com chaves para TDE. Como com TDE, no local especial deve ter cuidado para garantir a capacidade de recuperação e quando mover bases de dados. Em cenários mais sofisticados, as chaves podem ser explicitamente geridas no Azure Key Vault através da gestão de chave extensível (consulte [ativar a TDE no SQL Server usando EKM](/sql/relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm)). Isto também permite para Bring Your Own Key (BYOK) através de capacidade de BYOK de cofres de chaves do Azure.

SQL do Azure fornece encriptação para colunas através de [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Isso permite que o acesso apenas aplicativos autorizados para colunas confidenciais. Usar esse tipo de criptografia limita a consultas SQL em colunas encriptadas para valores com base na igualdade.

Encriptação de nível de aplicativo também deve ser utilizada para dados seletivos. Preocupações de soberania de dados, às vezes, podem ser atenuadas ao encriptar dados com uma chave que é mantida no país correto. Isto impede que a transferência de dados acidentais até mesmo causar um problema, uma vez que é impossível descriptografar os dados sem a chave, partindo do princípio de que um algoritmo forte seja usado (como AES 256).

Pode usar as precauções adicionais para ajudar a proteger a base de dados, como a criação de um sistema seguro, criptografia de ativos confidenciais e a criação de uma firewall ao redor dos servidores da base de dados.

## <a name="next-steps"></a>Passos Seguintes
Este artigo apresentou-a uma coleção de base de dados SQL e SQL Data Warehouse melhores práticas de segurança para proteger as suas aplicações móveis e web de PaaS. Para saber mais sobre como proteger as suas implementações de PaaS, consulte:

- [Proteger implementações PaaS](security-paas-deployments.md)
- [Proteger web de PaaS e aplicativos móveis usando os serviços de aplicações do Azure](security-paas-applications-using-app-services.md)
