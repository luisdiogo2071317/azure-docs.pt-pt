---
title: Alterar e volte a implementar um microsserviço | Microsoft Docs
description: Este tutorial mostra como alterar e volte a implementar um microsserviço na monitorização remota
services: ''
suite: iot-suite
author: giyeh
manager: hegate
ms.author: giyeh
ms.service: iot-suite
ms.date: 04/19/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: b9be74b4ef5a1239f6ce753ebf41af6b5dbacb5e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="customize-and-redeploy-a-microservice"></a>Personalizar e voltar a implementar um microsserviço

Este tutorial mostra como editar um micro-serviços na solução de monitorização remota, criar uma imagem do seu microsserviço, implementar a imagem para o seu hub de docker e, em seguida, utilizá-lo na solução de monitorização remota. Para apresentar este conceito, o tutorial utiliza um cenário básico onde chamar um API de microsserviço e alterar a mensagem de estado de "Alive e bem" para "Novo edita Made aqui!"

Solução de monitorização remota utiliza micro-serviços criados com as imagens de docker que são solicitadas a partir de um hub de docker. 

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Editar e criar um microsserviço na solução de monitorização remota
> * Construir uma imagem do docker
> * Emitir uma imagem de docker para o seu hub de docker
> * Solicitar a nova imagem do docker
> * Visualizar as alterações 

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, tem de:

>[!div class="checklist"]
> * [Implementar a solução monitorização remota pré-configurada localmente](../iot-accelerators/iot-accelerators-remote-monitoring-deploy-local.md)
> * [Uma conta de Docker](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - necessário para ver a resposta da API

## <a name="call-the-api-and-view-response-status"></a>Chamar o estado de resposta da API e vista

Nesta parte, tem de chamar o predefinição IoT hub manager microsserviço API. A API devolve uma mensagem de estado que alterar mais tarde ao personalizar o microsserviço.

1. Certifique-se que a solução de monitorização remota é executada localmente no seu computador.
2. Localize onde transferiu o Postman e abri-lo.
3. Na Postman, introduza o seguinte no GET: http://localhost:8080/iothubmanager/v1/status.
4. Ver o retorno e deverá ver, "Estado": "OK: Alive e bem".
![Mensagem de Alive e bem Postman](media/iot-suite-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Alterar o estado e criar a imagem

Alterar agora a mensagem de estado de microsserviço o Gestor de Hub Iot para "Novo edições efetuadas aqui!" e, em seguida, recriar a imagem de docker com este novo Estado. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Certifique-se o que seu terminal está aberta e altere o diretório onde tem clonou a solução de monitorização remota. 
2. Altere o diretório para ".. azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/WebService/v1/Controllers".
3. Abra StatusController.cs em qualquer editor de texto ou IDE que pretender. 
4. Localize o seguinte código:

    ```javascript
    return new StatusApiModel(true, "Alive and well");
    ```

    Altere-a para o código abaixo e guarde-o.

    ```javascript
    return new StatusApiModel(true, "New Edits Made Here!");
    ```

5. Volte ao seu terminal, mas agora alterar ao diretório seguinte: "... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/scripts/docker".
6. Para criar a nova imagem de docker, escreva

    ```cmd/sh
    sh build
    ```

7. Para verificar a que sua nova imagem foi criada com êxito, escreva

    ```cmd/sh
    docker images 
    ```

O repositório deve ser "azureiotpcs/iothub-manager-dotnet".

![Imagem de docker com êxito](media/iot-suite-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Etiqueta e a imagem de push
Antes de pode enviar a nova imagem de docker a um concentrador de docker, Docker espera que as imagens sejam etiquetados. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Localize o ID da imagem da imagem do docker que criou, escrevendo:

    ```cmd/sh
    docker images
    ```

2. Para marcar a imagem com o tipo de "teste"

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Para enviar a imagem recentemente marcada para o seu hub de docker, escreva

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Abra o seu browser e aceda à sua [docker hub](https://hub.docker.com/) e iniciar sessão.
5. Deverá ver a imagem de docker recentemente premido no seu hub de docker.
![Imagem de docker no hub de docker](media/iot-suite-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Atualizar a sua solução de monitorização remota
Agora tem de atualizar o seu docker-Compose.yml local para solicitar a sua nova imagem de docker do seu hub de docker. Caso se depare com problemas aqui, consulte a nossa [resolução de problemas](#Troubleshoot) secção.

1. Volte para o terminal e altere o diretório seguinte: ".. Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local".
2. Abra docker-Compose.yml em qualquer editor de texto ou IDE que pretender.
3. Localize o seguinte código:

    ```docker
    image: azureiotpcs/pcs-auth-dotnet:testing
    ```

    e altere-o para o aspeto a imagem abaixo e guarde-o.

    ```cmd/sh
    image: [docker ID]/pcs-auth-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Ver o novo estado de resposta
Concluir a cópia de segurança através da reimplementação uma instância local da solução de monitorização remota e ver a resposta de estado de novo no Postman.

1. Volte ao seu terminal e altere o diretório seguinte: ".. Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local".
2. Inicie a instância local da solução de monitorização remota, escrevendo o comando seguinte no terminal:

    ```cmd/sh
    docker-compose up
    ```

3. Localize onde transferiu o Postman e abri-lo.
4. Na Postman, introduza o seguinte pedido no GET: http://localhost:8080/iothubmanager/v1/status. Deverá ver, "Estado": "OK: novo edições efetuadas aqui!".

![Nova edita efetuadas aqui postman mensagem](media/iot-suite-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Resolver problemas

Se estiver a executar para problemas, tente remover a imagens de docker e contentores no computador local.

1. Para remover todos os contentores, primeiro terá de parar todos os contentores em execução. Abra o terminal e introduza

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Para remover todas as imagens, abra o terminal e o tipo 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Pode verificar se existem quaisquer contentores no computador, escrevendo

    ```cmd/sh
    docker ps -aq 
    ```

    Se removeu com êxito todos os contentores, nada deverão aparecer no.

4. Pode verificar se existem quaisquer imagens no computador, escrevendo

    ```cmd/sh
    docker images
    ```

    Se removeu com êxito todos os contentores, nada deverão aparecer no.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vimos como:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Editar e criar um microsserviço na solução de monitorização remota
> * Construir uma imagem do docker
> * Emitir uma imagem de docker para o seu hub de docker
> * Solicitar a nova imagem do docker
> * Visualizar as alterações 

O seguinte para tentar é [personalizar o microsserviço do simulador de dispositivo na solução de monitorização remota](iot-suite-remote-monitoring-test.md)

Para obter mais informações de programador sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

