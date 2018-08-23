---
title: Como migrar e publicar uma aplicação Web num serviço Cloud do Azure a partir do Visual Studio | Documentos da Microsoft
description: Saiba como migrar e publicar a sua aplicação web num serviço cloud do Azure com o Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: cf1e01d0165d91570c3985850b0a780b61c6b8e5
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058219"
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Como: migrar e publicar uma aplicação Web num serviço Cloud do Azure a partir do Visual Studio

Para tirar partido dos serviços de alojamento e capacidade de dimensionamento do Azure, pode querer migrar e implementar a sua aplicação web num serviço cloud do Azure. Apenas alterações mínimas são necessárias. Este artigo aborda a implantação em serviços de cloud apenas; para o serviço de aplicações, consulte [implementar uma aplicação web no App Service do Azure](app-service/app-service-deploy-local-git.md).

> [!Important]
> Esta migração só é suportada para os projetos específicos do ASP.NET, o Silverlight, o WCF e o fluxo de trabalho do WCF. Não é suportada para projetos do ASP.NET Core. Ver [modelos de projeto suportados](#supported-project-templates).

## <a name="migrate-a-project-to-cloud-services"></a>Migrar um projeto para serviços cloud

1. O projeto de aplicativo web com o botão direito e selecione **converter > converter para o projeto de serviço em nuvem do Microsoft Azure**. (Observe que este comando não é apresentada se já tiver um projeto de função da web na solução).
1. Visual Studio cria um projeto de serviço em nuvem na solução que contém a função da web necessária. O nome deste projeto é o mesmo que o seu projeto de aplicativo com mais o sufixo `.Azure`.
1. O Visual Studio também define a **Copy Local** propriedade como true para todos os assemblies que são necessários para a MVC 2, MVC 3, 4 de MVC e aplicativos de negócios do Silverlight. Esta propriedade adiciona esses assemblies ao pacote de serviço que é utilizado para implementação.

   > [!Important]
   > Se tiver outros assemblies ou ficheiros que são necessários para esta aplicação web, tem de definir manualmente as propriedades para esses ficheiros. Para obter informações sobre como definir estas propriedades, consulte [incluir ficheiros no pacote de serviço](#include-files-in-the-service-package).

### <a name="errors-and-warnings"></a>Erros e avisos

Todos os avisos e erros que ocorrem indicam problemas a serem corrigidos antes de implementar para o Azure, tais como assemblies em falta.

Se criar seu aplicativo, executá-lo localmente, utilizando o emulador de computação ou publicá-lo para o Azure, poderá ver o erro: "o caminho especificado, o nome de ficheiro ou ambos são demasiado longos." Este erro indica que o comprimento do nome do projeto do Azure completamente qualificado excede 146 carateres. Para corrigir o problema, mova a sua solução para uma pasta diferente com um caminho mais curto.

Para obter mais informações sobre como devem tratar os avisos, como erros, consulte [configurar um projeto de serviço de nuvem do Azure com o Visual Studio](vs-azure-tools-configuring-an-azure-project.md).

### <a name="test-the-migration-locally"></a>Testar a migração localmente

1. No Visual Studio **Explorador de soluções**, o projeto de serviço cloud foi adicionado com o botão direito e selecione **Set as Startup Project**.
1. Selecione **depurar > Iniciar depuração** (F5) para iniciar o ambiente de depuração do Azure. Esse ambiente oferece, especificamente, a emulação de vários serviços do Azure.

### <a name="use-an-azure-sql-database-for-your-application"></a>Utilizar uma base de dados do SQL do Azure para a sua aplicação

Se tiver uma cadeia de ligação para a sua aplicação web que utiliza uma base de dados do SQL Server no local, tem de migrar a base de dados para a base de dados do Azure SQL em vez disso e atualizar a cadeia de ligação. Para obter orientações com este processo, consulte os tópicos seguintes:

- [Migração de base de dados do SQL Server para a base de dados do SQL na cloud](sql-database/sql-database-cloud-migrate.md)
- [Utilizar .NET (c#) com o Visual Studio para ligar e consultar e base de dados SQL do Azure](sql-database/sql-database-connect-query-dotnet-visual-studio.md).

## <a name="publish-the-application-to-azure-cloud-service"></a>Publicar a aplicação no serviço Cloud do Azure

1. Criar nuvem necessária contas de armazenamento e de serviço na sua subscrição do Azure conforme descrito em [preparar para publicar ou implementar uma aplicação do Azure a partir do Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
1. No Visual Studio, clique com o botão direito no projeto de aplicação e selecione **publicar para o Microsoft Azure...**  (que é diferente do comando "Publicar...".).
1. Na **publicar a aplicação do Azure** que aparece, inicie sessão com a conta com a sua subscrição do Azure e selecione **próxima >**.
1. No **definições > definições comuns** separador, selecione o serviço de nuvem de destino do **serviço Cloud** na lista pendente, junto com o seu ambiente escolhido e configurações. 
1. Na **definições > Definições Avançadas**, selecione a conta de armazenamento a utilizar, em seguida, selecione **próxima >**.
1. Na **diagnóstico**, escolha se pretende enviar informações para o Application Insights.
1. Selecione **seguinte >** para ver um resumo, em seguida, selecione **Publish** para iniciar a implementação.
1. Visual Studio abre uma janela de registo de atividade em que pode controlar o progresso:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Opcional) Para cancelar o processo de implantação, clique com o botão direito do item de linha no registo de atividades e escolha **Cancelar e remover**. Este comando interrompe o processo de implantação e elimina o ambiente de implantação do Azure. Nota: para remover este ambiente de implantação após ele ter sido implementado, tem de utilizar o [portal do Azure](https://portal.azure.com).
1. (Opcional) Depois de iniciaram as instâncias de função, o Visual Studio mostra automaticamente o ambiente de implantação no **Explorador de servidores > Serviços Cloud** nó. Aqui pode ver o estado das instâncias de função individuais.
1. Para aceder à sua aplicação após a implementação, selecione a seta junto a sua implementação quando o estado **concluído** é apresentado na **registo de atividades do Azure** juntamente com o URL. Consulte a tabela seguinte para obter os detalhes sobre como iniciar um tipo específico de aplicativo web do Azure.

## <a name="using-the-compute-emulator-and-starting-application-in-azure"></a>Utilizar o emulador de computação e iniciar a aplicação no Azure

Todos os tipos de aplicativos podem ser iniciados num navegador ligado para o depurador do Visual Studio selecionando **depurar > Iniciar depuração** (F5). Com um projeto de aplicativo Web vazio do ASP.NET, primeiro tem de adicionar um `.aspx` página em seu aplicativo e defina-a como a página de início do projeto web.

A tabela seguinte fornece detalhes sobre como iniciar a aplicação no Azure:

   | Tipo de aplicação Web | Em execução no Azure |
   | --- | --- | --- |
   | Aplicação ASP.NET Web<br/>(incluindo MVC 2, MVC 3, 4 de MVC) | Selecione o URL na **implantação** separador para o **registo de atividades do Azure**. |
   | Aplicativo da Web ASP.NET vazio | Se tiver um padrão `.aspx` na sua aplicação, selecione o URL na **implantação** separador para o **registo de atividades do Azure**. Para navegar para uma página diferente, introduza um URL no formato seguinte num browser: `<deployment_url>/<page_name>.aspx` |
   | Aplicativo do Silverlight<br/>Aplicativo de negócios Silverlight<br/>Aplicativo de navegação do Silverlight | Navegue para a página específica para a sua aplicação utilizando a forma de URL seguinte: `<deployment_url>/<page_name>.aspx` |
    Aplicação de serviço do WCF<br/>Aplicação de serviço de fluxo de trabalho do WCF | Definir o `.svc` ficheiro como a página inicial do seu projeto de serviço do WCF. Em seguida, navegue para `<deployment_url>/<service_file>.svc` |
   | Entidades de dinâmicos do ASP.NET<br/>Linq de dados dinâmicos do ASP.NET para o SQL | Atualize a cadeia de ligação, conforme descrito na secção seguinte. Em seguida, navegue para `<deployment_url>/<page_name>.aspx`. Para o Linq to SQL, tem de utilizar uma base de dados SQL do Azure. |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Atualizar uma cadeia de ligação para as entidades de dinâmicos do ASP.NET

1. Crie uma base de dados do SQL Azure para um aplicativo da web de entidades dinâmicos do ASP.NET, conforme descrito anteriormente (#use-an-azuresql-database-for-your-application).
1. Adicione as tabelas e campos de que precisa para esta base de dados do portal do Azure.
1. Especifique uma cadeia de ligação no `web.config` com o seguinte formato de ficheiro e guarde o ficheiro:

    ```xml
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Atualização do *connectionString* valor com a cadeia de ligação do ADO.NET para a base de dados do SQL Azure, da seguinte forma:

    ```xml
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

## <a name="supported-project-templates"></a>Modelos de projeto suportados

Aplicações que podem ser migradas e publicadas nos serviços cloud tem de utilizar um dos modelos na tabela abaixo. Não é suportado o ASP.NET Core.

| Grupo de modelo | Modelo de projeto |
| --- | --- |
| Web | Aplicação Web do ASP.NET (.NET Framework) |
| Web | Aplicação Web ASP.NET MVC 2 |
| Web | Aplicação Web ASP.NET MVC 3 |
| Web | Aplicação Web de ASP.NET MVC4 |
| Web | Aplicativo da Web ASP.NET vazio (ou Site) |
| Web | Aplicação do ASP.NET MVC 2 Web vazio |
| Web | Aplicação Web de entidades de dados dinâmicos do ASP.NET |
| Web | Linq de dados dinâmicos do ASP.NET para a aplicação Web do SQL |
| Silverlight | Aplicativo do Silverlight |
| Silverlight | Aplicativo de negócios Silverlight |
| Silverlight | Aplicativo de navegação do Silverlight |
| WCF | Aplicação de serviço do WCF |
| WCF | Aplicação de serviço de fluxo de trabalho do WCF |
| Fluxo de trabalho | Aplicação de serviço de fluxo de trabalho do WCF |

## <a name="next-steps"></a>Passos Seguintes

- [Preparar para publicar ou implementar uma aplicação do Azure a partir do Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)
- [Configurar com o nome credenciais de autenticação](vs-azure-tools-setting-up-named-authentication-credentials.md).
