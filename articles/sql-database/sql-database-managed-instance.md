---
title: Descrição geral de instância de geridos de base de dados SQL do Azure | Microsoft Docs
description: Este tópico descreve uma instância do Azure SQL da base de dados geridos e explica como funciona e como é diferente da base de dados individual na SQL Database do Azure.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/10/2018
ms.author: bonova
ms.openlocfilehash: 85a0157751a1c26fb7f37152d7c12f56b1c423d1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-a-managed-instance-preview"></a>O que é uma instância geridos (pré-visualização)?

Instância do Azure SQL da base de dados geridos (pré-visualização) é uma nova funcionalidade da SQL Database do Azure, fornecendo quase 100% a compatibilidade com o SQL Server no local (Enterprise Edition), fornecendo um nativo [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) implementação endereços preocupações de segurança comuns, e um [modelo de negócio](https://azure.microsoft.com/pricing/details/sql-database/) favoráveis para clientes de SQL Server no local. Instância gerida permite que os clientes existentes do SQL Server para comparação de precisão e deslocar às suas aplicações no local para a nuvem com alterações mínimas de aplicação e da base de dados. Ao mesmo tempo, a instância geridos preserva todas as PaaS capacidades (atualizações automáticas de aplicação de patches e a versão da cópia de segurança, elevada disponibilidade), que reduz significativamente a sobrecarga de gestão e o TCO.

> [!IMPORTANT]
> Para obter uma lista das regiões nas quais a Instância Gerida está atualmente disponível, consulte [Migrar as bases de dados para um serviço completamente gerido com Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
O diagrama a seguir descreve as principais funcionalidades de instância geridos:

![principais funcionalidades](./media/sql-database-managed-instance/key-features.png)

Instância gerida é envisioned como plataforma preferencial para os seguintes cenários: 

- SQL Server no local / alterações de design de procura para migrar as aplicações para um serviço completamente gerido com o mínimo de clientes de IaaS.
- Os ISVs depender de bases de dados do SQL Server, que pretendem ativar os seus clientes migrar para a nuvem e, por conseguinte, alcançar substancial vantagem competitiva ou entre em contacto mercado global. 

Através da disponibilidade geral, instância geridos visa para a entrega de perto com a última versão de SQL Server no local através de um plano de versão de teste de compatibilidade de área de superfície de 100%. 

Destaques de tabela seguintes diferenças da chave e envisioned cenários de utilização entre IaaS do SQL Server, SQL Database do Azure e a instância de gerido da base de dados do SQL Server:

| | Cenário de utilização | 
| --- | --- | 
|Instância Gerida da Base de Dados SQL |Para os clientes procura para migrar um grande número de aplicações no local ou IaaS, personalizada incorporada, ou ISV fornecido, com como esforço de migração baixa quanto possível, propor instância geridos. Utilizar totalmente automatizado [serviço de migração de dados (DMS)](/sql/dma/dma-overview) no Azure, os clientes podem de comparação de precisão e deslocar o respetivo SQL Server no local a uma instância de geridos oferece compatibilidade com o SQL Server no local e o isolamento completado do instâncias de cliente com suporte VNET nativo.  Com o Software Assurance, pode trocar as respetivas licenças existentes para taxas com desconto durante uma instância de geridos de base de dados do SQL Server utilizando o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  A instância de gerido da base de dados do SQL Server é o destino de migração melhores na nuvem para instâncias do SQL Server que necessitam de alta segurança e uma superfície de programação para avançado. |
|Base de dados SQL do Azure (único ou agrupamento) |**Conjuntos elásticos**: para os clientes desenvolver novas aplicações da multi-inquilinos de SaaS ou intencionalmente transformar existente no local aplicações numa aplicação SaaS multi-inquilino, propor conjuntos elásticos. Benefícios deste modelo são: <br><ul><li>Conversão do modelo de negócio de vender licenças para vender serviço subscrições (ISVs)</li></ul><ul><li>Isolamento de inquilinos fácil e marca-prova</li></ul><ul><li>Um modelo de programação de base de dados centrada simplificado</li></ul><ul><li>O potencial de ampliar sem atingir um limite rígido</li></ul>**Único bases de dados**: para os clientes a desenvolver novas aplicações que não sejam SaaS multi-inquilino, cuja carga de trabalho é estáveis e previsíveis, propor a bases de dados individuais. Benefícios deste modelo são:<ul><li>Um modelo de programação de base de dados centrada simplificado</li></ul>  <ul><li>Desempenho previsível para cada base de dados</li></ul>|
|Máquina virtual IaaS do SQL Server|Para os clientes que necessitam personalizar o sistema operativo ou o servidor de base de dados, bem como os clientes ter requisitos específicos em termos de aplicações em execução por terceiros pelo lado com o SQL Server (na mesma VM), propor VMs de SQL / IaaS como a solução ideal|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Como identificar através de programação de uma instância geridos

A tabela seguinte mostra várias propriedades, acessíveis através do Transact SQL, que pode utilizar para detetar que a aplicação está a funcionar com instância geridos e obter as propriedades importantes.

|Propriedade|Valor|Comentário|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation.|Este valor é mesmo do base de dados SQL.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Este valor é mesmo do base de dados SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Este valor identifica exclusivamente geridos instância.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nome DNS de instância completa no seguinte formato:<instanceName>.<dnsPrefix>. Database.Windows.NET, onde <instanceName> é o nome fornecido pelo cliente, enquanto <dnsPrefix> é gerado automaticamente parte do nome guaranteeing global exclusividade de nome DNS ("wcus17662feb9ce98", por exemplo)|Exemplo: os meus-geridos-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>As principais funcionalidades e capacidades de uma instância geridos 

> [!IMPORTANT]
> Executa uma instância geridos com todas as funcionalidades da versão mais recente do SQL Server, incluindo operações online, as correções plano automática e outros melhoramentos de desempenho de empresa. 

| **Vantagens de PaaS** | **Continuidade do negócio** |
| --- | --- |
|Não é a compra de hardware e gestão <br>Nenhuma gestão de sobrecarga de gerir a infraestrutura subjacente <br>Aprovisionamento rápido e dimensionamento de serviço <br>Atualização de versão e de aplicação de patches automatizada <br>Integração com outros serviços de dados PaaS |SLA de 99,99% disponibilidade  <br>Incorporada na elevada disponibilidade <br>Dados protegidos com cópias de segurança automatizadas <br>Período de retenção de cópias de segurança configurável de cliente (fixo para pré-visualização pública de 7 dias) <br>Cópias de segurança iniciada pelo utilizador <br>Capacidade de restaurar ponto na base de dados do tempo |
|**Segurança e conformidade** | **Gestão**|
|Ambiente isolado (integração de VNet, service de inquilino único, cálculo dedicado e armazenamento <br>Encriptação de dados em trânsito <br>Autenticação do Azure AD, suporte de início de sessão único <br>Respeite as normas de conformidade mesmo como base de dados SQL do Azure <br>Auditoria do SQL <br>Deteção de ameaças |API de Gestor de recursos do Azure para automatizar o aprovisionamento de serviço e dimensionamento <br>Funcionalidade portal do Azure para o serviço manual de aprovisionamento e dimensionamento <br>Serviço de migração de dados 

![O início de sessão único](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model-preview"></a>com base em vCore modelo de compra (pré-visualização)

O modelo de compra baseado em vCore (pré-visualização) permite a flexibilidade, o controlo, a transparência uma forma simples de traduzir locais e requisitos de carga de trabalho para a nuvem. Este modelo permite-lhe dimensionar a computação, memória e armazenamento com base nas suas necessidades de carga de trabalho. O modelo de vCore também é elegível para cópia de segurança para as poupanças de 30 por cento com o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Um núcleo virtual representa a CPU lógica oferecida com a opção de escolha entre gerações de hardware.
- As CPUs Lógicas de Geração 4 baseiam-se nos processadores Intel E5-2673 v3 (Haswell) de 2,4 GHz.
- Gen 5 CPUs lógicas baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores.

A tabela seguinte ajuda-o a compreender como selecionar a configuração ideal do computação, memória, armazenamento e recursos de e/s.

||Geração 4|Geração 5|
|----|------|-----|
|Hardware|Intel E5-2673 v3 (Haswell) processadores 2.4 GHz, ligado SSD vCore = 1 PP (núcleos físicos)|Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores, rápido eNVM SSD, vCore = 1 LP (hyper thread)|
|Níveis de desempenho|8, 16, 24 vCores|8, 16, 24, 32, 40 vCores|
|Memória|7GB por vCore|5.5GB por vCore|
||||

## <a name="managed-instance-service-tier"></a>Geridos de camada de serviços de instância

Instância gerida está inicialmente disponível numa camada de serviço única - fins gerais - que foi concebida para aplicações com disponibilidade típica e requisitos de latência de e/s comuns.

A lista seguinte descreve uma característica das chave da camada de serviços de objetivo geral: 

- Design para a maioria das aplicações empresariais de desempenho típico e requisitos de HA 
- Armazenamento Premium do Azure de elevado desempenho (8 TB) 
- 100 bases de dados / instância 

Nesta camada, pode selecionar armazenamento e a capacidade de computação independentemente. 

O diagrama seguinte ilustra a capacidade de cálculo ativa e os nós redundantes neste escalão de serviço.
 
![Camada de serviço de objetivo geral](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

A seguir descreve as funcionalidades principais da camada de serviços de objetivo geral:

|Funcionalidade | Descrição|
|---|---|
| Número de vCores * | 8, 16, 24 (gen 4)<br>8, 16, 24, 32, 40 (Gen5)|
| Versão do SQL Server / compilar | SQL Server mais recente (disponível) |
| Tamanho de armazenamento mín. | 32 GB |
| Tamanho máximo de armazenamento | 8 TB |
| Armazenamento máximo por base de dados | 8 TB |
| Armazenamento esperado IOPS | 500-7500 IOPS por ficheiro de dados (depende o ficheiro de dados). Consulte [armazenamento Premium](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Número de ficheiros de dados (linhas) pela base de dados | Vários | 
| Número de ficheiros de registo (registo) por base de dados | 1 | 
| Gerido cópias de segurança automatizadas | Sim |
| HA | Com base no armazenamento remoto e [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Instância incorporada e monitorização de base de dados e métricas | Sim |
| Aplicação de patches de software automáticas | Sim |
| VNet - implementação do Azure Resource Manager | Sim |
| VNet - modelo de implementação clássica | Não |
| Suporte de portal | Sim|
|||

\* Um núcleo virtual representa a CPU lógica oferecida com a opção de escolha entre gerações de hardware. Gen 4 CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) processadores 2.4 GHz e Gen 5 lógica CPUs baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores. 

## <a name="advanced-security-and-compliance"></a>Segurança e conformidade avançadas 

### <a name="managed-instance-security-isolation"></a>Isolamento de segurança de instância gerido 

Instância gerida proporcionar o isolamento de segurança adicionais de outros inquilinos na nuvem do Azure. Isolamento de segurança inclui: 

- [Implementação de rede virtual nativo](sql-database-managed-instance-vnet-configuration.md) e a conectividade ao seu ambiente no local utilizando o Azure Express Route ou VPN Gateway 
- Ponto final do SQL Server está exposta apenas através de um endereço IP privado, permitindo que a conectividade segura do Azure privado ou redes híbridas
- Single-inquilino com a infraestrutura subjacente dedicada (computação, armazenamento)

O diagrama a seguir descreve a estrutura de isolamento: 

![Elevada disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

### <a name="auditing-for-compliance-and-security"></a>Auditoria para conformidade e segurança 

[Gerido instância auditoria](sql-database-managed-instance-auditing.md) controla da base de dados eventos e escreve-los para uma auditoria iniciar sessão na sua conta do storage do Azure. Auditoria pode ajudar a manter a conformidade de regulamentação, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que poderão indicar preocupações para a empresa ou suspeitas de violação de segurança. 

### <a name="data-encryption-in-motion"></a>Encriptação de dados ativa 

Instância gerida protege os seus dados ao fornecer a encriptação dos dados em movimento utilizando Transport Layer Security.

Além da segurança de camada de transporte, a instância de gerido da base de dados do SQL Server proporciona proteção de dados confidenciais em trânsito, inativos e durante a consulta de processamento com [sempre encriptados](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted é uma funcionalidade pioneira da indústria que oferece segurança de dados sem paralelo contra violações que envolvam o roubo de dados confidenciais. Por exemplo, com sempre encriptados, números de cartão de crédito são armazenados de forma encriptada na base de dados sempre, mesmo durante a consulta de processamento, permitindo a desencriptação at the point of utilização por aplicações que precisam para processar dados ou pessoal autorizado. 

### <a name="dynamic-data-masking"></a>Máscara de dados dinâmica 

Base de dados SQL [máscara de dados dinâmicos](/sql/relational-databases/security/dynamic-data-masking) limita a exposição de dados confidenciais através da máscara-lo para os utilizadores não privilegiados. Máscara de dados dinâmicos ajuda a impedir o acesso não autorizado aos dados confidenciais, permitindo-lhe designar a quantidade dos dados confidenciais para revelar com um impacto mínimo na camada da aplicação. É uma funcionalidade de segurança baseada em políticas que omite os dados confidenciais no conjunto de resultados de uma consulta em campos da base de dados designados, sendo que os dados na base de dados não são alterados. 

### <a name="row-level-security"></a>Segurança ao Nível da Linha 

[Ao nível da linha segurança](/sql/relational-databases/security/row-level-security) permite-lhe controlo de acesso a linhas numa tabela da base de dados com base nas características do utilizador executar uma consulta (tais como pelo contexto ou execução da associação de grupo). A segurança ao nível da linha (RLS) simplifica o design e a programação da segurança na sua aplicação. O RLS permite-lhe implementar restrições ao acesso à linha de dados. Por exemplo, garantindo que os funcionários podem aceder apenas as linhas de dados que são relevantes para o respetivo departamento ou restringir o acesso a dados a apenas os dados relevantes. 

### <a name="threat-detection"></a>Deteção de ameaças 

[Gerido a deteção de ameaças de instância](sql-database-managed-instance-threat-detection.md) complementa [geridos instância auditoria](sql-database-managed-instance-auditing.md) , fornecendo uma camada adicional de intelligence de segurança incorporada para o serviço que Deteta tentativas invulgares e potencialmente prejudiciais acesso ou exploração bases de dados. Se for alertado sobre atividades suspeitas, potenciais vulnerabilidades, e ataques de injeção de SQL, bem como os padrões de acesso de base de dados anómalas. Alertas de deteção de ameaças podem ser visualizadas [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) e forneça detalhes de atividade suspeita e recomendamos ação sobre a investigar e mitigar a ameaça.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integração do Azure Active Directory e a autenticação multifator 

A Base de Dados SQL permite-lhe gerir centralmente identidades de utilizadores de bases de dados e outros serviços Microsoft com a [integração do Azure Active Directory](sql-database-aad-authentication.md). Esta capacidade simplifica a gestão de permissões e melhora a segurança. O Azure Active Directory suporta a [autenticação multifator](sql-database-ssms-mfa-authentication-configure.md) (MFA), para aumentar a segurança de dados e aplicações, suportando, ao mesmo tempo, um processo de início de sessão único. 

### <a name="authentication"></a>Autenticação 
Autenticação do SQL Server da base de dados refere-se à forma como os utilizadores provar a sua identidade ao ligar à base de dados. A Base de Dados SQL suporta dois tipos de autenticação:  

- Autenticação do SQL Server, que utiliza um nome de utilizador e palavra-passe.
- Authentication do Azure Active Directory, que utiliza identidades geridas pelo Azure Active Directory e é suportada nos domínios geridos e integrados. 

### <a name="authorization"></a>Autorização

Autorização refere-se de que um utilizador pode fazê-lo dentro de uma base de dados do SQL do Azure e é controlado pelas permissões ao nível do objeto e as associações de função de base de dados da sua conta de utilizador. Instância gerida tem o mesmo capacidades de autorização como 2017 do SQL Server. 

## <a name="database-migration"></a>Migração de bases de dados 

Gerido cenários de utilizador de destinos de instância com a migração de base de dados em massa no local ou implementações de base de dados de IaaS. Gerido suporta instância várias opções de migração de base de dados: 

### <a name="data-migration-service"></a>Serviço de migração de dados

O serviço de migração de base de dados do Azure é um serviço completamente gerido concebido para permitir migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. Este serviço simplifica as tarefas necessárias para mover terceiros existente e bases de dados do SQL Server para o Azure. Opções de implementação incluem SQL Database do Azure, geridos instância e do SQL Server numa VM do Azure em pré-visualização pública. Consulte [como migrar a base de dados no local para a instância geridos utilizando o DMS](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Cópia de segurança e restauro  

A abordagem de migração tira partido das cópias de segurança do SQL Server para o armazenamento de Blobs do Azure. Cópias de segurança armazenadas no blob storage do Azure podem ser diretamente restauradas na instância geridos. Para restaurar uma base de dados existente do SQL Server para uma instância de gerido, pode:

- Utilize [o serviço de migração de dados (DMS)](/sql/dma/dma-overview). Para um tutorial, consulte [migrar para uma instância geridos utilizando o serviço de migração de base de dados do Azure (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados
- Utilize o [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Para um tutorial que mostra como restaurar Wide a World Importers - ficheiro de cópia de segurança da base de dados padrão, consulte [restaurar um ficheiro de cópia de segurança para uma instância geridos](sql-database-managed-instance-restore-from-backup-tutorial.md). Este tutorial mostra que tem de carregar um ficheiro de cópia de segurança para o armazenamento do blogue do Azure e segura-la utilizando uma chave de assinatura (SAS) de acesso partilhado.
  - Para obter informações sobre o restauro a partir do URL, consulte [restaurar nativo a partir do URL](sql-database-managed-instance-migrate.md#native-restore-from-url).
- [Importar de um ficheiro BACPAC](sql-database-import.md)

## <a name="sql-features-supported"></a>Funcionalidades do SQL Server suportadas 

Gerido objetivos de instância para fornecer próximo de compatibilidade de área de superfície de 100%, com futuras nas fases até que a disponibilidade geral do serviço de SQL Server no local. Para funcionalidades e a lista de comparação, consulte [funcionalidades comuns do SQL Server](sql-database-features.md).
 
Gerido instância suporta retrocompatibilidade para bases de dados do SQL Server 2008. É suportada a migração direta de servidores de base de dados do SQL Server 2005, nível de compatibilidade para bases de dados de SQL Server 2005 migrados são atualizados para o SQL Server 2008. 
 
O diagrama a seguir descreve a compatibilidade da área de superfície na instância geridos:  

![Migração](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Principais diferenças entre o SQL Server no local e instância geridos 

Gerido vantagens de instância de ser sempre-cópia de segurança-para-data na nuvem, o que significa que algumas funcionalidades do SQL Server no local poderão ser obsoleto, extinto ou ter alternativas. Existem casos específicos quando precisam de ferramentas reconhecer que um determinado recurso funciona de forma ligeiramente diferente ou que o serviço não está em execução num ambiente que não controlar totalmente: 

- Elevada disponibilidade é incorporada e pré-configurados. Funcionalidades de elevada disponibilidade Always On não são expostas de forma mesma porque esta está no implementações IaaS de SQL 
- As cópias de segurança automatizadas e ponto de restauro de tempo. Pode iniciar o cliente `copy-only` cópias de segurança que não interferem com a cadeia de cópia de segurança automática. 
- Gerido instância não permite a especificação de caminhos físicos completa para todos os cenários correspondentes tem de ser suportado de forma diferente: restaurar a base de dados não suporta com mover, criar a base de dados permite que os caminhos físicos, BULK INSERT funciona com Blobs do Azure apenas, etc. 
- Gerido suporta instância [autenticação do Azure AD](sql-database-aad-authentication.md) como alternativa de nuvem para a autenticação do Windows. 
- Instância gerida gere automaticamente XTP grupo de ficheiros e ficheiros de bases de dados que contém objetos de OLTP na memória
 
### <a name="managed-instance-administration-features"></a>Funcionalidades de administração de instância geridas  

Gerido instância ativar administrador de sistema focar-se naquilo que realmente interessa mais para empresas. Muitas atividades de administrador/DBA de sistema não são necessárias, ou estão simples. Por exemplo, SO / RDBMS instalação e a aplicação de patches, dinâmica instância redimensionamento e configuração, as cópias de segurança, replicação de base de dados (incluindo bases de dados do sistema), configuração de elevada disponibilidade e configuração de estado de funcionamento e dados de monitorização de desempenho fluxos. 

> [!IMPORTANT]
> Para obter uma lista das funcionalidades suportadas, parcialmente suportadas e não suportadas, consulte [funcionalidades da base de dados SQL](sql-database-features.md). Para obter uma lista das diferenças de T-SQL em instâncias geridas em comparação com o SQL Server, consulte [geridos diferenças de T-SQL de instância do SQL Server](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Passos Seguintes

- Para funcionalidades e a lista de comparação, consulte [funcionalidades comuns do SQL Server](sql-database-features.md).
- Para obter mais informações sobre a configuração da VNet, veja [Configuração de VNet de Instância Gerida](sql-database-managed-instance-vnet-configuration.md).
- Para um tutorial que cria uma instância geridos e restaura uma base de dados a partir de um ficheiro de cópia de segurança, consulte [criar uma instância geridos](sql-database-managed-instance-create-tutorial-portal.md).
- Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md).
