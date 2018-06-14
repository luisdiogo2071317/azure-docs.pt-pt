---
title: Melhores práticas e guia de resolução de problemas para aplicações de nó na Web Apps do Azure
description: Conheça as melhores práticas e passos de resolução de problemas para aplicações de nó na Web Apps do Azure.
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: ''
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: ranjithr
ms.openlocfilehash: 860874ed49056e6b4695c060b06bf061820c390e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789678"
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Melhores práticas e guia de resolução de problemas para aplicações de nó na Web Apps do Azure

Neste artigo, aprende as melhores práticas e passos de resolução de problemas para [aplicações de nó](app-service-web-get-started-nodejs.md) em execução no Web Apps do Azure (com [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Tenha cuidado quando utilizar os passos de resolução de problemas no seu site de produção. Recomenda-se resolver problemas relacionados com a aplicação numa configuração de não produção por exemplo o bloco de transição e quando é corrigido o problema, trocar o bloco de transição com a ranhura de produção.
>

## <a name="iisnode-configuration"></a>Configuração do IISNODE

Isto [ficheiro de esquema](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) mostra todas as definições que pode configurar para o iisnode. Algumas das definições que são úteis para a sua aplicação:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Esta definição controla o número de processos de nó que são iniciadas por aplicações de IIS. O valor predefinido é 1. Pode iniciar node.exes tantos como a contagem de vCPU VM alterando o valor como 0. O valor recomendado é 0 para a maioria das aplicações, pelo que pode utilizar todas as vCPUs no seu computador. NODE.exe é único thread para que um node.exe consome um máximo de 1 vCPU. Para obter o máximo desempenho fora da aplicação de nó, que pretende utilizar todos os vCPUs.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Esta definição controla o caminho para o node.exe. Pode definir este valor para apontar para a versão de node.exe.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Esta definição controla o número máximo de pedidos simultâneos enviados por iisnode para cada node.exe. No Web Apps do Azure, o valor predefinido é infinito. Quando não é alojada num Web Apps do Azure, o valor predefinido é de 1024. Pode configurar o valor, dependendo de quantos pedidos que recebe a sua aplicação e a rapidez a aplicação processa cada pedido.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Esta definição controla o número máximo de vezes que efetuar a ligação no pipe nomeado para enviar pedidos para node.exe de tentativas de iisnode. Esta definição em combinação com namedPipeConnectionRetryDelay determina o total de tempo limite de cada pedido de iisnode. O valor predefinido é de 200 na Web Apps do Azure. Total de tempo limite em segundos = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Esta definição controla a quantidade de tempo (em ms) iisnode aguarda entre cada tentativa para enviar o pedido para node.exe através do pipe nomeado. O valor predefinido é 250 ms.
Total de tempo limite em segundos = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

Por predefinição, o total de tempo limite no iisnode no Web Apps do Azure é de 200 \* 250 ms = 50 segundos.

### <a name="logdirectory"></a>logDirectory

Esta definição controla o diretório onde o iisnode os registos de stdout/stderr. O valor predefinido é iisnode, que é relativos ao diretório principal do script (diretório onde se encontra presente server.js principal)

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Esta definição controla que versão do nó-inspector iisnode utiliza quando depurar a aplicação de nó. Atualmente, o iisnode-inspector-0.7.3.dll e iisnode inspector.dll são apenas dois valores válidos para esta definição. O valor predefinido é iisnode-inspector-0.7.3.dll. A versão do iisnode-inspector-0.7.3.dll utiliza o nó-inspector-0.7.3 e utiliza os sockets web. Ative os sockets web no seu webapp do Azure para utilizar esta versão. Consulte <http://ranjithblogs.azurewebsites.net/?p=98> para obter mais detalhes sobre como configurar o iisnode para utilizar o novo nó-inspector.

### <a name="flushresponse"></a>flushResponse

O comportamento predefinido do IIS é que coloca-os dados de resposta cópias de segurança para 4 MB antes de libertar ou até ao fim da resposta, o que ocorrer primeiro. o iisnode oferece uma definição de configuração para substituir este comportamento: descarregar um fragmento de corpo de entidade de resposta, assim que o iisnode recebe-lo a partir de node.exe, tem de definir o iisnode/@flushResponse atributo em Web. config como 'true':

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Ativar a libertar de cada fragmento da resposta de corpo de entidade adiciona a sobrecarga de desempenho que reduz o débito do sistema % ~ 5 (a partir da v0.1.13). A melhor para definir o âmbito esta definição apenas para pontos finais que necessitam de resposta de transmissão em fluxo (por exemplo, utilizando o `<location>` elemento na Web. config)

Além disto, para transmissão em fluxo de aplicações, também tem de definir responseBufferLimit do processador do iisnode para 0.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Lista de ficheiros que são observada alterações valores separados por ponto e vírgula. Qualquer alteração a um ficheiro faz com que a aplicação reciclar. Cada entrada é composta por um nome de diretório opcional, bem como um nome de ficheiro necessárias, que são relativos ao diretório onde está localizado o ponto de entrada de aplicação principal. São permitidos carateres universais no apenas a parte de nome de ficheiro. O valor predefinido é `*.js;web.config`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

O valor predefinido é falso. Se estiver ativada, a aplicação de nó pode ligar a um pipe nomeado (variável de ambiente IISNODE\_controlo\_PIPE) e enviar uma mensagem de "Reciclagem". Isto faz com que o w3wp reciclar corretamente.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

O valor predefinido é 0, o que significa que esta funcionalidade está desativada. Quando definido como um valor maior que 0, iisnode será página enviados todos os respetivos processos subordinados cada 'idlePageOutTimePeriod' em milissegundos. Consulte [documentação](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) para compreender quais página saída significa. Esta definição é útil para aplicações que consumam uma elevada quantidade de memória e pretendem página fora de memória para o disco ocasionalmente para libertar de RAM.

> [!WARNING]
> Tenha cuidado ao ativar as seguintes definições de configuração em aplicações de produção. A recomendação é não ativá-los em aplicações de produção em direto.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

O valor predefinido é falso. Se definido como VERDADEIRO, iisnode adiciona um cabeçalho de resposta HTTP `iisnode-debug` cada envia a resposta de HTTP a `iisnode-debug` valor do cabeçalho é um URL. Partes individuais de informações de diagnóstico podem ser obtidos ao observar o fragmento de URL, no entanto, uma visualização está disponível, abrindo o URL num browser.

### <a name="loggingenabled"></a>loggingEnabled

Esta definição controla o registo de stdout e stderr por iisnode. O Iisnode captura stdout/stderr de processos de nó que inicia e grava no diretório especificado na definição de 'logDirectory'. Quando esta estiver ativada, a aplicação escreve registos para o sistema de ficheiros e dependendo da quantidade de registo efetuada pela aplicação, pode haver as implicações de desempenho.

### <a name="deverrorsenabled"></a>devErrorsEnabled

O valor predefinido é falso. Quando definido como VERDADEIRO, iisnode apresenta o código de estado HTTP e o código de erro do Win32 no seu browser. O código win32 é útil em determinados tipos de problemas de depuração.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (não ativar no site de produção em direto)

Esta definição controla a funcionalidade de depuração. O Iisnode está integrado com o node-inspector. Ao ativar esta definição, ativar a depuração da sua aplicação de nó. Após a ativação desta definição, o iisnode cria ficheiros de nó-inspector no diretório 'debuggerVirtualDir' o primeiro pedido de depuração para a aplicação de nó. Pode carregar o nó-inspector ao enviar um pedido para http://yoursite/server.js/debug. Pode controlar o segmento de URL de depuração com a definição de 'debuggerPathSegment'. Por predefinição, debuggerPathSegment = 'debug'. Pode definir `debuggerPathSegment` como um GUID, por exemplo, por isso, que é mais difícil para que sejam detetados por outros.

Leitura [depurar aplicações node.js no Windows](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) para obter mais detalhes sobre a depuração.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Cenários e recomendações/de resolução de problemas

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>A minha aplicação do nó é efetuar chamadas de saída excessivos

Muitas aplicações iriam querer efetuar ligações de saída como parte da respetiva operação regular. Por exemplo, quando um pedido é apresentada no, a aplicação de nó seria deve contactar uma API de REST noutro local e obter algumas informações para processar o pedido. Iria querer utilizar um agente do keep alive ao efetuar chamadas http ou https. Pode utilizar o módulo de agentkeepalive como o agente de acesso keep alive ao efetuar estas chamadas de saída.

O módulo de agentkeepalive garante que os sockets são reutilizadas no seu webapp VM do Azure. Criar um novo socket em cada pedido de saída adiciona Sobrecarga à sua aplicação. Ter a sua aplicação reutilizar sockets para pedidos de saída garante que a aplicação não excede o maxSockets são alocadas por VM. A recomendação na Web Apps do Azure é definir o valor de maxSockets agentKeepAlive para um total de (4 instâncias de node.exe \* 40 maxSockets/instância) 160 sockets por VM.

Exemplo [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) configuração:

```nodejs
var keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> Este exemplo assume que tem node.exe 4 em execução numa VM. Se tiver um número diferente de node.exe em execução na VM, tem de modificar o maxSockets definição em conformidade.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>A minha aplicação do nó está a consumir demasiados da CPU

Poderá receber uma recomendação de Web Apps do Azure no seu portal sobre elevado consumo de cpu. Também pode definir monitores para ver alguns [métricas](web-sites-monitor.md). Quando a verificar a utilização da CPU no [Dashboard do Portal do Azure](../application-insights/app-insights-web-monitor-performance.md), verifique os valores máximo para a CPU, pelo que não perder os valores das horas de ponta.
Se considerar a aplicação está a consumir demasiados CPU e não é possível a explicar o motivo, pode perfil da aplicação do nó para descobrir.

#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>A aplicação de nó num Web Apps do Azure com o gerador de perfis V8 de criação de perfis

Por exemplo, vamos supor que tiver uma aplicação do mundo Olá que pretende para criar um perfil da seguinte forma:

```nodejs
var http = require('http');
function WriteConsoleLog() {
    for(var i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    WriteConsoleLog();
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Vá para o site da consola de depuração https://yoursite.scm.azurewebsites.net/DebugConsole

Vá para o diretório site/wwwroot. Verá uma linha de comandos, conforme mostrado no exemplo seguinte:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Execute o comando `npm install v8-profiler`.

Este comando instala o v8-gerador de perfis no nó\_diretório de módulos e todas as dependências dele.
Agora, edite o server.js para criar um perfil da aplicação.

```nodejs
var http = require('http');
var profiler = require('v8-profiler');
var fs = require('fs');

function WriteConsoleLog() {
    for(var i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    profiler.startProfiling('HandleRequest');
    WriteConsoleLog();
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Os perfis de código anterior a WriteConsoleLog funcionar e, em seguida, escreve o resultado de perfil para o ficheiro 'profile.cpuprofile' em seu wwwroot de site. Envie um pedido para a aplicação. Pode ver um ficheiro de 'profile.cpuprofile' criado no seu wwwroot de site.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Transferir este ficheiro e abri-lo com as ferramentas de F12 do Chrome. Premir a tecla F12 no Chrome, em seguida, escolha o **perfis** separador. Escolha o **carga** botão. Selecione o ficheiro de profile.cpuprofile que transferiu. Clique no perfil de que acabou de carregar.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Pode ver que 95% do tempo consumida pela função WriteConsoleLog. O resultado mostra também o números de linha exata e ficheiros de origem que causou o problema.

### <a name="my-node-application-is-consuming-too-much-memory"></a>A minha aplicação do nó está a consumir demasiada memória

Se a aplicação está a consumir demasiada memória, pode ver um aviso de Web Apps do Azure no seu portal sobre elevado consumo de memória. Pode configurar monitores para ver alguns [métricas](web-sites-monitor.md). Quando a verificar a utilização de memória no [Dashboard do Portal do Azure](../application-insights/app-insights-web-monitor-performance.md), certifique-se verificar os valores máximo da memória, pelo que não perder os valores das horas de ponta.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Deteção de fuga e área dinâmica para dados Diff para node.js

Pode utilizar [memwatch de nó](https://github.com/lloyd/node-memwatch) para o ajudar a identificar memória fugas.
Pode instalar `memwatch` , tal como v8-gerador de perfis e editar o código de captura e diff pilhas para identificar a memória fugas na sua aplicação.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>A minha node.exe são obter cancelado aleatoriamente

Existem alguns motivos por que motivo node.exe é encerrado aleatoriamente:

1. A aplicação que está a gerar exceções não identificadas – d: de verificação\\raiz\\LogFiles\\aplicação\\ficheiro de registo errors.txt para obter os detalhes na exceção emitida. Este ficheiro tem o rastreio da pilha para ajudar a depurar e corrigir a sua aplicação.
2. A aplicação está a consumir demasiada memória que está a afetar outros processos de introdução. Se a memória total da VM está próximo 100%, o node.exe foi cancelado pelo Gestor de processo. Gestor de processo inutilizam alguns processos para permitir que outros processos obter uma oportunidade de fazer alguns cálculos. Para corrigir este problema, perfil de aplicação para fugas de memória. Se a sua aplicação precisar de grandes quantidades de memória, aumentar verticalmente a uma VM maior (o que aumenta a RAM disponível para a VM).

### <a name="my-node-application-does-not-start"></a>A minha aplicação do nó não é iniciado

Se a aplicação está a devolver 500 erros quando é iniciado, pode dever-se a algumas razões:

1. NODE.exe não está presente na localização correta. Verifique a definição de nodeProcessCommandLine.
2. Ficheiro de script principal não está presente na localização correta. Verifique o Web. config e certifique-se de que o nome do ficheiro de script principal na secção de processadores corresponde ao ficheiro de script principal.
3. Configuração do Web. config não está correta – verificar nomes/valores das definições.
4. Arranque a frio – a aplicação está a demorar demasiado tempo a iniciar. Se a sua aplicação demora mais de (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 segundos, o iisnode devolve um erro 500. Aumente os valores destas definições para corresponder a hora de início de aplicações para impedir que o iisnode de exceder o tempo limite e devolver o erro 500.

### <a name="my-node-application-crashed"></a>A minha aplicação nó falhada

A aplicação que está a gerar exceções não identificadas – verificação `d:\\home\\LogFiles\\Application\\logging-errors.txt` ficheiros para obter os detalhes na exceção emitida. Este ficheiro tem o rastreio da pilha para ajudar a diagnosticar e corrigir a sua aplicação.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>A minha aplicação do nó demora demasiado tempo a iniciar (início amovíveis)

A causa comum para longa tempos de início da aplicação é um elevado número de ficheiros no nó\_módulos. A aplicação tenta ao carregar a maioria destes ficheiros ao iniciar. Por predefinição, uma vez que os ficheiros são armazenados na partilha de rede na Web Apps do Azure, carregar muitos ficheiros pode demorar tempo.
Algumas soluções para tornar este processo mais rápido são:

1. Lembre-se de que tem uma estrutura de dependência simples e não existem dependências duplicadas utilizando npm3 para instalar os módulos.
2. Tente lento para carregar o nó\_módulos e não carregar todos os módulos no início da aplicação. Módulos de carregamento lento, a chamada para require('module') deve ser efetuada quando precisar realmente o módulo na função antes da primeira execução do código do módulo.
3. As aplicações Web do Azure oferece uma funcionalidade denominada local cache. Esta funcionalidade copia o conteúdo da partilha de rede para o disco local na VM. Uma vez que os ficheiros são locais, o tempo de carregamento do nó\_módulos é muito mais rápido.

## <a name="iisnode-http-status-and-substatus"></a>Subestado e o estado de http do IISNODE

O `cnodeconstants` [ficheiro de origem](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) apresenta uma lista de todos os iisnode de combinações possíveis de estado/subestado podem devolver devido a um erro.

Ativar FREB para a sua aplicação ver o código de erro do win32 (Lembre-se de ativar FREB apenas em sites de não produção por motivos de desempenho).

| Estado de Http | Subestado http | Razão possível? |
| --- | --- | --- |
| 500 |1000 |Ocorreu algum problema ao emitir o pedido para o IISNODE – Verifique se node.exe foi iniciada. NODE.exe foi ter falhado ao iniciar. Verifique a configuração do Web. config para erros. |
| 500 |1001 |-Win32Error 0x2 - App não está a responder ao URL. Verifique as regras de reescrever de URL ou marcar se a aplicação express tiver as rotas corretas definidas. -Win32Error 0x6d – pipe nomeado está ocupado – Node.exe não está a aceitar pedidos porque o pipe está ocupado. Verifique a utilização elevada da cpu. -Outros erros – Verifique se node.exe falhado. |
| 500 |1002 |NODE.exe falhou – verificar d:\\raiz\\LogFiles\\errors.txt de registo de rastreio da pilha. |
| 500 |1003 |Configuração de pipe problema – a configuração de pipe nomeado é incorreta. |
| 500 |1004-1018 |Ocorreu um erro ao enviar o pedido ou processamento da resposta do node.exe. Verifique se node.exe falhado. verificação de d:\\raiz\\LogFiles\\errors.txt de registo de rastreio da pilha. |
| 503 |1000 |Não existe memória suficiente para atribuir mais ligações de pipe nomeado. Por que motivo está a consumir memória, tanto que a aplicação de verificação. Verifique o valor da definição maxConcurrentRequestsPerProcess. Se não for infinita e tiver muitos pedidos, aumente este valor para evitar este erro. |
| 503 |1001 |Não foi possível emitir pedido node.exe porque a aplicação é Reciclagem. Depois da aplicação tem reciclados, devem ser normalmente servidos pedidos. |
| 503 |1002 |Não foi possível emitir o código de erro do win32 verificação motivo real – pedido para um node.exe. |
| 503 |1003 |Pipe nomeado está demasiado ocupado – Certifique-se de que se node.exe está a consumir excessiva da CPU |

NODE.exe tem uma definição denominada `NODE_PENDING_PIPE_INSTANCES`. Por predefinição, quando não implementada nos Web Apps do Azure, este valor é 4. O que significa que node.exe só podem aceitar pedidos de quatro num momento no pipe nomeado. No Web Apps do Azure, este valor é definido como 5000. Este valor deve ser suficientemente ideal para a maioria de nós aplicações em execução nas Web Apps do Azure. Não deve ser apresentado 503.1003 na Web Apps do Azure devido ao valor superior para o `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Mais recursos

Siga estas ligações para saber mais sobre aplicações node.js no App Service do Azure.

* [Introdução ao Web Apps Node.js no App Service do Azure](app-service-web-get-started-nodejs.md)
* [Como depurar uma aplicação Web Node.js no Serviço de Aplicações do Azure](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Utilizar Módulos do Node.js com aplicações do Azure](../nodejs-use-node-modules-azure-apps.md)
* [Aplicações Web do Serviço de Aplicações do Azure: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Centro de Programadores do Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Explorar a Consola de Depuração do Kudu Super Secreta](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)