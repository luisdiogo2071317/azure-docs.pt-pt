---
title: Recriar uma aplicação no local da Contoso para o Azure | Documentos da Microsoft
description: Saiba como Contoso recria uma aplicação no Azure através dos serviços de aplicações do Azure, o Kubernetes service, cosmos DB, as funções do Azure e os serviços cognitivos.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 3d835a7bd93426e57c5ab204d277faca22ae0638
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "42057627"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Migração de Contoso: reconstrua uma aplicação no local para o Azure

Este artigo demonstra como Contoso migra e recria a respetiva aplicação SmartHotel no Azure. Estes migrar o front-end VM da aplicação para aplicações Web de serviços de aplicações do Azure. O back-end de aplicação é criado usando microsserviços implementados para os contentores geridos pelo Azure Kubernetes Service (AKS). O site interage com o Azure Functions, fornecendo funcionalidade de estimação fotos. 

Este documento é um de uma série de artigos que mostram como a empresa fictícia Contoso migra os seus recursos no local para a cloud do Microsoft Azure. A série contém informações gerais e cenários que ilustram como configurar uma infraestrutura de migração, avaliação dos recursos no local para migração e execução de diferentes tipos de migrações. Cenários de crescem em complexidade e vamos adicionar artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojar a uma aplicação em VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
[Artigo 5: Realojar a uma aplicação em VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migrar a aplicação de SmartHotel VMs com apenas o Site Recovery. | Disponível
[Artigo 6: Realojar a uma aplicação para as VMs do Azure e o SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux em VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux em VMs do Azure e o servidor MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação na base de dados de aplicações Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux para aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação para contentores do Azure e base de dados SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
Artigo 13: Recriar uma aplicação para o Azure | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Este artigo.

Neste artigo, a Contoso migra o Windows de duas camadas. Aplicação de NET SmartHotel em execução em VMs de VMware para o Azure. Se gostaria de utilizar esta aplicação, são fornecidas como código-fonte aberto e pode baixá-lo partir [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com seus parceiros de negócios para compreender o que quer alcançar com esta migração:

- **Resolver o crescimento do negócio**: Contoso está a crescer. Elas querem fornecer experiências diferenciadas para seus clientes nos respetivos sites.
- **Agilidade**: Contoso deve poder reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso numa economia global. 
- **Dimensionamento**: à medida que a empresa cresce com êxito, TI da Contoso tem de fornecer sistemas que são capazes de aumentar ao ritmo mesmo.
- **Os custos**: Contoso, além de minimizar os custos de licenciamento.

## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud de Contoso tiver afixado para baixo de requisitos de aplicações para essa migração. Esses requisitos foram utilizados para determinar o melhor método de migração:
 - A aplicação no Azure permanecerá como crítica, pois é hoje mesmo. Ele deve executar bem e dimensione com facilidade.
 - A aplicação não deve utilizar componentes de IaaS. Tudo o que deve ser criado para utilizar o PaaS ou dos serviços sem servidor.
 - As compilações de aplicação devem ser executados nos serviços cloud e contentores devem residir num registo de contentor de toda a empresa privada na cloud.
 - O serviço de API utilizado para fotos pet deve ser precisos e confiáveis no mundo real, uma vez que as decisões tomadas pela aplicação tem cumpridas seus hotéis. Qualquer animal de estimação concedido o acesso é permitido para se manter nos hotéis.

## <a name="solution-design"></a>Design da solução

Depois de fixar-se para baixo de seus objetivos e requisitos, a Contoso projeta e rever uma solução de implantação e identifica o processo de migração, incluindo os serviços do Azure que eles usarão para a migração.

### <a name="current-app"></a>Aplicação atual

- A aplicação no local de SmartHotel é em camadas em duas VMs (WEBVM e SQLVM).
- As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5)
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.


### <a name="proposed-architecture"></a>Arquitetura proposta

- O front-end da aplicação será implementado como uma aplicação Web de serviços de aplicações do Azure, na respetiva região primária.
- Uma função do Azure irá fornecer carregamentos de estimação fotos, e o site irão interagir com esta funcionalidade.
- A função pet photo tira partido da API de imagem digitalizada dos serviços cognitivos e cosmos DB.
- O back-end do site é criado com microsserviços. Estas serão implementadas para os contentores geridos no Azure Kubernetes service (AKS).
- Contentores serão criados com o VSTS e enviados por push para o Azure Container Registry (ACR).
- Por enquanto, o Contoso irá implementar manualmente o código de aplicação e a função de Web com o Visual Studio.
- Microsserviços serão implementados utilizar um script do PowerShell que chama as ferramentas de linha de comandos do Kubernetes.

    ![Arquitetura do cenário](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Revisão de solução
Contoso avalia o design proposto ao juntar-se de uma lista de prós e contras.

**Consideração** | **Detalhes**
--- | ---
**Profissionais de TI** | Com a PaaS e soluções sem servidor para a implementação de ponto-a-ponto significativamente reduz o tempo de gestão que a Contoso tem de fornecer.<br/><br/> Mover para uma arquitetura de microsserviços permite que a Contoso ampliar facilmente a sua solução ao longo do tempo.<br/><br/> Nova funcionalidade pode ser colocada online sem interromper qualquer uma das bases de código de soluções existentes.<br/><br/> A aplicação Web será configurada com várias instâncias com nenhum ponto único de falha.<br/><br/> Dimensionamento automático será ativado para que a aplicação conseguir processar o volume de tráfego diferentes.<br/><br/> Com a mudança para os serviços PaaS Contoso pode extinguir Desatualizadas soluções em execução no sistema operativo Windows Server 2008 R2.<br/><br/> CosmosDB tem tolerância a falhas incorporada, que não requer configurações pela Contoso. Isso significa que a camada de dados já não é um ponto único de ativação pós-falha.
**Contras** | Os contentores são mais complexos do que outras opções de migração. A curva de aprendizado pode ser um problema para a Contoso.  Eles introduzem um novo nível de complexidade que fornece muito valor apesar da curva.<br/><br/> A equipe de operações na Contoso terá a incrementação para compreender e suporte Azure, contentores e microsserviços para a aplicação.<br/><br/> Contoso totalmente ainda não implementado DevOps para a solução inteira. Eles precisam de pensar que para a implementação de serviços para o AKS, funções e serviços de aplicações.



### <a name="migration-process"></a>Processo de migração

1. Contoso aprovisionar o ACR, AKS e cosmos DB.
2. Eles aprovisionam a infraestrutura para a implementação, incluindo a aplicação Web do Azure, a conta de armazenamento, a função e a API. 
3. Depois da infraestrutura no local, eles criar seus microsserviços imagens de contentor com o VSTS, que envia-as para o ACR.
4. Contoso irá implementar estes microsserviços ASK utilizando um script do PowerShell.
5. Por fim, eles vai implementar a função do Azure e a aplicação Web.

    ![Processo de migração](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Simplifica o gerenciamento, implantação e operações do Kubernetes. Fornece um serviço de orquestração de contentores do Kubernetes totalmente gerido.  | AKS é um serviço gratuito.  Pague apenas a máquinas virtuais e armazenamento associado e recursos de rede consumidos. [Saiba mais](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Funções do Azure](https://azure.microsoft.com/services/functions/) | Acelera o desenvolvimento com uma experiência de computação sem servidor condicionadas por eventos. Dimensione sob pedido.  | Paga apenas pelos recursos consumidos. Plano é faturado com base no consumo de recursos por segundo e execuções. [Saiba mais](https://azure.microsoft.com/pricing/details/functions/).
[Registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/) | Armazena imagens de todos os tipos de implementações de contentor. | Custo com base em recursos, armazenamento e a duração de utilização. [Saiba mais](https://azure.microsoft.com/pricing/details/container-registry/).
[Serviço de Aplicações do Azure](https://azure.microsoft.com/services/app-service/containers/) | Crie, implemente e dimensione rapidamente aplicações API, Web e móveis em execução em qualquer plataforma. | Os planos do Serviço de Aplicações são cobrados com base por segundo. [Saiba mais](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Pré-requisitos

Eis o que (e Contoso) precisam executar este cenário:

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando realizada a avaliação no primeiro artigo desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.
**Pré-requisitos do desenvolvedor** | A Contoso precisa de ferramentas a seguir numa estação de trabalho do desenvolvedor:<br/><br/> - [Visual Studio 2017 Community Edition: Versão 15.5](https://www.visualstudio.com/)<br/><br/> Carga de trabalho .NET ativada.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) ou o Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) definido para utilizar contentores do Windows.



## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso será executada a migração:

> [!div class="checklist"]
> * **Passo 1: Aprovisionar AKS e ACR**: Contoso Aprovisiona o cluster do AKS gerido e o registo de contentor do Azure com o PowerShell
> * **Passo 2: Criar contentores de Docker**: configurar o CI para contentores do Docker com o VSTS e emiti-las para o ACR.
> * **Passo 3: Implementar microsserviços de back-end**: que implementam o resto da infra-estrutura que irá ser aproveitada por microsserviços de back-end.
> * **Passo 4: Implementar a infraestrutura de front-end**: que implementam a infraestrutura de front-end, inlcuding o armazenamento de BLOBs para os telefones pet, o Cosmos DB e API de imagem digitalizada.
> * **Passo 5: Migrar o back-end**: eles implementar microsserviços e execute no AKS, para migrar o back-end.
> * **Passo 6: Publicar o front-end**: eles publicam a aplicação de SmartHotel para o serviço de aplicações do Azure e a aplicação de função que será chamado pelo serviço de estimação.



## <a name="step-1-provision-an-aks-cluster-and-acr"></a>Passo 1: Aprovisionar um cluster do AKS e o ACR

Contoso executa um script de implementação para criar o cluster de Kubernetes gerido com o AKS e o Azure Container Registry.

- As instruções para esta secção utilizam o **SmartHotel360-Azure-back-end** repositório.
- O **SmartHotel360-Azure-back-end** repositório do GitHub contém todo o software para esta parte da implementação.

Eles aprovisionam da seguinte forma:

1. Antes de este começar, a Contoso garante que todo o software de pré-requisito está instalado no computador de desenvolvimento estiver a utilizar. 
2. Eles clonagem o repositório localmente para o computador de desenvolvimento com o Git.

    **clone de Git https://github.com/Microsoft/SmartHotel360-Azure-backend.git**

3.  Contoso abre a pasta com o Visual Studio Code e se move para o **/implementar/k8s** diretório, que contém o script **gen-aks-env.ps1**.
4. Eles são executados o script para criar o cluster de Kubernetes gerido, com o AKS e o registo de contentor.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
5.  Com o ficheiro aberto, atualizam o parâmetro $location **eualeste2**e guarde o ficheiro.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

6. Clicarem **View** > **Terminal integrado** para abrir o terminal integrado no código.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

7. No terminal integrado do PowerShell, iniciarem sessão no Azure com o comando Connect-AzureRmAccount. [Saiba mais](https://docs.microsoft.com/powershell/azure/get-started-azureps) como começar a trabalhar com o PowerShell.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

8. Se autenticar a CLI do Azure ao executar o **início de sessão az** de comandos e seguir as instruções para autenticar usando seus navegadores da web. [Saiba mais](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) quanto ao log de sessão com a CLI do Azure.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

9. Eles são executados o seguinte comando, passando o nome do grupo de recursos do ContosoRG, o nome do AKS cluster smarthotel-aks-eus2 e o novo nome de registo.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```

    ![AKS](./media/contoso-migration-rebuild/aks6.png)

10. O Azure cria outro grupo de recursos, que contém os recursos para o cluster do AKS.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

11. Depois de concluída a implementação, a Contoso instalar o **kubectl** ferramenta da linha de comandos. A ferramenta já está instalada o Azure CloudShell.

    **az aks install-cli**

12. Eles verificarem a ligação ao cluster ao executar o **kubectl obter nós** comando. O nó é o mesmo nome que a VM no grupo de recursos criado automaticamente.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

13. Eles executam o seguinte comando para iniciar o Dashboard do Kubernetes: 

    **procurar o AZ aks – ContosoRG do grupo de recursos – smarthotelakseus2 de nome**

14. É aberto um separador do browser para o Dashboard. Esta é uma ligação de túnel com a CLI do Azure. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-build-a-docker-container"></a>Passo 2: Criar um contentor do Docker

### <a name="create-a-vsts-and-build"></a>Criar um VSTS e criar

Contoso cria um projeto VSTS e configura uma compilação CI para criar o contentor e, em seguida, envia-o para o ACR. As instruções nesta secção utilizam o [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) repositório.

1. Do visualstudio.com, eles criam uma nova conta (**contosodevops360.visualstudio.com**) e configure-o a utilizar o Git.

2. Eles criam um novo projeto (**smarthotelrefactor**) com o Git para controle de versão e o Agile para o fluxo de trabalho.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Eles importam o repositório do GitHub.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. Na **criar e lançar**, eles criam uma nova definição de utilizando o Git do VSTS como uma origem, o importados do **smarthotel** repositório. 

    ![VSTS](./media/contoso-migration-rebuild/vsts3.png)

6. Devem selecionar para iniciar com um pipeline vazio.

    ![VSTS](./media/contoso-migration-rebuild/vsts4.png)  

7. Eles selecionam **alojada pré-visualização do Linux** para a definição de compilação.

    ![VSTS](./media/contoso-migration-rebuild/vsts5.png) 
 
8. Na **fase 1**, adicionam um **Docker Compose** tarefas. Esta tarefa de compilações do Docker compõem.

    ![VSTS](./media/contoso-migration-rebuild/vsts6.png) 

9. Eles repetir e adicione outro **Docker Compose** tarefas. Este envia os contentores para o ACR.

     ![VSTS](./media/contoso-migration-rebuild/vsts7.png) 

8. Selecione a primeira tarefa (para compilar) e configurar a compilação com a subscrição do Azure, autorização e o ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts8.png)

9. Especifica o caminho da **docket yaml** , além de ficheiros a **src** pasta do repositório. Eles selecionam para criar imagens de serviço e incluem a marca mais recente. Quando a ação é alterado para **criar imagens de serviço**, o nome da tarefa VSTS é alterado para **criar serviços automaticamente**

    ![VSTS](./media/contoso-migration-rebuild/vsts9.png)

10. Agora, a Contoso configura a segunda tarefa do Docker (enviar por push). Selecionar a subscrição e o **smarthotelacreus2** ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. Novamente, o introduza o ficheiro para o ficheiro de docker yaml e selecione **Push manutenção de imagens** e incluir a etiqueta mais recente. Quando a ação é alterado para **Push manutenção de imagens**, o nome da tarefa VSTS é alterado para **automaticamente os serviços de Push**

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Com as tarefas VSTS configuradas, o Contoso guarda a definição de compilação e inicia o processo de compilação.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Clicarem na tarefa de compilação para verificar o progresso.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Após a conclusão da compilação, o ACR mostra os novo repositórios, que são preenchidos com os contentores utilizados pelos microsserviços.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


## <a name="step-3-deploy-back-end-microservices"></a>Passo 3: Implementar microsserviços de back-end

Com o cluster do AKS criado e a compilação de imagens do Docker, Contoso implementa agora o resto da infra-estrutura que irá ser aproveitada por microsserviços de back-end.

- As instruções na secção utilização a [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) repositório.
- Na **/deploy/k8s/arm** pasta, há um script único para criar todos os itens. 

Que implementam da seguinte forma:

1. A Contoso utiliza o ficheiro cmd para implementar os recursos do Azure no grupo de recursos de ContosoRG e região EUS2, digitando o seguinte comando:

    **. \deploy azuredeploy eualeste2 - c do ContosoRG**

    ![Implementar back-end](./media/contoso-migration-rebuild/backend1.png)

2. Estes capturarem a cadeia de ligação para cada base de dados a ser utilizado mais tarde.

    ![Implementar back-end](./media/contoso-migration-rebuild/backend2.png)

## <a name="step-4-deploy-front-end-infrastructure"></a>Passo 4: Implementar a infraestrutura de front-end

A Contoso precisa de implementar a infraestrutura que será utilizada pelas aplicações de front-end. Ser criar um contentor de armazenamento de BLOBs para armazenar as imagens de estimação; a base de dados do Cosmos, para armazenar documentos com as informações de estimação; e a API de imagem digitalizada para o Web site. 

Instruções para esta secção utilizam o [SmartHotel-public-web](https://github.com/Microsoft/SmartHotel360-public-web) repositório.

### <a name="create-storage-containers"></a>Criar contentores de armazenamento

1.  No portal do Azure, Contoso abre-se a conta de armazenamento que foi criada e clica em **Blobs**.
2.  Eles criam um novo contentor (**animais de estimação**) com o nível de acesso público definido para o contentor. Os utilizadores irão carregar suas fotos pet para este contentor.

    ![Blob de armazenamento](./media/contoso-migration-rebuild/blob1.png)

3. Eles criam um segundo novo contentor designado **definições**. Um ficheiro com todas as definições de aplicação de front-end vai ser colocado neste contentor.

    ![Blob de armazenamento](./media/contoso-migration-rebuild/blob2.png)

4. Estes capturarem os detalhes de acesso para a conta de armazenamento num arquivo de texto, para referência futura.

    ![Blob de armazenamento](./media/contoso-migration-rebuild/blob2.png)

## <a name="provision-a-cosmos-database"></a>Aprovisionar uma base de dados do Cosmos

Contoso Aprovisiona uma base de dados do Cosmos para ser utilizado para obter informações de estimação.

1. Eles criam uma **do Azure Cosmos DB** no Azure Marketplace.

    ![BD do Cosmos](./media/contoso-migration-rebuild/cosmos1.png)

2. Especifica um nome (**contosomarthotel**), selecione a API de SQL e coloque-o no grupo de recursos de produção ContosoRG, na região E.U.A. Leste 2 principal.

    ![BD do Cosmos](./media/contoso-migration-rebuild/cosmos2.png)

3. Eles adicionar uma nova coleção na base de dados, com capacidade predefinida e o débito.

    ![BD do Cosmos](./media/contoso-migration-rebuild/cosmos3.png)


4. Eles tenha em atenção as informações de ligação da base de dados, para referência futura. 

    ![BD do Cosmos](./media/contoso-migration-rebuild/cosmos4.png)


## <a name="provision-computer-vision"></a>Aprovisionamento de imagem digitalizada

Contoso Aprovisiona a API de imagem digitalizada. A API será chamada pela função, para avaliar imagens carregadas por utilizadores.

1. Eles criam uma **de imagem digitalizada** instância no Azure Marketplace.

     ![Imagem Digitalizada](./media/contoso-migration-rebuild/vision1.png)

2. Que Aprovisiona a API (**smarthotelpets**) no recurso da produção grupo ContosoRG, na região E.U.A. Leste 2 principal.

    ![Imagem Digitalizada](./media/contoso-migration-rebuild/vision2.png)

3. Eles guardam as definições de ligação para a API num arquivo de texto para futura referência.

     ![Imagem Digitalizada](./media/contoso-migration-rebuild/vision3.png)

## <a name="step-5-deploy-the-back-end-services-in-azure"></a>Passo 5: Implementar os serviços de back-end no Azure

Agora, a Contoso precisa implementar o controlador de entrada do NGINX para permitir tráfego de entrada para os serviços e, em seguida, implementar os microsserviços para o cluster do AKS.

As instruções nesta secção utilizam o [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) repositório.



1. Utilizam o Visual Studio Code para atualizar o **/deploy/k8s/config_loal.yml** ficheiro. Eles atualizam as várias ligações de base de dados de informações que anotou anteriormente.

     ![Implementar microsserviços](./media/contoso-migration-rebuild/deploy-micro.png)

    > [!NOTE]
    > Algumas das definições de configuração (por exemplo Active Directory B2C) não são abordadas neste artigo. Há mais informações sobre estas definições no repositório.

2. O ficheiro de deploy.ps1 elimina todos os conteúdos de cluster (com exceção de entrada e controlador de entrada) e implementa microsserviços. Eles execução-lo da seguinte forma:

  ```
  .\deploy.ps1 -configFile .\conf_local.yml -dockerUser smarthotelacreus2  -dockerPassword [passwordhere] -registry smarthotelacreus2.azurecr.io -imageTag latest -deployInfrastructure $false -buildImages $false -pushImages $false
  ```

3. Eles executam o seguinte comando para verificar o estado dos serviços:

    ```
    kubectl get services
    ```
4. Abrir o dashboard do Kubernetes para rever a implementação:

    ```
    az aks browse --resource-group ContosoRG --name smarthotelakseus2
    ```


## <a name="step-6-publish-the-frontend"></a>Passo 6: Publicar o front-end

Como passo final, a Contoso publica a aplicação de SmartHotel para o serviço de aplicações do Azure e para a aplicação de função que é chamado pelo serviço animal de estimação.

As instruções nesta secção utilizam o [repositório SmartHotel-public-web.](https://github.com/Microsoft/SmartHotel360-public-web) repositório.

### <a name="set-up-web-app-to-use-contoso-resources"></a>Configurar a aplicação Web para utilizar recursos da Contoso

1. Contoso clona o repositório localmente para o computador de desenvolvimento com o Git:

    **clone de Git https://github.com/Microsoft/SmartHotel360-public-web.git**

2. No Visual Studio, abrir a pasta para mostrar todos os ficheiros no repositório.

    ![Publicar o front-end](./media/contoso-migration-rebuild/front-publish1.png)

3. Eles faça as alterações de configuração necessárias para a configuração padrão.

    - Quando a aplicação web é iniciado, ele procura o **SettingsUrl** definição de aplicação.
    - Esta variável tem de conter um URL para um ficheiro de configuração.
    - Por predefinição, a definição utilizada é um ponto final público.

4. Eles atualizam o **/config-sample.json/sample.json** ficheiro. Este é o ficheiro de configuração para a web ao utilizar o ponto final público.

    - Eles editar ambos os **urls** e **pets_config** seções, com os valores para os pontos finais da API do AKS, contas de armazenamento e base de dados do Cosmos. 
    - Os URLs devem corresponder ao nome DNS da nova aplicação web que Contoso criará.
    - Para a Contoso, trata-se **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![Publicar o front-end](./media/contoso-migration-rebuild/front-publish2.png)

5. Depois do ficheiro é atualizado, eles renomeá-lo para **smarthotelsettingsurl**e carregá-lo para o blob de armazenamento que criaram anteriormente.

     ![Publicar o front-end](./media/contoso-migration-rebuild/front-publish3.png)

6. Eles clicam no ficheiro para obter o URL. Este URL é utilizado pela aplicação, quando começa a puxar o arquivo de configuração.

    ![Publicar o front-end](./media/contoso-migration-rebuild/front-publish4.png)

7. Atualizam **SettingsUrl** no **appsettings. Production.JSON** ficheiros, para o URL do novo arquivo.

    ![Publicar o front-end](./media/contoso-migration-rebuild/front-publish5.png)


### <a name="deploy-the-website-to-the-azure-app-service"></a>Implementar o Web site para o serviço de aplicações do Azure

Agora a Contoso pode publicar o site.


1. Eles permitem a monitorização do Application Insights no Visual Studio 2017. Para tal, selecione o **PublicWeb** do projeto no Explorador de soluções e procure **adicionar o Application Insights**. Eles registram a aplicação com a instância do Application Insights que foi criada quando implementadas a infraestrutura.

    ![Publicar Web site](./media/contoso-migration-rebuild/deploy-website1.png)

2. No Visual Studio que ligue o projeto de PublicWeb às informações da aplicação e atualizado para mostrar que está configurado.
 
    ![Publicar Web site](./media/contoso-migration-rebuild/deploy-website2.png)

3. No Visual Studio, que criarem e publicar a aplicação web.

    ![Publicar Web site](./media/contoso-migration-rebuild/deploy-website3.png)

5. Especifique um nome de aplicação e coloque-o no grupo de recursos de produção **ContosoRG**, na região E.U.A. Leste 2 principal.

    ![Publicar Web site](./media/contoso-migration-rebuild/deploy-website4.png)

### <a name="deploy-the-function-to-azure"></a>Implementar a função no Azure

1. Eles usam o Visual Studio para criar e publicar a função. Para fazer isso, eles com o botão direito a **PetCheckerFunction** > **publicar**. Em seguida, é criar uma nova função de serviço de aplicações.

     ![Implementar a função](./media/contoso-migration-rebuild/function1.png)

2. Especifica o nome **smarthotelpetchecker**e coloque-o no grupo de recursos de ContosoRG e um novo plano do serviço de aplicações.

     ![Implementar a função](./media/contoso-migration-rebuild/function2.png)

3. Selecione o armazenamento de conta e criar a função.

    ![Implementar a função](./media/contoso-migration-rebuild/function3.png)

4. A implementação começa com a aplicação de funções para o Azure de aprovisionamento. Na **publicar**, Contoso adiciona as definições de aplicação para o armazenamento, a base de dados do Cosmos e a API de imagem digitalizada.

    ![Implementar a função](./media/contoso-migration-rebuild/function4.png)

5. Para executar localmente a função em primeiro lugar, eles atualizam definições no **PetCheckerFunction/local.settings.json**.

    ![Implementar a função](./media/contoso-migration-rebuild/function5.png)

6. Depois da função é implementada, é apresentado no portal do Azure, com o **em execução** estado.

    ![Implementar a função](./media/contoso-migration-rebuild/function6.png)


7. Eles navegam para a aplicação para testar a aplicação do Verificador de animal de estimação está a funcionar conforme esperado, em [ http://smarthotel360public.azurewebsites.net/Pets ](http://smarthotel360public.azurewebsites.net/Pets).
8. Clicarem no avatar para carregar uma imagem.

    ![Implementar a função](./media/contoso-migration-rebuild/function7.png)

9. A primeira foto que pretendem verificar é de um pequeno dog.

    ![Implementar a função](./media/contoso-migration-rebuild/function8.png)

10. A aplicação devolver uma mensagem de aceitação.

    ![Implementar a função](./media/contoso-migration-rebuild/function9.png)




## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, a Contoso precisa totalmente operacionalizar e proteger sua infra-estrutura de novo.

### <a name="security"></a>Segurança

- Contoso precisa garantir que seus bancos de dados novos são seguros. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- A aplicação tem de ser atualizado para utilizar SSL com certificados. A instância de contentor deve ser reimplantada para responder a 443.
- Eles devem considerar a utilização do Cofre de chaves para proteger segredos para as aplicações do Service Fabric. [Saiba mais](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>As cópias de segurança e recuperação após desastre

- A Contoso precisa de analisar os requisitos de cópia de segurança da base de dados do SQL do Azure. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Deve considerar implementar grupos de ativação pós-falha do SQL para fornecer ativação pós-falha para a base de dados. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Podem tirar partido de georreplicação para a SKU de premium do ACR. [Saiba mais](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

- Depois de todos os recursos são implementados, Contoso deve atribuir etiquetas do Azure com base em seus [planejamento da infraestrutura](contoso-migration-infrastructure.md#set-up-tagging).
- Licenciamento de todos os baseia-se para o custo dos serviços PaaS que Contoso está a consumir. Isso é deduzido do EA.
- Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management.

## <a name="conclusion"></a>Conclusão

Neste artigo, a Contoso reconstruir a aplicação de SmartHotel no Azure. Eles reconstruído a aplicação no local VM de front-end para aplicações Web de serviços de aplicações do Azure. Eles criaram o aplicação back-end com microsserviços implementados para os contentores geridos pelo Azure Kubernetes Service (AKS). Eles avançada funcionalidade de aplicação com uma aplicação de fotografias de estimação.




