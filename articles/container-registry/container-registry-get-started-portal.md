---
title: "Guia de introdução - Criar um registo do Docker privado no Azure com o portal do Azure"
description: Aprenda rapidamente a criar um registo do contentor do Docker com o portal do Azure.
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 514fa3490e480647f0923c99bd9606a3726d4d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Criar um registo de contentores com o portal do Azure

Um registo de contentor do Azure é um registo do Docker privado no Azure, onde pode armazenar e gerir as imagens privadas de contentor do Docker. Neste guia de introdução, irá criar um registo de contentor com o portal do Azure.

Para concluir este guia de introdução, tem de ter o Docker instalado localmente. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ou [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Selecione **Novo** > **Contentores** > **Azure Container Registry**.

![Criar um registo de contentor no portal do Azure][qs-portal-01]

Introduza os valores para o **Nome do registo** e o **Grupo de recursos**. O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. Crie um novo grupo de recursos com o nome `myResourceGroup` e para a **SKU**, selecione "Básico". Selecione **Criar** para implementar a instância ACR.

![Criar um registo de contentor no portal do Azure][qs-portal-03]

Neste guia de introdução, criámos um registo *Básico*. O Azure Container Registry está disponível em várias SKUs diferentes, descritas brevemente na seguinte tabela. Para obter mais detalhes sobre cada uma, veja [Container registry SKUs (SKUs do registo de contentor)](container-registry-skus.md).

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

Antes de emitir e solicitar imagens de contentor, tem de iniciar sessão na instância do ACR. Para tal, utilize o comando [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/). Substitua os valores do *nome de utilizador*, da *palavra-passe* e do *servidor de início de sessão* pelos que anotou no passo anterior.

```bash
docker login --username <username> --password <password> <login server>
```

O comando devolve `Login Succeeded` depois de estar concluído. Também poderá ver um aviso de segurança que recomenda a utilização do parâmetro `--password-stdin`. Enquanto a sua utilização está fora do âmbito deste artigo, recomendamos que siga esta melhor prática. Veja a referência do comando [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/) para obter mais informações.

## <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para o seu Azure Container Registry, primeiro tem de ter uma imagem. Se for necessário, execute o seguinte comando para solicitar uma imagem existente do Hub do Docker.

```bash
docker pull microsoft/aci-helloworld
```

Antes de enviar a imagem para o registo, tem de etiquetar a imagem com o nome de servidor de início de sessão do ACR. Crie uma etiqueta para a imagem com o comando [etiqueta do docker](https://docs.docker.com/engine/reference/commandline/tag/). Substitua *servidor de início de sessão* pelo nome do servidor de início de sessão que registou anteriormente.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Por último, utilize o [envio do docker](https://docs.docker.com/engine/reference/commandline/push/) para enviar a imagem para a instância do ACR. Substitua *servidor de início de sessão* pelo nome do servidor de início de sessão da sua instância do ACR.

```
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

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar, elimine o grupo de recursos **myResourceGroup**. Se o fizer, irá eliminar o grupo de recursos, a instância do ACR e todas as imagens de contentor.

![Criar um registo de contentor no portal do Azure][qs-portal-08]

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, irá criar um Azure Container Registry a CLI do Azure. Se gostaria de utilizar o Azure Container Registry com o Azure Container Instances, continue para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances tutorials](../container-instances/container-instances-tutorial-prepare-app.md) (Tutoriais do Azure Container Instances)

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