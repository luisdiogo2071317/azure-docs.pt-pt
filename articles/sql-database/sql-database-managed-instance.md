---
title: Descrição geral de instância de gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este tópico descreve uma instância de gerida de base de dados do Azure SQL e explica como funciona e como isso é diferente de uma base de dados na base de dados do Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: e94b9e6d39a8a2694658a4231c54a027523af10c
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52889247"
---
# <a name="use-sql-database-managed-instance-with-virtual-networks-and-near-100-compatibility"></a>Utilizar a instância gerida do SQL da base de dados com as redes virtuais e quase 100% de compatibilidade

Instância de gerida de base de dados de SQL do Azure é um novo modelo de implementação do Azure SQL Database, fornecendo quase 100% de compatibilidade com o mais recente do SQL Server no local (Enterprise Edition) motor de base de dados, fornecendo um nativo [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) implementação que resolva problemas de segurança comuns, e um [modelo de negócio](https://azure.microsoft.com/pricing/details/sql-database/) favoráveis para clientes de SQL Server no local. Instância gerida permite que os clientes existentes do SQL Server levantar e deslocar as suas aplicações no local para a cloud com alterações mínimas de aplicativo e a base de dados. Ao mesmo tempo, a instância gerida preserva todos os recursos de PaaS (atualizações automáticas de aplicação de patches e de versão, [cópias de segurança automatizadas](sql-database-automated-backups.md), [elevada disponibilidade](sql-database-high-availability.md) ), que reduz significativamente a sobrecarga de gerenciamento e o custo total de propriedade.

> [!IMPORTANT]
> Para obter uma lista de regiões em que a instância gerida está atualmente disponível, consulte [regiões suportadas](sql-database-managed-instance-resource-limits.md#supported-regions).

O diagrama seguinte descreve as principais funcionalidades de instância gerida:

![principais recursos](./media/sql-database-managed-instance/key-features.png)

Instância de gerida de base de dados de SQL do Azure foi concebida para os clientes que pretendem para migrar um grande número de aplicações de IaaS, personalizada criada, ou no local ou ISV fornecido ambiente para o ambiente de nuvem de PaaS totalmente gerido, com como esforço de migração de baixa possível. Utilizar totalmente automatizada [serviço de migração de dados (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) no Azure, os clientes podem lift- and -shift seu SQL Server no local para uma instância gerida, que oferece compatibilidade com SQL Server no local e completo isolamento de instâncias de cliente com suporte nativo a VNet.  Com o Software Assurance, pode trocar suas licenças existentes para tarifas com desconto numa instância gerida do SQL da base de dados utilizando o [benefício híbrido do SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Instância gerida do SQL da base de dados é o melhor destino de migração na cloud para instâncias do SQL Server que requerem segurança elevada e uma superfície de programação avançado.

Ao disponibilidade geral, a instância gerida tem como objetivo fornecer próximo de compatibilidade de área de superfície de 100% com a última versão de SQL Server no local através de um plano de versão em etapas.

Para decidir entre o Azure SQL da base de dados única base de dados, instância gerida da base de dados SQL do Azure e alojada no consulte de Máquina Virtual de IaaS do SQL Server [como escolher a versão correta do SQL Server na cloud do Azure](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Principais funcionalidades e capacidades

Instância de gerida de base de dados de SQL do Azure combina os melhores recursos que estão disponíveis no Azure SQL Database e o motor de base de dados do SQL Server.

> [!IMPORTANT]
> Executa uma instância gerida com todas as funcionalidades da versão mais recente do SQL Server, incluindo operações online, correções de planos automática e outros aprimoramentos de desempenho do enterprise. Uma comparação dos recursos disponíveis é explicada na [comparação de funcionalidades: base de dados do Azure SQL em comparação com o SQL Server](sql-database-features.md).

| **Vantagens de PaaS** | **Continuidade do negócio** |
| --- | --- |
|Sem comprar hardware e gestão <br>Nenhuma sobrecarga de gerenciamento para gerir a infraestrutura subjacente <br>Aprovisionamento rápido e dimensionamento do serviço <br>Atualização de aplicação de patches e versão automatizada <br>Integração com outros serviços de dados de PaaS |tempo de atividade de 99,99% SLA  <br>Incorporado [elevada disponibilidade](sql-database-high-availability.md) <br>Os dados protegidos com [cópias de segurança automatizadas](sql-database-automated-backups.md) <br>Período de retenção de cópia de segurança configuráveis do cliente <br>Iniciado pelo utilizador [cópias de segurança](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>[Ponto de restauro de base de dados de tempo](sql-database-recovery-using-backups.md#point-in-time-restore) capacidade |
|**Segurança e conformidade** | **Gestão**|
|Ambiente isolado ([integração VNet](sql-database-managed-instance-vnet-configuration.md)único inquilino de serviço, dedicada de computação e armazenamento) <br>[Encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Autenticação do Azure AD](sql-database-aad-authentication.md), único suporte de início de sessão <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Inícios de sessão do Azure AD</a> (**pré-visualização pública**) <br>Cumpre as normas de conformidade mesmo como base de dados SQL do Azure <br>[Auditoria de SQL](sql-database-managed-instance-auditing.md) <br>[Deteção de ameaças](sql-database-managed-instance-threat-detection.md) |API do Resource Manager do Azure para automatizar o serviço de aprovisionamento e dimensionamento <br>Funcionalidade de portal do Azure para o serviço de aprovisionamento e dimensionamento manual <br>Serviço de migração de dados

Os principais recursos de instância gerida são mostrados na tabela a seguir:

|Funcionalidade | Descrição|
|---|---|
| Versão do SQL Server / criar | Motor de base de dados do SQL Server (estável mais recente) |
| Gerido cópias de segurança automáticas | Sim |
| Instância incorporada e monitorização de base de dados e métricas | Sim |
| Aplicação de patches de software automáticas | Sim |
| As funcionalidades do motor de base de dados mais recentes | Sim |
| Número de ficheiros de dados (linhas) pela base de dados | Vários |
| Número de ficheiros de registo (registo) por base de dados | 1 |
| VNet - implementação do Azure Resource Manager | Sim |
| VNet - modelo de implementação clássica | Não |
| Suporte do portal | Sim|
| Serviço de integração incorporada (SSIS) | Não, o SSIS é uma parte do [PaaS de fábrica de dados do Azure](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Serviço de análise incorporada (SSAS) | Não, o SSAS está separado [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Serviço de relatórios incorporado (SSRS) | Não, utilizar o Power BI ou do IaaS do SSRS |
|||

## <a name="vcore-based-purchasing-model"></a>Modelo de compras baseado em vCore

O [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) na instância gerida dá-lhe flexibilidade, controlo, transparência e uma forma direta de traduzir os requisitos de carga de trabalho no local para a cloud. Este modelo permite-lhe alterar a computação, memória e armazenamento com base nas suas necessidades de carga de trabalho. O modelo de vCore também é elegível para a economia de 30 por cento com o [benefício híbrido do SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

No modelo de vCore, pode escolher entre gerações de hardware.

- **Geração 4** CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) físicas de processadores de 2,4 GHz, SSD anexado, núcleos, 7 GB de RAM por núcleo e tamanhos de computação entre 8 e 24 vCores.
- **Fins gerais 5** CPUs lógicas são baseados em Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores, rápida eNVM SSD, core com hyper-threading de lógica e tamanhos entre 8 e 80 núcleos de computação.

Encontrar mais informações sobre a diferença entre gerações de hardware no [limites de recursos de instância gerida](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

## <a name="managed-instance-service-tiers"></a>Gerido escalões de serviço da instância

Instância gerida está disponível em dois escalões de serviço:

- **Fins gerais**: concebido para aplicações com o desempenho típico e requisitos de latência de e/s.
- **Crítico para a empresa**: concebido para aplicações com requisitos de latência baixos e/s e o mínimo impacto das operações de manutenção subjacente na carga de trabalho.

Ambas as camadas de serviço garantem 99,99% de disponibilidade e permitem-lhe selecionar o tamanho de armazenamento e a capacidade de computação de forma independente. Para obter mais informações sobre a arquitetura de elevada disponibilidade da base de dados do Azure SQL, consulte [elevada disponibilidade e a base de dados do Azure SQL](sql-database-high-availability.md).

### <a name="general-purpose-service-tier"></a>Escalão de serviço de fins geral

A lista seguinte descreve característica-chave da camada de serviços de fins gerais:

- Design para a maioria dos aplicativos de negócios com requisitos de desempenho típico
- Armazenamento Premium do Azure de alto desempenho (8 TB)
- Incorporado [elevada disponibilidade](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) com base em fiável o armazenamento Premium do Azure e [do Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Para obter mais informações, consulte [em geral de armazenamento camada escalão fins](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) e [práticas recomendadas de armazenamento e as considerações para o Azure SQL DB instância gerida (fins gerais)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Encontrar mais informações sobre a diferença entre os escalões de serviço no [limites de recursos de instância gerida](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio

Camada de serviços críticos de negócios foi concebida para aplicações com requisitos de e/s elevados. Ele oferece maior resiliência a falhas com várias réplicas isoladas.

A lista seguinte descreve as principais características da camada de serviços críticos de negócio:

- Concebido para aplicações de negócio com mais elevado desempenho e requisitos de HA
- É fornecido com o armazenamento SSD extremamente rápido (até 1 TB em Gen 4 e até 4 TB nos fins 5)
- Incorporado [elevada disponibilidade](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) com base nos [grupos de Disponibilidade AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) e [do Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Incorporado adicional [réplica de base de dados só de leitura](sql-database-read-scale-out.md) que pode ser utilizado em relatórios e outras cargas de trabalho só de leitura
- [OLTP dentro da memória](sql-database-in-memory.md) que podem ser utilizados para a carga de trabalho com requisitos de alto desempenho  

Encontrar mais informações sobre a diferença entre os escalões de serviço no [limites de recursos de instância gerida](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="advanced-security-and-compliance"></a>Segurança e conformidade avançadas

Instância de gerida de base de dados de SQL do Azure combina os recursos avançados de segurança fornecidos pela cloud do Azure e o motor de base de dados do SQL Server.

### <a name="managed-instance-security-isolation-in-azure-cloud"></a>Gerido isolamento de segurança de instância na cloud do Azure

Instância gerida fornecem isolamento de segurança adicionais de outros inquilinos na nuvem do Azure. Isolamento de segurança inclui:

- [Implementação de rede virtual nativo](sql-database-managed-instance-vnet-configuration.md) e a conectividade ao seu ambiente no local com o Express Route do Azure ou o Gateway de VPN
- Ponto final SQL é exposto apenas por meio de um endereço IP privado, que permite conectividade segura do Azure privado ou redes híbridas
- Inquilino único com a infraestrutura subjacente dedicada (computação, armazenamento)

O diagrama seguinte descreve várias opções de conectividade para as suas aplicações:

![elevada disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Para obter mais detalhes sobre a integração de VNet e a imposição de políticas ao nível da sub-rede de rede, consulte [configurar uma VNet para a instância gerida da base de dados SQL do Azure](sql-database-managed-instance-vnet-configuration.md) e [ligar a sua aplicação à base de dados do Azure SQL Instância gerida](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Coloca a instância gerida vários na mesma sub-rede, onde quer que o que é permitido por seus requisitos de segurança, uma vez que irá fornecer benefícios adicionais. Instâncias collocating na mesma sub-rede irão significativamente simplificar a manutenção da infraestrutura de rede e reduzir a instância do provisionamento de tempo, uma vez que o aprovisionamento de longa duração está associado com o custo da instância gerida de primeira implementação numa sub-rede.

### <a name="azure-sql-database-security-features"></a>Recursos de segurança de base de dados SQL do Azure

Base de dados SQL do Azure fornece um conjunto de funcionalidades de segurança avançadas que podem ser utilizadas para proteger os seus dados.

- [Geridos a auditoria de instância](sql-database-managed-instance-auditing.md) regista os eventos de base de dados e escreve-as para um arquivo de log de auditoria colocado na sua conta de armazenamento do Azure. A auditoria pode ajudar a manter a conformidade regulamentar, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que podem indicar preocupações empresariais ou suspeitas de violações de segurança.
- Encriptação de dados em movimento - instância gerida protege os seus dados ao fornecer encriptação de dados em movimento com o Transport Layer Security. Além da segurança de camada de transporte, a instância de gerida da base de dados do SQL oferece proteção de dados confidenciais em movimento, inativos e durante o processamento de consultas com [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted é uma funcionalidade pioneira da indústria que oferece segurança de dados sem paralelo contra violações que envolvam o roubo de dados confidenciais. Por exemplo, com o Always Encrypted, os números de cartão de crédito são armazenados encriptados na base de dados sempre, mesmo durante o processamento de consultas, permitindo a desencriptação no uso por equipas autorizadas ou das aplicações que têm de processar esses dados.
- [Deteção de ameaças](sql-database-managed-instance-threat-detection.md) complementa [auditoria de instância gerida](sql-database-managed-instance-auditing.md) tentativas, fornecendo uma camada adicional de segurança de acesso ou exploração de inteligência incorporada no serviço que Deteta invulgares e potencialmente prejudiciais bases de dados. É alertado sobre atividades suspeitas, potenciais vulnerabilidades, e ataques de injeção de SQL, bem como os padrões de acesso de base de dados anómalas. Alertas de deteção de ameaças podem ser visualizadas [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) e fornecer detalhes de atividade suspeita e a ação sobre como investigar e mitigar a ameaça recomendada.  
- [Máscara de dados dinâmicos](/sql/relational-databases/security/dynamic-data-masking) limita a exposição de dados confidenciais ao mascará-los para utilizadores sem privilégios. Máscara de dados dinâmica ajuda a impedir o acesso não autorizado a dados confidenciais ao permitir-lhe designar a quantidade de dados confidenciais a revelar com um impacto mínimo na camada da aplicação. É uma funcionalidade de segurança baseada em políticas que omite os dados confidenciais no conjunto de resultados de uma consulta em campos da base de dados designados, sendo que os dados na base de dados não são alterados.
- [Segurança de nível de linha](/sql/relational-databases/security/row-level-security) permite-lhe controlo de acesso às linhas numa tabela de base de dados com base nas características do utilizador que executa uma consulta (por exemplo, com o contexto de associação ou execução de grupo). A segurança ao nível da linha (RLS) simplifica o design e a programação da segurança na sua aplicação. O RLS permite-lhe implementar restrições ao acesso à linha de dados. Por exemplo, garantindo que os funcionários possam acessar apenas as linhas de dados que são pertinentes para o departamento deles ou restringir o acesso a dados a apenas os dados relevantes.
- [Encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) encripta os ficheiros de dados de instância gerida da base de dados SQL do Azure, conhecidos como encriptar dados inativos. TDE realiza a encriptação em tempo real de e/s e a descriptografia dos ficheiros de dados e de registo. A encriptação utiliza uma chave de encriptação de base de dados (DEK), que é armazenada no registo de arranque da base de dados de disponibilidade durante a recuperação. Pode proteger todos os bancos de dados na instância gerida com encriptação de dados transparente. TDE é a do SQL Server comprovada tecnologia de encriptação em repouso, que é necessário por muitas normas de conformidade para proteger contra o roubo de mídias de armazenamento.

Migração de uma base de dados encriptado para a instância gerida de SQL é suportada através do Azure Database Migration Service (DMS) ou o restauro nativo. Se planear migrar base de dados encriptado através de restauro nativo, a migração do certificado TDE existente do SQL Server no local ou VM do SQL Server para instância gerida é uma etapa necessária. Para obter mais informações sobre as opções de migração, consulte [migração de instância do SQL Server para instância gerida da base de dados SQL do Azure](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Integração do Azure Active Directory

Instância de gerida de base de dados de SQL do Azure suporta o tradicional logons de motor de base de dados do SQL server e inícios de sessão integrados com o Azure Active Directory (AAD). Inícios de sessão do AAD (**pré-visualização pública**) têm a versão de cloud do Azure no local inícios de sessão da base de dados que está a utilizar no seu ambiente no local. Inícios de sessão do AAD permite-lhe especificar os utilizadores e grupos do seu inquilino do Azure Active Directory como verdadeiros no âmbito do instância principais, capazes de realizar qualquer operação de nível de instância, incluindo consultas entre bases de dados dentro da instância gerida do mesmo.

Uma nova sintaxe é introduzida para criar inícios de sessão do AAD (**pré-visualização pública**), **do fornecedor externo**. Para obter mais informações sobre a sintaxe, consulte <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>e reveja a [aprovisionar um administrador do Azure Active Directory da sua instância gerida](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) artigo.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integração do Azure Active Directory e a autenticação multifator

Instância gerida permite-lhe gerir centralmente identidades de utilizador de base de dados e outros serviços da Microsoft com [integração do Azure Active Directory](sql-database-aad-authentication.md). Esta capacidade simplifica a gestão de permissões e melhora a segurança. O Azure Active Directory suporta a [autenticação multifator](sql-database-ssms-mfa-authentication-configure.md) (MFA), para aumentar a segurança de dados e aplicações, suportando, ao mesmo tempo, um processo de início de sessão único.

### <a name="authentication"></a>Autenticação

Autenticação diz respeito à forma como os utilizadores provam a sua identidade quando se liga à base de dados de instância de gerida. A Base de Dados SQL suporta dois tipos de autenticação:  

- **Autenticação do SQL**:

  Este método de autenticação utiliza um nome de utilizador e palavra-passe.
- **O Azure Active Directory Authentication**:

  Este método de autenticação utiliza identidades geridas pelo Azure Active Directory e é suportado para domínios geridos e integrados. Utilize a autenticação do Active Directory (segurança integrada) [sempre que possível](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

### <a name="authorization"></a>Autorização

Autorização diz respeito ao que um utilizador pode fazer dentro de uma base de dados do SQL do Azure e é controlado pelas associações das funções de base de dados de sua conta de utilizador e permissões ao nível do objeto. A instância gerida tem as mesmas capacidades de autorização como SQL Server 2017.

## <a name="database-migration"></a>Migração de bases de dados

Gerido cenários de usuário de destinos de instância com a migração de base de dados em massa de implementações de base de dados de IaaS ou no local. Gerida suporta instância várias opções de migração de base de dados:

### <a name="back-up-and-restore"></a>Criar cópias de segurança e restauro  

A abordagem de migração tira partido de cópias de segurança SQL para o armazenamento de Blobs do Azure. As cópias de segurança armazenadas no blob de armazenamento do Azure podem ser diretamente restauradas na instância gerida utilizando o [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current).

- Para um início rápido que mostra como restaurar Wide a World Importers - ficheiro de cópia de segurança da base de dados padrão, consulte [restaurar um ficheiro de cópia de segurança para uma instância gerida](sql-database-managed-instance-get-started-restore.md). Este início rápido mostra de que tem de carregar um ficheiro de cópia de segurança para o armazenamento do blogue do Azure e seguro mesmo com uma chave de assinatura (SAS) de acesso partilhado.
- Para obter informações sobre o restauro a partir do URL, consulte [nativo restaurar a partir do URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Cópias de segurança de uma instância gerida só podem ser restauradas para a outra instância gerida. Não é possível restaurar para um servidor de SQL no local ou para uma base de dados do Azure SQL server lógico individual ou agrupada base de dados.

### <a name="data-migration-service"></a>Serviço de migração de dados

O serviço de migração de base de dados do Azure é um serviço completamente gerido criado para ativar migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. Este serviço simplifica as tarefas necessárias para mover existente de terceiros e bases de dados do SQL Server para o Azure SQL Database (base de dados, conjuntos elásticos e a instância gerida) e o SQL Server na VM do Azure. Ver [como migrar a sua base de dados no local para a instância gerida com o DMS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Funcionalidades do SQL suportadas

Gerido tem como objetivo instância entregar próximo de compatibilidade de área de superfície de 100% com a chegar em fases até disponibilidade geral do serviço de SQL Server no local. Para funcionalidades e lista de comparação, veja [comparação de funcionalidades de base de dados SQL](sql-database-features.md)e para obter uma lista de diferenças do T-SQL em instâncias geridas em comparação com o SQL Server, consulte [diferenças de T-SQL de instância gerida do SQL Server](sql-database-managed-instance-transact-sql-information.md).

Gerido instância compatibilidade com versões anteriores do oferece suporte para bases de dados do SQL 2008. É suportada a migração direta de servidores de base de dados do SQL 2005, o nível de compatibilidade para bases de dados do SQL 2005 migrados são atualizadas para o SQL 2008.
  
O diagrama seguinte descreve a compatibilidade de área de superfície na instância gerida:  

![Migração](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Principais diferenças entre o SQL Server no local e a instância gerida

Gerido benefícios de instância de ser sempre up-até à data na nuvem, o que significa que algumas funcionalidades do SQL Server no local podem ser obsoletos, extinto ou tem alternativas. Existem casos específicos ao ferramentas precisam reconhecer que um determinado recurso funciona de forma ligeiramente diferente ou que o serviço não está em execução num ambiente, que não controlar totalmente:

- Elevada disponibilidade incorporada no e previamente configuradas usando tecnologia semelhante [grupos de Disponibilidade AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).
- Cópias de segurança automáticas e o ponto no tempo. Pode iniciar o cliente `copy-only` cópias de segurança que não interferem com a cadeia de cópia de segurança automática.
- Instância gerida não permite a especificação de caminhos físicos completa para todos os cenários correspondentes tenham de ser suportadas de forma diferente: Restaurar DB não suporta a WITH MOVE, criar DB não permite caminhos físicos, a inserção em massa funciona com Blobs do Azure, apenas, etc.
- Suporta a instância de geridos [autenticação do Azure AD](sql-database-aad-authentication.md) como alternativa de cloud para a autenticação do Windows.
- Instância gerida gere automaticamente o grupo de ficheiros XTP e ficheiros de bases de dados que contém objetos de OLTP dentro da memória
- Instância gerida suporta SQL Server Integration Services (SSIS) e pode anfitrião catálogo SSIS (SSISDB) que armazena os pacotes do SSIS, mas eles são executados num gerido do Azure-SSIS Integration Runtime (IR) no Azure Data Factory (ADF), consulte [Create Runtime de integração Azure-SSIS no ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Para comparar os recursos do SSIS na base de dados SQL e a instância gerida, veja [servidor lógico de comparar a base de dados do SQL e a instância gerida](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance).

### <a name="managed-instance-administration-features"></a>Funcionalidades de administração de instância geridas

Geridos pelo administrador de sistema de ativação de instância para se focar no que é importante ao máximo para empresas. Muitas atividades de administrador/DBA do sistema não são necessárias, ou eles são simples. Por exemplo, o sistema operacional / instalação RDBMS e aplicação de patches, dinâmica instância redimensionamento e configuração, as cópias de segurança, [replicação de base de dados](replication-with-sql-database-managed-instance.md) (incluindo bases de dados do sistema), configuração de elevada disponibilidade e configuração do Estado de funcionamento e [monitorização do desempenho](../azure-monitor/insights/azure-sql.md) fluxos de dados.

> [!IMPORTANT]
> Para obter uma lista dos recursos suportados, parcialmente suportadas e não suportados, consulte [funcionalidades de base de dados SQL](sql-database-features.md). Para obter uma lista de diferenças do T-SQL em instâncias geridas em comparação com o SQL Server, consulte [diferenças de T-SQL de instância gerida do SQL Server](sql-database-managed-instance-transact-sql-information.md)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Como identificar programaticamente uma instância gerida

A tabela seguinte mostra várias propriedades, acessíveis através de Transact SQL, que pode utilizar para detetar se a aplicação está a funcionar com a instância gerida e obter propriedades importantes.

|Propriedade|Valor|Comentário|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation.|Este valor é a mesmo como na base de dados SQL.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Este valor é a mesmo como na base de dados SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Este valor identifica exclusivamente a instância gerida.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nome DNS de instância completa no seguinte formato:`<instanceName>`.`<dnsPrefix>`.Database.Windows.NET, onde `<instanceName>` é o nome fornecido pelo cliente, enquanto `<dnsPrefix>` é gerado automaticamente parte do mesmo garantindo a exclusividade de nome DNS global ("wcus17662feb9ce98", por exemplo)|Exemplo: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Passos Seguintes

- Para saber como criar a sua primeira instância gerida, veja [guia de início rápido](sql-database-managed-instance-get-started.md).
- Para funcionalidades e lista de comparação, veja [recursos comuns de SQL](sql-database-features.md).
- Para obter mais informações sobre a configuração da VNet, veja [Configuração de VNet de Instância Gerida](sql-database-managed-instance-vnet-configuration.md).
- Para um início rápido que cria uma instância gerida e restaura uma base de dados a partir de um ficheiro de cópia de segurança, consulte [criar uma instância gerida](sql-database-managed-instance-get-started.md).
- Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para avançadas de monitorização de desempenho da base de dados de instância gerida com inteligência incorporada de resolução de problemas, consulte [SQL Database do Azure Monitor, através da análise de SQL do Azure](../azure-monitor/insights/azure-sql.md)
- Para obter informações sobre preços, consulte [preços da SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
