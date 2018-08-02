---
title: Descrição geral de instância de gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este tópico descreve uma instância de gerida de base de dados do Azure SQL e explica como funciona e como isso é diferente de uma base de dados na base de dados do Azure SQL.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: bonova
ms.openlocfilehash: ff571035c9360730546732205f34d75ace38fbff
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414321"
---
# <a name="what-is-a-managed-instance-preview"></a>O que é uma instância gerida (pré-visualização)?

O Azure SQL Database Managed Instance (pré-visualização) é uma nova funcionalidade do Azure SQL Database, fornecendo quase 100% de compatibilidade com SQL Server no local (Enterprise Edition), fornecendo um nativo [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) implementação que resolva problemas de segurança comuns, e um [modelo de negócio](https://azure.microsoft.com/pricing/details/sql-database/) favoráveis para clientes de SQL Server no local. Instância gerida permite que os clientes existentes do SQL Server levantar e deslocar as suas aplicações no local para a cloud com alterações mínimas de aplicativo e a base de dados. Ao mesmo tempo, a instância gerida preserva todas as funcionalidades de PaaS (atualizações automáticas de aplicação de patches e de versão, cópia de segurança, elevada disponibilidade), que reduz drasticamente os custos de gestão e o custo total de propriedade.

> [!IMPORTANT]
> Para obter uma lista das regiões nas quais a Instância Gerida está atualmente disponível, consulte [Migrar as bases de dados para um serviço completamente gerido com Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
O diagrama seguinte descreve as principais funcionalidades de instância gerida:

![principais recursos](./media/sql-database-managed-instance/key-features.png)

Instância gerida é vislumbramos como plataforma preferencial para os seguintes cenários: 

- SQL Server no local / IaaS os clientes que pretendem para migrar seus aplicativos para um serviço totalmente gerido com um mínimo de alterações de design.
- Os ISVs depender de bases de dados SQL, que pretendem permitem aos seus clientes a migrar para a cloud e, portanto, obter vantagem competitiva substancial ou alcançar o mercado global. 

Ao disponibilidade geral, a instância gerida tem como objetivo fornecer próximo de compatibilidade de área de superfície de 100% com a última versão de SQL Server no local através de um plano de versão em etapas. 

A tabela a seguir descreve as diferenças da chave e vislumbramos cenários de utilização entre SQL IaaS, a base de dados do Azure SQL e SQL Database Managed Instance:

| | Cenário de utilização | 
| --- | --- | 
|Instância Gerida da Base de Dados SQL |Para os clientes que pretendem para migrar um grande número de aplicações de IaaS, personalizada criada ou no local ou a ISV fornecido, com como esforço de migração de baixa possível, propor a instância gerida. Utilizar totalmente automatizada [serviço de migração de dados (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) no Azure, os clientes podem lift- and -shift seu SQL Server no local para uma instância gerida, que oferece compatibilidade com SQL Server no local e completo isolamento de instâncias de cliente com suporte nativo a VNET.  Com o Software Assurance, pode trocar suas licenças existentes para tarifas com desconto numa instância gerida do SQL da base de dados utilizando o [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Instância gerida do SQL da base de dados é o melhor destino de migração na cloud para instâncias do SQL Server que requerem segurança elevada e uma superfície de programação avançado. |
|Base de dados SQL do Azure (único ou agrupamento) |**Conjuntos elásticos**: para os clientes a desenvolver novas aplicações de multi-inquilinos de SaaS ou intencionalmente transformar existente no local aplicações numa aplicação SaaS multi-inquilino, propor conjuntos elásticos. Vantagens desse modelo são: <br><ul><li>Conversão do modelo comercial de venda de licenças para vender subscrições de serviços (para ISVs)</li></ul><ul><li>Isolamento de inquilino de fácil e segura</li></ul><ul><li>Um modelo de programação centrado no banco de dados simplificado</li></ul><ul><li>O potencial para aumentar horizontalmente, sem ter de acessar um teto de disco rígido</li></ul>**Bases de dados únicas**: para os clientes desenvolver novas aplicações que não seja o SaaS multi-inquilino, cuja carga de trabalho é estável e previsível, propor bases de dados individuais. Vantagens desse modelo são:<ul><li>Um modelo de programação centrado no banco de dados simplificado</li></ul>  <ul><li>Desempenho previsível para cada base de dados</li></ul>|
|Máquina virtual IaaS do SQL|Para os clientes que precisam personalizar o sistema operativo ou o servidor de base de dados, bem como que os clientes tenham requisitos específicos em termos de executar aplicações de terceiros pelo lado com o SQL Server (na mesma VM), para propor as VMs do SQL / IaaS como a solução ideal|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Como identificar programaticamente uma instância gerida

A tabela seguinte mostra várias propriedades, acessíveis através de Transact SQL, que pode utilizar para detetar se a aplicação está a funcionar com a instância gerida e obter propriedades importantes.

|Propriedade|Valor|Comentário|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation.|Este valor é a mesmo como na base de dados SQL.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Este valor é a mesmo como na base de dados SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Este valor identifica exclusivamente a instância gerida.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nome DNS de instância completa no seguinte formato:<instanceName>.<dnsPrefix>. Database.Windows.NET, onde <instanceName> é o nome fornecido pelo cliente, enquanto <dnsPrefix> é gerado automaticamente parte do mesmo garantindo a exclusividade de nome DNS global ("wcus17662feb9ce98", por exemplo)|Exemplo: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Principais funcionalidades e capacidades de uma instância gerida 

> [!IMPORTANT]
> Executa uma instância gerida com todas as funcionalidades da versão mais recente do SQL Server, incluindo operações online, correções de planos automática e outros aprimoramentos de desempenho do enterprise. 

| **Vantagens de PaaS** | **Continuidade do negócio** |
| --- | --- |
|Sem comprar hardware e gestão <br>Nenhuma sobrecarga de gerenciamento para gerir a infraestrutura subjacente <br>Aprovisionamento rápido e dimensionamento do serviço <br>Atualização de aplicação de patches e versão automatizada <br>Integração com outros serviços de dados de PaaS |tempo de atividade de 99,99% SLA  <br>Elevada disponibilidade incorporada <br>Dados protegidos com cópias de segurança automáticas <br>Período de retenção de cópia de segurança configuráveis do cliente (fixado a 7 dias em pré-visualização pública) <br>Cópias de segurança iniciada pelo utilizador <br>Capacidade de restaurar ponto na base de dados de tempo |
|**Segurança e conformidade** | **Gestão**|
|Ambiente isolado (integração de VNet, o serviço de inquilino único, computação dedicados e armazenamento) <br>Encriptação de Dados Transparente<br>Autenticação do Azure AD, suporte de início de sessão único <br>Cumpre as normas de conformidade mesmo como base de dados SQL do Azure <br>Auditoria do SQL <br>Deteção de ameaças |API do Resource Manager do Azure para automatizar o serviço de aprovisionamento e dimensionamento <br>Funcionalidade de portal do Azure para o serviço de aprovisionamento e dimensionamento manual <br>Serviço de migração de dados 

![Início de sessão único](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model"></a>modelo de compra baseado em vCore

O modelo de compra baseado em vCore oferece-lhe flexibilidade, controlo, transparência e uma forma direta de traduzir locais requisitos de carga de trabalho para a cloud. Este modelo permite-lhe dimensionar a computação, memória e armazenamento com base nas suas necessidades de carga de trabalho. O modelo de vCore também é elegível para a economia de 30 por cento com o [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Um núcleo virtual representa a CPU lógica oferecida com a opção de escolher entre gerações de hardware.
- As CPUs Lógicas de Geração 4 baseiam-se nos processadores Intel E5-2673 v3 (Haswell) de 2,4 GHz.
- Geração 5 de CPUs lógicas baseiam-se no Intel E5-2673 v4 (Broadwell) 2,3 GHz.

A tabela seguinte ajuda-o a compreender como selecionar a configuração ideal de sua computação, memória, armazenamento e recursos de e/s.

||Geração 4|Geração 5|
|----|------|-----|
|Hardware|Intel E5-2673 v3 (Haswell) 2,4 GHz, anexado SSD vCore = 1 PP (núcleos físicos)|Intel E5-2673 v4 (Broadwell) rápida de 2,3 GHz, eNVM SSD, vCore = 1 LP (hyper-thread)|
|Níveis de desempenho|8, 16, 24 vCores|8, 16, 24, 32, 40, 64, 80 vCores|
|Memória|7 GB por vCore|5,5 GB por vCore|
||||

## <a name="managed-instance-service-tiers"></a>Gerido escalões de serviço da instância

Instância gerida está disponível em dois escalões de serviço:
- **Fins gerais**: concebido para aplicações com típicas de disponibilidade e requisitos de latência de e/s comuns.
- **Crítico para a empresa**: concebido para aplicações com elevada disponibilidade e requisitos de latência baixos e/s.
 
> [!IMPORTANT]
> Alterar a camada de serviços de fins gerais para crítico para a empresa ou vice-versa não é suportado em pré-visualização pública. Se pretender migrar as bases de dados para uma instância na camada de serviço diferente, pode criar a nova instância, restaurar bases de dados com o ponto no tempo da instância original e, em seguida, remover a instância original se ela não é mais necessária. 

### <a name="general-purpose-service-tier"></a>Escalão de serviço de fins geral

A lista seguinte descreve característica-chave da camada de serviços de fins gerais: 

- Conceber para a maioria dos aplicativos de negócios com o desempenho típico e requisitos de HA 
- Armazenamento Premium do Azure de alto desempenho (8 TB) 
- 100 bases de dados / instância 

Neste escalão, pode selecionar o armazenamento e a capacidade de computação independentemente. 

O diagrama seguinte ilustra a computação de Active Directory e os nós redundantes neste escalão de serviço.
 
![Escalão de serviço de fins geral](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

A lista seguinte descreve as principais características da camada de serviços de fins gerais:

|Funcionalidade | Descrição|
|---|---|
| Número de vCores * | 8, 16, 24 (geração 4)<br>8, 16, 24, 32, 40, 64, 80 (fins 5)|
| Versão do SQL Server / criar | SQL Server mais recente (disponível) |
| Tamanho de armazenamento min | 32 GB |
| Tamanho máximo de armazenamento | 8 TB |
| Armazenamento máximo por base de dados | Determinado pelo tamanho de armazenamento máximo por instância |
| IOPS de armazenamento esperado | IOPS de 500-7500 por ficheiro de dados (depende do ficheiro de dados). Consulte [o armazenamento Premium](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Número de ficheiros de dados (linhas) pela base de dados | Vários | 
| Número de ficheiros de registo (registo) por base de dados | 1 | 
| Gerido cópias de segurança automáticas | Sim |
| HA | Com base no armazenamento remoto e [do Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Instância incorporada e monitorização de base de dados e métricas | Sim |
| Aplicação de patches de software automáticas | Sim |
| VNet - implementação do Azure Resource Manager | Sim |
| VNet - modelo de implementação clássica | Não |
| Suporte do portal | Sim|
|||

\* Um núcleo virtual representa a CPU lógica oferecida com a opção de escolher entre gerações de hardware. Geração 4 CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) 2,4 GHz e de CPUs lógicas de geração 5 baseiam-se no Intel E5-2673 v4 (Broadwell) 2,3 GHz. 

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio

Camada de serviços críticos de negócios foi concebida para aplicações com requisitos de e/s elevados. Ele oferece maior resiliência a falhas com várias Always On réplicas isoladas. O diagrama seguinte ilustra a arquitetura subjacente neste escalão de serviço:

![Camada de serviços críticos de negócio](./media/sql-database-managed-instance/business-critical-service-tier.png)  

A lista seguinte descreve as principais características da camada de serviços críticos de negócio: 
-   Concebido para aplicações de negócio com mais elevado desempenho e requisitos de HA 
-   É fornecido com o armazenamento SSD extremamente rápido (até 1 TB em Gen 4 e até 4 TB nos fins 5)
-   Suporta até 100 bases de dados por instância 

|Funcionalidade | Descrição|
|---|---|
| Número de vCores * | 8, 16, 24, 32 (geração 4)<br>8, 16, 24, 32, 40, 64, 80 (fins 5)|
| Versão do SQL Server / criar | SQL Server mais recente (disponível) |
| Recursos adicionais | [OLTP dentro da memória](sql-database-in-memory.md)<br> 1 réplica só de leitura adicional ([Escalamento leitura](sql-database-read-scale-out.md))
| Tamanho de armazenamento min | 32 GB |
| Tamanho máximo de armazenamento | Gen 4: 1 TB (todos os vCore tamanhos<br> Geração 5:<ul><li>1 TB para 8, 16 vCores</li><li>2 TB para 24 vCores</li><li>4 TB para 32, 40, 64, 80 vCores</ul>|
| Armazenamento máximo por base de dados | Determinado pelo tamanho de armazenamento máximo por instância |
| Número de ficheiros de dados (linhas) pela base de dados | Vários | 
| Número de ficheiros de registo (registo) por base de dados | 1 | 
| Gerido cópias de segurança automáticas | Sim |
| HA | Com base na [grupos de disponibilidade Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) e [do Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Instância incorporada e monitorização de base de dados e métricas | Sim |
| Aplicação de patches de software automáticas | Sim |
| VNet - implementação do Azure Resource Manager | Sim |
| VNet - modelo de implementação clássica | Não |
| Suporte do portal | Sim|
|||

## <a name="advanced-security-and-compliance"></a>Segurança e conformidade avançadas 

### <a name="managed-instance-security-isolation"></a>Isolamento de segurança de instância gerido 

Instância gerida fornecem isolamento de segurança adicionais de outros inquilinos na nuvem do Azure. Isolamento de segurança inclui: 

- [Implementação de rede virtual nativo](sql-database-managed-instance-vnet-configuration.md) e a conectividade ao seu ambiente no local com o Express Route do Azure ou o Gateway de VPN 
- Ponto final SQL é exposto apenas por meio de um endereço IP privado, que permite conectividade segura do Azure privado ou redes híbridas
- Inquilino único com a infraestrutura subjacente dedicada (computação, armazenamento)

O diagrama seguinte descreve várias opções de conectividade para as suas aplicações: 

![elevada disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Para obter mais detalhes sobre a integração de VNet e enforcements de política ao nível da sub-rede de rede, consulte [configurar uma VNet para a instância gerida da base de dados SQL do Azure](sql-database-managed-instance-vnet-configuration.md) e [ligar a sua aplicação à base de dados do Azure SQL Instância gerida](sql-database-managed-instance-connect-app.md). 

> [!IMPORTANT]
> Coloca a instância gerida vários na mesma sub-rede, onde quer que o que é permitido por seus requisitos de segurança, uma vez que irá fornecer benefícios adicionais. Instâncias collocating na mesma sub-rede irão significativamente simplificar a manutenção da infraestrutura de rede e reduzir a instância do provisionamento de tempo, uma vez que o aprovisionamento de longa duração está associado com o custo da instância gerida de primeira implementação numa sub-rede.


### <a name="auditing-for-compliance-and-security"></a>Auditoria para conformidade e segurança 

[Geridos a auditoria de instância](sql-database-managed-instance-auditing.md) faixas de base de dados eventos e escreve-os para uma auditoria registo na sua conta de armazenamento do Azure. A auditoria pode ajudar a manter a conformidade regulamentar, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que podem indicar preocupações empresariais ou suspeitas de violações de segurança. 

### <a name="data-encryption-in-motion"></a>Encriptação de dados ativa 

Instância gerida protege os seus dados ao fornecer encriptação de dados em movimento com o Transport Layer Security.

Além da segurança de camada de transporte, a instância de gerida da base de dados do SQL oferece proteção de dados confidenciais em movimento, inativos e durante o processamento de consultas com [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted é uma funcionalidade pioneira da indústria que oferece segurança de dados sem paralelo contra violações que envolvam o roubo de dados confidenciais. Por exemplo, com o Always Encrypted, os números de cartão de crédito são armazenados encriptados na base de dados sempre, mesmo durante o processamento de consultas, permitindo a desencriptação no uso por equipas autorizadas ou das aplicações que têm de processar esses dados. 

### <a name="data-encryption-at-rest"></a>Encriptação de dados inativos 
[Encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) encripta os ficheiros de dados de instância gerida do SQL do Azure, conhecidos como encriptar dados inativos. TDE realiza a encriptação em tempo real de e/s e a descriptografia dos ficheiros de dados e de registo. A encriptação utiliza uma chave de encriptação de base de dados (DEK), que é armazenada no registo de arranque da base de dados de disponibilidade durante a recuperação. Pode proteger todos os bancos de dados na instância gerida com encriptação de dados transparente. A TDE é a tecnologia comprovada de encriptação inativa do SQL e que é exigida por muitas normas de conformidade para proteger de roubos de suportes de dados de armazenamento. Durante a pré-visualização pública, o modelo de gerenciamento de chave principal é suportado (realizadas pela plataforma PaaS). 

Migração de uma base de dados encriptado para a instância gerida de SQL é suportada através do Azure Database Migration Service (DMS) ou o restauro nativo. Se planear migrar base de dados encriptado através de restauro nativo, a migração do certificado TDE existente do SQL Server no local ou VM do SQL Server para instância gerida é uma etapa necessária. Para obter mais informações sobre as opções de migração, consulte [migração de instância do SQL Server para instância gerida da base de dados SQL do Azure](sql-database-managed-instance-migrate.md).

### <a name="dynamic-data-masking"></a>Máscara de dados dinâmica 

Base de dados SQL [máscara de dados dinâmicos](/sql/relational-databases/security/dynamic-data-masking) limita a exposição de dados confidenciais ao mascará-lo para os utilizadores não privilegiados. Máscara de dados dinâmica ajuda a impedir o acesso não autorizado a dados confidenciais ao permitir-lhe designar a quantidade de dados confidenciais a revelar com um impacto mínimo na camada da aplicação. É uma funcionalidade de segurança baseada em políticas que omite os dados confidenciais no conjunto de resultados de uma consulta em campos da base de dados designados, sendo que os dados na base de dados não são alterados. 

### <a name="row-level-security"></a>Segurança ao Nível da Linha 

[Segurança de nível de linha](/sql/relational-databases/security/row-level-security) permite-lhe controlo de acesso às linhas numa tabela de base de dados com base nas características do utilizador que executa uma consulta (por exemplo, com o contexto de associação ou execução de grupo). A segurança ao nível da linha (RLS) simplifica o design e a programação da segurança na sua aplicação. O RLS permite-lhe implementar restrições ao acesso à linha de dados. Por exemplo, garantindo que os funcionários possam acessar apenas as linhas de dados que são pertinentes para o departamento deles ou restringir o acesso a dados a apenas os dados relevantes. 

### <a name="threat-detection"></a>Deteção de ameaças 

[Geridos a deteção de ameaças da instância](sql-database-managed-instance-threat-detection.md) complementa [auditoria de instância gerida](sql-database-managed-instance-auditing.md) , fornecendo uma camada adicional de inteligência de segurança incorporada no serviço que Deteta tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados. É alertado sobre atividades suspeitas, potenciais vulnerabilidades, e ataques de injeção de SQL, bem como os padrões de acesso de base de dados anómalas. Alertas de deteção de ameaças podem ser visualizadas [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) e fornecer detalhes de atividade suspeita e a ação sobre como investigar e mitigar a ameaça recomendada.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integração do Azure Active Directory e a autenticação multifator 

A Base de Dados SQL permite-lhe gerir centralmente identidades de utilizadores de bases de dados e outros serviços Microsoft com a [integração do Azure Active Directory](sql-database-aad-authentication.md). Esta capacidade simplifica a gestão de permissões e melhora a segurança. O Azure Active Directory suporta a [autenticação multifator](sql-database-ssms-mfa-authentication-configure.md) (MFA), para aumentar a segurança de dados e aplicações, suportando, ao mesmo tempo, um processo de início de sessão único. 

### <a name="authentication"></a>Autenticação 
Autenticação do SQL da base de dados refere-se como os usuários comprovem sua identidade quando se liga à base de dados. A Base de Dados SQL suporta dois tipos de autenticação:  

- Autenticação de SQL, que utiliza um nome de utilizador e palavra-passe.
- Authentication do Azure Active Directory, que utiliza identidades geridas pelo Azure Active Directory e é suportada para domínios geridos e integrados. 

### <a name="authorization"></a>Autorização

Autorização diz respeito ao que um utilizador pode fazer dentro de uma base de dados do SQL do Azure e é controlado pelas associações das funções de base de dados de sua conta de utilizador e permissões ao nível do objeto. A instância gerida tem as mesmas capacidades de autorização como SQL Server 2017. 

## <a name="database-migration"></a>Migração de bases de dados 

Gerido cenários de usuário de destinos de instância com a migração de base de dados em massa de implementações de base de dados de IaaS ou no local. Gerida suporta instância várias opções de migração de base de dados: 

### <a name="data-migration-service"></a>Serviço de migração de dados

O serviço de migração de base de dados do Azure é um serviço completamente gerido criado para ativar migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. Este serviço simplifica as tarefas necessárias para mover de terceiros existente e bases de dados do SQL Server para o Azure. Opções de implementação incluem a base de dados do Azure SQL, instância gerida e do SQL Server na VM do Azure em pré-visualização pública. Ver [como migrar a sua base de dados no local para a instância gerida com o DMS](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Cópia de segurança e restauro  

A abordagem de migração tira partido de cópias de segurança SQL para o armazenamento de Blobs do Azure. As cópias de segurança armazenadas no blob de armazenamento do Azure podem ser restauradas diretamente para instância gerida. Para restaurar uma base de dados SQL existente para uma instância gerida, pode:

- Uso [serviço de migração de dados (DMS)](../dms/dms-overview.md). Para obter um tutorial, veja [migrar para uma instância gerida com o Azure Database Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados
- Utilize o [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Para obter um tutorial que mostra como restaurar Wide a World Importers - ficheiro de cópia de segurança da base de dados Standard, veja [restaurar um ficheiro de cópia de segurança para uma instância gerida](sql-database-managed-instance-restore-from-backup-tutorial.md). Este tutorial mostra de que tem de carregar um ficheiro de cópia de segurança para o armazenamento do blogue do Azure e seguro mesmo com uma chave de assinatura (SAS) de acesso partilhado.
  - Para obter informações sobre o restauro a partir do URL, consulte [nativo restaurar a partir do URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

## <a name="sql-features-supported"></a>Funcionalidades do SQL suportadas 

Gerido tem como objetivo instância entregar próximo de compatibilidade de área de superfície de 100% com a chegar em fases até disponibilidade geral do serviço de SQL Server no local. Para funcionalidades e lista de comparação, veja [recursos comuns de SQL](sql-database-features.md).
 
Gerido instância compatibilidade com versões anteriores do oferece suporte para bases de dados do SQL 2008. É suportada a migração direta de servidores de base de dados do SQL 2005, o nível de compatibilidade para bases de dados do SQL 2005 migrados são atualizadas para o SQL 2008. 
 
O diagrama seguinte descreve a compatibilidade de área de superfície na instância gerida:  

![Migração](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Principais diferenças entre o SQL Server no local e a instância gerida 

Gerido benefícios de instância de ser sempre up-até à data na nuvem, o que significa que algumas funcionalidades do SQL Server no local podem ser obsoletos, extinto ou tem alternativas. Existem casos específicos ao ferramentas precisam reconhecer que um determinado recurso funciona de forma ligeiramente diferente ou que o serviço não está em execução num ambiente, que não controlar totalmente: 

- Elevada disponibilidade é incorporada e pré-configuradas. Recursos de alta disponibilidade Always On não são expostos de forma mesmo que se encontre na implementações de IaaS do SQL 
- Cópias de segurança automáticas e o ponto no tempo. Pode iniciar o cliente `copy-only` cópias de segurança que não interferem com a cadeia de cópia de segurança automática. 
- Instância gerida não permite a especificação de caminhos físicos completa para todos os cenários correspondentes tenham de ser suportadas de forma diferente: Restaurar DB não suporta a WITH MOVE, criar DB não permite caminhos físicos, a inserção em massa funciona com Blobs do Azure, apenas, etc. 
- Suporta a instância de geridos [autenticação do Azure AD](sql-database-aad-authentication.md) como alternativa de cloud para a autenticação do Windows. 
- Instância gerida gere automaticamente o grupo de ficheiros XTP e ficheiros de bases de dados que contém objetos de OLTP dentro da memória
- Instância gerida suporta SQL Server Integration Services (SSIS) e pode anfitrião catálogo SSIS (SSISDB) que armazena os pacotes do SSIS, mas eles são executados num gerido do Azure-SSIS Integration Runtime (IR) no Azure Data Factory (ADF), consulte [Create Runtime de integração Azure-SSIS no ADF](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). Para comparar os recursos do SSIS na base de dados SQL e a instância gerida, veja [comparar a base de dados do SQL e a instância gerida (pré-visualização)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview).

### <a name="managed-instance-administration-features"></a>Funcionalidades de administração de instância geridas  

Geridos pelo administrador de sistema de ativação de instância para se focar no que é importante ao máximo para empresas. Muitas atividades de administrador/DBA do sistema não são necessárias, ou eles são simples. Por exemplo, o sistema operacional / instalação RDBMS e aplicação de patches, dinâmica instância redimensionamento e configuração, as cópias de segurança, replicação de base de dados (incluindo bases de dados do sistema), configuração de elevada disponibilidade e configuração de estado de funcionamento e os dados de monitorização de desempenho fluxos. 

> [!IMPORTANT]
> Para obter uma lista dos recursos suportados, parcialmente suportadas e não suportados, consulte [funcionalidades de base de dados SQL](sql-database-features.md). Para obter uma lista de diferenças do T-SQL em instâncias geridas em comparação com o SQL Server, consulte [diferenças de T-SQL de instância gerida do SQL Server](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Passos Seguintes

- Para funcionalidades e lista de comparação, veja [recursos comuns de SQL](sql-database-features.md).
- Para obter mais informações sobre a configuração da VNet, veja [Configuração de VNet de Instância Gerida](sql-database-managed-instance-vnet-configuration.md).
- Para obter um tutorial que cria uma instância gerida e restaura uma base de dados a partir de um ficheiro de cópia de segurança, consulte [criar uma instância gerida](sql-database-managed-instance-create-tutorial-portal.md).
- Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para obter informações sobre preços, consulte [preços da SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
