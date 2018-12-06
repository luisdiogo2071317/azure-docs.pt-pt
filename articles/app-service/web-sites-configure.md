---
title: Configurar aplicações Web no Serviço de Aplicações do Azure
description: Como configurar uma aplicação web no serviço de aplicações do Azure
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: 60675d7d40dbab863677ea120c25986cff61a308
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960146"
---
# <a name="configure-web-apps-in-azure-app-service"></a>Configurar aplicações Web no Serviço de Aplicações do Azure

Este tópico explica como configurar uma aplicação web com o [Portal do Azure].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Definições da aplicação
1. Na [Portal do Azure], abra o painel da aplicação web.
3. Clique em **Definições da aplicação**.

![Definições da Aplicação][configure01]

O **as definições da aplicação** painel tem as definições de agrupadas em várias categorias.

### <a name="general-settings"></a>Definições gerais
**Versões de estrutura**. Se a aplicação utilizar qualquer uma dessas estruturas, defina estas opções: 

* **.NET framework**: definir a versão do .NET framework. 
* **PHP**: definir a versão PHP, ou **OFF** para desativar o PHP. 
* **Java**: selecione a versão de Java ou **OFF** para desativar a Java. Utilize o **contentor Web** opção de escolher entre as versões do Tomcat e Jetty.
* **Python**: selecione a versão de Python, ou **OFF** para desativar o Python.

Por motivos técnicos, a ativar o Java para a sua aplicação desativa as opções de .NET, PHP e Python.

<a name="platform"></a>
**Plataforma**. Seleciona se a sua aplicação web é executado num ambiente de 32 bits ou 64 bits. O ambiente de 64 bits requer o escalão básico ou Standard. Gratuito e escalão partilhado é sempre executado num ambiente de 32 bits.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Web Sockets**. Definir **ON** para ativar o protocolo WebSocket; por exemplo, se a sua aplicação web utiliza [SignalR de ASP.NET] ou [socket.io](https://socket.io/).

<a name="alwayson"></a>
**Always On**. Por predefinição, aplicações web são descarregadas que estejam Inativos durante um determinado período de tempo. Isso permite que o sistema poupar recursos. No modo básico ou Standard, pode habilitar **Always On** para que o aplicativo carregado o tempo todo. Se a aplicação for executada trabalhos Web contínuos ou execuções WebJobs acionados usando uma expressão CRON, deve habilitar **Always On**, ou os trabalhos web não podem executar de forma fiável.

**Geridos a versão de Pipeline**. Define o IIS [modo de pipeline]. Deixe esta definida para integrada (predefinição), a menos que tenha uma aplicação legada que requer uma versão mais antiga do IIS.

**Versão de HTTP**. Defina como **2.0** para ativar o suporte para [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protocolo. 

> [!NOTE]
> Os navegadores mais modernos suportam protocolo HTTP/2 por TLS apenas, enquanto o tráfego não encriptadas continua a utilizar HTTP/1.1. Para se certificar de que o cliente navegadores ligar à sua aplicação com o HTTP/2, seja [compre um certificado de serviço de aplicações](web-sites-purchase-ssl-web-site.md) para o domínio personalizado da sua aplicação ou [vincular um certificado de terceiros](app-service-web-tutorial-custom-ssl.md).

**Afinidade ARR**. Numa aplicação que é aumentada horizontalmente para várias instâncias VM, afinidade ARR cookies garantem que o cliente é encaminhado para a mesma instância para o ciclo de vida da sessão. Para melhorar o desempenho de aplicações sem estado, defina esta opção como **desativar**.   

**Auto troca**. Se habilitar a troca automática para um bloco de implementação, o serviço de aplicações será automaticamente a mudar a aplicação web para produção, quando emitir uma atualização para que a ranhura. Para obter mais informações, consulte [implementar em blocos para aplicações web no App Service do Azure de teste](web-sites-staged-publishing.md).

### <a name="debugging"></a>Depurar
**Depuração remota**. Permite a depuração remota. Quando ativada, pode utilizar o depurador remoto no Visual Studio para ligar diretamente à sua aplicação web. Depuração remota permanecerá ativada para 48 horas. 

### <a name="app-settings"></a>Definições de aplicação
Esta secção contém pares nome/valor que a aplicação web será carregado em Iniciar cópia de segurança. 

* Para aplicações de .NET, estas definições são injetadas na sua configuração de .NET `AppSettings` em tempo de execução, substituindo as definições existentes. 
* Para o serviço de aplicações no Linux ou aplicação Web para contentores, se aninha a estrutura de chave do json no seu nome, como `ApplicationInsights:InstrumentationKey` terá de ter `ApplicationInsights__InstrumentationKey` como nome da chave. Por isso, tenha em atenção que qualquer `:` deve ser substituído por `__` (ou seja, sublinhado duplo).
* Aplicações PHP, Python, Java e nó podem aceder a estas definições como variáveis de ambiente em tempo de execução. Para cada definição de aplicação, são criadas duas variáveis de ambiente; um com o nome especificado pela entrada de definição de aplicação e outro com um prefixo de APPSETTING_. Ambos contêm o mesmo valor.

Definições da aplicação são sempre encriptadas quando armazenada (encriptados em repouso).

Definições da aplicação podem ser resolvidas da utilização do Key Vault [referencia o Key Vault](app-service-key-vault-references.md).

### <a name="connection-strings"></a>Cadeias de ligação
Cadeias de ligação para recursos ligados. 

Para aplicações de .NET, essas cadeias de caracteres de conexão são injetadas na sua configuração de .NET `connectionStrings` definições em tempo de execução, substituindo as entradas existentes em que a chave é igual ao nome de base de dados ligada. 

Para aplicações PHP, Python, Java e Node, essas configurações estarão disponíveis como variáveis de ambiente em tempo de execução, o prefixo com o tipo de ligação. Os prefixos de variável de ambiente são os seguintes: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* Base de dados SQL: `SQLAZURECONNSTR_`
* Personalizada: `CUSTOMCONNSTR_`

Por exemplo, se uma cadeia de ligação do MySql eram nomeada `connectionstring1`, ele poderia ser acedido através da variável de ambiente `MYSQLCONNSTR_connectionString1`.

Cadeias de ligação são sempre encriptadas quando armazenada (encriptados em repouso).

Cadeias de ligação podem ser resolvidas da utilização do Key Vault [referencia o Key Vault](app-service-key-vault-references.md).

### <a name="default-documents"></a>Documentos predefinidos
O documento predefinido é a página da web que é apresentado no URL raiz para um Web site.  É utilizado o primeiro arquivo correspondente na lista. 

Aplicações Web poderão utilizar os módulos que rota com base na URL, em vez de servir conteúdo estático, caso em que existem não é nenhum documento predefinido como tal.    

### <a name="handler-mappings"></a>Mapeamentos de processador
Use essa área para adicionar processadores de script personalizado para processar pedidos para extensões de ficheiro específicas. 

* **Extensão**. A extensão de ficheiro a ser processados, como PHP ou handler.fcgi. 
* **Caminho do processador de script**. O caminho absoluto do processador de script. Pedidos para ficheiros correspondentes à extensão de ficheiro serão processados pelo processador do script. Utilize o caminho `D:\home\site\wwwroot` para fazer referência ao diretório de raiz da sua aplicação.
* **Argumentos adicionais**. Argumentos da linha de comandos opcionais para o processador de scripts 

### <a name="virtual-applications-and-directories"></a>Aplicações e diretórios e virtuais
Para configurar aplicações virtuais e diretórios, especifica cada diretório virtual e o caminho físico correspondente relativo à raiz do Web site. Opcionalmente, pode selecionar o **aplicativo** caixa de verificação para marcar um diretório virtual como uma aplicação.

## <a name="enabling-diagnostic-logs"></a>Ativar registos de diagnóstico
Para ativar os registos de diagnóstico:

1. No painel da sua aplicação web, clique em **todas as definições**.
2. Clique em **Registos de diagnóstico**. 

Opções para criar registos de diagnóstico a partir de uma aplicação web que suporta o início: 

* **Registo de aplicações**. Escreve os registos de aplicação para o sistema de ficheiros. Registo durante um período de 12 horas. 

**Nível**. Quando o registo de aplicação é ativado, esta opção especifica a quantidade de informações que serão registados (erro, aviso, informações ou Verbose).

**Registo do servidor Web**. Os registos são guardados no formato de ficheiro de registo expandido de W3C. 

**Mensagens de erro detalhadas**. Poupa de erro detalhada mensagens arquivos. htm. Os ficheiros são guardados em /LogFiles/DetailedErrors. 

**Pedidos falhados**. Registos de pedidos falhados para os arquivos XML. Os ficheiros são guardados em/LogFiles/W3SVC*xxx*, onde xxx é um identificador exclusivo. Esta pasta contém um arquivo XSL e um ou mais arquivos XML. Certifique-se baixar o arquivo XSL, porque fornece funcionalidade de formatação e filtrar o conteúdo dos arquivos XML.

Para ver os ficheiros de registo, tem de criar as credenciais de FTP, da seguinte forma:

1. No painel da sua aplicação web, clique em **todas as definições**.
2. Clique em **credenciais de implementação**.
3. Introduza um nome de utilizador e palavra-passe.
4. Clique em **Guardar**.

![Definir credenciais de implementação][configure03]

O nome de utilizador FTP completo é "app\username" onde *aplicação* é o nome da sua aplicação web. O nome de utilizador está listado no painel da aplicação web, em **Essentials**.

![Credenciais de implementação de FTP][configure02]

## <a name="other-configuration-tasks"></a>Outras tarefas de configuração
### <a name="ssl"></a>SSL
No modo básico ou Standard, pode carregar certificados SSL para um domínio personalizado. Para obter mais informações, consulte [permitir HTTPS para uma aplicação web](app-service-web-tutorial-custom-ssl.md). 

Para ver os certificados carregados, clique em **todas as definições** > **domínios personalizados e SSL**.

### <a name="domain-names"></a>Nomes de domínio
Adicione nomes de domínio personalizado para a sua aplicação web. Para obter mais informações, consulte [configurar um nome de domínio personalizado para uma aplicação web no App Service do Azure](app-service-web-tutorial-custom-domain.md).

Para ver os nomes de domínio, clique em **todas as definições** > **domínios personalizados e SSL**.

### <a name="deployments"></a>Implementações
* Configure a implementação contínua. Ver [utilizando o Git para implementar aplicações Web no App Service do Azure](app-service-deploy-local-git.md).
* Blocos de implementação. Ver [Implementar em ambientes de teste para aplicações Web no serviço de aplicações do Azure].

Para ver os seus blocos de implementação, clique em **todas as definições** > **blocos de implementação**.

### <a name="monitoring"></a>Monitorização
No modo básico ou Standard, pode testar a disponibilidade de pontos de extremidade HTTP ou HTTPS, de até três localizações geograficamente distribuídos. Um teste de monitorização falha se o código de resposta HTTP é um erro (4xx ou 5xx) ou a resposta demora mais de 30 segundos. Um ponto final é considerado disponível se os testes de monitorização com êxito a partir de todas as localizações especificadas. 

Para obter mais informações, consulte [como: monitorizar o estado do ponto final web].

> [!NOTE]
> Se pretender começar a utilizar o App Service do Azure antes de se inscrever numa conta do Azure, aceda a [Experimentar o App Service], onde pode criar de imediato uma aplicação Web de arranque de curta duração no App Service. Sem cartões de crédito; sem compromissos.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
* [Configurar um nome de domínio personalizado no Serviço de Aplicações do Azure]
* [Ativar HTTPS para uma aplicação no serviço de aplicações do Azure]
* [Dimensionar uma aplicação web no serviço de aplicações do Azure]
* [Noções básicas sobre monitorização para aplicações Web no App Service do Azure]

<!-- URL List -->

[SignalR de ASP.NET]: https://www.asp.net/signalr
[Portal do Azure]: https://portal.azure.com/
[Configurar um nome de domínio personalizado no Serviço de Aplicações do Azure]: ./app-service-web-tutorial-custom-domain.md
[Implementar em ambientes de teste para aplicações Web no serviço de aplicações do Azure]: ./web-sites-staged-publishing.md
[Ativar HTTPS para uma aplicação no serviço de aplicações do Azure]: ./app-service-web-tutorial-custom-ssl.md
[Como: monitorizar o estado do ponto final web]: https://go.microsoft.com/fwLink/?LinkID=279906
[Noções básicas sobre monitorização para aplicações Web no App Service do Azure]: ./web-sites-monitor.md
[modo de pipeline]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Dimensionar uma aplicação web no serviço de aplicações do Azure]: ./web-sites-scale.md
[Experimentar o App Service]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
