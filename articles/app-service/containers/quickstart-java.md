---
title: Um início rápido para criar uma aplicação Web Java no Serviço de Aplicações do Azure no Linux
description: Neste início rápido, implemente o seu primeiro Hello World Java no Serviço de Aplicações do Azure no Linux, em apenas alguns minutos.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: b9e8d2b9eacfa5c427ffe3f27ea99bbd35651d57
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165981"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Início Rápido: Criar uma aplicação Web Java no Serviço de Aplicações no Linux

O [Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com aplicação de patches automática através do sistema operativo Linux. Este guia de início rápido mostra como utilizar a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) com o [Plug-in do Maven para as Aplicações Web do Azure (Pré-visualização)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) para implementar um ficheiro de arquivo Web de aplicação Web Java (WAR).

![Aplicação de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Criar uma aplicação em Java

Execute o seguinte comando do Maven na linha de comandos do Cloud Shell para criar uma nova aplicação Web com o nome `helloworld`:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Configurar o plug-in do Maven

Para implementar a partir do Maven, utilize o editor de código no Cloud Shell para abrir o ficheiro do projeto `pom.xml` no diretório `helloworld`. 

```bash
code pom.xml
```

Em seguida, adicione a seguinte definição de plug-in dentro do elemento `<build>` do ficheiro `pom.xml`.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
      
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
   
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
   
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> Neste artigo, estamos a trabalhar apenas com aplicações Java em pacotes de ficheiros WAR. O plug-in também suporta aplicações Web JAR. Utilize a seguinte definição de plug-in alternativa para estas aplicações. Esta configuração irá implementar um JAR criado pelo Maven em `${project.build.directory}/${project.build.finalName}.jar` no seu sistema de ficheiros local.
>
>```xml
><plugin>
>            <groupId>com.microsoft.azure</groupId>
>            <artifactId>azure-webapp-maven-plugin</artifactId>
>            <version>1.4.0</version>
>            <configuration>
>                <deploymentType>jar</deploymentType>
>
>           <!-- Web App information -->
>            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
>            <appName>${WEBAPP_NAME}</appName>
>            <region>${REGION}</region>  
>
>                <!-- Java Runtime Stack for Web App on Linux-->
>                <linuxRuntime>jre8</linuxRuntime>
>            </configuration>
>         </plugin>
>```    


Atualize os seguintes marcadores de posição na configuração do plug-in:

| Marcador de posição | Descrição |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Nome do novo grupo de recursos no qual irá criar a sua aplicação Web. Ao colocar todos os recursos de uma aplicação num grupo, pode geri-los em conjunto. Por exemplo, eliminar o grupo de recursos eliminará todos os recursos associados à aplicação. Atualize este valor com um nome exclusivo do novo grupo de recursos, como, por exemplo, *TestResources*. Irá utilizar este nome de grupo de recursos para limpar todos os recursos do Azure numa secção posterior. |
| `WEBAPP_NAME` | O nome da aplicação irá fazer parte do nome do anfitrião da aplicação Web aquando da implementação no Azure (NOME_DA_APLICAÇÃO_WEB.azurewebsites.net). Atualize este valor com um nome exclusivo para a nova aplicação Web do Azure, o que irá alojar a sua aplicação Java, como, por exemplo, *contoso*. |
| `REGION` | Uma região do Azure onde a aplicação Web está alojada, por exemplo `westus2`. Pode obter uma lista de regiões do Cloud Shell ou da CLI com o comando `az account list-locations`. |

## <a name="deploy-the-app"></a>Implementar a aplicação

Implemente a sua aplicação Java no Azure com o seguinte comando:

```bash
mvn package azure-webapp:deploy
```

Uma vez concluída a implementação, navegue para a aplicação implementada com o seguinte URL no seu browser, por exemplo `http://<webapp>.azurewebsites.net/helloworld`. 

![Aplicação de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Parabéns!** Implementou a sua primeira aplicação Java no Serviço de Aplicações no Linux.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, utilizou o Maven para criar uma aplicação Web Java, configurou o [Plug-in do Maven para as Aplicações Web do Azure (Pré-visualização)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) e, em seguida, implementou uma aplicação Web Java em pacote de arquivo Web no Serviço de Aplicações no Linux. Para saber mais sobre como utilizar o Java com o Azure, siga a ligação abaixo.

> [!div class="nextstepaction"]
> [Azure para Programadores Java](https://docs.microsoft.com/java/azure/)

