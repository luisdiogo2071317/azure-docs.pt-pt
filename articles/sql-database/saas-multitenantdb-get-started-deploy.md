---
title: "Implementar uma aplicação SaaS a base de dados do multi-inquilino que utiliza a SQL Database do Azure | Microsoft Docs"
description: "Implementar e explorar em partição horizontal Wingtip pedidos da base de dados do multi-inquilino aplicação SaaS, que demonstra padrões de SaaS ao utilizar a SQL Database do Azure."
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: billgib;anjangsh
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: genemi
ms.openlocfilehash: a7e6e319fb2fa8fee762055b625427403d14d679
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Implementar e explorar uma a aplicação multi-inquilino que utiliza a SQL Database do Azure

Neste tutorial, implementar e explorar uma aplicação de base de dados do multi-inquilino de SaaS de exemplo denominada Wingtip pedidos de suporte. A aplicação de Wingtip foi concebida para demonstram as funcionalidades da SQL Database do Azure que simplificam a implementação de cenários de SaaS.

Esta implementação do Wingtips utiliza um padrão da base de dados do multi-inquilino. É a fragmentação pelo identificador de inquilino. Dados do inquilino são distribuídos para uma determinada base de dados, de acordo com os valores de identificador de inquilino. Independentemente quantos inquilinos qualquer base de dados fornecido contém, todas as bases de dados são o multi-inquilinos na medida em que os esquemas de tabela incluem um identificador de inquilino. 

Neste padrão de base de dados permite-lhe armazenar um ou mais inquilinos em cada partição horizontal ou a base de dados. Pode otimizar o custo mais baixo, fazendo com que cada base de dados ser partilhado por vários inquilinos. Ou pode otimizar para isolamento, fazendo com que cada base de dados armazenar apenas um inquilino. A opção de otimização pode ser efetuada separadamente para cada inquilino específico. Pode ser efetuada à sua escolha, quando o inquilino é armazenado pela primeira vez, ou pode mudar de ideias mais tarde. A aplicação foi concebida para funcionar bem qualquer forma.

#### <a name="app-deploys-quickly"></a>Aplicação implementa rapidamente

A secção de implementação que se segue fornece a azul **implementar no Azure** botão. Quando o botão é premido, a aplicação de Wingtip totalmente é implementada um cinco minutos mais tarde. A aplicação de Wingtip é executado na nuvem do Azure e utiliza a SQL Database do Azure. Wingtip é implementada a sua subscrição do Azure. Tem acesso total ao trabalhar com os componentes de aplicações individuais.

A aplicação é implementada com os dados para os três inquilinos de exemplo. Os inquilinos são armazenados em conjunto numa base de dados do multi-inquilino.

Qualquer pessoa pode transferir o código de origem do c# e do PowerShell para bilhetes de Wingtip de [o repositório do GitHub][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Saiba neste tutorial

> [!div class="checklist"]
> - Como implementar a aplicação Wingtip SaaS.
> - Onde obter o código fonte da aplicação e os scripts de gestão.
> - Sobre os servidores e bases de dados que constituem a aplicação.
> - Como os inquilinos estão mapeados para os seus dados com o *catálogo*.
> - Como aprovisionar um novo inquilino.
> - Como monitorizar a atividade de inquilino na aplicação.

Uma série de tutoriais relacionados está disponível que tirar partido desta implementação inicial. Os tutoriais explorar padrões de conceção e gestão de intervalo de SaaS. Quando completar os tutoriais, são encorajados a passo através de scripts fornecidos para ver como os diferentes padrões de SaaS são implementados.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

- O Azure PowerShell mais recente está instalado. Para obter mais informações, consulte [introdução ao Azure PowerShell][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Implementar o Wingtip pedidos de aplicação

#### <a name="plan-the-names"></a>Planear os nomes

Os passos desta secção, existem dois locais onde tem de introduzir nomes de utilizador, como um *utilizador* e para a nova *grupo de recursos*. Para uma pessoa designada *Ann Finley*, sugerimos que os nomes seguintes:
- *Utilizador:* &nbsp; **af1** &nbsp; *(respetivo iniciais, mais um dígito.)*
- *Grupo de recursos:* &nbsp; **wingtip af1** &nbsp; *(Recomendamos todo em minúsculas. Acrescentar um hífen, em seguida, o nome de utilizador.)*

Escolha os nomes agora e escrevê-las para baixo.

#### <a name="steps"></a>Passos

1. Clique na seguinte blue **implementar no Azure** botão.
    - É aberto o portal do Azure com o modelo de implementação Wingtip bilhetes SaaS.

    [![Botão para implementar no Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Introduza os valores de parâmetro necessário para a implementação.

    > [!IMPORTANT]
    > Para esta demonstração, não utilize qualquer pré-existente grupos de recursos, servidores ou conjuntos. Em vez disso, escolher **criar um novo grupo de recursos**. Elimine este grupo de recursos quando tiver terminado com a aplicação para parar a faturação relacionada.
    > Não utilize a aplicação ou a quaisquer recursos cria, para produção. Alguns aspetos de autenticação e as definições de firewall do servidor, são intencionalmente inseguras na aplicação para facilitar a demonstração.

    - Para **grupo de recursos** - selecione **criar nova**e, em seguida, forneça um **nome** para o grupo de recursos (sensível às maiúsculas e).
        - Selecione um **localização** na lista pendente.
    - Para **utilizador** -que recomendamos que escolha um curto **utilizador** valor.

3. **Implemente a aplicação**.

    - Clique para aceitar os termos e condições.
    - Clique em **Comprar**.

4. Monitorizar o estado de implementação, clicando em **notificações**, que é o ícone de campainha para a direita da caixa de pesquisa. Implementar a aplicação de Wingtip demora cerca de cinco minutos.

   ![implementação concluída com êxito](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Transferir e desbloquear os scripts de gestão

Enquanto a aplicação está a implementar, transfira os scripts de código e gestão da origem de aplicação.

> [!NOTE]
> Conteúdo executável (scripts, DLLs) pode estar bloqueado pelo Windows quando zip ficheiros são transferidos a partir de uma origem externa e extraiu. Quando extrair os scripts a partir de um ficheiro zip, utilize os seguintes passos para desbloquear o ficheiro. zip antes de a extrair. Ao desbloquear o ficheiro. zip, certifique-se que os scripts estão autorizados a executar.

1. Navegue até à [o repositório do GitHub WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Clique em **clonar ou transferir**.
3. Clique em **transferir ZIP** e guarde o ficheiro.
4. Clique com botão direito do **WingtipTicketsSaaS-MultiTenantDb-master.zip** do ficheiro e selecione **propriedades**.
5. No **geral** separador, selecione **desbloqueio**e clique em **aplicar**.
6. Clique em **OK**.
7. Extraia os ficheiros.

Os scripts localizados no *... \\Mestre de MultiTenantDb WingtipTicketsSaaS\\Learning módulos\\*  pasta.

## <a name="update-the-configuration-file-for-this-deployment"></a>Atualizar o ficheiro de configuração para esta implementação

Antes de executar quaisquer scripts, defina o *grupo de recursos* e *utilizador* valores na **UserConfig.psm1**. Defina estas variáveis com os mesmos valores que definiu durante a implementação.

1. Abra... \\Learning módulos\\*UserConfig.psm1* no *ISE do PowerShell*.
2. Atualização *ResourceGroupName* e *nome* com os valores específicos para a sua implementação (em linhas 10 e 11 apenas).
3. Guarde as alterações.

Os valores definidos neste ficheiro são utilizados por todos os scripts, pelo que é importante estão corretas. Se voltar a implementar a aplicação, tem de escolher valores diferentes para o utilizador e grupo de recursos. Em seguida, atualize novamente o ficheiro de UserConfig.psm1 com os novos valores.

## <a name="run-the-application"></a>Executar a aplicação

Na aplicação Wingtip, os inquilinos são venues. Um venue pode ser concert hall, um club desporto ou qualquer outra localização que aloja os eventos. Os venues registar no Wingtip como os clientes e é gerado um identificador de inquilino para cada venue. Cada venue apresenta os eventos futuros no Wingtip, para que o público pode comprar pedidos para os eventos.

Cada venue obtém uma aplicação web personalizado para listar os respetivos eventos e propor pedidos de suporte. Cada aplicação web é independente e isolado de outros inquilinos. Internamente na SQL Database do Azure, cada os dados para cada inquilino são armazenados numa base de dados multi-inquilino em partição horizontal, por predefinição. Todos os dados são etiquetados com o identificador de inquilino.

Um centro **Hub de eventos** página Web fornece uma lista de ligações para os inquilinos na sua implementação específica. Utilize os seguintes passos para experimentar o **Hub de eventos** página Web e uma aplicação web individuais:

1. Abra o **Hub de eventos** no seu browser:
    - http://events.Wingtip. &lt;Utilizador&gt;. trafficmanager.net &nbsp; *(substituir &lt;utilizador&gt; com o valor de utilizador da sua implementação.)*

    ![hub de eventos](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Clique em **Fabrikam Jazz Club** no **Hub de Eventos**.

   ![Eventos](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Traffic Manager do Azure

Para controlar a distribuição de pedidos recebidos, utiliza a aplicação de Wingtip [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md). A página de eventos para cada inquilino inclui o nome do inquilino no respetivo URL. Cada URL também inclui o valor de utilizador específico. Cada URL obeys o formato mostrado utilizando os seguintes passos:

- http://events.Wingtip. &lt;Utilizador&gt;.trafficmanager.net/*fabrikamjazzclub*

1. A aplicação de eventos analisa o nome de inquilino a partir do URL. O nome do inquilino é *fabrikamjazzclub* no URL do exemplo anterior.
2. A aplicação, em seguida, codifica o nome do inquilino para criar uma chave para aceder a um catálogo utilizando [gestão de mapa de partições horizontais](sql-database-elastic-scale-shard-map-management.md).
3. A aplicação encontra a chave no catálogo e obtém a localização da base de dados do inquilino correspondente.
4. A aplicação utiliza as informações de localização para localizar e aceder a uma base de dados que contém todos os dados para o inquilino.

#### <a name="events-hub"></a>Hub de eventos

1. O **Hub de eventos** apresenta uma lista de todos os inquilinos estão registados no catálogo e os respetivos venues.
2. O **Hub de eventos** utiliza metadados expandido no catálogo para obter o nome do inquilino associado a cada mapeamento para construir os URLs.

Num ambiente de produção, normalmente, crie um registo CNAME DNS para [apontar um domínio de internet da empresa](../traffic-manager/traffic-manager-point-internet-domain.md) para o perfil do Gestor de tráfego.

## <a name="start-generating-load-on-the-tenant-databases"></a>Começar a gerar carga nas bases de dados de inquilinos

Agora que a aplicação é implementada, vamos colocá-la para funcionar! O *demonstração LoadGenerator* script do PowerShell inicia uma carga de trabalho em execução para cada inquilino. A carga do mundo real em muitas aplicações de SaaS é normalmente esporádicas e imprevisíveis. Para simular este tipo de carga, o gerador de dimensões produz uma carga distribuída a todos os inquilinos. A carga inclui bursts aleatório em cada inquilino ocorrer em intervalos aleatório. Demora alguns minutos até o padrão de carga ideia, pelo que é melhor permitir que o gerador de execução para, pelo menos, três ou quatro minutos antes da carga de monitorização.

1. No *ISE do PowerShell*, abra a... \\Learning módulos\\utilitários\\*demonstração LoadGenerator.ps1* script.
2. Prima **F5** para executar o script e iniciar o gerador de carga (deixe os valores predefinidos do parâmetro por agora).

O *demonstração LoadGenerator.ps1* script abre-se a outra sessão de PowerShell onde é executado o gerador de carga. O gerador de carga executa nesta sessão como uma tarefa de primeiro plano invoca tarefas de geração de carregamento em segundo plano, um para cada inquilino.

Depois de inicia a tarefa de primeiro plano, permanece no Estado invocar a tarefa. A tarefa inicia tarefas em segundo plano adicionais para os novos inquilinos subsequentemente aprovisionados.

Fechar a sessão do PowerShell deixa de todas as tarefas.

Pode querer reiniciar a sessão do gerador de carga a utilizar valores de parâmetro diferentes. Em caso afirmativo, feche o PowerShell sessão geração e, em seguida, execute novamente o *demonstração LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Aprovisionar um novo inquilino na base de dados em partição horizontal

A implementação inicial inclui três inquilinos de exemplo no *Tenants1* base de dados. Vamos criar outro inquilino e observe o respetivos efeitos na aplicação implementada. Neste passo, prima uma chave para criar um novo inquilino:

1. Abra... \\Learning módulos\\aprovisionar e catálogo\\*demonstração ProvisionTenants.ps1* no *ISE do PowerShell*.
2. Prima **F5** (não **F8**) para executar o script (mantenha os valores predefinidos por agora).

   > [!NOTE]
   > Tem de executar os scripts do PowerShell só, premindo o **F5** chave, premindo não **F8** para executar uma peça selecionada do script. O problema com **F8** é que o *$PSScriptRoot* variável não é avaliada. Esta variável é necessário pelos scripts de muitos para navegar de pastas, para invocar outros scripts ou para importar módulos.

É adicionado ao novo inquilino Racing Maple vermelho para o *Tenants1* da base de dados e registadas no catálogo. Ao novo inquilino da permissão a vender **eventos** site abre no browser:

![Novo inquilino](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Atualize o **Hub de eventos**, e ao novo inquilino é apresentado na lista.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Aprovisionar um novo inquilino na sua própria base de dados

O modelo de multi-inquilino em partição horizontal permite-lhe escolher se pretende aprovisionar um novo inquilino na base de dados que contém outros inquilinos ou numa base de dados do seu próprio. Um inquilino isolado na sua própria base de dados gostar as seguintes vantagens:
- O desempenho da base de dados do inquilino pode ser gerido sem a necessidade de comprometer com as necessidades de outros inquilinos.
- Se necessário, a base de dados pode ser restaurado para um ponto anterior no tempo, porque não existem outros inquilinos seriam afetados.

Pode optar por colocar os clientes de avaliação gratuita ou os clientes economia em bases de dados do multi-inquilinos. Pode colocar a cada inquilino premium na sua própria base de dados dedicado. Se criar muitas bases de dados que contenham apenas um inquilino, pode geri-los todos os coletivamente num conjunto elástico para otimizar os custos de recursos.

Em seguida, iremos aprovisionar outro inquilino, desta vez na sua própria base de dados:

1. Em... \\Learning módulos\\aprovisionar e catálogo\\*demonstração ProvisionTenants.ps1*, modificar *$TenantName* para **Salix Salsa**, *$VenueType* para **dance** e *$Scenario* para **2**.

2. Prima **F5** para executar novamente o script.
    - Isto **F5** prima Aprovisiona ao novo inquilino na base de dados separada. A base de dados e de inquilino são registadas no catálogo. Em seguida, o browser abre-se para a página de eventos do inquilino.

   ![Página de eventos de Salix Salsa](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Desloque-se na parte inferior da página. Não existe na faixa da, pode ver o nome de base de dados na qual os dados de inquilino estão armazenados.

3. Atualize o **Hub de eventos** e os dois novos inquilinos aparece na lista.

## <a name="explore-the-servers-and-tenant-databases"></a>Explorar os servidores e bases de dados de inquilino

Agora vamos ver alguns dos recursos que foram implementados:

1. No [portal do Azure](http://portal.azure.com), navegue para a lista de grupos de recursos. Abra o grupo de recursos que criou quando implementou a aplicação.

   ![grupo de recursos](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Clique em **catálogo mt&lt;utilizador&gt;**  servidor. O servidor de catálogo contém duas bases de dados com o nome *tenantcatalog* e *basetenantdb*. O *basetenantdb* base de dados é uma base de dados do modelo em branco. É copiado para criar uma nova base de dados de inquilino, se utilizado para vários inquilinos ou apenas um inquilino.

   ![servidor de catálogo](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Volte ao grupo de recursos e selecione o *tenants1 mt* servidor que contém as bases de dados do inquilino.
    - A base de dados tenants1 é uma base de dados do multi-inquilino na qual os três inquilinos originais, mais inquilino primeiro adicionadas, é armazenado. Este é configurado como uma base de dados padrão de DTU 50.
    - O **salixsalsa** base de dados contém o venue de dance Salix Salsa como respetivo inquilino apenas. Este é configurado como uma base de dados de edição Standard com 50 DTUs por predefinição.

   ![servidor de inquilinos](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>Monitorizar o desempenho da base de dados

Se o gerador de carga tem estado em execução durante vários minutos, telemetria suficiente está disponível para observar a base de dados incorporadas no portal do Azure de capacidades de monitorização.

1. Navegue para o **tenants1 mt&lt;utilizador&gt;**  servidor e clique em **tenants1** para ver a utilização de recursos para a base de dados tem quatro inquilinos na mesma. Cada inquilino está sujeita a sobrecarga esporádicas do gerador de dimensões de carga:

   ![monitorizar tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   O gráfico de utilização da DTU corretamente ilustra como uma base de dados do multi-inquilino pode suportar uma carga de trabalho imprevisível em vários inquilinos. Neste caso, o gerador de carga é aplicar uma carga de aproximadamente 30 DTUs esporádicas a cada inquilino. Este carregamento equivale a 60% de utilização de uma base de dados DTU 50. Picos que excedem os 60% são o resultado de carga a ser aplicada a mais do que um inquilino ao mesmo tempo.

2. Navegue para o **tenants1 mt&lt;utilizador&gt;**  servidor e clique em de **salixsalsa** base de dados. Pode ver a utilização de recursos nesta base de dados que contém apenas um inquilino.

   ![salixsalsa base de dados](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

O gerador de carga é aplicar uma carga semelhante a cada inquilino, independentemente da base de dados que cada inquilino está a ser. Com apenas um inquilino no **salixsalsa** base de dados, pode ver que a base de dados pode suportar um muito superior de carga que a base de dados com vários inquilinos. 

#### <a name="resource-allocations-vary-by-workload"></a>Alocações de recurso variam consoante a carga de trabalho

Por vezes, uma base de dados do multi-inquilino requer mais recursos para um bom desempenho do que uma base de dados único inquilino, mas nem sempre. A alocação de recursos ideal depende as características de carga de trabalho específica para os inquilinos no seu sistema.

As cargas de trabalho geradas pelo script de gerador de carga destinam-se apenas a fins de ilustração.

## <a name="additional-resources"></a>Recursos adicionais

- Para saber mais sobre as aplicações de SaaS multi-inquilino, consulte [padrões para aplicações de SaaS multi-inquilino de conceção](saas-tenancy-app-design-patterns.md).

- Para mais informações sobre conjuntos elásticos, consulte:
    - [Ajudar a gerir e dimensionar várias bases de dados SQL do Azure de conjuntos elásticos](sql-database-elastic-pool.md)
    - [Aumentar horizontalmente com a Base de Dados SQL do Azure](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber:

> [!div class="checklist"]
> - Como implementar a aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino.
> - Sobre os servidores e bases de dados que constituem a aplicação.
> - Os inquilinos estão mapeados para os seus dados com o *catálogo*.
> - Como aprovisionar novos inquilinos, numa base de dados do multi-inquilino e da base de dados único inquilino.
> - Como ver utilização de agrupamento para monitorizar a atividade de inquilino.
> - Como eliminar os recursos de exemplo para parar a faturação relacionada.

Experimente agora o [tutorial de aprovisionamento e o catálogo](sql-database-saas-tutorial-provision-and-catalog.md).


<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/



<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."
-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Botão de implementação para o Azure."

