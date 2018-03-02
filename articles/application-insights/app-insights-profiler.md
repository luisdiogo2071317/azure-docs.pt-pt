---
title: "Perfil de aplicações web em direto no Azure com o gerador de perfis do Application Insights | Microsoft Docs"
description: "Identifica o caminho de acesso frequente no seu código de servidor web com um gerador de perfis de requisitos de espaço insuficiente."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: c65ef9141898369b8fcadd4c52972b767aca7cfe
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Perfil live aplicações web do Azure com o Application Insights

*Esta funcionalidade do Azure Application Insights está normalmente disponível para a funcionalidade Web Apps do App Service do Azure e está em pré-visualização para os recursos de computação do Azure.*

Este artigo aborda a quantidade de tempo é gasto cada método de aplicação web em direto quando utilizar [Application Insights](app-insights-overview.md). A ferramenta de gerador de perfis do Application Insights apresenta perfis de detalhado de pedidos em direto que foram processados pela sua aplicação. O gerador de perfis realça o *caminho frequente* que utiliza mais tempo. Pedidos com vários tempos de resposta são profiled numa base de amostragem. Ao utilizar uma variedade de técnicas, pode minimizar os custos gerais de tem associados à aplicação.

Atualmente, o gerador de perfis funciona para as aplicações do ASP.NET e o ASP.NET Core web que estão em execução nas aplicações Web. A básica camada de serviço ou superior, é necessária utilizar o gerador de perfis.

## <a id="installation"></a> Ativar o gerador de perfis para a sua aplicação web de aplicações Web
Se já tiver a aplicação publicada numa aplicação web, mas não o fez nada com o código de origem para utilizar o Application Insights, efetue o seguinte:
1. Vá para o **serviços aplicacionais** painel no portal do Azure.
2. Em **monitorização**, selecione **Application Insights**e, em seguida, siga as instruções ou no painel para criar um novo recurso ou selecione um recurso do Application Insights existente para monitorizar o seu web aplicação.

   ![Ativar o Insights de aplicação no portal de serviços aplicacionais][appinsights-in-appservices]

3. Se tiver acesso para o código de origem do projeto, [instale Application Insights](app-insights-asp-net.md).  
   Se já estiver instalado, certifique-se de que dispõe da versão mais recente. Para verificar a versão mais recente, no Explorador de soluções, clique no seu projeto e, em seguida, selecione **pacotes NuGet gerir** > **atualizações** > **Atualize todos os pacotes**. Em seguida, implemente a sua aplicação.

Aplicações de ASP.NET Core requerem a instalação do 2.1.0-beta6 de pacote Microsoft.ApplicationInsights.AspNetCore NuGet ou posterior para trabalhar com o gerador de perfis. A partir 27 de Junho de 2017, as versões anteriores não são suportadas.

1. No [portal do Azure](https://portal.azure.com), abra o recurso do Application Insights para a sua aplicação web. 
2. Selecione **desempenho** > **ativar o gerador de perfis do Application Insights**.

   ![Selecione a faixa de gerador de perfis de ativação][enable-profiler-banner]

3. Em alternativa, pode selecionar o **gerador de perfis** configuração para ver o estado e ativar ou desativar o gerador de perfis.

   ![Seleccione a configuração do gerador de perfis][performance-blade]

   Aplicações Web que estão configuradas com o Application Insights estão listadas no **gerador de perfis** painel de configuração. Se seguiu os passos anteriores, deve ser instalado o agente do gerador de perfis. 

4. No **gerador de perfis** painel de configuração, selecione **ativar o gerador de perfis**.

5. Se necessário, siga as instruções para instalar o agente do gerador de perfis. Se nenhuma aplicação web foram configuradas com o Application Insights, selecione **adicionar aplicações ligadas**.

   ![Configurar opções do painel][linked app services]

Ao contrário das aplicações web alojadas através de planos de aplicações Web, as aplicações alojadas nos recursos de computação do Azure (por exemplo, Virtual Machines do Azure, conjuntos de dimensionamento de máquina virtual, do Azure Service Fabric ou Cloud Services do Azure) não são geridas diretamente pelo Azure. Neste caso, não há nenhuma aplicação web para ligar a. Em vez de ligar a uma aplicação, selecione o **ativar o gerador de perfis** botão.

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Ativar o gerador de perfis para recursos de computação do Azure (pré-visualização)

Para informações, consulte o [versão de pré-visualização do gerador de perfis para os recursos de computação do Azure](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Ver dados do gerador de perfis

Certifique-se a que sua aplicação estiver a receber tráfego. Se estão a fazer uma experimentação, pode gerar pedidos para a sua aplicação web ao utilizar [teste de desempenho do Application Insights](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Se tiver ativado recentemente o gerador de perfis, pode executar um teste de carga curto para cerca de 15 minutos, que deve gerar rastreios de gerador de perfis. Se tenha sido ativada para tempo já o gerador de perfis, tenha em atenção que o gerador de perfis aleatoriamente executa duas vezes a cada hora e durante um período de dois minutos cada vez que é executado. Recomendamos que primeiro a execução do teste de carga para uma hora para se certificar de que a obter rastreios de gerador de perfis de exemplo.

Depois da aplicação recebe algum tráfego, avance para o **desempenho** painel, selecione **tomar ações** para ver os rastreios de gerador de perfis e, em seguida, selecione o **rastreios de gerador de perfis** botão.

![Pré-visualização do Application Insights desempenho painel rastreios de gerador de perfis][performance-blade-v2-examples]

Selecione uma amostra para apresentar uma divisão de nível de código de tempo que despende a executar o pedido.

![Explorador de rastreio do Application Insights][trace-explorer]

O Explorador de rastreio apresenta as seguintes informações:

* **Mostrar caminho frequente**: abre o biggest folha nó ou, pelo menos, algo fechar. Na maioria dos casos, este nó está adjacente a um estrangulamento do desempenho.
* **Etiqueta**: O nome de função ou de eventos. A árvore apresenta uma combinação de código e os eventos que ocorreram (como eventos do SQL Server e HTTP). O evento superior representa a duração de pedido geral.
* **Decorrido**: O intervalo de tempo entre o início da operação e de fim da operação.
* **Quando**: A hora quando a função ou o evento estava a ser executado em relação a outras funções.

## <a name="how-to-read-performance-data"></a>Como ler dados de desempenho

Gerador de perfis de serviço da Microsoft utiliza uma combinação de métodos de amostragem e instrumentação para analisar o desempenho da sua aplicação. Quando a coleção de detalhado está em curso, o gerador de perfis de serviço amostras o ponteiro de instrução de cada máquina CPU cada milissegundo. Cada amostra captura a pilha de chamadas completa do thread que está atualmente em execução. Fornece informações detalhadas sobre que nesse thread estava a fazer, um nível elevado e um nível de abstração baixa. O gerador de perfis de serviço também recolhe outros eventos para controlar a correlação de atividade e causality, incluindo o contexto de mudança de eventos, eventos de tarefas paralelas biblioteca (TPL) e eventos de conjunto de threads.

A pilha de chamadas que é apresentada na vista de linha cronológica é o resultado da amostragem e instrumentação. Porque cada amostra captura a pilha de chamadas completa do thread, inclui código do Microsoft .NET Framework e a partir de outras estruturas que referenciar.

### <a id="jitnewobj"></a>Alocação de objeto CLR (Common! JIT\_novo ou clr! JIT\_Newarr1)
**CLR! JIT\_novo** e **clr! JIT\_Newarr1** são funções de programa auxiliar no .NET Framework que alocar memória a partir de uma pilha gerida. **CLR! JIT\_novo** é invocada quando existe um objeto é alocado. **CLR! JIT\_Newarr1** é invocada quando é atribuída uma matriz de objetos. Estas duas funções são, normalmente, rápidas em tomar relativamente pequenas quantidades de tempo. Se vir **clr! JIT\_novo** ou **clr! JIT\_Newarr1** demorar uma quantidade substancial de tempo na sua linha cronológica, ele indica que o código pode alocar muitos objetos e consumir quantidades significativas de memória.

### <a id="theprestub"></a>Código de carregamento de CLR (Common! ThePreStub)
**CLR! ThePreStub** é uma função de programa auxiliar no .NET Framework que prepara o código para executar pela primeira vez. Isto normalmente inclui, mas não está limitado a compilação de just-in-time (JIT). Para cada método c#, **clr! ThePreStub** deve ser invocado no máximo uma vez durante a duração de um processo.

Se **clr! ThePreStub** demora uma quantidade substancial de tempo de um pedido, isto indica que o pedido é o primeiro que executa este método. O tempo de execução .NET Framework para carregar o primeiro método é significativo. Poderá considerar a utilização de um processo de aquecimento que executa a que parte do código antes dos utilizadores aceder ao mesmo ou considere executar o gerador de imagens nativas (ngen.exe) com as assemblagens.

### <a id="lockcontention"></a>Contenção CLR (Common! JITutil\_MonContention ou clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** ou **clr! JITutil\_MonEnterWorker** indica que o thread atual está a aguardar um bloqueio ser libertado. Este texto normalmente, é apresentado quando executar um c# **bloqueio** declaração, ao invocar o **Monitor.Enter** método, ou ao invocar um método com o **Methodimploptions** atributo. Contenção ocorre normalmente quando thread _A_ adquirir um bloqueio e o thread _B_ tenta adquirir o bloqueio mesmo antes de thread _A_ versões-lo.

### <a id="ngencold"></a>Carregar código ([frio])
Se o nome do método contém **[amovíveis]**, tais como **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, o tempo de execução de .NET Framework está a executar o código para a primeira vez que não está otimizada por <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">orientado por perfil otimização</a>. Para cada método, deverá ser apresentada no máximo uma vez durante a duração do processo.

Se carregar código demora uma quantidade substancial de tempo de um pedido, isto indica que o pedido é o primeiro para executar a parte do método não otimizada. Considere a utilização de um processo de aquecimento que executa a que parte do código antes dos utilizadores aceder ao mesmo.

### <a id="httpclientsend"></a>Enviar o pedido HTTP
Métodos como **HttpClient.Send** indicar que o código está à espera que um pedido HTTP para ser concluída.

### <a id="sqlcommand"></a>Operação de base de dados
Métodos como **SqlCommand.Execute** indicar que o código está à espera que uma operação de base de dados concluir.

### <a id="await"></a>A aguardar (AWAIT\_tempo)
**AWAIT\_tempo** indica que o código está a aguardar outra tarefa concluir. Isto acontece normalmente com a c# **AWAIT** instrução. Quando o código um c# **AWAIT**, o thread unwinds e devolve o controlo ao conjunto de threads e não há nenhum thread que está bloqueado a aguardar o **AWAIT** para concluir. No entanto, logicamente, o thread que foi o **AWAIT** é "está bloqueado" e está a aguardar a conclusão da operação. O **AWAIT\_tempo** instrução indica o tempo de bloqueados a aguardar a conclusão da tarefa.

### <a id="block"></a>Tempo bloqueado
**BLOCKED_TIME** indica que o código está a aguardar outro recurso estejam disponíveis. Por exemplo, poderá esperar por um objeto de sincronização, com um thread para estar disponível ou para concluir um pedido.

### <a id="cpu"></a>Tempo de CPU
A CPU está ocupada executar as instruções.

### <a id="disk"></a>Tempo de disco
A aplicação está a efetuar operações de disco.

### <a id="network"></a>Tempo da rede
A aplicação está a efetuar operações de rede.

### <a id="when"></a>Quando coluna
O **quando** coluna é uma visualização de como os exemplos INCLUSIVE recolhidos para um nó variam ao longo do tempo. O intervalo total do pedido está dividido em 32 registos de tempo. Os exemplos inclusive para esse nó são acumulados nesses registos 32. Cada registo é representado como uma barra. A altura da barra representa um valor expandido. Para nós que estão marcados **CPU_TIME** ou **BLOCKED_TIME**, ou onde existe uma relação óbvias para consumir um recurso (por exemplo, uma CPU, disco ou thread), a barra representa o consumo de um dos os recursos durante o período desse registo. Para estas métricas, é possível obter um valor superior a 100 por cento de consumir a vários recursos. Por exemplo, se utilizar, em média, duas CPUs durante um intervalo, obter 200 por cento.

## <a name="limitations"></a>Limitações

O período de retenção de dados predefinido é de cinco dias. Os dados máximos que é ingeridos diário são de 10 GB.

Não existem sem custos de utilização do serviço de gerador de perfis. Para que possa utilizar o serviço do gerador de perfis, a aplicação web tem de ser alojados em, pelo menos, o escalão básico de aplicações Web.

## <a name="overhead-and-sampling-algorithm"></a>Sobrecarga e o algoritmo de amostragem

O gerador de perfis aleatoriamente executa dois minutos a cada hora em cada máquina virtual que aloja a aplicação que tenha ativado para capturar rastreios de gerador de perfis. Quando estiver a executar o gerador de perfis, adiciona de 5 porcento a percentagem de 15 a sobrecarga da CPU no servidor.

Os mais servidores que estão disponíveis para alojar a aplicação, tem do gerador de perfis o menor impacto no desempenho da aplicação global. Isto acontece porque o algoritmo de amostragem resulta no gerador de perfis em apenas 5 por cento dos servidores a executar em qualquer altura. Estão disponíveis para servir pedidos web e a sobrecarga de servidor causada executando o gerador de perfis de deslocamento mais servidores.

## <a name="disable-profiler"></a>Desativar o gerador de perfis
Para interromper ou reiniciar o gerador de perfis para uma instância de aplicações web individuais, em **trabalhos Web**, vá para o recurso de aplicações Web. Para eliminar o gerador de perfis, aceda a **extensões**.

![Desativar o gerador de perfis de um trabalho web][disable-profiler-webjob]

Recomendamos que tiver ativado todas as suas aplicações web para detetar problemas de desempenho antecipadamente possível o gerador de perfis.

Se utilizar o WebDeploy para implementar as alterações à sua aplicação web, certifique-se de que excluir pasta App_Data seja eliminado durante a implementação. Caso contrário, a extensão de gerador de perfis são eliminados ficheiros da próxima vez que implementar a aplicação web no Azure.


## <a id="troubleshooting"></a>Resolução de Problemas

### <a name="too-many-active-profiling-sessions"></a>Existem demasiadas sessões ativas de criação de perfis

Atualmente, pode ativar o gerador de perfis num máximo de quatro aplicações web do Azure e as ranhuras de implementação que estão em execução no mesmo plano de serviço. Se a tarefa do gerador de perfis web está a comunicar demasiadas sessões ativas de criação de perfis, mova algumas aplicações web para um plano de serviço diferente.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Como determinar se o gerador de perfis do Application Insights está em execução?

O gerador de perfis é executado como um trabalho web contínua na aplicação web. Pode abrir o recurso de aplicação web no [portal do Azure](https://portal.azure.com). No **WebJobs** painel, verifique o estado do **ApplicationInsightsProfiler**. Se não está em execução, abra **registos** para obter mais informações.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Por que motivo não consigo encontrá qualquer exemplos de pilha, apesar do gerador de perfis está em execução?

Eis algumas coisas que pode verificar:

* Certifique-se de que o seu plano de serviço de aplicações web é o escalão básico ou superior.
* Certifique-se de que a aplicação web tem Application Insights SDK 2.2 Beta ou posterior ativado.
* Certifique-se de que a aplicação web tem o **APPINSIGHTS_INSTRUMENTATIONKEY** definição configurada com a mesma chave de instrumentação que é utilizada pelo Application Insights SDK.
* Certifique-se de que a aplicação web está em execução no .NET Framework 4.6.
* Verifique se a sua aplicação web é uma aplicação ASP.NET Core, [dependências necessárias](#aspnetcore).

Depois de iniciado o gerador de perfis, não há um período de aquecimento curto durante os quais o gerador de perfis recolhe ativamente várias rastreios de desempenho. Depois disso, o gerador de perfis recolhendo rastreios de desempenho de dois minutos a cada hora.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Estava a utilizar o gerador de perfis de serviço do Azure. O que aconteceu ao mesmo?

Quando ativa o gerador de perfis do Application Insights, o agente do gerador de perfis de serviço do Azure está desativado.

### <a id="double-counting"></a>Valor de duplo contando em threads paralelas

Em alguns casos, a métrica do tempo total no Visualizador de pilha é maior do que a duração do pedido.

Esta situação poderá ocorrer quando duas ou mais threads estão associados um pedido e que estão a funcionar em paralelo. Nesse caso, o tempo total de threads é maior do que o tempo decorrido. Poderá ser a aguardar um thread no outro seja concluída. O Visualizador tenta detetar esta e omite a espera desinteressantes, mas errs ao lado dos apresenta de demasiada informação em vez de omitir que poderá ser informações críticas.

Quando vir threads paralelas nos seus rastreios, determine que threads estão a aguardar pelo que pode ascertain o caminho de crítico para o pedido. Na maioria dos casos, o thread que rapidamente entra no estado de espera é simplesmente aguardar a resposta de outros threads. Nos podermos concentrar nos outros threads e ignorar o tempo nos threads de espera.

### <a id="issue-loading-trace-in-viewer"></a>Não existem dados de criação de perfis

Eis algumas coisas que pode verificar:

* Se os dados que está a tentar ver com mais de duas semanas, experimente a limitar o filtro de tempo e tente novamente.
* Certifique-se de que uma firewall ou proxies têm não bloqueou o acesso à https://gateway.azureserviceprofiler.net.
* Certifique-se de que a chave de instrumentação do Application Insights que está a utilizar na sua aplicação é o mesmo que o recurso do Application Insights que utilizou para a criação de perfis ativado. A chave é normalmente no ficheiro Applicationinsights, mas também pode ser o ficheiro. App. config ou Web. config.

### <a name="error-report-in-the-profiling-viewer"></a>Relatório de erros no Visualizador de criação de perfis

Submeta um pedido de suporte no portal. Lembre-se de que inclui o ID de correlação da mensagem de erro.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Erro de implementação: diretório não vazio ' D:\\raiz\\site\\wwwroot\\App_Data\\das tarefas

Se estão a reimplementação a aplicação web a um recurso de aplicações Web com o gerador de perfis ativado, poderá ver uma mensagem como o seguinte:

*Diretório não vazio ' D:\\raiz\\site\\wwwroot\\App_Data\\das tarefas*

Este erro ocorre se executar o Web Deploy de scripts ou a partir do Pipeline de implementação de serviços de equipa do Visual Studio. A solução é adicionar os seguintes parâmetros de implementação adicional para a tarefa de implementação Web:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Estes parâmetros elimine a pasta que é utilizada pelo gerador de perfis do Application Insights e desbloquear o processo da implementar. Estes não afetam a instância do gerador de perfis que está atualmente em execução.


## <a name="manual-installation"></a>Instalação manual

Quando configura o gerador de perfis, as atualizações são efetuadas às definições da aplicação web. Pode aplicar as atualizações manualmente se requer o seu ambiente-lo. Um exemplo poderá ser que a aplicação está a ser executada num ambiente de Web Apps para PowerApps.

1. No **controlo de aplicação Web** painel, abra **definições**.
2. Definir **.Net Framework versão** para **v4.6**.
3. Definir **Always On** para **no**.
4. Adicionar o **APPINSIGHTS_INSTRUMENTATIONKEY** aplicação definição e defina o valor para a mesma chave de instrumentação que é utilizada pelo SDK.
5. Abra **avançadas ferramentas**.
6. Selecione **aceda** para abrir o Web site do Kudu.
7. No site do Kudu, selecione **Site extensões**.
8. Instalar **Application Insights** na Galeria de aplicações Web do Azure.
9. Reinicie a aplicação web.

## <a id="profileondemand"></a> Acionar manualmente o gerador de perfis
Quando foi desenvolvido gerador de perfis, foi adicionada uma interface de linha de comandos para que iremos de teste do gerador de perfis para serviços de aplicações. Ao utilizar esta mesma interface, os utilizadores também podem personalizar como inicia o gerador de perfis. Um nível elevado, o gerador de perfis utiliza o sistema de Kudu de aplicações Web para gerir a criação de perfis em segundo plano. Quando instalar a extensão do Application Insights, criamos um trabalho web contínuo que aloja o gerador de perfis. Esta mesma tecnologia são utilizadas para criar uma nova tarefa de web que pode personalizar para se ajustarem às suas necessidades.

Esta secção explica como:

* Crie uma tarefa web, que pode começar a utilizar o gerador de perfis de dois minutos prima de um botão.
* Crie uma tarefa web, que pode agendar a execução do gerador de perfis.
* Conjunto de argumentos para o gerador de perfis.


### <a name="set-up"></a>Configuração
Em primeiro lugar, familiarize-se com o dashboard do trabalho web. Em **definições**, selecione o **WebJobs** separador.

![Painel de webjobs](./media/app-insights-profiler/webjobs-blade.png)

Como pode ver, este dashboard mostra todas as tarefas de web que estão actualmente instaladas no seu site. Pode ver a tarefa de web ApplicationInsightsProfiler2, que tem a tarefa de gerador de perfis em execução. Este é onde iremos criar novas tarefas de web para manual e agendada de criação de perfis.

Para obter os binários que precisa, efetue o seguinte:

1.  No site do Kudu, no **ferramentas de desenvolvimento** separador, selecione o **ferramentas avançadas** separador com o logótipo do Kudu e, em seguida, selecione **aceda**.  
   É aberto um novo site e tem sessão iniciada automaticamente.
2.  Para transferir os binários do gerador de perfis, vá para o Explorador de ficheiros através de **consola de depuração** > **CMD**, que está localizado na parte superior da página.
3.  Selecione **Site** > **wwwroot** > **App_Data** > **tarefas**  >   **Contínua**.  
   Deverá ver uma pasta denominada *ApplicationInsightsProfiler2*. 
4. À esquerda da pasta, selecione o **transferir** ícone.  
   Esta ação transfere o *ApplicationInsightsProfiler2.zip* ficheiro. Recomendamos que crie um diretório de raiz para mover este arquivo zip para.

### <a name="setting-up-the-web-job-archive"></a>Configurar o arquivo de trabalho web
Quando adiciona uma nova tarefa de web site do Azure, criar, essencialmente, um arquivo zip com um *run.cmd* dentro do ficheiro. O *run.cmd* ficheiro indica o sistema de tarefa web o que fazer quando executar a tarefa de web.

1.  Crie uma nova pasta (por exemplo, *RunProfiler2Minutes*).
2.  Copie os ficheiros de extraídos da *ApplicationInsightProfiler2* pasta para esta nova pasta.
3.  Crie um novo *run.cmd* ficheiro.  
    Para sua comodidade, pode abrir a pasta de trabalho no Visual Studio Code antes de começar.
4.  No ficheiro, adicione o comando `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`. Os comandos são descritos da seguinte forma:

    * `start`: Indica ao iniciar o gerador de perfis.  
    * `--engine-mode immediate`: Indica o gerador de perfis para iniciar imediatamente a criação de perfis.  
    * `--single`: Indica o gerador de perfis para executar e, em seguida, parar automaticamente.  
    * `--immediate-profiling-duration 120`: Indica o gerador de perfis para ser executado por 120 segundos ou dois minutos.

5.  Guarde as alterações.
6.  A pasta de arquivo ao clicá-lo e, em seguida, selecionar **enviar para** > **pasta (zipped) Compressed**.  
   Esta ação cria um ficheiro. zip que utiliza o nome da sua pasta.

![Comando de gerador de perfis de início](./media/app-insights-profiler/start-profiler-command.png)

Acabou de criar um ficheiro. zip, tarefa web, que pode utilizar para configurar as tarefas da web no seu site.

### <a name="add-a-new-web-job"></a>Adicionar uma nova tarefa de web
Nesta secção, é possível adicionar uma nova tarefa de web no seu site. O exemplo seguinte mostra como adicionar um trabalho web accionadas manualmente. Depois de adicionar a tarefa manualmente accionadas web, o processo é quase o mesmo para uma tarefa agendada web.

1.  Vá para o **Web tarefas** dashboard.
2.  Na barra de ferramentas, selecione **adicionar**.
3.  Dê um nome de tarefa web.  
    Para efeitos de clareza, pode ajudar a corresponder ao nome do seu arquivo e abri-lo para uma variedade de versões do *run.cmd* ficheiro.
4.  No **carregamento de ficheiros** área do formulário, selecione o **ficheiro aberto** ícone e, em seguida, procure o ficheiro. zip que criou na secção anterior.

    a.  No **tipo** caixa, selecione **Triggered**.  
    b.  No **Acionadores** caixa, selecione **Manual**.

5.  Selecione **OK**.

![Comando de gerador de perfis de início](./media/app-insights-profiler/create-webjob.png)

### <a name="run-profiler"></a>Execute o gerador de perfis

Agora que tem uma nova tarefa de web que podem acionar manualmente, pode tentar executá-la seguindo as instruções nesta secção.

Por predefinição, só pode ter um *ApplicationInsightsProfiler.exe* processo em execução numa máquina em qualquer momento. Por isso, antes de começar, desative o *Continuous* tarefa web, este dashboard. 
1. Selecione a linha com a nova tarefa de web e, em seguida, selecione **parar**. 
2. Na barra de ferramentas, selecione **atualizar**e confirme que o estado indica que a tarefa está parada.
3. Selecione a linha com a nova tarefa de web e, em seguida, selecione **executar**.
4. Com a linha ainda selecionada, na barra de ferramentas, selecione o **registos** comando.  
    Esta ação abre um dashboard de trabalhos web para a nova tarefa de web e lista o executa mais recente e os respetivos resultados.
5. Selecione a instância de execução que tiver começar.  
    Se tiver acionado com êxito a nova tarefa web, pode ver alguns registos de diagnóstico proveniente de gerador de perfis que confirme que a criação de perfis foi iniciado.

### <a name="things-to-consider"></a>Aspetos a considerar

Embora este método é relativamente simples, considere o seguinte:

* Porque a tarefa de web não é gerida pelo nosso serviço, não temos nenhuma forma de atualizar os binários do agente para a tarefa de web. Não atualmente temos uma página de transferência estável para o nosso binários, pelo que é a única forma de obter os binários mais recentes ao atualizar a extensão e a obtenção de *Continuous* pasta conforme foi nos passos anteriores.

* Porque este processo utiliza os argumentos da linha de comandos que originalmente foram concebidos para programadores em vez dos utilizadores finais, os argumentos poderão alterar no futuro. Tenha em atenção as possíveis alterações ao atualizar. Não deve ser muito um problema, porque pode adicionar uma tarefa web, executá-lo e testar para garantir que funciona. Eventualmente, iremos criar uma IU para lidar com isto sem o processo manual.

* A funcionalidade de tarefas de Web de aplicações Web é exclusiva. Quando é executada a tarefa web, assegura que o processo tem o mesmo variáveis de ambiente e as definições de aplicação com o Web site. Isto significa que não tem de passar a chave de instrumentação através da linha de comandos para o gerador de perfis. O gerador de perfis deve processará a chave de instrumentação do ambiente. No entanto, se pretender executar o gerador de perfis na sua caixa de desenvolvimento ou num computador fora de aplicações Web, terá de fornecer uma chave de instrumentação. Pode fazê-lo através da transmissão de um argumento, `--ikey <instrumentation-key>`. Este valor tem de corresponder a chave de instrumentação que está a utilizar a sua aplicação. A saída de registo do gerador de perfis indica que ikey gerador de perfis começar a utilizar e se foi detetada atividade a partir dessa chave de instrumentação enquanto que foram criação de perfis.

* Podem ser acionadas manualmente accionadas web tarefas via Web Hook. Pode obter este URL, clicar o trabalho web no dashboard e visualize as propriedades. Ou, na barra de ferramentas, pode selecionar **propriedades** depois de selecionar o trabalho web na tabela. Esta abordagem abre-se as possibilidades endless, tais como a acionar o gerador de perfis a partir do pipeline de CI/CD (como VSTS) ou algo semelhante Microsoft Flow (https://flow.microsoft.com/en-us/). À sua escolha depende basicamente complexos como pretende que o *run.cmd* ficheiros (que também pode ser um *run.ps1* ficheiro), mas existe a flexibilidade.

## <a name="next-steps"></a>Passos Seguintes

* [Trabalhar com o Application Insights no Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
