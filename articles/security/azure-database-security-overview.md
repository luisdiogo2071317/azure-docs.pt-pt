---
title: Descrição geral de segurança da base de dados do Azure | Microsoft Docs
description: Este artigo fornece uma descrição geral da base de dados do Azure funcionalidades de segurança.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 27f6ccadaa89042ebced759ac3040b5b79e1f8f8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051530"
---
# <a name="azure-database-security-overview"></a>Descrição geral de segurança da base de dados do Azure

Segurança é uma preocupação superior para a gestão de bases de dados e foi sempre uma prioridade para a SQL Database do Azure. Base de dados SQL do Azure suporta segurança de ligação com as regras de firewall e a encriptação da ligação. Suporta a autenticação com o nome de utilizador e palavra-passe e a autenticação do Azure Active Directory (Azure AD), que utiliza identidades geridas pelo Azure Active Directory. Autorização utiliza o controlo de acesso baseado em funções.

Base de dados SQL do Azure suporta a encriptação efetuando em tempo real encriptação e desencriptação de bases de dados, cópias de segurança associadas e os ficheiros de registo de transações Inativos sem necessidade de alterações à aplicação.

A Microsoft fornece formas adicionais para encriptar dados da empresa:

-   Encriptação de nível de células está disponível para encriptar colunas específicas ou até mesmo células de dados com diferentes chaves de encriptação.
-   Se precisar de um módulo de hardware de segurança ou gestão central da hierarquia de chave de encriptação, considere utilizar o Cofre de chaves do Azure com o SQL Server numa máquina virtual do Azure (VM).
-   Sempre encriptado (atualmente em pré-visualização) faz encriptação transparente para as aplicações. Também permite que os clientes encriptar dados confidenciais dentro de aplicações de cliente sem partilhar as chaves de encriptação com base de dados SQL.

Auditoria de base de dados SQL do Azure permite às empresas registo de eventos de registo no armazenamento do Azure. A Auditoria da Base de Dados SQL também pode ser integrada no Microsoft Power BI para facilitar a leitura detalhada de relatórios e análises.

Bases de dados SQL do Azure podem ser totalmente protegidos para satisfazer mais regulamentação ou requisitos de segurança, incluindo HIPAA, ISO 27001/27002 e PCI DSS nível 1. Uma lista atual de certificações de conformidade de segurança está disponível na [site Microsoft Azure Trust Center](http://azure.microsoft.com/support/trust-center/services/).

Este artigo explica as noções básicas de proteger bases de dados SQL do Microsoft Azure para dados estruturados, tabela e relacionais. Em particular, este artigo irá ajudá-lo a começar a utilizar recursos para proteger dados, controlar o acesso e a monitorização proativa.

## <a name="protection-of-data"></a>Proteção de dados

Base de dados SQL ajuda a proteger os dados ao fornecer encriptação:

- Para dados em movimento através de [Transport Layer Security (TLS)](https://support.microsoft.com/kb/3135244).
- Para dados Inativos através de [encriptação transparente de dados](http://go.microsoft.com/fwlink/?LinkId=526242).
- Para dados de utilização através de [sempre encriptados](https://msdn.microsoft.com/library/mt163865.aspx).

Para outras formas de encriptar os seus dados, considere:

-   A [encriptação ao nível da célula](https://msdn.microsoft.com/library/ms179331.aspx), para encriptar colunas específicas ou até mesmo células de dados com chaves de encriptação diferentes.
-   [O Cofre de chaves do Azure com o SQL Server numa VM do Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), se necessita de um módulo de hardware de segurança ou de gestão central da hierarquia de chave de encriptação.

### <a name="encryption-in-motion"></a>Encriptação em movimento

Um problema comum para todas as aplicações de cliente/servidor é a necessidade de privacidade como dados movem-se em redes públicas e privadas. Se mover através de uma rede de dados não for encriptados, é provável que podem ser capturada e roubada por utilizadores não autorizados. Quando estiver a lidar com os serviços de base de dados, certifique-se de que os dados são encriptados entre o cliente de base de dados e o servidor. Certifique-se também que os dados são encriptados entre servidores de base de dados que comunicam entre si e com aplicações de camada média.

Um problema ao administrar uma rede é proteger os dados que estão a ser enviados entre aplicações através de uma rede não fidedigna. Pode utilizar [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) para autenticar servidores e clientes e, em seguida, utilizá-la para encriptar mensagens entre as partes autenticadas.

No processo de autenticação, um cliente TLS/SSL envia uma mensagem para um servidor TLS/SSL. O servidor responde com as informações que o servidor tem de se autenticar perante. O cliente e o servidor efetuam uma troca adicional de chaves de sessão e a caixa de diálogo de autenticação termina. Quando autenticação for concluída, a comunicação protegida por SSL pode começar entre o servidor e o cliente através de chaves de encriptação simétricas que são estabelecidas durante o processo de autenticação.

Todas as ligações à base de dados do Azure SQL exigem encriptação (TLS/SSL) em todas as horas enquanto dados são "em trânsito" e da base de dados. Base de dados do SQL Server utiliza o TLS/SSL para autenticar servidores e clientes e, em seguida, utilizá-la para encriptar mensagens entre as partes autenticadas. 

Na cadeia de ligação da sua aplicação, tem de especificar parâmetros para encriptar a ligação e não para confiar no certificado de servidor. (Isto é feito automaticamente se copiar a cadeia de ligação fora do portal do Azure.) Caso contrário, a ligação não irá verificar a identidade do servidor e vai ser susceptível a ataques "man-in-the-middle". Para o controlador do ADO.NET, por exemplo, estes parâmetros de cadeia de ligação são `Encrypt=True` e `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Encriptação inativa
Pode demorar várias precauções para ajudar a proteger a base de dados. Por exemplo, um sistema seguro de design, encriptar ativos confidenciais e criar uma firewall em torno de servidores de base de dados. Mas, num cenário em que o suporte de dados físico (por exemplo, discos ou bandas de cópia de segurança) é roubados, uma parte maliciosa pode apenas restaurar ou expor a base de dados e procurar os dados.

É uma solução encriptar os dados sensíveis na base de dados e proteger as chaves que são utilizadas para encriptar os dados com um certificado. Esta solução impede que qualquer pessoa sem as chaves de utilizar os dados, mas este tipo de proteção têm de ser planeado.

Para resolver este problema, o SQL Server e o suporte de base de dados SQL [encriptação transparente de dados](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current&viewFallbackFrom=sql-server-2017). A encriptação transparente de dados encripta os ficheiros de dados do SQL Server e base de dados SQL, conhecidos como dados de encriptação de inativos.

A encriptação transparente de dados ajuda a proteger contra a ameaça de atividade maliciosa. Efetua a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e os ficheiros de registo de transações Inativos sem necessidade de alterações à aplicação.  

A encriptação transparente de dados encripta o armazenamento de uma base de dados completa através de uma chave simétrica chamada a chave de encriptação da base de dados. Na base de dados do SQL Server, a chave de encriptação da base de dados está protegida por um certificado de servidor incorporada. O certificado de servidor incorporada é exclusivo para cada servidor de base de dados SQL.

Se uma base de dados está numa relação de DR Georreplicação, está protegido por uma chave diferente em cada servidor. Se duas bases de dados estiverem ligados ao mesmo servidor, estes partilham o mesmo certificado incorporado. Microsoft roda automaticamente estes certificados, pelo menos, todos os 90 dias. 

Para obter mais informações, consulte [encriptação transparente de dados](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Encriptação em utilização (cliente)

A maioria das falhas de dados envolvem o roubo de dados críticos, como números de cartão de crédito ou informações pessoais identificáveis. Bases de dados podem ser treasure troves de informações confidenciais. Podem conter dados pessoais (como números de identificação national) dos clientes, informações competitivos confidenciais e propriedade intelectual. Dados perdidos ou roubados, especialmente os dados de cliente, podem resultar em danos marca, desvantagem competitiva e graves fines – lawsuits mesmo.

![A funcionalidade sempre encriptados ilustrada como um bloqueio e uma chave](./media/azure-databse-security-overview/azure-database-fig1.png)

[Sempre encriptado](https://msdn.microsoft.com/library/mt163865.aspx) é uma funcionalidade que foi concebida para proteger os dados confidenciais armazenados em bases de dados SQL Database do Azure ou SQL Server. Sempre encriptado permite aos clientes encriptar dados confidenciais dentro de aplicações de cliente e nunca revelar as chaves de encriptação para o motor de base de dados (base de dados SQL ou SQL Server).

Sempre encriptado fornece uma separação entre as pessoas que possuem os dados (e podem vê-la) e as pessoas que gerem os dados (mas devem ter sem acesso). Ajuda a garantir que no local da base de dados administradores, operadores de base de dados de nuvem ou outros privilégios elevados, mas os utilizadores não autorizados não é possível aceder aos dados encriptados.

Além disso, sempre encriptados faz encriptação transparente para as aplicações. Um controlador ativada sempre encriptado está instalado no computador cliente para que possam automaticamente encriptar e desencriptar dados sensíveis na aplicação de cliente. O controlador encripta os dados nas colunas confidenciais antes de passar os dados para o motor de base de dados. O controlador reescreve automaticamente consultas para que a semântica para a aplicação é preservada. Da mesma forma, o controlador de forma transparente desencripta os dados, armazenados em colunas de base de dados encriptados contidas nos resultados de consulta.

## <a name="access-control"></a>Controlo de acesso
Para fornecer segurança, base de dados SQL controla o acesso ao utilizar:

- Regras de firewall que limitam a conectividade ao endereço IP.
- Mecanismos de autenticação que requerem que os utilizadores provar a sua identidade.
- Mecanismos de autorização que limitam os utilizadores aos seus dados e as ações específicas.

### <a name="database-access"></a>Acesso à Base de Dados

Proteção de dados começa com a controlar o acesso aos seus dados. O datacenter que aloja os seus dados gere o acesso físico. Pode configurar uma firewall para gerir a segurança na camada de rede. Também pode controla o acesso ao configurar os inícios de sessão para autenticação e definir permissões para funções de servidor e base de dados.

#### <a name="firewall-and-firewall-rules"></a>Firewall e regras de firewall

[Base de dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) fornece um serviço de base de dados relacional para o Azure e outras aplicações baseadas na internet. Para ajudar a proteger os seus dados, as firewalls impedem todos os acessos ao seu servidor de base de dados enquanto não especificar que computadores têm acesso. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido. Para obter mais informações, veja [Descrição geral das regras de firewall da Base de Dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

O serviço SQL Database do Azure está disponível apenas através da porta TCP 1433. Para aceder a uma base de dados do SQL Server do seu computador, certifique-se de que a firewall de computador cliente permite a comunicação de TCP de saída na porta TCP 1433. Se as ligações de entrada não são necessários para outras aplicações, bloqueá-las na porta TCP 1433.

#### <a name="authentication"></a>Autenticação

A autenticação diz respeito à forma como prova a sua identidade quando se liga à base de dados. A Base de Dados SQL suporta dois tipos de autenticação:

-   **Autenticação do SQL Server**: é criada uma conta de início de sessão único quando é criada uma instância do SQL Server lógica, denominada a conta de subscritor do SQL Server da base de dados. Esta conta liga-se utilizando [autenticação do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (nome de utilizador e palavra-passe). Esta conta é administradora na instância do servidor lógico e em todas as bases de dados de utilizador ligadas a essa instância. As permissões da conta do subscritor não podem ser limitadas. Só pode existir uma destas contas.
-   **Autenticação do Azure Active Directory**: [autenticação do Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) é um mecanismo de ligar à SQL Database do Azure e Azure SQL Data Warehouse, utilizando as identidades no Azure AD. Pode utilizá-lo a gerir centralmente identidades de utilizadores de base de dados.

![Azure AD autenticação com base de dados SQL](./media/azure-databse-security-overview/azure-database-fig2.png)

 Vantagens da autenticação do Azure AD incluem:
  - Fornece uma alternativa à autenticação do SQL Server.
  - Isto ajuda a parar a proliferação de identidades de utilizador em servidores de base de dados e permite a rotação de palavra-passe num único local.
  - Pode gerir as permissões de base de dados através da utilização de grupos externos (Azure AD).
  - -Pode eliminar armazenar palavras-passe através da autenticação integrada do Windows e outras formas de autenticação que suporte do Azure AD.

#### <a name="authorization"></a>Autorização
[Autorização](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) refere-se a que um utilizador pode fazer dentro de uma base de dados SQL do Azure. Este é controlado pela base de dados da sua conta de utilizador [associações de função](https://msdn.microsoft.com/library/ms189121) e [ao nível do objeto permissões](https://msdn.microsoft.com/library/ms191291.aspx). Autorização é o processo de determinar quais os recursos com capacidade de segurança pode aceder a um principal e as operações são permitidas para esses recursos.

### <a name="application-access"></a>Acesso à aplicação

#### <a name="dynamic-data-masking"></a>Máscara de dados dinâmica
Um representante de serviço no Centro de atendimento telefónico pode identificar os chamadores por vários dígitos do seu número de segurança social ou um número de cartão de crédito. Mas esses itens de dados não devem ser expostas completamente para o representante de serviço.

Pode definir uma regra de máscara dissimula todas, exceto os últimos quatro dígitos de um número de segurança social ou o número de cartão de crédito no conjunto de resultados de qualquer consulta.

![Máscara num número enviado entre aplicações de base de dados e de empresas SQL](./media/azure-databse-security-overview/azure-database-fig3.png)

Outro exemplo, uma máscara de dados adequada pode ser definida para proteger informações pessoais identificáveis. Um programador, em seguida, pode consultar os ambientes de produção para fins de resolução de problemas sem violar as normas de conformidade.

A [máscara de dados dinâmica da Base de Dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados confidenciais ao mascará-los para utilizadores sem privilégios. A máscara de dados dinâmica é suportada para a versão V12 da base de dados do Azure SQL.

[Máscara de dados dinâmicos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ajuda a impedir o acesso não autorizado aos dados confidenciais, permitindo-lhe designar a quantidade dos dados confidenciais para revelar com um impacto mínimo na camada da aplicação. É uma funcionalidade de segurança baseada em políticas que omite os dados confidenciais no conjunto de resultados de uma consulta em campos da base de dados designados, sendo que os dados na base de dados não são alterados.


> [!Note]
> Máscara de dados dinâmicos pode ser configurada pelo administrador de base de dados do Azure, administrador de servidor ou funções de responsável pela segurança.

#### <a name="row-level-security"></a>Segurança ao Nível da Linha
É outro requisito de segurança comuns para bases de dados multi-inquilino [segurança ao nível da linha](https://msdn.microsoft.com/library/dn765131.aspx). Pode utilizar esta funcionalidade para controlar o acesso às linhas numa tabela da base de dados com base em caraterísticas do utilizador que está a executar uma consulta. (As características de exemplo são contexto de associação e execução de grupo.)

![Segurança ao nível da linha, permitindo que um utilizador para acesso linhas numa tabela através de uma aplicação de cliente](./media/azure-databse-security-overview/azure-database-fig4.png)

A lógica de restrição de acesso está localizado na camada de base de dados em vez de away dos dados na camada da aplicação em outro. O sistema de base de dados aplica-se as restrições de acesso sempre que esse acesso de dados é tentado a partir de qualquer camada. Isto torna o seu sistema de segurança mais fiável e robusta, reduzindo a área de superfície do seu sistema de segurança.

Segurança ao nível da linha apresenta o controlo de acesso baseado em predicado. Funcionalidades de uma avaliação flexível e centralizada que pode ter em consideração metadados ou quaisquer outros critérios, que o administrador determina conforme adequada. O predicado é utilizado como um critério para determinar se o utilizador tem o acesso adequado aos dados com base nos atributos de utilizador. Pode implementar o controlo de acesso baseado em etiqueta utilizando o controlo de acesso baseado em predicado.

## <a name="proactive-monitoring"></a>Monitorização proativa
Base de dados SQL ajuda a proteger os dados, fornecendo *auditoria* e *deteção de ameaças* capacidades.

### <a name="auditing"></a>Auditoria
[Auditoria de base de dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) aumenta a capacidade para obter conhecimentos aprofundados sobre eventos e as alterações que ocorrem na base de dados. Os exemplos são atualizações e consultas em relação as dados.

Auditoria de base de dados SQL controla os eventos de base de dados e escreve-los para uma auditoria iniciar sessão na sua conta do storage do Azure. Auditoria pode ajudar a manter a conformidade de regulamentação, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que poderão indicar preocupações para a empresa ou suspeitas de violação de segurança. Auditoria permite e facilita a conformidade com as normas de conformidade, mas não garante a compatibilidade.

Pode utilizar a base de dados SQL auditoria:

-   **Manter** um registo de auditoria de eventos selecionados. Pode definir categorias de ações de base de dados a serem auditados.
-   **Relatório** na atividade de base de dados. Pode utilizar relatórios pré-configurados e um dashboard para começar a trabalhar rapidamente com atividade e o relatório de eventos.
-   **Analisar** relatórios. Pode encontrar eventos suspeitos, atividade invulgar e tendências.

Existem dois métodos de auditorias:

-   **Auditoria de blob**: os registos são escritos para armazenamento de Blobs do Azure. Este é um método de auditoria mais recente. Fornece um desempenho superior, suporta a auditoria de maior granularidade ao nível do objeto e é mais económico.
-   **Auditoria de tabela**: os registos são escritos para armazenamento de tabelas do Azure.

### <a name="threat-detection"></a>Deteção de ameaças
[A deteção de ameaças de base de dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) Deteta atividades suspeitas que indicam potenciais ameaças de segurança. Pode utilizar a deteção de ameaças para responder a eventos suspeitos na base de dados, tais como SQL injections, à medida que ocorrem. Disponibiliza alertas e permite a utilização de auditoria de SQL Database do Azure para explorar os eventos suspeitos.

![Deteção de ameaças para a base de dados SQL e uma aplicação web, com um atacante externo e um insider malicioso](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Por exemplo, a injeção de SQL é um dos problemas de segurança comuns para aplicações web. É utilizado para atacar aplicações condicionada por dados. Os atacantes tirar partido de vulnerabilidades de aplicação para injetar maliciosas instruções SQL para campos de entrada de aplicação, ser, ou modificar dados na base de dados.

Officers de segurança ou outros administradores designados podem obter uma notificação imediata sobre atividades suspeitas da base de dados à medida que ocorrem. Cada notificação fornece detalhes da atividade suspeita e recomenda como ainda mais a investigar e mitigar a ameaça.        

## <a name="centralized-security-management"></a>Gestão de segurança centralizada

O [Centro de Segurança do Azure](https://azure.microsoft.com/documentation/services/security-center/) ajuda-o a evitar, detetar e responder a ameaças. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure. Ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

[Centro de segurança](https://docs.microsoft.com/azure/security-center/security-center-sql-database) ajuda a salvaguardar dados na base de dados do SQL Server, fornecendo visibilidade para a segurança de todos os servidores e bases de dados. Centro de segurança, pode:

-   Defina políticas para auditoria e de encriptação de base de dados SQL.
-   Monitorize a segurança dos recursos de base de dados SQL em todas as subscrições.
-   Rapidamente identificar e resolver problemas de segurança.
-   Integrar alertas a partir do [deteção de ameaças da SQL Database do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Centro de segurança suporta o acesso baseado em funções.

## <a name="azure-marketplace"></a>Azure Marketplace

O Azure Marketplace é um mercado online de aplicações e serviços que permite às empresas recém-criadas e aos fabricantes independentes de software (ISV) oferecerem as suas soluções aos clientes Azure de todo o mundo.
O Azure Marketplace, combine ecossistemas de parceiros do Microsoft Azure de uma plataforma unificada para melhor servirem os clientes e os parceiros. Pode [executar uma procura](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) para ver os produtos de segurança de base de dados disponíveis no Azure Marketplace.

## <a name="next-steps"></a>Passos Seguintes

- [Proteger a base de dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial)
- [Serviço de centro de segurança do Azure e SQL Database do Azure](https://docs.microsoft.com/azure/security-center/security-center-sql-database)
- [Deteção de ameaças de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Melhorar o desempenho de base de dados do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial)
