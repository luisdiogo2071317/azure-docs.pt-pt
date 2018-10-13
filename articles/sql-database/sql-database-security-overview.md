---
title: Descrição Geral da Segurança da Base de Dados SQL do Azure | Microsoft Docs
description: Saiba mais sobre segurança de base de dados do Azure SQL e SQL Server, incluindo as diferenças entre o SQL Server no local e na cloud.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, ronitr
manager: craigg
ms.date: 10/11/2018
ms.openlocfilehash: b8bb9cbf53b297d8dca1ac67bae8765edcc2c9f4
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311206"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Uma descrição geral das capacidades de segurança da base de dados do Azure SQL

Este artigo explica como utilizar a Base de Dados SQL do Azure para proteger a camada de dados de uma aplicação. Em particular, este artigo apresenta-lhe com recursos para proteger dados, controlar o acesso e o monitoramento proativo.

Para obter uma descrição geral completa das funcionalidades de segurança disponíveis em todos os tipos de SQL, veja [Security Center for SQL Server Database Engine and Azure SQL Database (Centro de Segurança do Motor da Base de Dados SQL e da Base de Dados SQL do Azure)](https://msdn.microsoft.com/library/bb510589). Também estão disponíveis informações adicionais no [documento técnico Security and Azure SQL Database (A Base de Dados SQL do Azure e a Segurança)](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Proteger os dados

### <a name="encryption"></a>Encriptação

A Base de Dados SQL protege os seus dados ao oferecer encriptação para dados em movimento com [Transport Layer Security](https://support.microsoft.com/kb/3135244), para dados inativos com [Encriptação de Dados Transparente](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) e para dados em utilização com [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

> [!IMPORTANT]
> Todas as ligações à Base de Dados SQL do Azure precisam de encriptação (SSL/TLS) em todos os momentos enquanto os dados se encontram "em trânsito" para e a partir da base de dados. Na cadeia de ligação da sua aplicação, tem de especificar parâmetros para encriptar a ligação e *não* para confiar o certificado de servidor (isto é feito por si se copiar a cadeia de ligação fora do portal do Azure), caso contrário, o ligação não verifica a identidade do servidor e é suscetível a ataques "man-in-the-middle". Para o controlador ADO.NET, por exemplo, estes parâmetros da cadeia de ligação são **Encrypt=True** e **TrustServerCertificate=False**. Para obter informações sobre TLS e conectividade, consulte [considerações de TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

Para outras formas de encriptar os seus dados, considere:

- A [encriptação ao nível da célula](https://msdn.microsoft.com/library/ms179331.aspx), para encriptar colunas específicas ou até mesmo células de dados com chaves de encriptação diferentes.
-  Se precisar de uma tecnologia de módulo de Hardware de segurança ou de Bring Your Own Key (BYOK) para encriptação de dados transparente, considere utilizar [encriptação transparente de dados do Azure SQL: suporte de traga a sua própria chave](transparent-data-encryption-byok-azure-sql.md).

### <a name="data-discovery--classification"></a>Dados de deteção e classificação

Deteção de dados e classificação (atualmente em pré-visualização) fornece capacidades avançadas incorporadas na base de dados SQL do Azure para detetar, classificação, etiquetagem e proteger os dados confidenciais em seus bancos de dados. A detetar e classificar os dados confidenciais extrema (empresas financeiras, cuidados de saúde, PII, etc.) pode desempenham um papel essencial na sua organização escritor de proteção de informações. Que possa servir como a infraestrutura para:

- Vários cenários de segurança, como monitorização (auditoria) e os alertas no anómalo acesso a dados confidenciais.
- Controlar o acesso ao e o sistema de proteção de segurança, bases de dados conter os dados altamente confidenciais.
- Ajudar a atender a requisitos de conformidade a normas e de normas de privacidade de dados.

Para obter mais informações, consulte [introdução à deteção de dados do SQL DB e classificação](sql-database-data-discovery-and-classification.md).

## <a name="control-access"></a>Controlar o acesso

A Base de Dados SQL protege os seus dados ao limitar o acesso à base de dados com regras de firewall, mecanismos de autenticação que exigem que os utilizadores forneçam a sua identidade e autorização a dados através de permissões e associações baseadas em funções, bem como através de segurança ao nível da linha e máscara de dados dinâmicos. Para ver um debate da utilização das funcionalidades de controlo de acesso na Base de Dados SQL, veja [Controlar o acesso](sql-database-control-access.md).

> [!IMPORTANT]
> A gestão de bases de dados e servidores lógicos no Azure é controlada pelas atribuições de função da sua conta de utilizador do portal. Para obter mais informações sobre este artigo, consulte [controlo de acesso baseado em funções no portal do Azure](../role-based-access-control/overview.md).

### <a name="firewall-and-firewall-rules"></a>Firewall e regras de firewall

Para ajudar a proteger os seus dados, as firewalls impedem todo o acesso ao seu servidor de base de dados enquanto não especificar que computadores têm permissão com [regras de firewall](sql-database-firewall-configure.md). A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.

### <a name="authentication"></a>Autenticação

A autenticação da Base de Dados SQL diz respeito à forma como o utilizador prova a sua identidade quando se liga à base de dados. A Base de Dados SQL suporta dois tipos de autenticação:

- **Autenticação do SQL**

  Este método de autenticação utiliza um nome de utilizador e palavra-passe. Quando criou o servidor lógico para a sua base de dados, especificou um início de sessão "administrador do servidor" com um nome de utilizador e palavra-passe. Com estas credenciais, pode autenticar-se em qualquer base de dados nesse servidor como o proprietário da base de dados ou "dbo".

- **Autenticação do Active Directory do Azure**

  Este método de autenticação utiliza identidades geridas pelo Azure Active Directory e é suportado para domínios geridos e integrados. Utilize a autenticação do Active Directory (segurança integrada) [sempre que possível](https://msdn.microsoft.com/library/ms144284.aspx). Se pretender utilizar a autenticação do Azure Active Directory, tem de criar outro administrador de servidor, denominado "Administrador do Azure AD", que tem permissão para administrar utilizadores e grupos do Azure AD. Este administrador também pode fazer todas as operações que um administrador de servidor normal faz. Veja [Connecting to SQL Database By Using Azure Active Directory Authentication (Utilizar a Autenticação do Azure Active Directory para Ligar à Base de Dados SQL)](sql-database-aad-authentication.md) para obter instruções sobre como criar um administrador do Azure AD e ativar a Autenticação do Azure Active Directory.

### <a name="authorization"></a>Autorização

A autorização diz respeito ao que um utilizador pode fazer dentro de uma Base de Dados SQL do Azure e é controlada pelas permissões de nível de objeto e associações das funções de base de dados da sua conta de utilizador. Como melhor prática, deverá conceder aos utilizadores o mínimo de privilégios necessários. A conta de administrador do servidor que está a ligar é membro de db_owner, que tem autoridade para fazer todas as operações na base de dados. Guarde esta conta para a implementação de atualizações de esquema e outras operações de gestão. Utilize a conta "ApplicationUser" com permissões mais limitadas para se ligar da sua aplicação à base de dados com o mínimo de privilégios necessários para a sua aplicação.

### <a name="row-level-security"></a>Segurança ao Nível da Linha

A Segurança ao Nível da Linha permite aos clientes controlar o acesso às linhas numa tabela de base de dados com base nas características do utilizador que executa uma consulta (por exemplo, associação a um grupo ou contexto de execução). Para obter mais informações, veja [Segurança ao Nível da Linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security).

### <a name="dynamic-data-masking"></a>Máscara de dados dinâmica

Máscara de dados dinâmicos da base de dados SQL limita a exposição de dados confidenciais ao mascará-los para utilizadores sem privilégios. Automaticamente a máscara de dados dinâmicos Deteta dados potencialmente confidenciais na base de dados do Azure SQL e fornece recomendações passíveis de ação para mascarar estes campos, com um impacto mínimo na camada da aplicação. Funciona ao ofuscar os dados confidenciais no conjunto de resultados de uma consulta em campos de base de dados designados, enquanto os dados na base de dados não são alterados. Para obter mais informações, consulte [introdução à máscara de dados dinâmicos da base de dados SQL](sql-database-dynamic-data-masking-get-started.md).

## <a name="proactive-monitoring"></a>Monitorização proativa

A Base de Dados SQL protege os seus dados, fornecendo capacidades de auditoria e deteção de ameaças.

### <a name="auditing"></a>Auditoria

A Auditoria da Base de Dados SQL controla as atividades de base de dados e ajuda a manter a conformidade regulamentar, ao registar eventos de base de dados num registo de auditoria na conta de Armazenamento do Azure. A Auditoria permite-lhe compreender as atividades de base de dados em curso, bem como analisar e investigar atividade do histórico para identificar potenciais ameaças ou suspeita de abuso e violações de segurança. Para obter informações adicionais, veja [Introdução à Auditoria da Base de Dados SQL](sql-database-auditing.md).  

### <a name="threat-detection"></a>Deteção de ameaças

Deteção de ameaças complementa a auditoria, fornecendo uma camada adicional de inteligência de segurança incorporada no serviço de base de dados do Azure SQL que Deteta tentativas invulgares e potencialmente prejudiciais de aceder ou explorar bases de dados. São alertados sobre atividades suspeitas, potenciais vulnerabilidades e ataques de injeção de SQL, bem como os padrões de acesso de base de dados anómalas. Alertas de deteção de ameaças podem ser visualizadas [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) e fornecer detalhes de atividade suspeita e a ação sobre como investigar e mitigar a ameaça recomendada. Deteção de ameaças custa us $15/servidor/mês. É gratuito durante os primeiros 60 dias. Para obter mais informações, veja [Get started with SQL Database Threat Detection (Introdução à Deteção de Ameaças da Base de Dados SQL)](sql-database-threat-detection.md).

## <a name="compliance"></a>Conformidade

Além das funcionalidades acima e que podem ajudar a sua aplicação a cumprir diversos requisitos de segurança, a base de dados do Azure SQL também participa em auditorias regulares e foi certificada por uma série de normas de conformidade. Para obter mais informações, veja o [Centro de Fidedignidade do Microsoft Azure](https://azure.microsoft.com/support/trust-center/), onde pode encontrar a lista mais recente de [certificações de conformidade da Base de Dados SQL](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="security-management"></a>Gestão de segurança

Base de dados SQL ajuda-o a gerir a segurança de dados ao fornecer análises de base de dados e um dashboard de segurança centralizada através de [avaliação de vulnerabilidades do SQL](sql-vulnerability-assessment.md).

**Avaliação de vulnerabilidades**: [avaliação de vulnerabilidades do SQL](sql-vulnerability-assessment.md) (atualmente em pré-visualização) é uma fácil de configurar a ferramenta incorporada na base de dados do SQL do Azure que pode ajudá-lo a detetar, controlar e remediar potencial da base de dados vulnerabilidades. A avaliação executa uma análise de vulnerabilidade na base de dados e gera um relatório que permite-lhe ver o estado da segurança, incluindo passos acionáveis para resolver problemas de segurança e melhorar a segurança da base de dados. Os relatórios das avaliações podem ser personalizados para o seu ambiente, definindo uma linha de base aceitável para configurações de permissão, configurações de funcionalidades e definições de base de dados. Isso pode ajudá-lo a:

- Cumprir os requisitos de conformidade que exigem os relatórios de análise de dados.
- Cumpre as normas de privacidade de dados.
- Monitorize um ambiente de base de dados dinâmicos em que as alterações são difíceis de acompanhar.

Para obter mais informações, consulte [avaliação de vulnerabilidades do SQL](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Passos Seguintes

- Para ver um debate da utilização das funcionalidades de controlo de acesso na Base de Dados SQL, veja [Controlar o acesso](sql-database-control-access.md).
- Para uma discussão sobre auditoria de base de dados, consulte [auditoria de base de dados SQL](sql-database-auditing.md).
- Para uma discussão sobre a deteção de ameaças, consulte [deteção de ameaças da base de dados SQL](sql-database-threat-detection.md).
