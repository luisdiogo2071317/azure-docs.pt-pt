---
title: "Gerir após a migração - SQL Database do Azure | Microsoft Docs"
description: "Saiba como gerir a sua base de dados após a migração para a SQL Database do Azure."
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/06/2016
ms.author: Joe.Sack
ms.suite: sql
ms.prod_service: sql-database
ms.component: migration
ms.openlocfilehash: b65236fb2d11473d626ee2602237ed4a49380702
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="new-dba-in-the-cloud--managing-your-database-in-azure-sql-database"></a>Novo DBA na nuvem – gerir a sua base de dados SQL Database do Azure

Mover o tradicionais de automática gerido, ambiente controlado personalizada para um ambiente de PaaS pode parecer um pouco complicado em primeiro lugar. Como um programador de aplicações ou DBA, iria querer saber as capacidades centrais da plataforma que iria ajudar a manter a sua aplicação disponível, performant, segura e que sejam resiliente - sempre. Este artigo visa fazê-lo exatamente. O artigo succinctly organiza recursos e dá-lhe algumas orientações sobre a melhor utilização principais funcionalidades da base de dados do SQL Server para gerir e manter a sua aplicação em execução de forma eficiente e obter os melhores resultados na nuvem. Típico público-alvo para este artigo seria aqueles que:-
- Estiver a avaliar a migração da sua aplicação (ões) para a BD SQL do Azure – Modernizing a sua aplicação (ões).
- São no processo de migrar as respetivas aplicações – o cenário de migração em curso.
- Recentemente concluiu a migração para a BD SQL do Azure – DBA novos na nuvem.

Este artigo descreve algumas das características principais da BD SQL do Azure como uma plataforma que prontamente podem tirar partido. Estes são os seguintes:- 
- Recuperação de continuidade e desastre de negócio (BCDR)
- Segurança e conformidade
- Monitorização de base de dados inteligente e manutenção
- Movimento de dados


## <a name="business-continuity-and-disaster-recovery-bcdr"></a>Recuperação de continuidade e desastre de negócio (BCDR)
Capacidades de recuperação de continuidade e desastre de negócio permitem-lhe continuar a sua empresa, como habitualmente, em caso de desastre. Desastre pode ser um evento de nível de base de dados (por exemplo, alguém por engano ignora uma tabela fundamental) ou um evento de nível de dados centre (catastrophe regional, por exemplo um multiplex). 

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Como criar e gerir cópias de segurança na base de dados SQL?
Não criar cópias de segurança na BD SQL do Azure e que é porque não tem. Base de dados SQL automaticamente efetua cópias de segurança bases de dados para si, para que já não tem preocupar agendamento, a e gerir cópias de segurança. A plataforma demora uma cópia de segurança completa todas as semanas, cópia de segurança em poucas horas e um registo de cópia de segurança a cada 5 minutos para garantir a recuperação de desastre é eficiente e a mínimo de perda de dados diferencial. A primeira cópia de segurança completa ocorre assim que criar uma base de dados. Estas cópias de segurança estão disponíveis para si para um determinado período chamado "Período de retenção" e varia de acordo com a camada de desempenho que escolher.  Base de dados do SQL Server fornece-lhe a capacidade de restaurar para qualquer ponto no tempo nesta utilizando do período de retenção [ponto no tempo de recuperação (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore).

|Camada de desempenho|Período de retenção em dias|
|---|:---:|
|Básica|7|
|Standard|35|
|Premium|35|
|||

Além disso, o [retenção de longo prazo (imediatamente disponíveis)](sql-database-long-term-retention.md) funcionalidade permite-lhe manter para os ficheiros de cópia de segurança durante um período de tempo muito especificamente, para até 10 anos e restaurar dados a partir estas cópias de segurança em momento algum durante esse período. Além disso, as cópias de segurança da base de dados são mantidas no armazenamento de georreplicação para garantir que a resiliência da regional catastrophe. Também pode restaurar estas cópias de segurança em qualquer região do Azure em qualquer ponto de tempo dentro do período de retenção. Consulte [descrição geral da continuidade do negócio](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Como posso assegurar a continuidade do negócio na eventualidade de ocorrer um desastre ao nível do Centro de dados ou regional catastrophe?
Porque as cópias de segurança da base de dados são armazenadas num subsistema de armazenamento de georreplicação para garantir que em caso de desastre regional, pode restaurar a cópia de segurança noutra região do Azure. Esta opção é denominada georrestauro. O RPO (objetivo de ponto de recuperação) para isto é, geralmente, < 1 hora e a ERT (estimado tempo de recuperação – alguns minutos para horas).

Para bases de dados fundamentais, BD SQL do Azure oferece, georreplicação ativa. Que isto essencialmente faz é que cria uma cópia secundária georreplicação da base de dados original noutra região. Por exemplo, se a base de dados inicialmente está alojado na região do Azure EUA oeste e pretender resiliência desastre regional. Poderá criar uma réplica da base de dados de georreplicação ativa nos EUA oeste para indicar EUA Leste. Quando o calamity strikes nos EUA oeste, pode ativação pós-falha para a região EUA Leste. A configurar um grupo de ativação de pós-falha automática é ainda mais porque Isto garante que a base de dados automaticamente a ativação pós-falha para o secundário nos EUA Leste em caso de desastre. O RPO para isto é < 5 segundos e os segundos do ERT < 30.

Se não estiver configurado um grupo de ativação de pós-falha automática, em seguida, a aplicação tem ativamente monitorizar um desastre e inicie uma ativação pós-falha para o secundário. Pode criar até 4 esses Active Directory-réplicas geo em diferentes regiões do Azure. Obtém ainda mais. Também pode aceder a estes Active Directory georreplicação-réplicas secundárias para acesso só de leitura. Esta informação ser proveniente muito útil para reduzir a latência para um cenário de aplicação geo-distribuição. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Como meu plano de recuperação após desastre alterados no local para a base de dados SQL?
Em resumo, o programa de configuração do SQL Server tradicional no local necessário ativamente e gerir a disponibilidade, utilizando as funcionalidades, tais como Clustering de ativação pós-falha, o espelhamento da base de dados, replicação de transações, etc. de envio do registo e manter e gerir cópias de segurança para garantir que Continuidade do negócio. Com base de dados do SQL Server, a plataforma gere estes para si, para que possa concentrar-se por desenvolver e otimizar a sua aplicação de base de dados e não se preocupe sobre a gestão de desastre máximo. Pode fazer cópia de segurança e os planos de recuperação após desastre configurados e trabalhar com apenas alguns cliques no portal do Azure (ou alguns comandos utilizando as APIs do PowerShell). 

Para saber mais sobre a recuperação após desastre, consulte: [101 de recuperação de desastre do Azure SQL Db](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Segurança e conformidade
Base de dados SQL demora segurança e privacidade é muito importante que. Segurança numa base de dados SQL está disponível ao nível da base de dados e o nível de plataforma e melhor entendida quando categorizadas em várias camadas. Em cada camada depara-se controlar e fornecer segurança ideal para a sua aplicação. As camadas são:
- Serviços de identidade e autenticação ([autenticação do Windows/SQL e a autenticação do Azure Active Directory [AAD]](sql-database-control-access.md)).
- Atividade de monitorização ([auditoria](sql-database-auditing.md) e [deteção de ameaças](sql-database-threat-detection.md)).
- Proteger dados real ([encriptação transparente de dados [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) e [sempre encriptados [AE]](/sql/relational-databases/security/encryption/always-encrypted-database-engine)). 
- Controlar o acesso a dados confidenciais e com privilégios ([segurança ao nível da linha](/sql/relational-databases/security/row-level-security) e [máscara de dados dinâmicos](/sql/relational-databases/security/dynamic-data-masking)).

[Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) oferece gestão de segurança centralizada em cargas de trabalho em execução no Azure, no local e em outros nuvens. Pode ver se essencial proteção de base de dados SQL, tais como [auditoria](sql-database-auditing.md) e [encriptação transparente de dados [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) estão configurados em todos os recursos e criar políticas com base nos seus requisitos.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>Os métodos de autenticação de utilizador são fornecidos na base de dados SQL?
Existem [dois métodos de autenticação](sql-database-control-access.md#authentication) oferecido na base de dados SQL: 
- [Autenticação do Active Directory do Azure](sql-database-aad-authentication.md)
- Autenticação do SQL Server. 

Não é suportada a autenticação do windows tradicional. Azure Active Directory (AD) é um serviço de gestão de identidades e acessos centralizado. Com esta muito comodamente pode fornecer um único início de sessão acesso (SSO) a todos os empregados na sua organização. Isto significa que as credenciais são partilhadas entre todos os serviços do Azure para a autenticação mais simples. Suporta o AAD [MFA (Multi-factor Authentication)](sql-database-ssms-mfa-authentication.md) e um com uma [apenas alguns cliques](../active-directory/connect/active-directory-aadconnect-get-started-express.md) AAD pode ser integrado com o Windows Server Active Directory. A autenticação do SQL Server funciona exatamente da mesma forma que tiver sido utilizá-la no passado. Forneça uma nome de utilizador/palavra-passe e pode autenticar utilizadores para uma base de dados num determinado servidor lógico. Isto também permite que a base de dados do SQL Server e o SQL Data Warehouse oferecem autenticação multifator e contas de utilizador convidado dentro de um domínio do Azure AD. Se já tiver o Active Directory no local, podem federar o diretório no Azure Active Directory para expandir o seu diretório no Azure.

|**Se tiver...**|**Base de dados SQL / do armazém de dados do SQL Server**|
|---|---|
|Prefere para não utilizar o Azure Active Directory (AD) no Azure|Utilize [autenticação SQL](sql-database-security-overview.md)|
|AD utilizado no SQL Server no local|[Federar AD com o Azure AD](../active-directory/connect/active-directory-aadconnect.md)e utilizar a autenticação do Azure AD. Com esta opção, pode utilizar o início de sessão único.|
|Têm de impôr a autenticação multifator (MFA)|Exigir a MFA como uma política através de [acesso condicional do Microsoft](sql-database-conditional-access.md)e utilizar [Universal do Azure AD authentication com suporte MFA](sql-database-ssms-mfa-authentication.md).|
|Ter contas de convidado de contas Microsoft (live.com, outlook.com) ou de outros domínios (gmail.com)|Utilize [autenticação do Azure AD Universal](sql-database-ssms-mfa-authentication.md) no armazém de base de dados do SQL Server, que tira partido da [colaboração B2B do Azure AD](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
|Tem sessão iniciada Windows utilizando as credenciais do Azure AD a partir de um domínio federado|Utilize [a autenticação integrada do Azure AD](sql-database-aad-authentication-configure.md).|
|Tem sessão iniciada Windows utilizando credenciais de um domínio não Federado com o Azure|Utilize [a autenticação integrada do Azure AD](sql-database-aad-authentication-configure.md).|
|Possui os serviços de camada média que precisam para ligar à base de dados SQL ou SQL Data Warehouse|Utilize [a autenticação integrada do Azure AD](sql-database-aad-authentication-configure.md).|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Como limitar ou controlar o acesso de conectividade a minha base de dados?
Existem várias técnicas no seu disposal que pode utilizar para obter um organização conectividade ideal para a sua aplicação. 
- Regras da Firewall
- Pontos finais do serviço VNET
- IPs Reservados

#### <a name="firewall"></a>Firewall
Uma firewall impede o acesso ao seu servidor de uma entidade externa ao permitir o acesso apenas entidades específicas para o servidor lógico. Por predefinição, todas as ligações e as bases de dados no interior do servidor lógico não são permitidos, exceto as ligações feitos por outros serviços do Azure. Com uma regra de firewall pode abrir o acesso ao seu servidor apenas para entidades (por exemplo, uma máquina de programador) que aprove, ao permitir que o endereço IP desse computador através da firewall. Também permite-lhe especificar um intervalo de IPs que pretende permitir o acesso ao servidor lógico. Por exemplo, os endereços IP da máquina de programador na sua organização podem ser adicionados em simultâneo, especificando um intervalo na página de definições de Firewall. 

Pode criar regras de firewall ao nível do servidor ou ao nível da base de dados. As regras de firewall ao nível do servidor podem criados através do portal ou através do SSMS. Para obter mais informações sobre como configurar um servidor e uma regra de firewall ao nível da base de dados, consulte: [criar regras de firewall na base de dados do SQL Server](sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal).

#### <a name="service-endpoints"></a>pontos finais de serviço
Por predefinição, a base de dados do SQL Server está configurado para "Permitir que todos os serviços do Azure" – o que significa que qualquer máquina Virtual no Azure pode tentar ligar à base de dados. Estes tentativas ainda tem de obter autenticado. No entanto, se não pretender que a base de dados para estar acessível a qualquer IPs do Azure, pode desativar "Permitir que todos os serviços do Azure". Além disso, pode configurar [pontos finais do serviço de VNET](sql-database-vnet-service-endpoint-rule-overview.md).

Pontos finais de serviço (SE) permitem-lhe para expor os recursos do Azure críticos apenas para sua própria rede privada virtual no Azure. Ao fazê-lo, eliminar, essencialmente, acesso público aos seus recursos. O tráfego entre a rede virtual para o Azure permanece na rede principal do Azure. Sem SE depara forçada do túnel encaminhamento de pacotes. A rede virtual força o tráfego de internet para a sua organização e o tráfego do serviço do Azure para abordar a rota do mesma. Com pontos finais de serviço, pode otimizar esta desde o fluxo de pacotes diretamente a partir da sua rede virtual para o serviço de rede principal do Azure.

![Pontos finais do serviço VNET](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

#### <a name="reserved-ips"></a>IPs Reservados
Outra opção consiste em aprovisionar [reservado IPs](../virtual-network/virtual-networks-reserved-public-ip.md) para as VMs e lista branca específico os endereços IP da VM no servidor de definições de firewall. Ao atribuir IPs reservados, guarde o trabalho de ter de atualizar as regras de firewall com a alteração dos endereços IP.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>As portas ligar à base de dados do SQL Server no?

Porta 1433. Base de dados SQL comunica através desta porta. Para ligar a partir de uma rede empresarial, tem de adicionar uma regra de saída nas definições de firewall da sua organização. Como orientação, evite a exposição a porta 1433 fora do limite do Azure. Pode executar o SSMS no Azure com [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Isto requer a abrir ligações de saída à porta 1433, o IP estático, para que a base de dados pode ser aberto para apenas o RemoteApp e suporta a Multi-factor Authentication (MFA).

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>Como monitorizar e regular a atividade no meu servidor e base de dados na base de dados SQL?
#### <a name="sql-database-auditing"></a>Auditoria de base de dados SQL
Com base de dados do SQL Server, pode ativar no auditoria para controlar eventos de base de dados. [Auditoria de base de dados SQL](sql-database-auditing.md) regista eventos de base de dados e escreve-as para um ficheiro de registo de auditoria na sua conta do Storage do Azure. A auditoria é especialmente útil se pretender obter conhecimentos aprofundados sobre potenciais violações de segurança e a política, manter a conformidade regulamentar, etc. Permite-lhe definir e configurar determinadas categorias dos eventos que considera que tem de auditoria e com base no que pode obter relatórios pré-configurados e um dashboard para obter uma descrição geral de eventos ocorridos na base de dados. Pode aplicar estas políticas de auditoria ao nível da base de dados ou ao nível do servidor. Um guia sobre como ativar a auditoria para o servidor/base de dados, consulte: [ativar a auditoria de base de dados de SQL](sql-database-security-tutorial.md#enable-sql-database-auditing-if-necessary).

#### <a name="threat-detection"></a>Deteção de Ameaças
Com [deteção de ameaças](sql-database-threat-detection.md), obter a capacidade para agir sobre política de segurança ou violações detetadas pela auditoria muito facilmente. Não precisa de ser um especialista para resolver potenciais ameaças ou violações no seu sistema de segurança. A deteção de ameaças também tem algumas capacidades incorporadas, como a deteção de Injeção de SQL. Injeção de SQL é uma tentativa de alterar ou comprometer os dados e de uma forma bastante comum de atacar uma aplicação de base de dados em geral. Deteção de ameaças de base de dados do SQL Server é executado vários conjuntos de algoritmos que detetar potenciais vulnerabilidades e ataques de injeção de SQL, bem como os padrões de acesso de base de dados anómalas (por exemplo, o acesso a partir de uma localização invulgar ou por um principal de familiarizado). Officers de segurança ou outros administradores designados recebem uma notificação por e-mail, se for detetada uma ameaça na base de dados. Cada notificação fornece detalhes sobre a atividade suspeita e as recomendações sobre como ainda mais a investigar e mitigar a ameaça. Para saber como ativar a deteção de ameaças, consulte: [ativar a deteção de ameaças de base de dados do SQL Server](sql-database-security-tutorial.md#enable-sql-database-threat-detection). 
### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>Como posso proteger os meus dados em geral na base de dados SQL?
Encriptação fornece um mecanismo seguro para proteger e proteger os dados confidenciais de intrusos. Os dados encriptados são de nenhuma utilização intruso sem a chave de desencriptação. Assim, adiciona uma camada adicional de proteção por cima das camadas de segurança criadas na base de dados do SQL Server existentes. Existem dois aspetos a proteger os dados na base de dados do SQL Server: 
- Os dados que está em rest nos ficheiros de dados e de registo 
- Os dados que se encontra em trânsito. 

Na base de dados do SQL Server, por predefinição, os dados Inativos nos ficheiros de dados e de registo no subsistema de armazenamento é completamente e sempre encriptados através de [encriptação transparente de dados [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). As cópias de segurança também são encriptadas. Com a TDE existem sem alterações necessárias no seu lado de aplicação que está a aceder a estes dados. A encriptação e desencriptação acontecer transparente; Por conseguinte, o nome. Para proteger os dados confidenciais em trânsito e inativos, base de dados do SQL Server fornece uma funcionalidade denominada [sempre encriptados (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine). AE é uma forma de encriptação do lado do cliente que encripta confidenciais colunas na base de dados (para que se encontram no ficheiro de encriptação para administradores de base de dados e os utilizadores não autorizados). O servidor recebe lugar os dados encriptados. A chave para sempre encriptados também é armazenada no lado do cliente, pelo que apenas clientes autorizados podem desencriptar as colunas confidenciais. O servidor e os administradores de dados não podem ver os dados confidenciais, uma vez que as chaves de encriptação são armazenadas no cliente. AE encripta confidenciais colunas na tabela ponto a ponto, a partir de clientes não autorizados para o disco físico. AE suporta comparações de igualdade hoje em dia, DBAs pode continuar a consultar colunas encriptadas como parte do respetivos comandos SQL. Sempre encriptado pode ser utilizado uma variedade de opções de armazenamento de chaves, tais como [Cofre de chaves do Azure](sql-database-always-encrypted-azure-key-vault.md), arquivo de certificados do Windows e módulos de segurança de local hardware.

|**Características**|**Sempre encriptado**|**Encriptação transparente de dados**|
|---|---|---|
|**Intervalo de encriptação**|Ponto a ponto|Dados em rest|
|**Servidor de base de dados pode aceder a dados confidenciais**|Não|Sim, uma vez que a encriptação é para os dados Inativos|
|**Operações de T-SQL permitidas**|Comparação de igualdade|Todos os área de superfície de T-SQL está disponível|
|**Alterações de aplicação necessárias para utilizar a funcionalidade**|mínimo|Muito mínima|
|**Granularidade de encriptação**|Nível de coluna|Nível de base de dados|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Como posso limitar o acesso a dados confidenciais na minha base de dados?
Cada aplicação tem pouco dados sensíveis na base de dados que necessita para ser protegido seja visível para todos os utilizadores. Determinados técnico na sua organização tem a ver estes dados, no entanto outras pessoas não devem ser capazes de ver estes dados. Um exemplo é wages do empregado. Um gestor teriam acesso às informações da sua relatórios diretos wage no entanto, os membros da equipa individuais não devem ter acesso às informações wage dos respetivos parceiros. Outro cenário é programadores de dados que podem interagir com dados confidenciais durante as fases de desenvolvimento ou teste, por exemplo, números da segurança social dos clientes. Novamente estas informações não precisem de ser expostos ao programador. Nestes casos, os dados confidenciais ou tem de ser mascarado ou não ser expostos de todo. Base de dados do SQL Server oferece estas duas abordagens para impedir que utilizadores não autorizados a capacidade de ver dados confidenciais:

[Máscara de dados dinâmicos](sql-database-dynamic-data-masking-get-started.md) é uma funcionalidade de máscara de dados que lhe permite limitar a exposição de dados confidenciais através da máscara-lo a utilizadores não privilegiados na camada da aplicação. Definir uma regra de máscara que pode criar um padrão de máscara (por exemplo, para mostrar apenas últimos quatro dígitos de um SSN ID national: XXX-XX-0000 e marcá-la como Xs maioria) e identificar os utilizadores a serem excluídos da regra de máscara. A máscara acontece no momento e existem várias funções de máscara disponíveis para várias categorias de dados. A máscara de dados dinâmico permite-lhe detetar dados sensíveis na base de dados e aplicar máscara automaticamente.

[Segurança ao nível de linha](/sql/relational-databases/security/row-level-security) permite-lhe controlar o acesso ao nível da linha. Ou seja, algumas linhas numa tabela da base de dados com base no utilizador executar a consulta (grupo associação ou execução contexto) estão ocultas. A restrição de acesso é feita na camada de base de dados em vez de uma camada de aplicação, para simplificar a lógica de aplicação. Começar através da criação de um predicado de filtro, filtrar as linhas que não são expostos e a segurança política seguinte definir quem tem acesso a estas linhas. Por fim, o utilizador final executa a sua consulta e, dependendo de privilégio do utilizador, possam ver as linhas restritas ou não conseguem vê-las em todas as.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Como posso gerir chaves de encriptação na nuvem?

Existem opções de gestão de chaves para sempre encriptados (encriptação do lado do cliente) e a encriptação transparente de dados (encriptação de Inativos). Recomenda-se que regularmente Rodar chaves de encriptação. A frequência de rotação deve alinhar com as suas regulamentos interno da organização e os requisitos de conformidade.

**Encriptação de dados transparente (TDE)**: não há uma hierarquia de duas chave TDE – os dados em cada base de dados do utilizador são encriptados por uma simétrica AES 256 exclusivo para a base de dados da base de dados chave de encriptação (DEK), que por sua vez, é encriptada por um servidor único RSA assimétrico chave mestra de 2048. A chave mestra podem ser geridos a:
- Automaticamente pela plataforma do - base de dados SQL.
- Ou ao utilizar o [Cofre de chaves do Azure](sql-database-always-encrypted-azure-key-vault.md) como o armazenamento de chaves.

Por predefinição, a chave mestra para a encriptação transparente de dados é gerida pelo serviço de base de dados SQL para sua comodidade. Se a organização gostaria controlo sobre a chave mestra, é uma opção para utilizar Vault](sql-database-always-encrypted-azure-key-vault.md) de chave do Azure como o armazenamento de chaves. Ao utilizar o Cofre de chaves do Azure, organização assume o controlo sobre controlos de aprovisionamento, rotação e permissão de chaves. [Rotação ou mudar o tipo de uma chave mestra TDE](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) é rápido, uma vez que apenas encripta novamente o DEK. Para organizações com separação de funções entre segurança e gestão de dados, pode aprovisionar o material de chave para a chave mestra TDE no Cofre de chaves do Azure e fornecer um identificador de chave do Cofre de chaves do Azure para o administrador da base de dados a utilizar para um administrador de segurança encriptação de Inativos num servidor. O Cofre de chaves foi concebido de forma a que o Microsoft não consulte ou extraia as chaves de encriptação. Utilizador também obtém uma gestão centralizada de chaves para a sua organização. 

**Sempre encriptado**: há também um [hierarquia de duas chave](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) no sempre encriptados - uma coluna de dados confidenciais é encriptada por uma chave de encriptação de coluna AES 256 (CEK), que por sua vez, é encriptada por uma chave mestra da coluna (CMK). Os controladores de cliente fornecidos para sempre encriptado têm não existem limitações ao comprimento de CMKs. O valor encriptado do CEK é armazenado na base de dados e o CMK é armazenado num arquivo de chave fidedigno, como arquivo de certificados do Windows, o Cofre de chaves do Azure ou um módulo de hardware de segurança. 
- Tanto o [CEK e CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) pode rotação. 
- Rotação de CEK tem um tamanho de operação de dados e podem ser intensivas em termos de tempo, consoante o tamanho das tabelas que contém as colunas encriptadas. Por conseguinte, é recomendada uma investigação planear rotações de CEK em conformidade. 
- Rotação de CMK, no entanto, não interfere com o desempenho de base de dados e pode ser feita com funções separadas.
O diagrama seguinte mostra as opções de armazenamento de chaves para as chaves de mestra de coluna no sempre encriptado

![Sempre encriptado CMK armazenar fornecedores](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Como otimizar e proteger o tráfego entre a minha organização e a base de dados SQL?
O tráfego de rede entre a sua organização e a base de dados SQL, geralmente, seria obter encaminhado através da rede pública. No entanto, se optar por otimizar este caminho e torná-lo mais segura, pode ver no Expressroute. Essencialmente o expressroute permite-lhe expandir a sua rede empresarial para a plataforma do Azure através de uma ligação privada. Ao fazê-lo, não quer através da Internet pública. Utilizador também obtém uma maior segurança, fiabilidade e a otimização de encaminhamento que traduz para baixas latências de rede e muito mais rápidas velocidades que normalmente a experiência vai através da internet pública. Se estiver a planear sobre a transferência de um segmento significativo de dados entre a organização e o Azure, utilizar o Expressroute pode produzir benefícios de custos. Pode escolher entre três modelos de conetividade diferente para a ligação da sua organização para o Azure: 
- [Localização conjunta do Exchange na nuvem](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Qualquer a qualquer](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Ponto a ponto](../expressroute/expressroute-connectivity-models.md#Ethernet)

Também o Expressroute permite-lhe impulsar 2x até o limite de largura de banda que adquirir sem encargos adicionais. Também é possível configurar cruzada conectividade região a utilizar o expressroute. Para ver uma lista de fornecedores de conectividade de ER, consulte: [Express parceiros de rota e localizações de Peering](../expressroute/expressroute-locations.md). Os artigos seguintes descrevem Express Route mais detalhadamente:
- [Introdução no Expressroute](../expressroute/expressroute-introduction.md)
- [Pré-requisitos](../expressroute/expressroute-prerequisites.md)
- [Fluxos de trabalho](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>É a base de dados SQL em conformidade com os requisitos de regulamentação e forma que o ajuda a conformidade da minha própria organização?
Base de dados do SQL Server está em conformidade com um intervalo de regulamentação compliances. Para ver o conjunto mais recente de compliances que tiverem sido cumpridos, visite o [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) e desagregar compliances que são importantes para a sua organização para ver se a base de dados SQL está incluído em serviços do Azure em conformidade. É importante ter em atenção que apesar de base de dados SQL pode ser certificada como um serviço em conformidade, que ajuda na conformidade de serviço da sua organização, mas não automaticamente garante-lo.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Monitorização de base de dados inteligente e manutenção após a migração

Depois de ter migrado a base de dados à base de dados do SQL Server, são vai que pretende monitorizar a base de dados (para o exemplo, verifique como a utilização de recursos, como ou DBCC verifica) e executar a manutenção regular (por exemplo, reconstruir ou reorganize indexa, estatísticas etc.). Felizmente, base de dados SQL é Intelligent na medida em que utiliza as tendências históricas e métricas registadas e estatísticas para proativamente ajudar a monitorizar e manter a base de dados, para que a sua aplicação é executada sempre executadas de forma ideal. Em alguns casos, BD SQL do Azure automaticamente pode efetuar tarefas de manutenção, consoante a configuração de configuração. Existem três facetas para monitorizar a base de dados na base de dados do SQL Server:
- Monitorização de desempenho e a otimização.
- Otimização de segurança.
- Otimização de custos.

**Monitorização de desempenho e a otimização de**: com o Query Performance Insight, pode obter recomendações personalizáveis para a carga de trabalho de base de dados para que as aplicações podem manter em execução num nível ótimo - sempre. Pode também configurá-la para que estas recomendações ser aplicadas automaticamente e não precisa de bother realizar tarefas de manutenção. Com o Assistente do índice, pode implementar automaticamente recomendações do índice com base na sua carga de trabalho - esta opção é denominada otimização automática. As recomendações evoluem como as alterações de carga de trabalho de aplicação para lhe fornecer as sugestões mais relevantes. Também é apresentada a opção manualmente rever estas recomendações e aplicá-las ao seu critério.  

**Otimização de segurança**: base de dados do SQL Server fornece recomendações de segurança passíveis de ação para o ajudar a proteger os seus dados e a deteção de ameaças para identificar e investigar atividades suspeitas da base de dados que podem implicar um potencial thread para o base de dados. [Avaliação da vulnerabilidade SQL](sql-vulnerability-assessment.md) é uma base de dados de análise e relatórios de serviço que lhe permite monitorizar o estado de segurança das bases de dados à escala, identificar riscos de segurança e que se desviam de uma linha de base de segurança definida por si. Depois de cada análise, são fornecidos uma lista personalizada de passos acionáveis e scripts de remediação, bem como um relatório de avaliação que pode ser utilizado para ajudar a para estar em conformidade.

Centro de segurança do Azure, pode identificar as recomendações de segurança entre o painel e aplicá-las com um único clique. 

**Otimização de custos**: plataforma de SQL do Azure analisa o histórico de utilização entre as bases de dados num servidor para avaliar e recomendar opções de otimização de custos para si. Esta análise demora, normalmente, um fortnight para analisar e recomendações passíveis de ação de compilação. Agrupamento elástico é um essa opção. A recomendação é apresentado no portal como numa faixa de:

![recomendações de conjunto elástico](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png) 

Também pode ver esta análise na secção "Advisor":

![recomendações de conjunto elástico-advisor](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>Como monitorizar o desempenho e a utilização de recursos na base de dados SQL?

Na base de dados do SQL Server pode tirar partido dos insights inteligentes da plataforma para monitorizar o desempenho e otimizar em conformidade. Pode monitorizar o desempenho e a utilização de recursos na base de dados do SQL Server utilizando os seguintes métodos:
- **Portal do Azure**: portal do Azure mostra a utilização de um único da base de dados selecionando a base de dados e clicando em gráfico no painel de descrição geral. Pode modificar o gráfico para mostrar várias métricas, incluindo a percentagem de CPU, percentagem de DTU, percentagem de es de dados, percentagem de sessões e percentagem do tamanho da base de dados.

   ![Gráfico de monitorização](./media/sql-database-manage-after-migration/monitoring-chart.png)

   ![Monitorização gráfico 2](./media/sql-database-manage-after-migration/chart.png)

Neste gráfico, também pode configurar alertas por recurso. Estes alertas permitem-lhe responder às condições de recursos com uma mensagem de e-mail, escrever um ponto final HTTPS/HTTP ou efetuar uma ação. Consulte o [monitorização do desempenho de base de dados na base de dados do SQL Server](sql-database-single-database-monitor.md) para obter instruções detalhadas.

- **Vistas de gestão dinâmica**: pode consultar o [sys.dm db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) vista de gestão dinâmica para devolver o histórico de estatísticas de consumo de recursos de última hora e a [resource_stats ](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) vista de catálogo de sistema para devolver o histórico para os últimos 14 dias.
- **Query Performance Insight**: [Query Performance Insight](sql-database-query-performance.md) permite-lhe ver um histórico das consultas de consumo de recursos principais e consultas de execução longa para uma base de dados específica. Pode identificar rapidamente consultas superiores através da utilização de recursos, a duração e a frequência de execução. Pode controlar as consultas e detetar regressão. Esta funcionalidade requer [arquivo de consultas](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) ser ativada e o Active Directory para a base de dados.

   ![Query Performance Insight](./media/sql-database-manage-after-migration/query-performance-insight.png)

- **Análise de SQL do Azure (pré-visualização) no Log Analytics**: [Log Analytics do Azure](../log-analytics/log-analytics-azure-sql.md) permite-lhe recolher e visualizar as métricas de desempenho de chaves do Azure SQL do Azure, suportar até 150.000 bases de dados SQL e 5000 conjuntos elásticos SQL por área de trabalho. Pode utilizá-lo para monitorizar e receber notificações. Pode monitorizar a base de dados SQL e métricas de conjunto elástico entre várias subscrições do Azure e conjuntos elásticos e pode ser utilizado para identificar problemas em cada camada de uma pilha de aplicação.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Posso estou dar por isso problemas de desempenho: como does meu metodologia de resolução de problemas da base de dados SQL é diferente do SQL Server?
Uma parte principais as técnicas de resolução de problemas que utilizaria para diagnosticar consulta e problemas de desempenho de base de dados permanecerem o mesmo. Depois de todos os o mesmo SQL Server motor for ligado à nuvem. No entanto, a plataforma - BD SQL do Azure criada no 'intelligence'. Pode ajudar a resolver problemas e diagnosticar problemas de desempenho mesmo mais facilmente. -Pode também efetuar algumas destas ações corretivas em seu nome e, em alguns casos, proativamente corrija-as - a automaticamente. 

A abordagem para a resolução de problemas de desempenho significativamente pode beneficiar utilizando inteligentes funcionalidades, tais como [Insight(QPI) de desempenho de consulta](sql-database-query-performance.md) e [Database Advisor](sql-database-advisor.md) em conjunto e de modo a diferença no metodologia difere em que relativamente – já não precisa de executar o trabalho manual do grinding os detalhes essencial que poderá ajudar a resolver o problema em execução. A plataforma faz o trabalho por si. Um exemplo de que é QPI. Com QPI, pode desagregar até o nível de consulta e observe as tendências históricas e descobrir quando regressed exatamente a consulta. A base de dados do Advisor dá-lhe recomendações no coisas que poderão ajudar a melhorar o desempenho global em geral, como - índices em falta, remover índices, parameterizing as suas consultas etc. 

Com a resolução de problemas de desempenho, é importante identificar se está apenas a aplicação ou a base de dados como suporte, que está a afetar o desempenho da sua aplicação. Muitas vezes, o problema de desempenho situam-se na camada da aplicação. Isto pode dever a arquitetura ou o padrão de acesso de dados. Por exemplo, imagine que tem uma aplicação chatty que é sensível à latência de rede. Neste caso, a sua aplicação sofrerá porque seria muitos pedidos curtos vai anterior e descritos ("chatty") entre a aplicação e o servidor e numa rede congestionada, adicione estes e voltas segurança fast. Para melhorar o desempenho neste caso, pode utilizar [Batch consultas](sql-database-performance-guidance.md#batch-queries). Utilizar lotes ajuda-o a tremendously porque agora processados os pedidos de batch; Assim, ajudando-o a diminuir o tempo limite de latência de ida e volta e melhoram o desempenho da aplicação. 

Além disso, se reparar numa degradação no desempenho global da sua base de dados, pode monitorizar o [sys.dm db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) e [resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) vistas de gestão dinâmica para compreenda o consumo de CPU, memória e e/s. O desempenho pode ser afetado porque a base de dados é starved de recursos. É possível que poderá ter de alterar o nível de desempenho e/ou com base na crescer e reduzir a carga de trabalho exigências de camada de serviço. 

Para um conjunto abrangente de recomendações para problemas de desempenho de otimização, consulte: [otimizar a base de dados](sql-database-performance-guidance.md#tune-your-database).

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Como garantir que estiver a utilizar o nível de desempenho e o escalão de serviço apropriado?
Base de dados do SQL Server oferece vários escalões de serviço básico, Standard e Premium. Cada escalão de serviço, obter um desempenho previsível garantido associado a esse nível de serviço. Consoante a carga de trabalho, pode ter bursts da atividade em que a utilização de recursos poderá atingiu o limite de nível de desempenho atual, que se encontrem no. Nestes casos, é útil para primeiro início avaliando se qualquer otimização pode ajudá-lo (por exemplo, adicionar ou alterar um índice, etc.). Se ainda ocorrerem problemas de limite, considere mover a um nível de desempenho superior ou um nível de serviço. 

|**Nível de serviço**|**Cenários de caso de utilização comuns**|
|---|---|
|**Básica**|Aplicações com os utilizadores alguns e uma base de dados que não tem requisitos de concorrência, dimensionamento e desempenho elevados. |
|**Standard**|Aplicações com um requisitos de concorrência, dimensionamento e desempenho considerável conjugados com pouco a média dos pedidos de e/s. |
|**Premium**|Aplicações com muitos utilizadores em simultâneo, elevada da CPU/memória e alta exige a e/s. Simultaneidade elevada, débito alto e aplicações confidenciais latência podem tirar partido do nível de Premium. |
|||

Para certificar-se de que está no nível de desempenho à direita, pode monitorizar o consumo de recursos de consulta e da base de dados através de uma das formas above-mentioned em "Como posso monitorizar a utilização de recursos e de desempenho na base de dados do SQL Server". Deverá considerar que as consultas/bases de dados estão consistentemente em execução frequente no etc. da CPU/memória que pode considerar como aumentar verticalmente a um nível de desempenho superior. Da mesma forma, se, tenha em atenção que, mesmo durante o horário de pico, poderá parecem não corresponder a utilizar os recursos como muito; Considere dimensionamento do nível de desempenho atual. 

Se tiver um padrão de aplicação SaaS ou um cenário de consolidação de base de dados, considere utilizar um conjunto elástico para otimização de custos. Agrupamento elástico é uma excelente forma de alcançar a consolidação de base de dados e a otimização de custos. Para ler mais sobre como gerir várias bases de dados utilizando o conjunto elástico, consulte: [gerir bases de dados e agrupamentos](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal). 

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Como muitas vezes, é necessário executar verificações de integridade da base de dados para a minha base de dados?
Base de dados do SQL Server utiliza algumas inteligentes técnicas que lhe permite processar determinadas classes de danos nos dados sem perda de dados e automaticamente. Estes técnicas estão incorporadas no serviço e são aproveitadas pelo serviço precisar quando for. Regularmente, as cópias de segurança da base de dados em todo o serviço são testadas o restauro e executando DBCC CHECKDB no mesmo. Se existirem problemas, base de dados SQL proativamente endereços-los. [Reparação de página automática](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) é utilizado para corrigir as páginas que estão danificados ou tem problemas de integridade de dados. As páginas de base de dados são verificadas sempre com a definição predefinida da soma de verificação que verifica a integridade da página. Base de dados SQL proativamente monitoriza e analisa a integridade dos dados da base de dados e, se surgir um problema, os endereços-las com a prioridade mais elevada. Além destas, pode optar por executar opcionalmente o seus próprio verificações de integridade à sua vontade.  Para obter mais informações, consulte [integridade dos dados na base de dados do SQL Server](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="data-movement-after-migration"></a>Movimento de dados após a migração

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>Como exportar e importar dados como ficheiros BACPAC da base de dados do SQL Server?

- **Exportar**: pode exportar a base de dados SQL do Azure como um ficheiro BACPAC do portal do Azure

   ![Exportação de base de dados](./media/sql-database-export/database-export.png)

- **Importar**: também pode importar dados como um ficheiro BACPAC para a base de dados no portal do Azure.

   ![Importação de base de dados](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>Como devo sincronizar dados entre a base de dados SQL e SQL Server?
Tem várias formas de alcançar isto: 
- **[Sincronização de dados](sql-database-sync-data.md)**  – esta funcionalidade ajuda-o a sincronizar dados bidirecional entre várias bases de dados no local do SQL Server e base de dados SQL. Para sincronizar com bases de dados no local do SQL Server, terá de instalar e configurar o agente de sincronização num computador local, abra a porta de saída de TCP 1433.
- **[Replicação de transações](https://azure.microsoft.com/en-us/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)**  – com a replicação de transações que pode sincronizar os dados do local para a BD SQL do Azure com o local que está a ser o publicador e a BD SQL do Azure que está a ser o subscritor. Por agora, apenas esta configuração é suportada. Para obter mais informações sobre como migrar os dados do local para o Azure SQL com o período de indisponibilidade mínimo, consulte: [replicação de transações de utilização](sql-database-cloud-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Passos seguintes
Saiba mais sobre [base de dados SQL](sql-database-technical-overview.md).

