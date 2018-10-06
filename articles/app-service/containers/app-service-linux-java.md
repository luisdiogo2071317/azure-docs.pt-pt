---
title: Suporte de idiomas de Java para o serviço de aplicações do Azure no Linux | Documentos da Microsoft
description: Guia do programador para implementar aplicações de Java com o serviço de aplicações do Azure no Linux.
keywords: serviço de aplicações do Azure, aplicação web, linux, oss, java
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.openlocfilehash: e11b115d7a6421c34e7f1371ad8931b6affa0436
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815176"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Guia de programação Java para o serviço de aplicações no Linux

Serviço de aplicações do Azure no Linux permite aos programadores de Java para rapidamente criar, implementar e dimensionar seus Tomcat ou aplicativos de web num serviço baseado em Linux totalmente gerido de pacote de Java Standard Edition (SE). Implemente aplicações com o plug-ins do Maven na linha de comandos ou em editores de como o IntelliJ, o Eclipse ou o Visual Studio Code.

Este guia fornece os conceitos chave e instruções para programadores de Java no serviço de aplicações a utilizar para Linux. Se nunca tiver utilizado o serviço de aplicações do Azure para Linux, deve ler os [início rápido de Java](quickstart-java.md) primeiro. Perguntas gerais sobre como utilizar o serviço de aplicações para Linux que não sejam específicas para o desenvolvimento de Java são respondidas no [FAQ de Linux do serviço de aplicações](app-service-linux-faq.md).

## <a name="logging-and-debugging-apps"></a>Registo e depuração de aplicativos

Relatórios de desempenho, visualizações de tráfego e exames abrangentes de estado de funcionamento estão disponíveis para a aplicação de eeach através do portal do Azure. Consulte a [descrição geral de diagnóstico do serviço de aplicações do Azure](/azure/app-service/app-service-diagnostics) para obter mais informações sobre como aceder e utilizar estas ferramentas de diagnóstico.

### <a name="ssh-console-access"></a>Acesso à consola SSH 

Conectividade SSH para o ambiente do Linux com a sua aplicação está disponível. Ver [suporte SSH para o serviço de aplicações do Azure no Linux](/azure/app-service/containers/app-service-linux-ssh-support) para obter instruções completas ligar ao sistema Linux por meio de seu navegador da web ou um local terminal.

### <a name="streaming-logs"></a>Registos de transmissão em fluxo

Para rápida de depuração e resolução de problemas, pode transmitir em fluxo registos para a consola com a CLI do Azure. Configurar a CLI com o `az webapp log config` para ativar o registo:

```azurecli-interactive
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
 --web-server-logging filesystem
```

Em seguida, transmita registos para o seu console com `az webapp log tail`:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```

Para obter mais informações, consulte [transmissão em fluxo registos com a CLI do Azure](/azure/app-service/web-sites-enable-diagnostic-log#streaming-with-azure-command-line-interface).

### <a name="app-logging"></a>Registo de aplicação

Ativar [registo de aplicações](/azure/app-service/web-sites-enable-diagnostic-log#enablediag) através do portal do Azure ou [CLI do Azure](/cli/azure/webapp/log#az-webapp-log-config) para configurar o serviço de aplicações para escrever a saída do console padrão e os fluxos de erro de console padrão da sua aplicação no local sistema de ficheiros ou armazenamento de Blobs do Azure. O registo para o sistema de ficheiros local do serviço de aplicações instância está desativada 12 horas depois de estar configurada. Se precisar de retenção mais longa, configure a aplicação para escrever a saída para um contentor de armazenamento de Blobs.

Se o aplicativo usar [Logback](https://logback.qos.ch/) ou [Log4j](https://logging.apache.org/log4j) para o rastreio, pode reencaminhar estes rastreios para revisão para o Azure Application Insights com as instruções de configuração do Registro em log framework [Registos de rastreio de explorar o Java no Application Insights](/azure/application-insights/app-insights-java-trace-logs). 

## <a name="customization-and-tuning"></a>Personalização e Otimização

Serviço de aplicações do Azure para Linux oferece imediatamente suporte para a caixa de ajuste e a personalização através do Portal do Azure e a CLI. Reveja os seguintes artigos para a configuração de aplicações web específica de não-Java:

- [Configurar as definições do serviço de aplicações](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Configurar um domínio personalizado](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Ativar o SSL](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Adicionar uma CDN](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Definir opções de tempo de execução de Java

Para definir a memória alocada ou outras opções de tempo de execução JVM nos ambientes do Tomcat e Java SE, defina o JAVA_OPTS conforme mostrado abaixo, como um [definição de aplicação](/azure/app-service/web-sites-configure#app-settings). Serviço de aplicações Linux passa esta definição como uma variável de ambiente para o tempo de execução Java quando ele é iniciado.

No portal do Azure, em **as configurações do aplicativo** da aplicação web, criar uma nova definição de aplicação com o nome `JAVA_OPTS` que inclui as definições adicionais, tais como `$JAVA_OPTS -Xms512m -Xmx1204m`.

Para configurar a definição de aplicação o plug-in do Maven de Linux do Azure App Service, adicione marcas de definição/valor na secção de plug-in do Azure. O exemplo seguinte define uma heapsize específico de Java de máxima e mínima:

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Xms512m -Xmx1204m</value> 
    </property> 
</appSettings> 
```

Os desenvolvedores com um único aplicativo bloco de implementação de um em seu plano do serviço de aplicações podem utilizar as seguintes opções:

- Instâncias B1 e S1:-Xms1024m-Xmx1024m
- Instâncias de B2 e S2:-Xms3072m-Xmx3072m
- Instâncias B3 e S3:-Xms6144m-Xmx6144m


Quando o ajuste definições de área dinâmica para dados de aplicação, reveja os detalhes do seu plano de serviço de aplicações e levar em conta vários aplicativos e o bloco de implementação tem de localizar a alocação ideal de memória.

### <a name="turn-on-web-sockets"></a>Ativar o web sockets

Ativar o suporte de web sockets no portal do Azure no **as definições da aplicação** para a aplicação. Terá de reiniciar a aplicação para a definição tenha efeito.

Ative o suporte de socket web com a CLI do Azure com o seguinte comando:

```azurecli-interactive
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true 
```

Em seguida, reinicie a aplicação:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} 
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
```

### <a name="set-default-character-encoding"></a>Definir a codificação de caracteres padrão 

No portal do Azure, em **as configurações do aplicativo** da aplicação web, criar uma nova definição de aplicação com o nome `JAVA_OPTS` com o valor `$JAVA_OPTS -Dfile.encoding=UTF-8`.

Em alternativa, pode configurar a definição de aplicação com o plug-in do Maven do serviço de aplicações. Adicione as etiquetas de nome e valor de definição na configuração do plug-in: 

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Dfile.encoding=UTF-8</value> 
    </property> 
</appSettings> 
```

## <a name="secure-application"></a>Proteger aplicação

Aplicações de Java em execução no serviço de aplicações para Linux tem o mesmo conjunto de [melhores práticas de segurança](/azure/security/security-paas-applications-using-app-services) como outras aplicações. 

### <a name="authenticate-users"></a>Autenticar utilizadores

Configurar a autenticação de aplicação no Portal do Azure com o **autenticação e autorização** opção. A partir daí, pode ativar a autenticação com o Azure Active Directory ou inícios de sessão sociais como o Facebook, Google ou GitHub. Configuração do portal do Azure só funciona quando configurar um fornecedor de autenticação único.  Para obter mais informações, consulte [configurar a aplicação de serviço de aplicações para utilizar o início de sessão do Azure Active Directory](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) e os artigos relacionados para outros fornecedores de identidade.

Se precisar de vários fornecedores de início de sessão, siga as instruções no [personalizar a autenticação do serviço de aplicações](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to) artigo.

 Os programadores de arranque de Spring podem utilizar o [do Azure Active Directory Spring Boot starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) para proteger aplicações através de anotações de segurança de Spring familiares e APIs.

### <a name="configure-tlsssl"></a>Configurar TLS/SSL

Siga as instruções no [vincular um certificado SSL personalizado existente](/azure/app-service/app-service-web-tutorial-custom-ssl) para carregar um certificado SSL existente e vinculá-lo para o nome de domínio da sua aplicação. Por predefinição seu aplicativo ainda permitirá HTTP ligações-siga específica os passos no tutorial para impor SSL e TLS.

## <a name="tomcat"></a>Tomcat 

### <a name="connecting-to-data-sources"></a>Ligar a origens de dados

>[!NOTE]
> Se a sua aplicação utilizar o Spring Framework ou o Spring Boot, pode definir as informações de ligação de base de dados para a Primavera dados JPA como variáveis de ambiente [em seu arquivo de propriedades da aplicação]. Em seguida, utilize [as definições da aplicação](/azure/app-service/web-sites-configure#app-settings) para definir esses valores para a sua aplicação no portal do Azure ou na CLI.

Para configurar o Tomcat para utilizar ligações geridas para bases de dados com a conectividade de banco de dados de Java (JDBC) ou a API de persistência de Java (JPA), primeiro de personalizá-a variável de ambiente de CATALINA_OPTS lida por Tomcat na inicialização. Defina estes valores através de uma definição de aplicação no plug-in Maven do serviço de aplicações:

```xml
<appSettings> 
    <property> 
        <name>CATALINA_OPTS</name> 
        <value>"$CATALINA_OPTS -Dmysqluser=${mysqluser} -Dmysqlpass=${mysqlpass} -DmysqlURL=${mysqlURL}"</value> 
    </property> 
</appSettings> 
```

Ou um serviço de aplicações equivalente a definição do portal do Azure.

Em seguida, determine se a origem de dados tem de ser disponibilizados apenas para uma aplicação ou para todos os aplicativos em execução no plano de serviço de aplicações.

Para origens de dados de nível de aplicativo: 

1. Adicionar uma `context.xml` ficheiro se não existe para a aplicação web e adicioná-lo a `META-INF` diretório do ficheiro WAR quando o projeto é criado.

2. Nesse arquivo, adicione um `Context` entrada de caminho para ligar à origem de dados para um endereço JNDI. o

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Atualizar a sua aplicação `web.xml` para utilizar a origem de dados na sua aplicação.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

Para recursos partilhados de ao nível do servidor:

1. Copie o conteúdo do `/usr/local/tomcat/conf` em `/home/tomcat` no seu Linux do serviço de aplicações de instância através de SSH se ainda não tiver uma configuração de existir.

2. Adicionar o contexto de sua `server.xml`

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Atualizar a sua aplicação `web.xml` para utilizar a origem de dados na sua aplicação.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

4. Certifique-se de que os ficheiros de controlador JDBC estão disponíveis para o classloader Tomcat, colocando-os no `/home/tomcat/lib` diretório. Para carregar esses arquivos à sua instância do serviço de aplicações, execute os seguintes passos:  
    1. Instale a extensão de webpp do App Service do Azure:

      ```azurecli-interactive
      az extension add –name webapp
      ```

    2. Execute o seguinte comando da CLI para criar um túnel SSH do seu sistema local no serviço de aplicações:

      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```

    3. Ligue à porta de túnel local com o cliente SFTP e carregar os ficheiros para o `/home/tomcat/lib` pasta.

5. Reinicie a aplicação do Linux do serviço de aplicações. Tomcat redefinirá `CATALINA_HOME` para `/home/tomcat` e usar as classes e a configuração atualizada.

## <a name="docker-containers"></a>Contentores do Docker

Para utilizar o JDK de Zulu suportada pelo Azure em seus contentores, certifique-se obter e utilizar as imagens pré-criadas listadas na [página de download do Azul](https://www.azul.com/downloads/azure-only/zulu/#docker) ou utilizar o `Dockerfile` exemplos do [repositório do GitHub de Java do Microsoft](https://github.com/Microsoft/java/tree/master/docker).

## <a name="runtime-availability-and-statement-of-support"></a>Disponibilidade de tempo de execução e a declaração de suporte

Serviço de aplicações para Linux suporta dois runtimes de hospedagem gerenciada de aplicativos da web de Java:

- O [contentor servlet do Tomcat](http://tomcat.apache.org/) para a execução de aplicativos empacotados como arquivos mortos (WAR) da web. Versões suportadas são 8.5 e 9.0.
- Ambiente de tempo de execução Java SE para executar aplicações em pacote como arquivo de Java ficheiros (JAR). A única versão suportada de principais é Java 8.

## <a name="java-runtime-statement-of-support"></a>Instrução de tempo de execução Java de suporte 

### <a name="jdk-versions-and-maintenance"></a>Versões do JDK e manutenção

Suportados Java Development Kit (JDK do Azure) é [Zulu](https://www.azul.com/downloads/azure-only/zulu/) fornecido por meio [Azul Systems](https://www.azul.com/).

Atualizações de versão principal serão fornecidas através de novas opções de tempo de execução no serviço de aplicações do Azure para Linux. Atualizar para estas versões mais recentes do Java ao configurar a sua implementação do serviço de aplicações de clientes e são responsáveis por testar e garantir que a atualização importante atende às suas necessidades.

JDKs suportados sejam automaticamente corrigidos trimestralmente em Janeiro, Abril, Julho e Outubro de cada ano.

### <a name="security-updates"></a>Atualizações de segurança

Patches e correções para as vulnerabilidades de segurança importantes serão lançadas assim que estiverem disponíveis da Azul Systems. Uma vulnerabilidade de "maior" é definida por uma pontuação básica de 9.0 ou superior no [NIST vulnerabilidade de classificação sistema comum, versão 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Preterição e retirada

Se um runtime de Java suportada será extinto, os programadores do Azure com o tempo de execução afetado terá a chance um aviso de preterição, pelo menos, seis meses antes do tempo de execução foi descontinuado.

### <a name="local-development"></a>Desenvolvimento local

Os desenvolvedores podem transferir a produção edição de Azul Zulu Enterprise JDK para desenvolvimento local do [site de download do Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Suporte ao desenvolvimento

Suporte de produto para o [suportada pelo Azure JDK do Azul Zulu](https://www.azul.com/downloads/azure-only/zulu/) está disponível por meio de quando desenvolver para o Azure ou [do Azure Stack](https://azure.microsoft.com/overview/azure-stack/) com um [qualificado plano de suporte do Azure](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Suporte de tempo de execução

Os programadores podem [abra um problema](/azure/azure-supportability/how-to-create-azure-support-request) com o JDKs de Zulu da Azul através do suporte do Azure se tiverem um [plano de suporte qualificado](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Passos Seguintes

Visite o [do Azure para programadores de Java](/java/azure/) center para encontrar inícios rápidos do Azure, tutoriais e documentação de referência de Java.
