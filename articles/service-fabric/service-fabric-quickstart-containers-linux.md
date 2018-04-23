---
title: Criar uma aplicação de contentor do Azure Service Fabric no Linux | Microsoft Docs
description: Neste início rápido, vai criar a sua primeira aplicação de contentor do Linux no Azure Service Fabric.  Crie uma imagem do Docker com a sua aplicação, envie-a para um registo de contentor e crie e implemente uma aplicação de contentor do Service Fabric.
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 65f048d67ef5f250691700a382e781814c57e8a8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="quickstart-deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Início Rápido: implementar uma aplicação de contentor do Linux do Azure Service Fabric no Azure
O Azure Service Fabric é uma plataforma de sistemas distribuídos par implementar e gerir microsserviços e contentores dimensionáveis e fiáveis. 

Este guia de início rápido mostra como implementar contentores do Linux num cluster do Service Fabric. Quando tiver terminado, terá uma aplicação de voto constituída por um front-end da Web Python e um back-end Redis em execução num cluster do Service Fabric. Também irá aprender a efetuar a ativação pós-falha de uma aplicação e a dimensionar uma aplicação no seu cluster.

![Página Web da aplicação de Voto][quickstartpic]

Neste início rápido, vai utilizar o ambiente de Bash no Azure Cloud Shell para executar comandos da CLI do Service Fabric. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se estiver a executar o Cloud Shell pela primeira vez, é-lhe pedido para configurar a partilha de ficheiros do `clouddrive`. Pode aceitar as predefinições ou anexar uma partilha de ficheiros existente. Para obter mais informações, veja [Configurar uma `clouddrive` partilha de ficheiros](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share).

## <a name="get-the-application-package"></a>Obter o pacote de aplicação
Para implementar contentores no Service Fabric, precisa de um conjunto de ficheiros de manifesto (a definição da aplicação), que descrevem os contentores individuais e a aplicação.

No Cloud Shell, utilize o git para clonar uma cópia da definição da aplicação; em seguida, altere os diretórios para o diretório `Voting` do seu clone. 

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="create-a-service-fabric-cluster"></a>Criar um cluster do Service Fabric
Para implementar a aplicação no Azure, precisa de um cluster do Service Fabric para executar a aplicação. Os clusters comemorativos proporcionam uma forma fácil de criar rapidamente um cluster do Service Fabric. Os clusters comemorativos são clusters do Service Fabric gratuitos e de tempo limitado, alojados no Azure e executados pela equipa do Service Fabric. Pode utilizar os clusters comemorativos para implementar aplicações e obter informações sobre a plataforma. O cluster utiliza um certificado autoassinado para a segurança de "nó para nó" e de "cliente para nó".

Inicie sessão e adira a um [cluster do Linux](http://aka.ms/tryservicefabric). Transfira o certificado PFX para o seu computador ao clicar na ligação **PFX**. Clique na hiperligação **Leia-me** para localizar a palavra-passe do certificado e instruções sobre como configurar vários ambientes para utilizar o certificado. Mantenha as páginas de **Boas-vindas** e **Leia-me** abertas, irá utilizar algumas das instruções nos passos seguintes. 

> [!Note]
> Há um número limitado de clusters comemorativos por hora. Se obtiver uma mensagem um erro ao tentar inscrever-se num cluster comemorativo, pode ter de aguardar algum tempo e tentar novamente ou pode seguir estes passos em [Criar um cluster do Service Fabric no Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md) para criar um cluster na sua subscrição. 
> 
>Se não criar o seu próprio cluster, o serviço de front-end da Web está configurado para escutar tráfego de entrada na porta 80. Certifique-se de que a porta está aberta no seu cluster. (Se estiver a utilizar um cluster comemorativo, esta porta estará aberta.)
>

## <a name="configure-your-environment"></a>Configurar o ambiente
O Service Fabric fornece várias ferramentas que pode utilizar para gerir um cluster e as respetivas aplicações:

- O Service Fabric Explorer, uma ferramenta baseada no browser.
- A CLI (Interface de Linha de Comandos) do Service Fabric, que é executada na CLI 2.0 do Azure.
- Comandos do PowerShell. 

Neste início rápido,vai utilizar a CLI do Service Fabric no Cloud Shell e Service Fabric Explorer. As secções seguintes mostram como instalar o certificado necessário para ligar ao seu cluster seguro com estas ferramentas.

### <a name="configure-certificate-for-the-service-fabric-cli"></a>Configurar o certificado para a CLI do Service Fabric
Para utilizar a CLI no Cloud Shell, terá de carregar o ficheiro PFX de certificado para o Cloud Shell e, em seguida, utilizá-lo para criar um ficheiro PEM.

1. Para carregar o certificado para o seu atual diretório de trabalho no Cloud Shell, arraste o ficheiro PFX de certificado da pasta para onde foi transferido no seu computador e largue-o na janela do Cloud Shell.  

2. Para converter o ficheiro PFX para ficheiro PEM, utilize o comando seguinte. (Para os clusters comemorativos, pode copiar um comando específico para o ficheiro PFX e a palavra-passe das instruções da página **Leia-me**.)

    ```bash
    openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
    ``` 

### <a name="configure-certificate-for-service-fabric-explorer"></a>Configurar o certificado para o Service Fabric Explorer
De modo a utilizar o Service Fabric Explorer, tem de importar o ficheiro PFX de certificado que transferiu do site do Cluster Comemorativo para o seu arquivo de certificados (Windows ou Mac) ou para o próprio browser (Ubuntu). Precisará da palavra-passe da chave privada do PFX, que pode obter na página **Leia-me**.

Utilize o método que preferir para importar o certificado para o seu sistema. Por exemplo:

- No Windows: faça duplo clique no ficheiro PFX e siga as instruções para instalar o certificado no seu arquivo pessoal, `Certificates - Current User\Personal\Certificates`. Em alternativa, pode utilizar o comando do PowerShell das instruções **Leia-me**.
- No Mac: faça duplo clique no ficheiro PFX e siga as instruções para instalar o certificado na sua Keychain.
- No Ubuntu: o Mozilla Firefox é o browser predefinido do Ubuntu 16.04. Para importar o certificado para o Firefox, clique no botão de menu no canto superior direito do seu browser e, em seguida, clique em **Opções**. Na página **Preferências**, utilize a caixa de pesquisa para procurar "certificados". Clique em **Ver Certificados**, selecione o separador **Os Seus Certificados**, clique em **Importar** e siga as instruções para importar o certificado.
 
   ![Instalar o certificado no Firefox](./media/service-fabric-quickstart-containers-linux/install-cert-firefox.png) 

## <a name="deploy-the-service-fabric-application"></a>Implementar a aplicação do Service Fabric 
1. No Cloud Shell, ligue ao cluster do Service Fabric no Azure com a CLI. O ponto final é o ponto final de gestão do seu cluster. Criou o ficheiro PEM na secção anterior. (Para os clusters comemorativos, pode copiar um comando específico para o seu ficheiro PEM e ponto final de gestão, a partir das instruções da página **Leia-me**.)

    ```bash
    sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
    ```

2. Utilize o script de instalação para copiar a definição da aplicação de Voto para o cluster, registar o tipo de aplicação e criar uma instância da mesma.

    ```bash
    ./install.sh
    ```

2. Abra um browser e navegue para o ponto final do Service Fabric Explorer para o seu cluster. O ponto final tem o seguinte formato: https://\<my-azure-service-fabric-cluster-url>:19080/Explorer; por exemplo, `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. </br>(Para os clusters comemorativos, pode encontrar o ponto final do Service Fabric Explorer para o seu cluster na página de **Boas-vindas**.) 

3. Expanda o nó **Aplicações** para ver que existe agora uma entrada para o tipo de aplicação de Voto e a instância que criou.

    ![Service Fabric Explorer][sfx]

3. Para ligar ao contentor em execução, abra um browser e navegue para o URL do seu cluster; por exemplo, `http://linh1x87d1d.westus.cloudapp.azure.com:80`. Deverá ver a aplicação de Voto no browser.

    ![Página Web da aplicação de Voto][quickstartpic]


> [!NOTE]
> Também pode implementar aplicações do Service Fabric com o Docker Compose. Por exemplo, o comando seguinte pode ser utilizado para implementar e instalar a aplicação no cluster com o Docker Compose.
>  ```bash
> sfctl compose create --deployment-name TestApp --file-path ../docker-compose.yml
> ```

## <a name="fail-over-a-container-in-a-cluster"></a>Efetuar a ativação pós-falha de um contentor num cluster
O Service Fabric garante que as instâncias de contentor são movidas automaticamente para outros nós do cluster, se ocorrer uma falha. Também pode drenar manualmente um nó para contentores e, em seguida, mover harmoniosamente para outros nós do cluster. O Service Fabric fornece várias formas de dimensionar os seus serviços. Nos passos seguintes, vai utilizar o Service Fabric Explorer.

Para efetuar a ativação pós-falha do contentor de front-end, execute os seguintes passos:

1. Abra o Service Fabric Explorer no seu cluster; por exemplo, `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Clique no nó **fabric:/Voting/azurevotefront** na vista de árvore e expanda o nó de partição (representado por um GUID). Repare no nome do nó na vista de árvore, que mostra os nós nos quais o contentor está atualmente a ser executado; por exemplo, `_nodetype_4`.
3. Expanda o nó **Nós** na vista de árvore. Clique nas reticências (...) junto ao nó que está a executar o contentor.
4. Escolha **Reiniciar** para reiniciar esse nó e confirme a ação de reinício. O reinício provoca a ativação pós-falha do contentor para outro nó no cluster.

    ![Vista de nó no Service Fabric Explorer][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Dimensionar aplicações e serviços num cluster
Os serviços do Service Fabric podem ser facilmente dimensionados num cluster para se prepararem para a carga nos serviços. Para dimensionar um serviço, tem de alterar o número de instâncias em execução no cluster.

Para dimensionar o serviço de front-end da Web, execute os seguintes passos:

1. Abra o Service Fabric Explorer no seu cluster; por exemplo, `https://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Clique nas reticências (três pontos) junto ao nó **fabric:/Voting/azurevotefront** na vista de árvore e escolha **Dimensionar Serviço**.

    ![Início do serviço de dimensionamento do Service Fabric Explorer][containersquickstartscale]

  Agora, pode optar por dimensionar o número de instâncias do serviço de front-end da Web.

3. Altere o número para **2** e clique em **Dimensionar Serviço**.
4. Clique no nó **fabric:/Voting/azurevotefront** na vista de árvore e expanda o nó de partição (representado por um GUID).

    ![Serviço de dimensionamento do Service Fabric Explorer concluído][containersquickstartscaledone]

    Agora, pode ver que o serviço tem duas instâncias. Na vista de árvore, pode ver em que nós as instâncias são executadas.

Através desta simples tarefa de gestão, duplicou os recursos disponíveis para o serviço de front-end processar a carga de utilizador. É importante compreender que não precisa de várias instâncias de um serviço para que o mesmo seja executado de forma fiável. Se um serviço falhar, o Service Fabric certifica-se de que uma nova instância do serviço é executada no cluster.

## <a name="clean-up-resources"></a>Limpar recursos
1. Utilize o script de desinstalação (uninstall.sh) fornecido no modelo para eliminar a instância da aplicação do cluster local e anular o registo do tipo de aplicação. Este script demora algum tempo a limpar a instância, pelo que não deve executar o script de instalação imediatamente após este script. Pode utilizar o Service Fabric Explorer para determinar quando a instância foi removida e o tipo de aplicação cujo registo foi anulado. 

    ```bash
    ./uninstall.sh
    ```

2. Se tiver terminado de trabalhar com o cluster, pode remover o certificado do arquivo de certificados. Por exemplo:
   - No Windows: utilize o [Snap-in da MMC de certificados](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in). Certifique-se de que seleciona **A minha conta de utilizador** ao adicionar o snap-in. Navegue para `Certificates - Current User\Personal\Certificates` e remova o certificado.
   - No Mac: utilize a aplicação Keychain.
   - No Ubuntu: siga os passos utilizados para ver os certificados e remova o certificado.

3. Se não pretender continuar a utilizar o Cloud Shell, pode eliminar a conta de armazenamento associada ao mesmo para evitar encargos. Feche a sessão do Cloud Shell. No Portal do Azure, clique na conta de armazenamento associada ao Cloud Shell, clique em **Eliminar** na parte superior da página e siga as instruções.

## <a name="next-steps"></a>Passos seguintes
Neste início rápido, implementou uma aplicação de contentor do Linux num cluster do Service Fabric do Azure, efetuou a ativação pós-falha na aplicação e dimensionou a aplicação no cluster. Para saber mais sobre como trabalhar com contentores do Linux no Service Fabric, avance para o tutorial para aplicações de contentor do Linux.

> [!div class="nextstepaction"]
> [Criar uma aplicação de contentor do Linux](./service-fabric-tutorial-create-container-images.md)


[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
