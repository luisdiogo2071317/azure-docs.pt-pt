---
title: Blueprint de análise do Estado de funcionamento do Azure
description: Orientações para implementar um Blueprint de análise do Estado de funcionamento HIPAA/HITRUST
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 26566e0a-0a54-49f4-a91d-48e20b7cef71
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2018
ms.author: jomolesk
ms.openlocfilehash: e627019f6219509c21ca6245ee1f7f54f2e1669a
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "33894318"
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Segurança do Azure e conformidade Blueprint - os dados de estado de funcionamento HIPAA/HITRUST e AI

## <a name="overview"></a>Descrição geral

**A segurança do Azure e a conformidade Blueprint - os dados de estado de funcionamento HIPAA/HITRUST e AI oferece uma implementação de ativar a chave de uma solução de PaaS do Azure para demonstrar como de forma segura de inserção, armazenar, analisar e interaja com os dados de estado de funcionamento ao ser capaz de estar em conformidade da indústria requisitos. O blueprint ajuda a acelerar a adoção de nuvem e de utilização para clientes com dados que são regulados.**

A segurança do Azure e a conformidade Blueprint - os dados de estado de funcionamento HIPAA/HITRUST e AI Blueprint fornece ferramentas e orientações para ajudar a implementar segura, Health Insurance Portability e Accountability Act (HIPAA) e estado de funcionamento informações confiar Alliance (HITRUST) pronto ambiente de plataforma-como-um-serviço (PaaS) para ingestão relacionadas, armazenar, analisar e interagir com os registos médicas pessoais e não pessoais num ambiente de nuvem segura, multicamadas, implementado como uma solução ponto-a-ponto. Este showcases uma arquitetura de referência comuns e foi concebido para simplificar a adoção do Microsoft Azure. Esta arquitetura fornecida ilustra uma solução para satisfazer as necessidades das organizações para procurar uma abordagem baseada na nuvem para reduzir o custo de implementação e o fardo.

![](images/components.png)

A solução foi concebida para consumir um conjunto de dados de exemplo formatado utilizando rápido Healthcare interoperabilidade recursos (FHIR), um padrão em todo o mundo para trocar informações de cuidados de saúde eletronicamente e armazená-las de forma segura. Os clientes, em seguida, podem utilizar o Azure Machine Learning para tirar partido das ferramentas de inteligência de negócio poderoso e de análise para rever as predições efetuadas nos dados de exemplo. Como um exemplo de um tipo de experiência que pode facilitar o Azure Machine Learning, o blueprint inclui um conjunto de dados de exemplo, scripts e ferramentas para prever a duração do período de um patient num local hospital. 

Este blueprint destina-se para servir como base modular para os clientes ajustar os seus requisitos específicos, desenvolver novo Azure Machine learning experimentações para resolver ambos os cenários de caso de utilização clinical e operacional. Foi concebido para ser segura e compatível quando implementado; No entanto, os clientes são responsáveis para configurar corretamente as funções e a implementar quaisquer modificações. Tenha em atenção o seguinte:

-   Este blueprint fornece uma linha de base para ajudar os clientes utilizam o Microsoft Azure num HITRUST e ambiente HIPAA.

-   Embora o blueprint foi concebido para ser alinhado com a HIPAA e HITRUST (através do quadro comum de segurança - CSF), este não deve ser considerada compatível até certificadas por um auditor por lei HIPAA e HITRUST requisitos de certificação externo.

-   Os clientes são responsáveis por realizar revisões adequados de segurança e conformidade de qualquer solução criado com esta arquitetura dos.

## <a name="deploying-the-automation"></a>Implementar a automatização

- Para implementar a solução, siga as instruções fornecidas a orientação de implementação. 

[![](./images/deploy.png)](https://aka.ms/healthblueprintdeploy)

Para uma descrição geral de como funciona esta solução, veja este [vídeo](https://aka.ms/healthblueprintvideo) explicar e demonstrar a implementação.

- Pergunta frequentes pode ser encontrada no [FAQ](https://aka.ms/healthblueprintfaq) orientações.

-   **Diagrama da arquitetura.** O diagrama mostra a arquitetura de referência utilizada para o blueprint e cenário de utilizar o exemplo.

-   **Modelos de implementação**. Nesta implementação, [modelos Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview#template-deployment) são utilizados para implementar automaticamente os componentes da arquitetura no Microsoft Azure, especificando os parâmetros de configuração durante a configuração.

-   **[Scripts de implementação de automatizada](https://aka.ms/healthblueprintdeploy)**. Estes scripts ajudam a implementar a solução. Os scripts consistem em:


-   Uma instalação do módulo e [administrador global](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) script de configuração é utilizada para instalar e certifique-se de que funções de administrador global e de módulos do PowerShell necessários estão configuradas corretamente. 
-   Uma instalação de script do PowerShell é utilizada para implementar a solução, fornecida através de um ficheiro. zip que contém uma demonstração pré-criadas funções.

## <a name="solution-components"></a>Componentes da solução


A arquitetura dos é composto pelos seguintes componentes:

-   **[Modelo de ameaça](https://aka.ms/healththreatmodel)**  um modelo de ameaça abrangente é fornecido no formato tm7 para utilização com o [ferramenta de modelação de ameaça do Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=49168), que mostra os componentes da solução, os dados fluem entre-los e a confiança limites. O modelo pode ajudar os clientes os pontos de risco potencial na infraestrutura do sistema quando desenvolver os componentes do machine learning ou modificações adicionais.

-   **[Matriz de implementação do cliente](https://aka.ms/healthcrmblueprint)**  livro do Microsoft Excel de uma lista os requisitos de HITRUST relevantes e explica como o cliente e da Microsoft são responsáveis por reunião, cada um deles.

-   **[Revisão do Estado de funcionamento.](https://aka.ms/healthreviewpaper)** A solução foi revista por Coalfire sistemas, Inc. A conformidade de estado de funcionamento (HIPAA e PCI HITRUST) Reveja e orientações para a implementação fornece um auditor\'revisão s da solução e considerações para transformar blueprint para uma implementação de prontos para produção.

# <a name="architectural-diagram"></a>Diagrama da arquitetura


![](images/refarch.png)

## <a name="roles"></a>Funções


O blueprint define duas funções de utilizadores administrativos (operadores) e três funções de utilizador na gestão de hospital e patient cuidado. Uma função de sexto está definida para um auditor avaliar a compatibilidade com a HIPAA e outras normas. Azure baseada em funções controlo de acesso (RBAC) permite precisamente direcionadas para gestão de acesso para cada utilizador da solução através de funções incorporadas e personalizadas. Consulte [introdução ao controlo de acesso baseado em funções no portal do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview) e [funções incorporadas para controlo de acesso baseado em funções do Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) para obter informações detalhadas sobre o RBAC, funções e permissões.

### <a name="site-administrator"></a>Administrador do site


O administrador do site é responsável pela subscrição do cliente do Azure. Controlar a implementação geral, mas tem sem acesso a registos patient.

-   Predefinição atribuições de funções: [proprietário](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)

-   Atribuições de função personalizada: n/d

-   Âmbito: subscrição

### <a name="database-analyst"></a>Analista de base de dados

O analista de base de dados administra a instância do SQL Server e base de dados.
Têm sem acesso a registos patient.

-   Atribuições de função incorporada: [contribuinte da BD SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-db-contributor), [Contribuidor do SQL Server](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-server-contributor)

-   Atribuições de função personalizada: n/d

-   Âmbito: ResourceGroup

 ### <a name="data-scientist"></a>Scientist de dados


O scientist dados funciona o serviço Azure Machine Learning. Pode importar, exportar e gerir os dados e executar relatórios. O scientist de dados tem acesso a dados patient, mas não tem privilégios administrativos.

-   Atribuições de função incorporada: [contribuinte de conta de armazenamento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor)

-   Atribuições de função personalizada: n/d

-   Âmbito: ResourceGroup

### <a name="chief-medical-information-officer-cmio"></a>Diretor médicas informações responsável pela (CMIO)


O CMIO straddles divisão entre informatics/technology e profissionais de cuidados de saúde numa organização cuidados de saúde. Os seus deveres incluem, geralmente, através da análise para determinar se os recursos estão a ser atribuídos adequadamente dentro da organização.

-   Atribuições de função incorporada: nenhuma

### <a name="care-line-manager"></a>Se o Gestor de linha


O Gestor de linha cuidado é diretamente envolvido com o cuidado de patients.
Esta função exige monitorizar o estado de doentes individuais, bem como garantir que existe pessoal disponível para responder às necessidades específicas de prestação de cuidados de saúde dos seus doentes. O Gestor de linha cuidado é responsável pela adição e atualizar registos patient.

-   Atribuições de função incorporada: nenhuma

-   Atribuições de função personalizada: tem o privilégio para executar HealthcareDemo.ps1 para ambos os admissão Patient e Discharge.

-   Âmbito: ResourceGroup

### <a name="auditor"></a>Auditor


O auditor avalia a solução de compatibilidade. Têm sem acesso direto à rede.

-   Atribuições de função incorporada: [leitor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader)

-   Atribuições de função personalizada: n/d

-   Âmbito: subscrição

## <a name="example-use-case"></a>Caso de utilização de exemplo


O caso de utilização de exemplo incluído com este blueprint ilustra como o Blueprint pode ser utilizada para ativar o machine learning e análise dos dados de estado de funcionamento na nuvem. Contosoclinic é um pequeno hospital localizado nos Estados Unidos. Os administradores de rede do hospital pretendem utilizar o Azure Machine Learning para prever melhor a duração do período de um patient no momento da admittance, para aumentar a eficiência operacional da carga de trabalho e melhorar a qualidade dos cuidado pode fornecer.

### <a name="predicting-length-of-stay"></a>Duração do período de prever


O cenário de utilização por exemplo utiliza o Azure Machine Learning para prever a duração de um patient recentemente admitted ao comparar os detalhes médicas tirados na introdução patient agregados dados históricos de patients anteriores.
O blueprint inclui um grande conjunto de registos médicas anónimos para demonstrar as capacidades de formação e preditivos da solução. Numa implementação de produção, os clientes teria de utilizar os seus próprios registos para preparar a solução para as predições mais exatas ao refletir os detalhes do seu ambiente, instalações e patients exclusivos.

### <a name="users-and-roles"></a>Utilizadores e funções


**Administrador do site – Alex**

*E-mail: Alex\_SiteAdmin*

Tarefa de Alex consiste em avaliar as tecnologias que podem reduzir a carga de gestão de uma rede no local e reduzir os custos de gestão. Alex tem sido avaliar o Azure há algum tempo, mas tem enfrentou configurar os serviços que ele tem de cumprir os requisitos de conformidade HiTrust para armazenar dados Patient na nuvem. Selecionou AI de estado de funcionamento do Azure para implementar uma solução de estado de funcionamento de conformidade pronto, o que foi resolvido os requisitos para cumprir os requisitos de cliente para HiTrust Alex.

**Dados Scientist – Debra**

*E-mail: Debra\_DataScientist*

Debra é responsável pela utilizando e criar modelos de analisam registos médicas, para fornecer informações de patient cuidado. Debra utiliza o SQL Server e R análises linguagem de programação para criar os modelos.

**Analista de base de dados – Danny**

*E-mail: Danny\_DBAnalyst*

Danny é o contacto principal para tudo sobre o Microsoft SQL Server que armazena todos os dados patient para Contosoclinic. Danny é um administrador experiente do SQL Server que recentemente tornou-se familiarizado com a SQL Database do Azure.

**Diretor médicas informações responsável pela – Caroline**

Caroline está a trabalhar com Chris o Gestor de linha mais importantes para si e Debra Scientist os dados para determinar que fatores afetam patient duração do período.
Caroline utiliza serão as predições da solução (LOS) comprimento-de-mantenha-se para determinar se os recursos estão a ser atribuídos adequadamente na rede hospital. Por exemplo, através do dashboard fornecido nesta solução.

**Mais importantes para si linha Manager--Chris**

*E-mail: Chris\_CareLineManager*

Como o indivíduo diretamente responsável por gerir admissão patient e discharges em Contosoclinic, Chris utiliza serão as predições geradas pela solução LOS para garantir que está disponíveis para fornecer cuidado para patients enquanto estes são permanecendo no pessoal adequado a instalações.

**Auditor – Han**

*E-mail: Han\_Auditor*

Han é um auditor certificado que tenha a experiência de auditoria para o ficheiro ISO, SOC e HiTrust. Han foi recrutado para rever a rede do Contosoclinc. Han pode rever o cliente responsabilidade matriz fornecido com a solução para se certificar de que o blueprint e uma solução de LOS podem ser utilizados para armazenar, processo e apresentar dados pessoais confidenciais.


# <a name="design-configuration"></a>Configuração de design


Esta secção fornece detalhes sobre as configurações predefinidas e as medidas de segurança incorporadas no Blueprint indicada para:

- **INSERÇÃO** origens de dados não processados, incluindo a origem de dados FHIR
- **ARQUIVO** informações confidenciais
- **ANALISAR** e prever resultados
- **INTERAGIR** com os resultados e predições
- **IDENTIDADE** gestão da solução
- **SEGURANÇA** ativada funcionalidades


## <a name="identity"></a>IDENTIDADE 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory e o controlo de acesso baseado em funções (RBAC)


**Autenticação:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) é o Microsoft\'s multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão. Todos os utilizadores para a solução foram criados no Azure Active Directory, incluindo os utilizadores que acedem a base de dados do SQL Server.



-   Autenticação para a aplicação é executada a utilizar o Azure AD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory Identity Protection](/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades da organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com as identidades da organização, e investiga incidentes suspeitas e executa uma ação adequada resolvê-los.

-   [Azure baseada em funções controlo de acesso (RBAC)](/azure/role-based-access-control/role-assignments-portal) permite precisamente direcionadas para gestão de acesso ao Azure. Acesso de subscrição está limitado ao administrador da subscrição e o acesso do Cofre de chaves do Azure é limitado para o administrador do site. As palavras-passe seguras (12 carateres no mínimos com, pelo menos, uma letra em maiúsculas/inferior, o número e o caráter especial) são necessárias.

-   Autenticação multifator é suportada quando o parâmetro - enableMFA é ativado durante a implementação.

-   Expirarem palavras-passe após 60 dias quando o parâmetro - enableADDomainPasswordPolicy é ativado durante a implementação.

**Funções:**

-   A solução utiliza [funções incorporadas](/azure/role-based-access-control/built-in-roles) para gerir o acesso aos recursos.

-   Todos os utilizadores são atribuídos a funções incorporadas específicas por predefinição.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Incluem dados armazenados no Cofre de chaves:

    -   Chave de informações da aplicação
    -   Chave de acesso de armazenamento de dados patient
    -   Cadeia de ligação patient
    -   Nome da tabela de dados patient
    -   Ponto final do serviço Web do Azure ML
    -   Chave de API do serviço do Azure ML

-   Políticas de acesso avançadas são configuradas numa base necessário
-   As políticas de acesso do Cofre de chaves estão definidas com permissões mínimas para chaves e segredos
-   Todas as chaves e segredos no Cofre de chaves tem datas de expiração
-   Todas as chaves no Cofre de chaves protegidas pelo HSM \[tipo de chave = chave RSA 2048 protegidas de HSM bits\]
-   Todos os utilizadores/identidades são concedidas as permissões necessárias mínimas com controlo de acesso baseado em ' (RBAC) da função
-   Aplicações não partilham um cofre de chaves, a menos que confiam uns nos outros e precisam de acesso para os mesmos segredos no tempo de execução
-   Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, a 365 dias.
-   Operações de criptografia permitidas para as chaves estão limitadas a perfis necessários

## <a name="ingest"></a>INGESTÃO 

### <a name="azure-functions"></a>Funções do Azure
A solução foi concebida para utilizar [das funções do Azure](/azure/azure-functions/) para processar o comprimento de exemplo do mantenha-se de dados utilizadas em demonstração de análise. Capacidades de três nas funções foram criadas.

**1. Importação em volume de dados de phi de dados de cliente**

Ao utilizar o script de demonstração. . \\HealthcareDemo.ps1 com o **BulkPatientAdmission** comutador conforme descrito na **implementar e executar a demonstração** executa o pipeline de processamento seguintes:
1. **Armazenamento de Blobs do Azure** -exemplo de ficheiro. csv de Patient dados carregados para o armazenamento
2. **Grelha de evento** -eventos publica dados da função do Azure (importação em volume - eventos de blob)
3. **Função do Azure** - efetua o processamento e armazena os dados para o armazenamento de SQL Server utilizando a função segura - eventos (tipo; url do blob)
4. **BD do SQL** - o arquivo de base de dados de utilização de dados de Patient etiquetas para classificação e o processo de ML é arrancou para fazer a experimentação de preparação.

![](images/dataflow.png)

Além disso, a função do azure foi concebida para ler e proteger os dados confidenciais designados no conjunto de dados de exemplo utilizando as etiquetas seguintes:
- dataProfile = > "ePHI"
- proprietário = > \<UPN de administrador do Site\>
- ambiente = > "Piloto"
- departamento = > "Ecossistema Global" a etiquetagem foi aplicada ao conjunto de dados de exemplo onde patient 'names' foi identificado como texto não encriptado.

**2. Admissão de patients novo**

Ao utilizar o script de demonstração. . \\HealthcareDemo.ps1 com o **BulkPatientadmission** comutador conforme descrito na **implementar e executar a demonstração** executa o pipeline de processamento seguintes: ![](images/securetransact.png) 
 **1. Função do Azure** acionada e a função de pedidos para um [token de portador](/rest/api/) do Azure Active directory.

**2. O Cofre de chaves** pedida para um segredo associado ao token de pedido.

**3. Funções do Azure validar o pedido e autorizar o pedido de acesso ao Cofre de chaves.

**4. O Cofre de chaves** devolve o segredo, neste caso, a cadeia de ligação de base de dados do SQL Server.

**5. Função do Azure** utiliza a cadeia de ligação para ligar à base de dados do SQL Server de forma segura e continua o processamento adicional para armazenar dados ePHI.

Para alcançar o armazenamento dos dados, um esquema de API comuns foi implementado seguindo rápido Healthcare interoperabilidade recursos (FHIR, pronunciados fire). A função foi fornecida os elementos de exchange FHIR seguintes:

-   [Esquema patient](https://www.hl7.org/fhir/patient.html) "quem" inclui informações sobre um patient.

-   [Esquema de observação](https://www.hl7.org/fhir/observation.html) abrange o elemento central no healthcare, utilizada para suportar o diagnóstico, monitorizar o progresso, determinar as linhas de base e padrões de e até mesmo capturar demográficas características. 

-   [Encontrar o esquema](https://www.hl7.org/fhir/encounter.html) abrange os tipos de encontros como ambulatory, emergência, casa estado de funcionamento, inpatient e encontros virtuais.

-   [Esquema de condição](https://www.hl7.org/fhir/condition.html) inclui informações detalhadas sobre uma condição, problema, diagnóstico, ou outros eventos, situação, problema ou conceito clinical que tem risen a um nível de preocupação.  



### <a name="event-grid"></a>Event Grid


A solução suporta grelha de eventos do Azure, um único serviço para gerir o encaminhamento de todos os eventos de uma origem para qualquer destino, desde que:

-   [Segurança e autenticação](/azure/event-grid/security-authentication)

-   [Controlo de acesso baseado em funções](/azure/event-grid/security-authentication#management-access-control) para várias operações de gestão como listar as subscrições de evento, criar novos e gerar chaves

-   Auditoria

## <a name="store"></a>ARQUIVO 

### <a name="sql-database-and-server"></a>Base de dados do SQL Server e servidor 


-   [Encriptação de dados transparente (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) fornece encriptação em tempo real e a desencriptação de dados armazenados na SQL Database do Azure, utilizando uma chave armazenada num cofre de chaves do Azure.

-   [Avaliação da vulnerabilidade SQL](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) é uma fácil de configurar a ferramenta que pode detetar, controlar e remediar potenciais vulnerabilidades de base de dados.

-   [Deteção de ameaças de base de dados do SQL Server](/azure/sql-database/sql-database-threat-detection) ativada.

-   [Auditoria de base de dados SQL](/azure/sql-database/sql-database-auditing) ativada.

-   [Métricas de base de dados SQL e o registo de diagnóstico](/azure/sql-database/sql-database-metrics-diag-logging) ativada.

-   [Regras de firewall de nível do servidor e da base de dados](/azure/sql-database/sql-database-firewall-configure) ter sido tightened.

-   [Sempre encriptado colunas](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) são utilizadas para proteger patient nomes, média e o apelido.
    Além disso, a encriptação de coluna de base de dados também utiliza o Azure Active Directory (AD) para autenticar a aplicação a SQL Database do Azure.

-   Acesso à base de dados SQL e SQL Server está configurado, de acordo com o princípio do menor privilégio.

-   Apenas os endereços IP necessários são permissão de acesso através da firewall do SQL Server.

### <a name="storage-accounts"></a>Contas de armazenamento


-   [Dados em movimento são transferidos utilizando TLS/SSL apenas](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Não é permitido o acesso anónimo para contentores.

-   Regras de alertas são configuradas para o registo de atividade anónima.

-   HTTPS é necessário para aceder a recursos da conta de armazenamento.

-   Dados de pedido de autenticação são iniciados e monitorizados.

-   Dados no Blob storage são encriptados em pausa.

## <a name="analyze"></a>ANALISAR

### <a name="machine-learning"></a>Machine Learning


-   [O registo está ativado](/azure/machine-learning/studio/web-services-logging) do Machine Learning serviços web.
- utilizar [Machine Learning](/azure/machine-learning/desktop-workbench/experimentation-service-configuration) workbench necessita de um desenvolvimento de experimentações, que fornece a capacidade de prever a um conjunto de solução. [Integrar o workbench](/azure/machine-learning/desktop-workbench/using-git-ml-project) pode ajudar a simplificar a gestão de experimentações.

## <a name="security"></a>SEGURANÇA

### <a name="azure-security-center"></a>Centro de Segurança do Azure
- [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) fornece uma vista centralizada de estado de segurança dos seus recursos do Azure. De forma rápida, pode verificar que os controlos de segurança adequados são cumpridos e corretamente configurado e que possa identificar rapidamente quaisquer recursos que necessitam da atenção. 

- [Azure Advisor](/azure/advisor/advisor-overview) é um consultor de nuvem personalizado que o ajuda a seguir as melhores práticas para otimizar as implementações do Azure. Ele analisa a sua telemetria de configuração e utilização de recursos e, em seguida, recomenda soluções que o podem ajudar a melhorar a rentabilidade, o desempenho, a elevada disponibilidade e a segurança dos seus recursos do Azure.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) é um serviço de gestão de desempenho de aplicações (APM) extensível para programadores de web em várias plataformas. Utilize-o para monitorizar a sua aplicação Web online. Detetar anomalias de desempenho. inclui ferramentas de análise poderosas para o ajudar a diagnosticar problemas e a compreender o que os utilizadores efetivamente fazem com a sua aplicação. Foi concebido para o ajudar a melhorar continuamente o desempenho e a usabilidade.

### <a name="azure-alerts"></a>Alertas do Azure
- [Alertas oferecem um método de monitorização de serviços do Azure e permitem-lhe configurar condições através de dados. Alertas também fornecem notificações quando os dados de monitorização corresponde a uma condição de alerta.

### <a name="operations-management-suite-oms"></a>Operations Management Suite (OMS)
[Operations Management Suite (também conhecido como OMS)](/azure/operations-management-suite/operations-management-suite-overview) é uma coleção de serviços de gestão.

-   Área de trabalho está ativada para o Centro de segurança

-   Área de trabalho está ativada para a carga de trabalho de monitorização

-   Monitorização da carga de trabalho está ativada para:

    -   Identidade e Acesso

    -   Segurança e Auditoria

    -   Azure SQL DB Analytics

    -   [Análise do Azure WebApp](/azure/log-analytics/log-analytics-azure-web-apps-analytics) solução

    -   Análise do Key Vault

    -   Monitorização de Alterações

-   [Conector do Application Insights (pré-visualização)](/azure/log-analytics/log-analytics-app-insights-connector) está ativado

-   [Análise de registos de atividade](/azure/log-analytics/log-analytics-activity) está ativado
