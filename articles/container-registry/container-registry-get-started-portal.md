---
title: Guia de introdução - Criar um registo do Docker privado no Azure com o portal do Azure
description: Aprenda rapidamente a criar um registo do contentor do Docker com o portal do Azure.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: db112f7f8f486093509a86f9781c30133925c25f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Criar um registo de contentores com o portal do Azure

Um registo de contentor do Azure é um registo do Docker privado no Azure, onde pode armazenar e gerir as imagens privadas de contentor do Docker. Neste início rápido, vai criar um registo de contentor com o portal do Azure, enviar uma imagem de contentor para o registo e, por fim, implementar o contentor a partir do registo no Azure Container Instances (ACI).

Para concluir este guia de introdução, tem de ter o Docker instalado localmente. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.

![Criar um registo de contentor no portal do Azure][qs-portal-01]

Introduza os valores para o **Nome do registo** e o **Grupo de recursos**. O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. Crie um novo grupo de recursos com o nome `myResourceGroup` e para a **SKU**, selecione "Básico". Selecione **Criar** para implementar a instância ACR.

![Criar um registo de contentor no portal do Azure][qs-portal-03]

Neste guia de introdução, criámos um registo *Básico*. O Azure Container Registry está disponível em várias SKUs diferentes, descritas brevemente na seguinte tabela. Para obter mais detalhes sobre cada uma, veja [SKUs do registo de contentor][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Quando a mensagem **Implementação concluída com êxito** é apresentada, selecione o registo do contentor no portal e, em seguida, selecione **Chaves de acesso**.

![Criar um registo de contentor no portal do Azure][qs-portal-05]

Em **Utilizador administrador**, selecione **Ativar**. Tome nota dos seguintes valores:

* Servidor de início de sessão
* Nome de utilizador
* palavra-passe

Utilize estes valores nos passos seguintes ao trabalhar com o registo com a CLI do Docker.

![Criar um registo de contentor no portal do Azure][qs-portal-06]

## <a name="log-in-to-acr"></a>Iniciar sessão no ACR

Antes de emitir e solicitar imagens de contentor, tem de iniciar sessão na instância do ACR. Para tal, utilize o comando [início de sessão do docker][docker-login]. Substitua os valores do *nome de utilizador*, da *palavra-passe* e do *servidor de início de sessão* pelos que anotou no passo anterior.

```bash
docker login --username <username> --password <password> <login server>
```

O comando devolve `Login Succeeded` depois de estar concluído. Também poderá ver um aviso de segurança que recomenda a utilização do parâmetro `--password-stdin`. Enquanto a sua utilização está fora do âmbito deste artigo, recomendamos que siga esta melhor prática. Veja a referência do comando [início de sessão do docker][docker-login] para obter mais informações.

## <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para o seu Azure Container Registry, primeiro tem de ter uma imagem. Se for necessário, execute o seguinte comando para solicitar uma imagem existente do Hub do Docker.

```bash
docker pull microsoft/aci-helloworld
```

Antes de enviar a imagem para o registo, tem de etiquetar a imagem com o nome de servidor de início de sessão do ACR. Crie uma etiqueta para a imagem com o comando [etiqueta do docker][docker-tag]. Substitua *servidor de início de sessão* pelo nome do servidor de início de sessão que registou anteriormente.

```bash
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Por último, utilize o [envio do docker][docker-push] para enviar a imagem para a instância do ACR. Substitua *servidor de início de sessão* pelo nome do servidor de início de sessão da sua instância do ACR.

```bash
docker push <login server>/aci-helloworld:v1
```

O resultado de um comando `docker push` bem-sucedido é semelhante a:

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```

## <a name="list-container-images"></a>Listar imagens de contentor

Para listar as imagens na sua instância do ACR, navegue para o registo no portal e selecione **Repositórios** e, em seguida, selecione o repositório que criou com `docker push`.

Neste exemplo, selecionamos o repositório **aci-helloworld** e podemos ver a imagem etiquetada com `v1` em **ETIQUETAS**.

![Criar um registo de contentor no portal do Azure][qs-portal-09]

## <a name="deploy-image-to-aci"></a>Implementar a imagem no ACI

Para implementar uma instância a partir do registo, é necessário navegar para o repositório (aci-helloworld) e, em seguida, clicar nas reticências junto a v1.

![Iniciar uma Instância de Contentor do Azure a partir do portal][qs-portal-10]

No menu de contexto apresentado, selecione **Executar instância**:

![Iniciar o menu de contexto do ACI][qs-portal-11]

Preencha **Nome do contentor**, certifique-se de que a subscrição correta está selecionada, selecione o **Grupo de Recursos** existente ("myResourceGroup") e, em seguida, clique em **OK** para iniciar a Instância de Contentor do Azure.

![Iniciar as opções de implementação do ACI][qs-portal-12]

Quando inicia a implementação, é colocado um mosaico no dashboard do portal que indica o progresso da implementação. Uma vez concluída a implementação, o mosaico é atualizado para apresentar o seu novo grupo de contentores **mycontainer**.

![Estado da implementação do ACI][qs-portal-13]

Selecione o grupo de contentores mycontainer para apresentar as propriedades do grupo de contentores. Tome nota do **Endereço IP** do grupo de contentores, bem como do **ESTADO** do contentor.

![Detalhes do contentor do ACI][qs-portal-14]

## <a name="view-the-application"></a>Ver a aplicação

Assim que o contentor estiver no estado **Em execução**, utilize o seu browser preferido para navegar para o endereço IP que anotou no passo anterior para apresentar a aplicação.

![Aplicação Hello World no browser][qs-portal-15]

## <a name="clean-up-resources"></a>Limpar recursos

Para limpar os recursos, navegue para o grupo de recursos **myResourceGroup** no portal. Assim que o grupo de recursos estiver carregado, clique em **Eliminar grupo de recursos** para remover o grupo de recursos, o Azure Container Registry e todos os Azure Container Instances.

![Criar um registo de contentor no portal do Azure][qs-portal-08]

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um Azure Container Registry com a CLI do Azure e iniciou uma instância do mesmo através do Azure Container Instances. Avance para o tutorial do Azure Container Instances para ver mais detalhadamente o ACI.

> [!div class="nextstepaction"]
> [Tutoriais do Azure Container Instances][container-instances-tutorial-prepare-app]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png
[qs-portal-10]: ./media/container-registry-get-started-portal/qs-portal-10.png
[qs-portal-11]: ./media/container-registry-get-started-portal/qs-portal-11.png
[qs-portal-12]: ./media/container-registry-get-started-portal/qs-portal-12.png
[qs-portal-13]: ./media/container-registry-get-started-portal/qs-portal-13.png
[qs-portal-14]: ./media/container-registry-get-started-portal/qs-portal-14.png
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
