---
title: "Implementar uma aplicação do Azure Service Fabric num Party Cluster | Microsoft Docs"
description: "Saiba como implementar uma aplicação num Party Cluster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 91d4398589707e8007c4b93639ddb568e39f51a7
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a>Implementar uma aplicação num Party Cluster no Azure
Este tutorial é a segunda parte de uma série e mostra-lhe como implementar uma aplicação do Azure Service Fabric num Party Cluster no Azure.

Na segunda parte desta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * [Criar uma aplicação .NET do Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * Implementar a aplicação num cluster remoto
> * [Configurar o CI/CD com o Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Configurar a monitorização e os diagnósticos da aplicação](service-fabric-tutorial-monitoring-aspnet.md)

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * Implementar uma aplicação num cluster remoto com o Visual Studio
> * Remover uma aplicação de um cluster com o Service Fabric Explorer

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial:
- Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Instale o Visual Studio 2017](https://www.visualstudio.com/) e as cargas de trabalho de **desenvolvimento no Azure** e **desenvolvimento na Web e em ASP.NET**.
- [Instale o SDK do Service Fabric](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Transferir a aplicação de votação de exemplo
Se não conseguiu criar a aplicação de votação de exemplo na [primeira parte desta série de tutoriais](service-fabric-tutorial-create-dotnet-app.md), pode transferi-la. Numa janela do comando, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Configurar um Party Cluster
Os party clusters são clusters do Service Fabric gratuitos, limitados temporalmente, alojados no Azure e executados pela equipa do Service Fabric, nos quais qualquer pessoa pode implementar aplicações e saber mais sobre a plataforma. Gratuitamente!

Para obter acesso a um Party Cluster, navegue para este site: http://aka.ms/tryservicefabric e siga as instruções para obter acesso a um cluster. Precisa de uma conta do Facebook ou do GitHub para obter acesso a um Party Cluster.

Pode utilizar o seu próprio cluster em vez do Party Cluster, se quiser.  O front-end da Web ASP.NET Core utiliza o proxy inverso para comunicar com o back-end de serviço com estado.  Os Party Clusters e o cluster de desenvolvimento local têm o proxy inverso ativado por predefinição.  Se implementar a aplicação de votação de exemplo para o seu próprio cluster, tem de [ativar o proxy inverso no cluster](service-fabric-reverseproxy.md#setup-and-configuration).

> [!NOTE]
> Os Party Clusters não estão protegidos, pelo que as suas aplicações e quaisquer dados colocados nos mesmos podem estar visíveis para outros utilizadores. Não implemente nada que não quer que os outros vejam. Certifique-se de que lê os nossos Termos de Utilização para obter todos os detalhes.

Inicie sessão e [adira a um cluster do Windows](http://aka.ms/tryservicefabric). Transfira o certificado PFX para o seu computador ao clicar na ligação **PFX**. O certificado e o valor do **Ponto final da ligação** são utilizados nos passos seguintes.

![PFX e ponto final de ligação](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

Num computador Windows, instale o PFX no arquivo de certificados *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:
\CurrentUser\My


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```


## <a name="deploy-the-app-to-the-azure"></a>Implementar a aplicação no Azure
Agora que a aplicação está pronta, pode implementá-la no Party Cluster diretamente a partir do Visual Studio.

1. Clique com o botão direito do rato em **Voting**, no Explorador de Soluções, e escolha **Publish** (Publicar). 

    ![Caixa de diálogo Publicar](./media/service-fabric-quickstart-containers/publish-app.png)

2. Copie o **Ponto Final da Ligação** na página Cluster de terceiros para o campo **Ponto Final da Ligação**. Por exemplo, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Clique em **Parâmetros de Ligação Avançada** e preencha as informações seguintes.  Os valores *FindValue* e *ServerCertThumbprint* têm de coincidir com o thumbprint do certificado instalado no passo anterior. Clique em **Publicar**. 

    Após a conclusão da publicação, deve conseguir enviar um pedido para a aplicação através de um browser.

3. Abra o seu browser preferencial e escreva o endereço do cluster (o ponto final da ligação sem as informações de porta, por exemplo, win1kw5649s.westus.cloudapp.azure.com).

    Deverá ver agora o mesmo resultado que viu quando executou a aplicação localmente.

    ![Resposta da API a partir do Cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>Remover a aplicação de um cluster com o Service Fabric Explorer
O Service Fabric Explorer é uma interface de utilizador gráfica para explorar e gerir aplicações num cluster do Service Fabric.

Para remover a aplicação do Party Cluster:

1. Navegue para o Service Fabric Explorer através da ligação fornecida pela página de inscrição do Party Cluster. Por exemplo, https://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. No Service Fabric Explorer, navegue para o nó **fabric:/Voting** na vista de árvore no lado esquerdo.

3. Clique no botão **Ação** no painel direito **Essentials** e selecione **Eliminar Aplicação**. Confirme a eliminação da instância da aplicação, que remove a instância da aplicação em execução no cluster.

![Eliminar Aplicação no Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a>Remover o tipo de aplicação de um cluster com o Service Fabric Explorer
As aplicações são implementadas como tipos de aplicação num cluster do Service Fabric, o que lhe permite ter múltiplas instâncias e versões da aplicação em execução no cluster. Depois de remover a instância da aplicação em execução, também podemos remover o tipo, para concluir a limpeza da implementação.

Para obter mais informações sobre o modelo de aplicação no Service Fabric, veja [Modelar uma aplicação no Service Fabric](service-fabric-application-model.md).

1. Navegue para o nó **VotingType** na vista de árvore.

2. Clique no botão **Ação** no painel direito **Essentials** e selecione **Não Aprovisionar Tipo**. Confirme a anulação do aprovisionamento do tipo de aplicação.

![Não Aprovisionar Tipo de Aplicação no Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Isto conclui o tutorial.

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Implementar uma aplicação num cluster remoto com o Visual Studio
> * Remover uma aplicação de um cluster com o Service Fabric Explorer

Avance para o tutorial seguinte:
> [!div class="nextstepaction"]
> [Configurar a integração contínua com o Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
