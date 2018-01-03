---
title: Aprovisionar em SaaS multi-inquilino do Azure | Microsoft Docs
description: "Saiba como aprovisionar e novos inquilinos na aplicação de SaaS SQL Database do Azure multi-inquilino de catálogo"
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: MightyPen
ms.reviewer: billgib;andrela;genemi
ms.assetid: 
ms.service: sql-database
ms.custom: saas apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: billgib
ms.openlocfilehash: 42bbb6131aa71520410b22af4d74e99a63fe81cf
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Aprovisionar e catálogo novos inquilinos numa aplicação SaaS utilizando uma base de dados de SQL do Azure em partição horizontal do multi-inquilino

Este artigo abrange o aprovisionamento e cataloging de novos inquilinos, num *a multi-inquilino de base de dados* modelo ou padrão.

Este artigo tem duas partes principais:

- [Debate conceptual](#goto_2_conceptual) o aprovisionamento e cataloging de novos inquilinos.

- [Tutorial](#goto_1_tutorial) que realça o código de script do PowerShell que executa a operação de aprovisionamento e cataloging.
    - O tutorial utiliza a aplicação SaaS de bilhetes Wingtip, adaptada descrita o padrão de base de dados a multi-inquilino.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Padrão de base de dados

Nesta secção, mais alguns mais que siga, analisam os conceitos do padrão a multi-inquilino de base de dados.

Neste modelo a multi-inquilino, os esquemas de tabela dentro de cada base de dados incluem uma chave de inquilino na chave primária de tabelas que armazenam dados de inquilino. A chave de inquilino permite que cada base de dados individual armazenar 0, 1 ou muitos inquilinos. A utilização de bases de dados em partição horizontal torna mais fácil para o sistema de aplicação suportar um grande número de inquilinos. Todos os dados para qualquer um inquilino é armazenada na base de dados de um. Grande número de inquilinos é distribuído por muitas bases de dados em partição horizontal. Uma base de dados do catálogo armazena o mapeamento de cada inquilino para a respetiva base de dados.

#### <a name="isolation-versus-lower-cost"></a>Isolamento versus custo mais baixo

Um inquilino que tenha uma base de dados para si próprio gostar as vantagens de isolamento. O inquilino pode ter a base de dados restaurada para uma data anterior sem ser restringida pelo impacto nos outros inquilinos. Desempenho de base de dados pode ser otimizado para otimizar a um inquilino, novamente sem ter de comprometer com outros inquilinos. O problema é que o isolamento de custos mais do que os custos de partilhar uma base de dados com outros inquilinos.

Quando um novo inquilino é aprovisionado, pode partilhar uma base de dados com outros inquilinos ou pode ser colocado na sua própria base de dados de novo. Mais tarde pode mudar de ideias e mover a base de dados para outra a situação.

Bases de dados com múltiplos inquilinos e inquilinos único são misto na mesma aplicação SaaS, para otimizar os custos ou de isolamento para cada inquilino.

   ![A base de dados do multi-inquilino de aplicação com o catálogo de inquilino](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Padrão de catálogo de inquilino

Quando tiver duas ou mais bases de dados que cada uma contém, pelo menos, um inquilino, a aplicação tem de ter uma forma para detetar a base de dados armazena o inquilino de interesse atual. Uma base de dados do catálogo armazena desta mapeamento.

#### <a name="tenant-key"></a>Chave de inquilino

Para cada inquilino, a aplicação de Wingtip pode derivar uma chave exclusiva, que é a chave de inquilino. A aplicação extrai o nome do inquilino a partir do URL de página Web. A aplicação codifica o nome para obter a chave. A aplicação utiliza a chave para aceder ao catálogo. O catálogo cross-references informações sobre a base de dados na qual está armazenado o inquilino. A aplicação utiliza as informações da base de dados para se ligar. Também podem ser utilizados outros esquemas de chave de inquilino.

Utilizar um catálogo permite que o nome ou localização de uma base de dados do inquilino para ser alterada após o aprovisionamento sem perturbar a aplicação. No modelo de base de dados do multi-inquilino, o catálogo acomoda mover um inquilino entre bases de dados.

#### <a name="tenant-metadata-beyond-location"></a>Metadados de inquilino para além da localização

O catálogo também pode indicar se um inquilino está offline para manutenção ou outras ações. E o catálogo pode ser expandido para armazenar inquilino adicional ou os metadados da base de dados, tais como os seguintes itens:
- A camada de serviço ou a edição de uma base de dados.
- A versão do esquema da base de dados.
- O nome de inquilino e o respetivo SLA (contrato de nível de serviço).
- Informações para ativar a gestão de aplicações, o suporte ao cliente ou processos de devops.  

O catálogo também pode ser utilizado para ativar a gestão de esquema de relatórios, cross-inquilinos e extrair dados para efeitos de análise. 

### <a name="elastic-database-client-library"></a>Biblioteca de Clientes da Base de Dados Elástica 

No Wingtip, o catálogo está implementado a *tenantcatalog* base de dados. O *tenantcatalog* é criado utilizando às funcionalidades de gestão de partições horizontais o [biblioteca de cliente de base de dados elásticas (EDCL)](sql-database-elastic-database-client-library.md). A biblioteca ativa uma aplicação criar, gerir e utilizar um *mapa de partições horizontais* que está armazenada numa base de dados. Um mapa de partições horizontais cross-references a chave de inquilino com o ID de partição horizontal, que significa que a respetiva base de dados em partição horizontal.

Durante o aprovisionamento do inquilino, funções EDCL podem ser utilizadas de aplicações ou scripts do PowerShell para criar as entradas no mapa de partições horizontais. Mais tarde as funções EDCL podem ser utilizadas para ligar à base de dados correto. O EDCL coloca em cache as informações da ligação para minimizar o tráfego na base de dados de catálogo e acelerar o processo de ligação.

> [!IMPORTANT]
> Efetue *não* edite os dados na base de dados de catálogo através de acesso direto! Atualizações diretas não são suportadas devido a alto risco de danos nos dados. Em vez disso, edite os dados de mapeamento utilizando EDCL APIs apenas.

## <a name="tenant-provisioning-pattern"></a>Padrão de aprovisionamento do inquilino

#### <a name="checklist"></a>Lista de verificação

Quando pretender aprovisionar um novo inquilino na base de dados partilhado existente, da base de dados partilhado tem faz as perguntas seguintes:
- Tem espaço suficiente para o novo inquilino?
- Tem tabelas com os dados de referência necessários para o novo inquilino ou podem ser adicionados os dados?
- Tem a variação adequada do esquema de base para o novo inquilino?
- É a localização geográfica adequada próximos ao novo inquilino?
- É na camada de serviço à direita para o novo inquilino?

Quando pretender que o novo inquilino para ser isolada na sua própria base de dados, pode criar para satisfazer as especificações para o inquilino.

Após o aprovisionamento estiver concluído, tem de registar o inquilino no catálogo. Por fim, o mapeamento do inquilino pode ser adicionado para referenciar o ID de partição horizontal adequado.

#### <a name="template-database"></a>Base de dados do modelo

Aprovisione a base de dados ao executar scripts de SQL, implementar um bacpac ou copiar uma base de dados do modelo. As aplicações de Wingtip copiar uma base de dados do modelo para criar novas bases de dados do inquilino.

Como qualquer aplicação, será evoluir Wingtip ao longo do tempo. Por vezes, Wingtip irá exigir alterações à base de dados. As alterações podem incluir os seguintes itens:
- Esquema de nova ou alterada.
- Dados de referência de novas ou alteradas.
- Tarefas de manutenção de rotina da base de dados para garantir o desempenho da aplicação ideal.

Com uma aplicação SaaS, estas alterações têm de ser implementadas de forma coordenada através de uma frota potencialmente massiva de bases de dados de inquilinos. Para estas alterações no futuro estar bases de dados do inquilino, necessitam de ser incorporadas no processo de aprovisionamento. Este desafio é explorou posteriormente o [tutorial de gestão de esquema](saas-tenancy-schema-management.md).

#### <a name="scripts"></a>Scripts

Os scripts de aprovisionamento do inquilino neste tutorial suportam ambos os cenários seguintes:
- Aprovisionamento de um inquilino para uma base de dados partilhado com outros inquilinos.
- Aprovisionamento de um inquilino na sua própria base de dados.

Dados de inquilino, em seguida, são inicializados e registados no mapa de partições horizontais de catálogo. Na aplicação de exemplo, as bases de dados que contêm vários inquilinos recebem um nome genérico, tais como *tenants1* ou *tenants2*. Bases de dados que contém um único inquilino recebem o nome do inquilino. As convenções de nomenclatura específicas utilizadas no exemplo não são uma parte crucial do padrão, como a utilização de um catálogo permite que qualquer nome a atribuir à base de dados.  

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Tutorial de início

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Aprovisionar um inquilino para uma base de dados do multi-inquilino
> * Aprovisionar um inquilino para uma base de dados de inquilino único
> * Aprovisionar um lote de inquilinos em bases de dados do multi-inquilinos e de inquilino único
> * Registar uma base de dados e o mapeamento no catálogo do inquilino

#### <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

- O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

- A aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino](saas-multitenantdb-get-started-deploy.md)

- Obter os scripts de Wingtip e o código de origem:
    - Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) repositório do GitHub.
    - Consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip de desbloqueio. 

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Aprovisionar um inquilino numa base de dados *partilhado* com outros inquilinos

Nesta secção, é apresentada uma lista das ações principais para o aprovisionamento efetuadas pelos scripts do PowerShell. Em seguida, utilize o depurador do ISE do PowerShell para seguir os scripts para ver as ações no código.

#### <a name="major-actions-of-provisioning"></a>Ações de principais de aprovisionamento

Seguem-se entre os principais elementos do fluxo de trabalho aprovisionamento, siga os passos:

- **Calcular a nova chave de inquilino**: uma função de hash é utilizada para criar a chave de inquilino do nome do inquilino.
- **Verifique se a chave de inquilino já existe**: O catálogo é verificado para garantir a chave já não foi registada.
- **Inicializar o inquilino na base de dados do inquilino predefinido**: A base de dados do inquilino é atualizado para adicionar as novas informações de inquilino.  
- **Registar o inquilino no catálogo**: O mapeamento entre a nova chave de inquilino e a base de dados existente do tenants1 é adicionado ao catálogo. 
- **Adicionar o nome do inquilino a uma tabela de extensão de catálogo**: O nome de venue é adicionado à tabela inquilinos no catálogo.  Esta adição mostra como a base de dados do catálogo pode ser expandido para suportar dados específicos da aplicação adicionais.
- **Abra página de eventos para o novo inquilino**: O *Bushwillow Blues* é abrir a página de eventos no browser.

   ![eventos](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Passos de depurador

Para compreender a forma como a aplicação de Wingtip implementa novo inquilino aprovisionamento numa base de dados partilhada, adicione um ponto de interrupção e passo através do fluxo de trabalho:

1. No *ISE do PowerShell*, abra... \\Learning módulos\\ProvisionTenants\\*demonstração ProvisionTenants.ps1* e defina os seguintes parâmetros:
   - **$TenantName** = **Bushwillow Blues**, o nome de um novo venue.
   - **$VenueType** = **blues**, um dos tipos venue predefinidos: blues classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (em minúsculas, sem espaços).
   - **$DemoScenario** = **1**, para aprovisionar um inquilino na base de dados partilhada com outros inquilinos.

2. Adicionar um ponto de interrupção, colocando o cursor em qualquer lugar na linha 38, a linha que diz: *inquilino novo '*e, em seguida, prima **F9**.

   ![ponto de interrupção](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. Execute o script, premindo **F5**.

4. Depois de parar a execução do script, o ponto de interrupção, prima **F11** para o passo no código.

   ![depurar](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. Execução do script de rastreio utilizando a **depurar** opções de menu **F10** e **F11**, ao passo ao longo de ou para as funções de chamada.

Para obter mais informações sobre a depuração de scripts do PowerShell, consulte [sugestões sobre a trabalhar com e a depuração de scripts do PowerShell como](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

## <a name="provision-a-tenant-in-its-own-database"></a>Aprovisionar um inquilino na respetiva *própria* base de dados

#### <a name="major-actions-of-provisioning"></a>Ações de principais de aprovisionamento

Seguem-se entre os principais elementos do fluxo de trabalho, siga os passos enquanto o script de rastreio:

- **Calcular a nova chave de inquilino**: uma função de hash é utilizada para criar a chave de inquilino do nome do inquilino.
- **Verifique se a chave de inquilino já existe**: O catálogo é verificado para garantir a chave já não foi registada.
- **Criar uma nova base de dados do inquilino**: A base de dados é criado ao copiar o *basetenantdb* da base de dados com um modelo do Resource Manager.  O novo nome de base de dados é baseado no nome do inquilino.
- **Adicionar a base de dados ao catálogo**: A nova base de dados de inquilino está registado como um ID de partição horizontal no catálogo.
- **Inicializar o inquilino na base de dados do inquilino predefinido**: A base de dados do inquilino é atualizado para adicionar as novas informações de inquilino.  
- **Registar o inquilino no catálogo**: O mapeamento entre a nova chave de inquilino e a *sequoiasoccer* base de dados é adicionada ao catálogo.
- **Nome do inquilino é adicionada ao catálogo de**: O nome de venue é adicionado à tabela de extensão de inquilinos no catálogo.
- **Abra página de eventos para o novo inquilino**: O *Sequoia Soccer* é abrir a página de eventos no browser.

   ![eventos](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Passos de depurador

Agora guiá-lo durante o processo de script ao criar um inquilino na sua própria base de dados:

1. Ainda no... \\Learning módulos\\ProvisionTenants\\*demonstração ProvisionTenants.ps1* definir os seguintes parâmetros:
   - **$TenantName** = **sequoia Soccer**, o nome de um novo venue.
   - **$VenueType** = **soccer**, um dos tipos venue predefinidos: blues classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (minúsculas, sem espaços).
   - **$DemoScenario** = **2**, para aprovisionar um inquilino na sua própria base de dados.

2. Adicionar um novo ponto de interrupção, colocando o cursor em qualquer lugar na linha 57, a linha que diz:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*e prima **F9**.

   ![ponto de interrupção](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. Execute o script, premindo **F5**.

4. Depois de parar a execução do script, o ponto de interrupção, prima **F11** para o passo no código.  Utilize **F10** e **F11** passo ao longo e avance para as funções para a execução de rastreio.

## <a name="provision-a-batch-of-tenants"></a>Aprovisionar um lote de inquilinos

Neste exercício aprovisiona um lote de 17 inquilinos. Recomenda-se que aprovisionar este lote de inquilinos antes de iniciar os outros tutoriais de bilhetes Wingtip pelo que existem mais bases de dados para trabalhar com.

1. No *ISE do PowerShell*, abra... \\Learning módulos\\ProvisionTenants\\*demonstração ProvisionTenants.ps1* e altere o *$DemoScenario* parâmetro a 4:
   - **$DemoScenario** = **4**, para aprovisionar um lote de inquilinos numa base de dados partilhado.

2. Prima **F5** e execute o script.

### <a name="verify-the-deployed-set-of-tenants"></a>Verifique se o conjunto implementado de inquilinos 

Nesta fase, tem uma combinação de inquilinos implementados para uma base de dados partilhada e inquilinos implementados para as suas próprias bases de dados. O portal do Azure pode ser utilizado para inspecionar as bases de dados criadas. No [portal do Azure](https://portal.azure.com), abra o **tenants1-mt -\<utilizador\>**  servidor ao navegar para a lista de servidores SQL.  O **bases de dados SQL** lista deve incluir partilhado **tenants1** base de dados e as bases de dados para os inquilinos que estão na sua própria base de dados:

   ![lista de bases de dados](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Enquanto o portal do Azure mostra o inquilino bases de dados, não permitem-lhe ver os inquilinos *dentro* a base de dados partilhado. A lista completa dos inquilinos pode ser vista no **Hub de eventos** página Web de Wingtip e procurando o catálogo.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Utilizando a página de hub de eventos de pedidos de Wingtip

Abra a página de Hub de eventos no browser (http:events.wingtip-mt.\<utilizador\>. trafficmanager.net)  

#### <a name="using-catalog-database"></a>Utilizar base de dados do catálogo

A lista completa de inquilinos e a base de dados correspondente para cada está disponível no catálogo. Uma vista SQL é desde que o nome de inquilino de associações para o nome de base de dados. A vista será demonstra o valor de expandir os metadados armazenados no catálogo.
- A vista SQL está disponível na base de dados tenantcatalog.
- O nome do inquilino é armazenado na tabela de inquilinos.
- O nome de base de dados é armazenado nas tabelas de gestão de partições horizontais.

1. No SQL Server Management Studio (SSMS), ligar ao servidor de inquilinos em **catálogo mt.\<utilizador\>. database.windows.net**, com início de sessão = **programador**e a palavra-passe =**P@ssword1**

    ![Caixa de diálogo de ligação de SSMS](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. No Explorador de objeto do SSMS, navegue para as vistas na *tenantcatalog* base de dados.

3. Clique na vista *TenantsExtended* e escolha **selecionar primeiras 1000 linhas**. Tenha em atenção o mapeamento entre o nome de inquilino e a base de dados para os inquilinos diferentes.

    ![Vista de ExtendedTenants no SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Outros padrões de aprovisionamento

Esta secção descreve outras padrões interessantes de aprovisionamento.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Pré-aprovisionar bases de dados em conjuntos elásticos

O padrão de pré-aprovisionamento exploits o facto de que, quando utilizar conjuntos elásticos, faturação é para o conjunto não as bases de dados. Deste modo, bases de dados podem ser adicionados ao conjunto elástico antes de que forem necessárias sem incorrer em custos adicionais associados. Este previamente visioning significativamente reduz o tempo necessário para aprovisionar um inquilino numa base de dados. O número de bases de dados previamente aprovisionadas pode ser ajustado conforme necessário para manter uma memória intermédia adequado para a velocidade de aprovisionamento prevista.

#### <a name="auto-provisioning"></a>Aprovisionamento automático

O padrão de aprovisionamento automático de um serviço de aprovisionamento dedicada é utilizado para aprovisionar servidores, os conjuntos e as bases de dados automaticamente conforme necessário. Esta automatização inclui o aprovisionamento previamente das bases de dados em conjuntos elásticos. E se as bases de dados estão desativadas e eliminados, lacunas que isto cria em conjuntos elásticos podem ser preenchidas pelo serviço de aprovisionamento conforme pretendido.

Este tipo de serviço automático pode ser simples ou complexas. Por exemplo, a automatização foi possível processar a aprovisionar em várias localizações geográficas e conseguiu configurar a georreplicação para recuperação após desastre. Com o padrão de aprovisionamento automático, uma aplicação de cliente ou script seria submeter um pedido de aprovisionamento para uma fila para ser processado por um serviço de aprovisionamento. O script, em seguida, deverá consultar para detetar conclusão. Se for utilizado previamente aprovisionamento, pedidos seriam rapidamente, processados enquanto um serviço em segundo plano iria gerir o aprovisionamento de uma base de dados de substituição.

## <a name="additional-resources"></a>Recursos adicionais

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Biblioteca de clientes da base de dados elástica](sql-database-elastic-database-client-library.md)
- [How to Debug Scripts in Windows PowerShell ISE (Como Depurar os Scripts do ISE do Windows PowerShell)](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Aprovisionar um novo inquilino único para uma base de dados partilhada do multi-inquilino e a sua própria base de dados
> * Aprovisionar um lote de inquilinos adicionais
> * Siga os passos os detalhes de aprovisionamento de inquilinos e registá-los para o catálogo

Repita o [tutorial de monitorização de desempenho](saas-multitenantdb-performance-monitoring.md).

