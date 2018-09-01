---
title: Alterar e Reimplementar um microsserviço | Documentos da Microsoft
description: Este tutorial mostra como alterar e Reimplementar um microsserviço na monitorização remota
author: giyeh
manager: hegate
ms.author: giyeh
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: e15e17a499ad33a270b220fa7483d96c2945f6bb
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338082"
---
# <a name="customize-and-redeploy-a-microservice"></a>Personalizar e reimplementar um microsserviço

Este tutorial mostra como editar uma dos microsserviços na solução de monitorização remota, crie uma imagem de sua microsserviços, implantar a imagem ao seu hub do docker e, em seguida, utilizá-lo na solução de monitorização remota. Para apresentar esse conceito, o tutorial utiliza um cenário básico em que chama um API de microsserviços e alterar a mensagem de estado de "Ativo e bem" para "New edita Made aqui!"

Solução de monitorização remota utiliza microsserviços que são criados através de imagens do docker que são obtidas a partir de um hub do docker. 

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Editar e criar um microsserviço na solução de monitorização remota
> * Criar uma imagem do docker
> * Enviar uma imagem do docker para o docker hub
> * Extraia a imagem do docker nova
> * Visualizar as alterações 

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de:

>[!div class="checklist"]
> * [Implementar localmente a acelerador de soluções de monitorização remota](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Uma conta do Docker](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - necessário para ver a resposta de API

## <a name="call-the-api-and-view-response-status"></a>Chamar o estado de resposta de API e da vista

Nesta parte, chama o padrão IoT hub manager microsserviços API. A API devolve uma mensagem de estado que alterar mais tarde ao personalizar os microsserviços.

1. Certifique-se de que a solução de monitorização remota é executada localmente no seu computador.
2. Localize onde transferiu o Postman e abri-lo.
3. No Postman, introduza o seguinte no GET: http://localhost:8080/iothubmanager/v1/status.
4. Ver o retorno e deverá ver, "Status": "OK: ativo e bem".
![Mensagem de ativo e bem Postman](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Alterar o estado e criar a imagem

Agora, altere a mensagem de estado de microsserviços o Gestor do Hub Iot "Novas edições feitas aqui!" e, em seguida, recriar a imagem do docker com este novo Estado. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Certifique-se de que o seu terminal está aberto e altere o diretório em que tem um clone a solução de monitorização remota. 
2. Altere o diretório para "... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/WebService/v1/Controllers".
3. Abra StatusController.cs em qualquer editor de texto ou IDE que quiser. 
4. Localize o código a seguir:

    ```javascript
    return new StatusApiModel(true, "Alive and well");
    ```

    e alterá-lo para o código abaixo e guarde-o.

    ```javascript
    return new StatusApiModel(true, "New Edits Made Here!");
    ```

5. Voltar para o seu terminal, mas agora, altere ao diretório seguinte: "... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/scripts/docker".
6. Para criar a imagem do docker nova, escreva

    ```cmd/sh
    sh build
    ```

7. Para verificar a que sua nova imagem foi criada com êxito, escreva

    ```cmd/sh
    docker images 
    ```

O repositório deve ser "azureiotpcs/iothub-manager-dotnet".

![Imagem do docker com êxito](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Etiquetar e enviar a imagem
Antes de poder enviar a nova imagem do docker para um hub do docker, Docker espera que as imagens sejam marcadas. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Localize o ID da imagem da imagem do docker que criou ao escrever:

    ```cmd/sh
    docker images
    ```

2. Etiquetar a imagem com o tipo de "teste"

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Para enviar a sua imagem recentemente marcada ao seu hub do docker, escreva

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Abra o browser de internet e aceda ao seu [hub do docker](https://hub.docker.com/) e iniciar sessão.
5. Agora, deve ver a imagem do docker recentemente enviada por push no seu hub do docker.
![Imagem do docker no docker hub](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Atualizar a sua solução de monitorização remota
Agora tem de atualizar o docker-Compose local para extrair a imagem do docker nova do seu hub do docker. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Volte para o terminal e mude para o diretório seguinte: "... Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local".
2. Abra o docker-Compose em qualquer editor de texto ou IDE que quiser.
3. Localize o código a seguir:

    ```docker
    image: azureiotpcs/pcs-auth-dotnet:testing
    ```

    e altere-o para ser semelhante à imagem abaixo e guarde-o.

    ```cmd/sh
    image: [docker ID]/pcs-auth-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Ver o novo estado de resposta
Concluir voltar a implementar uma instância local da solução de monitorização remota e visualizar a nova resposta de estado no Postman.

1. Volte para o seu terminal e mude para o seguinte diretório: "... Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local".
2. Comece a sua instância local da solução de monitorização remota escrevendo o seguinte comando num terminal:

    ```cmd/sh
    docker-compose up
    ```

3. Localize onde transferiu o Postman e abri-lo.
4. No Postman, introduza o seguinte pedido no GET: http://localhost:8080/iothubmanager/v1/status. Deverá ver agora, "Status": "OK: novas edições feitas aqui!".

![Nova edita feita aqui postman mensagem](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Resolução de problemas

Se estiver a executar com problemas, tente remover as imagens do docker e contentores no computador local.

1. Para remover todos os contentores, primeiro terá de parar todos os contentores em execução. Abra o terminal e introduza

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Para remover todas as imagens, abra o terminal e o tipo 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Pode verificar se existem quaisquer contentores na máquina, escrevendo

    ```cmd/sh
    docker ps -aq 
    ```

    Se removeu com êxito todos os contentores, nada deverão aparecer no.

4. Pode verificar se existem quaisquer imagens na máquina, escrevendo

    ```cmd/sh
    docker images
    ```

    Se removeu com êxito todos os contentores, nada deverão aparecer no.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, viu como:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Editar e criar um microsserviço na solução de monitorização remota
> * Criar uma imagem do docker
> * Enviar uma imagem do docker para o docker hub
> * Extraia a imagem do docker nova
> * Visualizar as alterações 

A próxima coisa para experimentar é [personalizando os microsserviços de simulador de dispositivo na solução de monitorização remota](iot-accelerators-microservices-example.md)

Para obter mais informações para desenvolvedores sobre a solução de monitorização remota, consulte:

* [Guia de referência do Programador](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

