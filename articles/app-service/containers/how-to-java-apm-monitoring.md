---
title: Configurar o Java APM ferramentas e monitorização com o serviço de aplicações do Azure no Linux
description: Saiba como enviar os registos e métricas para as suas aplicações de Java em execução no App Service do Linux para fornecedores NewRelic e aplicação Dynamics APM
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 11/27/2018
ms.author: astay;routlaw
ms.openlocfilehash: 06ae71fea1b85a74d87588d2635038c64c8540cc
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500430"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Procedimentos: Ferramentas com aplicações Java no serviço de aplicações do Azure no Linux de monitorização de desempenho do aplicativo

Este artigo mostra como ligar aplicações de Java implementadas no serviço de aplicações do Azure no Linux com o desempenho da aplicação da NewRelic e AppDynamics monitorização plataformas (APM).

## <a name="configure-new-relic"></a>Configurar o New Relic
1. Criar uma conta da NewRelic no [NewRelic.com](https://newrelic.com/signup)
1. Transferir o agente Java a partir da NewRelic, ele terá um nome de ficheiro semelhante a `newrelic-java-x.x.x.zip`.
1. Copiar a chave de licença, precisará das mesmas para configurar o agente mais tarde.
1. [SSH para a instância de serviço de aplicações](/azure/app-service/containers/app-service-linux-ssh-support) e criar um novo diretório `/home/site/wwwroot/apm`. 
1. Carregar os ficheiros de agente NewRelic Java descompactados para um diretório na `/home/site/wwwroot/apm`. Os ficheiros para o agente devem estar no `/home/site/wwwroot/apm/newrelic`.
1. Modificar o ficheiro YAML em `/home/site/wwwroot/apm/newrelic/newrelic.yml` e substitua o valor de licença de marcador de posição pela sua chave de licença.
1. No portal do Azure, navegue para a sua aplicação no serviço de aplicações e criar uma nova definição de aplicação.
    - Se seu aplicativo está usando **Java SE**, criar uma variável de ambiente chamada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Se estiver a utilizar **Tomcat**, criar uma variável de ambiente chamada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Se já tiver uma variável de ambiente para `JAVA_OPTS` ou `CATALINA_OPTS`, acrescentar o `javaagent` opção ao final do valor atual.

## <a name="configure-appdynamics"></a>Configurar o AppDynamics
1. Criar uma conta do AppDynamics no [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. Transferir o agente de Java a partir do site do AppDynamics, o nome do ficheiro será semelhante a `AppServerAgent-x.x.x.xxxxx.zip`
1. [SSH para a instância de serviço de aplicações](/azure/app-service/containers/app-service-linux-ssh-support) e criar um novo diretório `/home/site/wwwroot/apm`. 
1. Carregar os ficheiros de agente Java para um diretório na `/home/site/wwwroot/apm`. Os ficheiros para o agente devem estar no `/home/site/wwwroot/apm/appdynamics`.
1. IIn portal do Azure, navegue até à sua aplicação no serviço de aplicações e criar uma nova definição de aplicação.
    - Se estiver a utilizar **Java SE**, criar uma variável de ambiente chamada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` onde `<YOUR_SITE_NAME>` é o seu nome de serviço de aplicações.
    - Se estiver a utilizar **Tomcat**, criar uma variável de ambiente chamada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` onde `<YOUR_SITE_NAME>` é o seu nome de serviço de aplicações.
