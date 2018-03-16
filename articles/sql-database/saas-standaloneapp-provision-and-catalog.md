---
title: Tutorial de SaaS multi-inquilino - SQL Database do Azure | Microsoft Docs
description: "Aprovisionar e novos inquilinos através do padrão de aplicação autónoma do catálogo"
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: SaaS
ms.topic: article
ms.date: 01/31/2018
ms.author: billgib
ms.openlocfilehash: 148a50d07d4cea7adda493b283766d22d26b81e2
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Aprovisionar e catálogo novos inquilinos utilizando a aplicação por inquilino padrão de SaaS

Este artigo abrange o aprovisionamento e cataloging de novos inquilinos com a aplicação autónoma por inquilino SaaS padrão.
Este artigo tem duas partes principais:
* Debate conceptual de aprovisionamento e cataloging novos inquilinos
* Um tutorial que realça o código do PowerShell de exemplo que elimina o aprovisionamento e cataloging
    * O tutorial utiliza a aplicação de SaaS Wingtip bilhetes de exemplo, adaptada descrita para a aplicação autónoma por padrão de inquilino.

## <a name="standalone-application-per-tenant-pattern"></a>Aplicação autónoma por padrão de inquilino
A aplicação autónoma por padrão de inquilino é uma das várias padrões para aplicações de SaaS multi-inquilino.  Neste padrão, uma aplicação autónoma é aprovisionada para cada inquilino. A aplicação é composto por componentes de nível de aplicação e uma base de dados do SQL Server.  Cada aplicação de inquilino pode ser implementada na subscrição do fornecedor.  Em alternativa, o Azure oferece uma [programa de aplicações geridas](https://docs.microsoft.com/azure/managed-applications/overview) em que uma aplicação pode ser implementada na subscrição do inquilino e gerida pelo fabricante em nome do inquilino. 

   ![padrão de aplicação por inquilino](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Quando implementar uma aplicação para um inquilino, a aplicação e a base de dados são aprovisionados num novo grupo de recursos criado para o inquilino.  Utilizar grupos de recursos separado isola os recursos de aplicação de cada inquilino e permite que sejam geridos de forma independente. Em cada grupo de recursos, cada instância da aplicação está configurada para aceder diretamente à respetiva base de dados correspondente.  Este modelo de ligação contrasta com outros padrões que utilizam um catálogo para o Mediador de ligações entre a aplicação e a base de dados.  E como não é não existe nenhuma partilha de recursos, cada base de dados do inquilino têm de ser aprovisionado com recursos suficientes para processar o pico de carga. Este padrão tende a ser utilizado para aplicações SaaS com menos de inquilinos, onde existe uma forte ênfase ao inquilino isolamento e menos ênfase em custos de recursos.  

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Utilizar um catálogo de inquilino com a aplicação por padrão de inquilino
Enquanto a aplicação e a base de dados de cada inquilino estão completamente isolados, vários cenários de análise e gestão poderão funcionar entre inquilinos.  Por exemplo, aplicar uma alteração de esquema para uma nova versão da aplicação requer alterações ao esquema de cada base de dados do inquilino. Cenários de relatórios e análise também poderão precisar de acesso para todos os inquilinos bases de dados, independentemente de onde são implementados.

   ![padrão de aplicação por inquilino](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

O catálogo de inquilino contém um mapeamento entre um identificador de inquilino e uma base de dados do inquilino, permitindo que um identificador que seja resolvido para um servidor e um nome de base de dados.  Na aplicação Wingtip SaaS, o identificador de inquilino é calculado como um hash do nome do inquilino, embora possam ser utilizados outros esquemas.  Enquanto o catálogo para gerir as ligações não precisam de aplicações autónomo, o catálogo pode ser utilizado para definir o âmbito outras ações para um conjunto de bases de dados do inquilino. Por exemplo, consulta elástico pode utilizar o catálogo para determinar o conjunto de bases de dados em que as consultas são distribuídas para os relatórios de inquilino em vários locais.

## <a name="elastic-database-client-library"></a>Biblioteca de Clientes da Base de Dados Elástica
Aplicação de exemplo Wingtip, o catálogo é implementado por às funcionalidades de gestão de partições horizontais o [biblioteca de clientes de base de dados elásticas](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (EDCL).  A biblioteca ativa uma aplicação criar, gerir e utilize um mapa de partições horizontais que está armazenado numa base de dados. No exemplo Wingtip permissões, o catálogo é armazenado no *catálogo inquilino* base de dados.  O ID de partição horizontal mapeia uma chave de inquilino para o ID de partição horizontal (base de dados) no qual os dados de nesse inquilino são armazenados.  Gerir funções EDCL um *mapa de partições horizontais global* armazenados nas tabelas no *catálogo inquilino* base de dados e um *mapa de partições horizontais local* armazenados em cada partição horizontal.

Funções EDCL podem ser chamadas a partir de aplicações ou scripts do PowerShell para criar e gerir as entradas no mapa de partições horizontais. Outras funções EDCL podem ser utilizadas para obter o conjunto de partições horizontais ou ligar à base de dados correta para fornecido chave de inquilino. 
    
> [!IMPORTANT] 
> Não edite os dados na base de dados de catálogo ou o mapa de partições horizontais local nas bases de dados do inquilino diretamente. Atualizações diretas não são suportadas devido a alto risco de danos nos dados. Em vez disso, edite os dados de mapeamento utilizando EDCL APIs apenas.

## <a name="tenant-provisioning"></a>Aprovisionamento do inquilino 
Cada inquilino requer um novo grupo de recursos do Azure, que tem de ser criado antes de recursos podem ser aprovisionados dentro da mesma. Assim que o grupo de recursos existe, um modelo de gestão de recursos do Azure pode ser utilizado para implementar os componentes da aplicação e a base de dados e, em seguida, configure a ligação de base de dados. Para inicializar o esquema de base de dados, o modelo pode importar um ficheiro de bacpac.  Em alternativa, a base de dados pode ser criado como uma cópia da base de dados 'template'.  A base de dados é, em seguida, atualizado com dados venue inicial adicional e registada no catálogo.

## <a name="tutorial"></a>Tutorial

Neste tutorial, ficará a saber como:
* Aprovisionar um catálogo
* Registar as bases de dados de inquilino de exemplo que implementou anteriormente no catálogo
* Aprovisionar um inquilino adicional e registá-lo no catálogo

Um modelo Azure Resource Manager é utilizado para implementar e configurar a aplicação, crie a base de dados do inquilino e, em seguida, importar um ficheiro de bacpac ao inicializá-lo. O pedido de importação pode ser colocados em fila durante vários minutos antes de ser alvo de ação.

No final deste tutorial, tem um conjunto de aplicações do inquilino autónoma, com cada base de dados registada no catálogo.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes: 
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* As três aplicações de inquilino de exemplo são implementadas. Para implementar estas aplicações em menos de cinco minutos, consulte [implementar e explorar o padrão de aplicação autónoma do Wingtip bilhetes SaaS](https://docs.microsoft.com/azure/sql-database/saas-standaloneapp-get-started-deploy).

## <a name="provision-the-catalog"></a>Aprovisionar o catálogo
Nesta tarefa, saiba como aprovisionar o catálogo utilizado para registar todas as bases de dados do inquilino. Irá: 
* **Aprovisionar a base de dados do catálogo** através de um modelo de gestão de recursos do Azure. A base de dados é inicializado ao importar um ficheiro de bacpac.  
* **Registe as aplicações de inquilino de exemplo** que implementou anteriormente.  Cada inquilino está registado com uma chave construída a partir de um hash do nome do inquilino.  O nome de inquilino também é armazenado numa tabela de extensão no catálogo.

1. No ISE do PowerShell, abra *...\Learning Modules\UserConfig.psm* e atualizar o  **\<utilizador\>**  valor para o valor utilizado durante a implementação de três aplicações de exemplo.  **Guarde o ficheiro**.  
1. No ISE do PowerShell, abra *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* e defina **$Scenario = 1**. Implementar o catálogo de inquilino e registe os inquilinos predefinidos.

1. Adicionar um ponto de interrupção, colocando o cursor em qualquer lugar na linha que indica, `& $PSScriptRoot\New-Catalog.ps1`e, em seguida, prima **F9**.

    ![a definição de um ponto de interrupção para o rastreio](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. Execute o script, premindo **F5**. 
1.  Depois de parar a execução do script, o ponto de interrupção, prima **F11** para o passo para o script New-Catalog.ps1.
1.  Execução do script utilizando as opções de menu de depuração, F10 e F11, ao passo ao longo de ou para as funções de chamada de rastreio.
    *   Para obter mais informações sobre a depuração de scripts do PowerShell, consulte [sugestões sobre a trabalhar com e a depuração de scripts do PowerShell como](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

Após a conclusão do script, o catálogo existirá e todos os inquilinos de exemplo serão registados. 

Ver agora os recursos que criou.

1. Abra o [portal do Azure](https://portal.azure.com/) e procure os grupos de recursos.  Abra o **wingtip-sa-catálogo -\<utilizador\>**  recursos de grupo e anote o servidor de catálogo e a base de dados.
1. Abrir a base de dados no portal e selecione *Explorador de dados* no menu esquerdo.  Clique no comando de início de sessão e, em seguida, introduza a palavra-passe =  **P@ssword1** .


1. Explorar o esquema do *tenantcatalog* base de dados.  
   * Os objetos para o `__ShardManagement` esquema são fornecidos pela biblioteca de clientes de base de dados elásticas.
   * O `Tenants` tabela e `TenantsExtended` vista são extensões adicionadas no exemplo que demonstram como pode expandir o catálogo para fornecer valor adicional.
1. Execute a consulta, `SELECT * FROM dbo.TenantsExtended`.          

   ![Explorador de dados](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Como alternativa à utilização do Explorador de dados pode ligar à base de dados do SQL Server Management Studio. Para fazê-lo, ligue ao servidor wingtip- 

    
    Tenha em atenção que não deve editar dados diretamente no catálogo - sempre utilizar as APIs de gestão de partições horizontais. 

## <a name="provision-a-new-tenant-application"></a>Aprovisionar uma nova aplicação de inquilino

Nesta tarefa, saiba como aprovisionar uma aplicação de inquilino único. Irá:  
* **Criar um novo grupo de recursos** para o inquilino. 
* **Aprovisionar a aplicação e a base de dados** para o novo grupo de recursos através de um modelo de gestão de recursos do Azure.  Esta ação inclui a inicializar a base de dados com o esquema comum e dados de referência através da importação de um ficheiro de bacpac. 
* **Inicializar a base de dados com as informações básicas de inquilinos**. Esta ação inclui a especificação do tipo venue, que determina a fotografia utilizada como o fundo no seu web site de eventos. 
* **Registar a base de dados na base de dados de catálogo**. 

1. No ISE do PowerShell, abra *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* e defina **$Scenario = 2**. Implemente o catálogo de inquilino e registe os inquilinos predefinidos

1. Adicionar um ponto de interrupção no script, colocando o cursor em qualquer lugar na linha 49 que indica, `& $PSScriptRoot\New-TenantApp.ps1`e, em seguida, prima **F9**.
1. Execute o script, premindo **F5**. 
1.  Depois de parar a execução do script, o ponto de interrupção, prima **F11** para o passo para o script New-Catalog.ps1.
1.  Execução do script utilizando as opções de menu de depuração, F10 e F11, ao passo ao longo de ou para as funções de chamada de rastreio.

Depois do inquilino tiver sido aprovisionado, é aberto o Web site de eventos ao novo inquilino.

   ![Red maple racing](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Pode, em seguida, Inspecione os novos recursos criados no portal do Azure. 

   ![vermelho maple racing recursos](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Para parar a faturação, eliminar grupos de recursos ##

Quando tiver concluído a explorar a amostra, elimine todos os grupos de recursos que criou para parar a faturação associada.

## <a name="additional-resources"></a>Recursos adicionais

- Para obter mais informações sobre aplicações de base de dados de SaaS multi-inquilino, consulte o artigo [padrões para aplicações de SaaS multi-inquilino de conceção](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber:

> [!div class="checklist"]
> * Como implementar a aplicação autónoma do Wingtip pedidos de suporte de SaaS.
> * Sobre os servidores e bases de dados que constituem a aplicação.
> * Como eliminar os recursos de exemplo para parar a faturação relacionada.

Pode explorar como o catálogo é utilizado para suportar vários cenários de inquilino entre utilizar a versão da base de dados por inquilino do [aplicação SaaS de bilhetes Wingtip](https://docs.microsoft.com/azure/sql-database/saas-dbpertenant-wingtip-app-overview).  
