---
title: "Criar uma Aplicação Java do Service Fabric | Microsoft Docs"
description: "Utilize o exemplo de guia de introdução do Service Fabric para criar uma aplicação Java para o Azure."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 8f4d121ba76d63b70fa6976125457942a0e98aa9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-java-application"></a>Criar uma Aplicação Java
O Azure Service Fabric é uma plataforma de sistemas distribuídos par implementar e gerir microsserviços e contentores. 

Este guia de introdução mostra como implementar a sua primeira aplicação de Java no Service Fabric com o ODE do Eclipse numa máquina de programador do Linux. Quando tiver terminado, terá uma aplicação de votações com um front-end da Web Java que guarda os resultados das votações num serviço de back-end com estado no cluster.

![Captura de Ecrã da Aplicação](./media/service-fabric-quickstart-java/votingapp.png)

Neste início rápido, vai aprender a:

> [!div class="checklist"]
> * Utilize o Eclipse como uma ferramenta para as suas aplicações de Java do Service Fabric
> * Implementar a aplicação no seu cluster local 
> * Implementar a aplicação num cluster no Azure
> * Escalar horizontalmente a aplicação em vários nós

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de início rápido:
1. [Instalar o SDK do Service Fabric e Interface de Linha de Comandos (CLI) do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Instale o Git](https://git-scm.com/).
3. [Instalar o Eclipse](https://www.eclipse.org/downloads/)
4. [Configure o Ambiente de Java](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), garantindo que segue os passos opcionais para instalar o plug-in do Eclipse 

## <a name="download-the-sample"></a>Transferir o exemplo
Numa janela do comando, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local.
```
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Executar a aplicação localmente
1. Inicie o seu cluster local com o seguinte comando:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    O arranque do cluster local demora algum tempo. Para confirmar que o cluster está totalmente operacional, aceda ao Service Fabric Explorer em **http://localhost:19080**. Os cinco nós em bom estado indicam que o cluster local está a funcionar. 
    
    ![Cluster local em bom estado de funcionamento](./media/service-fabric-quickstart-java/localclusterup.png)

2. Abra o Eclipse.
3. Clique em Ficheiro -> Abrir Projetos no Sistema de Ficheiros... 
4. Clique em Diretório e escolha o diretório `Voting` da pasta `service-fabric-java-quickstart` que clonou a partir do Github. Clique em Concluir. 

    ![Caixa de Diálogo Importar Eclipse](./media/service-fabric-quickstart-java/eclipseimport.png)
    
5. Tem agora o projeto `Voting` no Explorador do Pacote para o Eclipse. 
6. Clique com o botão direito do rato no projeto e selecione **Publicar Aplicação...** sob o menu pendente **Service Fabric**. Escolha **PublishProfiles/Local.json** como o Perfil de Destino e clique em Publicar. 

    ![Caixa de Diálogo Publicar](./media/service-fabric-quickstart-java/localjson.png)
    
7. Abra o browser favorito e aceda à aplicação em **http://localhost:8080**. 

    ![Front-end da aplicação Local](./media/service-fabric-quickstart-java/runninglocally.png)
    
Agora, pode adicionar um conjunto de opções de voto e começar a recolher votos. A aplicação é executada e armazena todos os dados no seu cluster do Service Fabric, sem que seja preciso uma base de dados separada.

## <a name="deploy-the-application-to-azure"></a>Implementar a aplicação no Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Configurar o Cluster do Azure Service Fabric
Para implementar a aplicação num cluster no Azure, crie o seu próprio cluster.

Party clusters são clusters do Service Fabric gratuitos e de tempo limitado, alojados no Azure. Estes são executados pela equipa do Service Fabric, onde qualquer pessoa pode implementar aplicações e saber mais sobre a plataforma. Para obter acesso a um Party Cluster, [siga as instruções](http://aka.ms/tryservicefabric). 

Para realizar operações de gestão no cluster de grupo seguro, pode utilizar o Service Fabric Explorer, a CLI ou o PowerShell. De modo a utilizar o Service Fabric Explorer, tem de transferir o ficheiro PFX do site do Cluster de Grupo e importar o certificado para o seu arquivo de certificados (Windows ou Mac) ou para o próprio browser (Ubuntu). Não há nenhuma palavra-passe para os certificados autoassinados do cluster de grupo. 

Para realizar operações de gestão com o Powershell ou a CLI, vai precisar do PFX (Powershell) ou do PEM (CLI). Para converter o PFX num ficheiro PEM, execute o seguinte comando:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Para obter informações sobre como criar o seu próprio cluster, veja [Crie um cluster do Service Fabric no Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> O serviço de Spring Boot está configurado para escutar tráfego de entrada na porta 8080. Certifique-se de que a porta está aberta no seu cluster. Se estiver a utilizar um Cluster de Grupo, esta porta estará aberta.
>

### <a name="add-certificate-information-to-your-application"></a>Adicionar informações de certificados à sua aplicação

Têm de ser adicionados thumbprints de certificados à sua aplicação, uma vez que está a utilizar modelos de programação do Service Fabric. 

1. Precisará do thumbprint do seu certificado no ficheiro ```Voting/VotingApplication/ApplicationManiest.xml``` ao executar num cluster seguro. Execute o seguinte comando para extrair o thumbprint do certificado.

    ```bash
    openssl x509 -in [CERTIFICATE_FILE] -fingerprint -noout
    ```

2. No ```Voting/VotingApplication/ApplicationManiest.xml```, adicione o seguinte fragmento de código na etiqueta **ApplicationManifest**. O **X509FindValue** deverá ser o thumbprint do passo anterior (sem ponto e vírgula). 

    ```xml
    <Certificates>
        <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
    </Certificates>   
    ```
    
### <a name="deploy-the-application-using-eclipse"></a>Implementar a aplicação com o Eclipse
Agora que a aplicação e o cluster estão prontos, pode implementá-los num cluster diretamente a partir do Eclipse.

1. Abra o ficheiro **Cloud.json** sob o diretório **PublishProfiles** e preencha os campos `ConnectionIPOrURL` e `ConnectionPort` adequadamente. É apresentado um exemplo: 

    ```bash
    {
         "ClusterConnectionParameters": 
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "[path_to_your_pem_file_on_local_machine]",
            "ClientCert": "[path_to_your_pem_file_on_local_machine]"
         }
    }
    ```

2. Clique com o botão direito do rato no projeto e selecione **Publicar Aplicação...** sob o menu pendente **Service Fabric**. Escolha **PublishProfiles/Cloud.json** como o Perfil de Destino e clique em Publicar. 

    ![Cloud da Caixa de diálogo Publicar](./media/service-fabric-quickstart-java/cloudjson.png)

3. Abra o browser favorito e aceda à aplicação em **http://\<ConnectionIPOrURL>:8080**. 

    ![Cloud de front-end da aplicação](./media/service-fabric-quickstart-java/runningcloud.png)
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Dimensionar aplicações e serviços num cluster
Os serviços podem ser facilmente dimensionados num cluster para se prepararem para alterações à carga nos serviços. Para dimensionar um serviço, tem de alterar o número de instâncias em execução no cluster. Existem várias formas de dimensionar os seus serviços. Pode utilizar scripts ou comandos na CLI do Service Fabric (sfctl). Neste exemplo, estamos a utilizar o Service Fabric Explorer.

O Service Fabric Explorer é executado em todos os clusters do Service Fabric e pode ser acedido num browser, navegando para a porta (19080) de gestão HTTP dos clusters; por exemplo `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

Para dimensionar o serviço de front-end da Web, execute os seguintes passos:

1. Abra o Service Fabric Explorer no seu cluster - por exemplo, `https://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Clique nas reticências (três pontos) junto ao nó **fabric:/Voting/VotingWeb**, na vista de árvore, e escolha**Dimensionar Serviço**.

    ![Dimensionar Serviço no Service Fabric Explorer](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Agora, pode optar por dimensionar o número de instâncias do serviço de front-end da Web.

3. Altere o número para **2** e clique em **Dimensionar Serviço**.
4. Clique no nó **fabric:/Voting/VotingWeb**, na vista de árvore, e expanda o nó de partição (representado por uma GUID).

    ![Dimensionar Serviço no Service Fabric Explorer Completo](./media/service-fabric-quickstart-java/servicescaled.png)

    Agora, pode ver que o serviço tem duas instâncias e na vista de árvore vê em que nós as instâncias são executadas.

Através desta simples tarefa de gestão, duplicámos os recursos disponíveis para o nosso serviço de front-end processar a carga de utilizador. É importante compreender que não precisa de várias instâncias de um serviço para o executar de forma fiável. Se um serviço falhar, o Service Fabric certifica-se de que uma nova instância de serviço é executado no cluster.

## <a name="next-steps"></a>Passos seguintes
Neste início rápido, aprendeu a:

> [!div class="checklist"]
> * Utilize o Eclipse como uma ferramenta para as suas aplicações de Java do Service Fabric
> * Implementar aplicações Java no seu cluster local 
> * Implementar aplicações Java num cluster no Azure
> * Escalar horizontalmente a aplicação em vários nós

* Saiba mais sobre a [depuração de serviços no Java com o Eclipse](service-fabric-debugging-your-application-java.md)
* Saiba mais sobre a [configuração da integração contínua e implementação com o Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
* Veja outros [Exemplos de Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)