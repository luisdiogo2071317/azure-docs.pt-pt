---
title: Aprovisionar novos inquilinos numa aplicação multi-inquilino que utiliza a SQL Database do Azure | Microsoft Docs
description: Saiba como aprovisionar e catálogo novos inquilinos numa aplicação SaaS multi-inquilino SQL Database do Azure
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: 1accc672e396c5a9405369654f9bc4f8463c9afc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Saiba como aprovisionar novos inquilinos e registe-os no catálogo

Neste tutorial, irá aprender a aprovisionar e padrões de SaaS de catálogo. Também Saiba como estão implementados na aplicação de base de dados por inquilino Wingtip bilhetes SaaS. Criar e inicializar novas bases de dados do inquilino e registe-os no catálogo de inquilino da aplicação. O catálogo é uma base de dados que mantém o mapeamento entre muitos inquilinos da aplicação SaaS e os respetivos dados. O catálogo desempenha um papel importante para instruir a aplicação e os pedidos de gestão para a base de dados correto.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]

> * Aprovisione um novo inquilino único.
> * Aprovisione um lote de inquilinos adicionais.


Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação de base de dados por inquilino Wingtip bilhetes SaaS é implementada. Para implementá-la em menos de cinco minutos, consulte o artigo [implementar e explorar a aplicação de base de dados por inquilino Wingtip bilhetes SaaS](saas-dbpertenant-get-started-deploy.md).
* O Azure PowerShell está instalado. Para obter mais informações, consulte [começar com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Introdução ao padrão do catálogo de SaaS

Uma cópia de base de dados multi-inquilino aplicação SaaS, é importante saber onde estão armazenadas informações para cada inquilino. O padrão de catálogo de SaaS, uma base de dados do catálogo é utilizado para conter o mapeamento entre cada inquilino e a base de dados na qual os dados são armazenados. Este padrão aplica-se sempre que os dados do inquilino são distribuídos por várias bases de dados.

Cada inquilino é identificado por uma chave no catálogo, que é mapeado para a localização da respetiva base de dados. Na aplicação Wingtip pedidos de suporte, a chave tem o formato de um hash do nome do inquilino. Este esquema permite que a aplicação construir a chave do nome do inquilino incluído no URL da aplicação. Outros esquemas de chave de inquilino podem ser utilizados.  

O catálogo permite que o nome ou localização da base de dados para ser alterados com um impacto mínimo na aplicação. No modelo de base de dados multi-inquilino, esta capacidade também permite a mover um inquilino entre bases de dados. Também o catálogo pode ser utilizado para indicar se um inquilino ou a base de dados está offline para manutenção ou outras ações. Esta capacidade é explorou no [tutorial de inquilino único restauro](saas-dbpertenant-restore-single-tenant.md).

O catálogo também pode armazenar inquilino adicional ou os metadados da base de dados, tais como a versão de esquema, o plano de serviço ou o SLAs oferecidos aos inquilinos. O catálogo pode armazenar outras informações que permite a gestão de aplicações, o suporte ao cliente ou DevOps. 

Para além da aplicação SaaS, o catálogo pode ativar as ferramentas de base de dados. No exemplo de base de dados por inquilino Wingtip bilhetes SaaS, o catálogo é utilizado para ativar consulta de inquilino em vários locais, que é explorou no [tutorial relatórios Ad-hoc](saas-tenancy-cross-tenant-reporting.md). Gestão de tarefas entre bases de dados é explorou no [gestão esquema](saas-tenancy-schema-management.md) e [inquilino análise](saas-tenancy-tenant-analytics.md) tutoriais. 

Nos exemplos Wingtip bilhetes SaaS, o catálogo é implementado utilizando às funcionalidades de gestão de partições horizontais o [biblioteca de clientes de base de dados elástica (EDCL)](sql-database-elastic-database-client-library.md). O EDCL está disponível em Java e o .NET Framework. O EDCL ativa uma aplicação criar, gerir e utilize um mapa de partições horizontais cópias de base de dados. 

Um mapa de partições horizontais contém uma lista de partições horizontais (bases de dados) e o mapeamento entre as chaves (inquilinos) e shards. Funções EDCL são utilizadas durante o aprovisionamento para criar as entradas no mapa de partições horizontais do inquilino. São utilizados em tempo de execução por aplicações para ligar à base de dados correto. EDCL coloca em cache as informações de ligação para minimizar o tráfego para a base de dados do catálogo e acelerar a aplicação. 

> [!IMPORTANT]
> Os dados de mapeamento estão acessíveis na base de dados do catálogo, mas *não editá-lo*. Edite dados de mapeamento de utilizando apenas APIs de biblioteca de cliente de base de dados elásticas. Manipular diretamente os dados de mapeamento riscos corrupting o catálogo e não é suportada.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Introdução ao padrão de aprovisionamento de SaaS

Quando adiciona um novo inquilino numa aplicação SaaS que utiliza um modelo de base de dados de inquilino único, terá de aprovisionar uma nova base de dados do inquilino. A base de dados tem de ser criado na localização adequada e escalão de serviço. Também deve ser inicializada com o esquema apropriado e dados de referência. E tem de estar registada no catálogo sob a chave de inquilino adequado. 

Diferentes abordagens para aprovisionamento de base de dados podem ser utilizadas. Pode executar scripts de SQL, implemente um bacpac ou copiar uma base de dados do modelo. 

Base de dados de aprovisionamento tem de ser parte da sua estratégia de gestão de esquema. Tem de se certificar de que as novas bases de dados são aprovisionados com o esquema mais recente. Este requisito é explorou no [tutorial de gestão de esquema](saas-tenancy-schema-management.md). 

A aplicação de base de dados por inquilino Wingtip bilhetes Aprovisiona novos inquilinos ao copiar uma base de dados do modelo com o nome _basetenantdb_, que é implementado no servidor de catálogo. Aprovisionamento pode ser integrado na aplicação como parte de uma experiência de inscrição. É também pode ser suportado offline utilizando scripts. Este tutorial explicar aprovisionamento através do PowerShell. 

Aprovisionamento de scripts cópia o _basetenantdb_ base de dados para criar uma nova base de dados do inquilino num agrupamento elástico. Em seguida, os scripts de inicializar a base de dados com informações específicas de inquilino e registá-lo no mapa de partições horizontais catálogo. Bases de dados de inquilino são nomes fornecido com base no nome do inquilino. Este esquema de nomenclatura não é uma parte crucial do padrão. O catálogo mapeia a chave de inquilino para o nome de base de dados, pelo que pode ser utilizada qualquer convenção de nomenclatura. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de aplicação de base de dados por inquilino Wingtip bilhetes SaaS

Os scripts de Wingtip bilhetes SaaS e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip SaaS de pedidos de desbloqueio.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Instruções detalhadas sobre o aprovisionamento e a catalogação

Para compreender como a aplicação de bilhetes Wingtip implementa novo inquilino aprovisionamento, adicionar um ponto de interrupção e siga o fluxo de trabalho ao aprovisionar um inquilino.

1. No ISE do PowerShell, abra... \\Learning módulos\\ProvisionAndCatalog\\_demonstração ProvisionAndCatalog.ps1_ e defina os seguintes parâmetros:

   * **$TenantName** = o nome do novo local (por exemplo, *Bushwillow Blues*).
   * **$VenueType** = um dos tipos venue predefinidos: _blues classicalmusic, dance, jazz, judo, motor racing multipurpose, opera, rockmusic, soccer_.
   * **$DemoScenario** = **1**, *aprovisionar um único inquilino*.

2. Para adicionar um ponto de interrupção, coloque o cursor em qualquer lugar na linha que indica que *inquilino novo '*. Em seguida, prima F9.

   ![Breakpoint](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Para executar o script, prima F5.

4. Depois de parar a execução do script, o ponto de interrupção, prima F11 passo no código.

   ![Depurar](media/saas-dbpertenant-provision-and-catalog/debug.png)



Execução do script de rastreio utilizando a **depurar** opções de menu. Prima F10 e F11 passo ao longo de ou para as funções de chamada. Para obter mais informações sobre a depuração de scripts do PowerShell, consulte [sugestões sobre a trabalhar com e a depuração de scripts do PowerShell como](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Não tem de seguir explicitamente este fluxo de trabalho. Explica como depurar o script.

* **Importe o módulo de CatalogAndDatabaseManagement.psm1.** Fornece um catálogo e ao nível do inquilino abstração relativamente a [gestão de partições horizontais](sql-database-elastic-scale-shard-map-management.md) funções. Este módulo encapsula muito o padrão de catálogo e é, explorar.
* **Importe o módulo de SubscriptionManagement.psm1.** Contém as funções para iniciar sessão no Azure e selecionar a subscrição do Azure que pretende trabalhar.
* **Obter os detalhes de configuração.** Avance para Get-configuração utilizando F11 e ver como a configuração de aplicação é especificada. Os nomes de recursos e outros valores específicos da aplicação são definidos aqui. Não altere estes valores, até que o se familiarizar com os scripts.
* **Obter o objeto de catálogo.** Avance para o Get-catálogo, que composes e devolve um objeto de catálogo que é utilizado no script de nível mais elevado. Esta função utiliza funções de gestão de partições horizontais que são importadas a partir **AzureShardManagement.psm1**. O objeto do catálogo é composto pelos seguintes elementos:

   * $catalogServerFullyQualifiedName é criada utilizando o padrão stem plus o seu nome de utilizador: _catálogo -\<utilizador\>. database.windows .net_.
   * $catalogDatabaseName é obtido a partir da configuração: *tenantcatalog*.
   * O objeto $shardMapManager é inicializado a partir da base de dados do catálogo.
   * O objeto $shardMap é inicializado a partir do mapa de partições horizontais _tenantcatalog_ na base de dados do catálogo. Um objeto de catálogo é composto por e devolvido. É utilizado no script de nível mais elevado.
* **Calcule a nova chave de inquilino.** É utilizada uma função hash para criar a chave de inquilino a partir do nome do inquilino.
* **Verifique se a chave de inquilino já existe.** O catálogo é verificado para assegurar-se de que a chave está disponível.
* **A base de dados do inquilino é aprovisionada com New-TenantDatabase**. Utilizar F11 passo na forma como a base de dados é aprovisionado utilizando um [modelo Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md).

    O nome de base de dados é construído a partir do nome de inquilino para ficar claro quais as partições horizontais que pertencem a que inquilino. Também pode utilizar outras convenções de nomenclatura de base de dados. Um modelo do Resource Manager cria uma base de dados do inquilino ao copiar uma base de dados do modelo (_baseTenantDB_) no servidor de catálogo. Como alternativa, pode criar uma base de dados e inicializá-lo através da importação de um bacpac. Ou pode executar um script de inicialização a partir de uma localização conhecida.

    O modelo do Resource Manager é a pasta de Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

* **A base de dados do inquilino ainda está a ser inicializado.** O nome de venue (inquilino) e o tipo de venue são adicionados. Também pode efetuar outra inicialização aqui.

* **A base de dados de inquilino está registado no catálogo.** Está registado com *adicionar TenantDatabaseToCatalog* utilizando a chave de inquilino. Utilize F11 passo para os detalhes:

    * A base de dados do catálogo é adicionada ao mapa de partições horizontais (a lista de bases de dados conhecidas).
    * É criado o mapeamento que liga o valor da chave à partição horizontal.
    * Metadados adicionais sobre o inquilino (nome do venue) estão adicionado à tabela inquilinos no catálogo. A tabela de inquilinos não faz parte do esquema de gestão de partições horizontais e não está instalado pelo EDCL. Esta tabela ilustra a forma como a base de dados do catálogo pode ser expandido para suportar dados específicos da aplicação adicionais.


Depois de ter concluído o aprovisionamento, execução devolve para original *demonstração ProvisionAndCatalog* script. O **eventos** página abre-se para o novo inquilino no browser.

   ![Página de eventos](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Aprovisionar um lote de inquilinos

Neste exercício aprovisiona um lote de 17 inquilinos. Recomendamos que Aprovisiona este lote de inquilinos antes de iniciar os outros tutoriais de base de dados por inquilino Wingtip bilhetes SaaS. Existem mais do que alguns bases de dados para trabalhar com.

1. No ISE do PowerShell, abra... \\Learning módulos\\ProvisionAndCatalog\\*demonstração ProvisionAndCatalog.ps1*. Alterar o *$DemoScenario* parâmetro para 3:

   * **$DemoScenario** = **3**, *aprovisionar um lote de inquilinos*.
2. Para executar o script, prima F5.

O script implementa um lote de inquilinos adicionais. Utiliza um [modelo Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md) que controla o batch e delega o aprovisionamento de cada base de dados para um modelo ligado. Utilizar modelos desta forma permite que o Azure Resource Manager funcione como o mediador no processo de aprovisionamento do script. Os modelos de aprovisionar bases de dados em paralelo e o identificador de tentativas, se necessário. O script é idempotent, por isso, se esta falha ou interrompe por qualquer motivo, execute-o novamente.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Verifique o lote de inquilinos que foi implementada com êxito

* No [portal do Azure](https://portal.azure.com), navegue até à sua lista de servidores e abra o *tenants1* servidor. Selecione **bases de dados SQL**e certifique-se de que o lote de 17 bases de dados adicionais está agora na lista.

   ![Lista de base de dados](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Outros padrões de aprovisionamento

Outros padrões de aprovisionamento não incluídos neste tutorial:

**Pré-bases de dados de aprovisionamento**: O padrão de pré-aprovisionamento exploits de facto de que as bases de dados num conjunto elástico não adicionar custos adicionais associados. Faturação é para o conjunto elástico, não as bases de dados. Bases de dados Inativas consumam não existem recursos. Através da pré-aprovisionamento bases de dados de um conjunto e atribuir-lhes quando for necessário, pode reduzir o tempo para adicionar os inquilinos. O número de bases de dados previamente aprovisionadas pode ser ajustado conforme necessário para manter uma memória intermédia adequado para a velocidade de aprovisionamento prevista.

**Aprovisionamento automático**: padrão de aprovisionamento automático, um serviço de aprovisionamento Aprovisiona servidores, os conjuntos e as bases de dados automaticamente, conforme necessário. Se quiser, pode incluir previamente aprovisionamento conjuntos elásticos de bases de dados. Se as bases de dados estão desativadas e eliminados, podem ser preenchidas lacunas na conjuntos elásticos pelo serviço de aprovisionamento. Tal serviço pode ser simples ou complexas, tais como processamento aprovisionar em várias localizações geográficas e ao configurar a georreplicação para recuperação após desastre. 

Com o padrão de aprovisionamento automático, uma aplicação de cliente ou script submete um pedido de aprovisionamento para uma fila a serem processados pelo serviço de aprovisionamento. Em seguida, consulta o serviço para determinar a conclusão. Se for utilizado previamente aprovisionamento, os pedidos são processados rapidamente. O serviço Aprovisiona uma base de dados de substituição em segundo plano.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]

> * Aprovisione um novo inquilino único.
> * Aprovisione um lote de inquilinos adicionais.
> * Avance para os detalhes de aprovisionamento de inquilinos e registando-los para o catálogo.

Repita o [tutorial de monitorização de desempenho](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Recursos adicionais

* Adicionais [tutoriais que criar na aplicação de base de dados por inquilino Wingtip bilhetes SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Biblioteca de clientes da base de dados elástica](sql-database-elastic-database-client-library.md)
* [Depurar scripts no ISE do Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
