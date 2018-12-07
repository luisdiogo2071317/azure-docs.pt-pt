---
title: Depurador de instantâneos de Azure Application Insights para aplicações de .NET | Documentos da Microsoft
description: Depure instantâneos são automaticamente recolhidos quando exceções forem lançadas em aplicações de .NET de produção
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/10/2018
ms.reviewer: pharring
ms.author: mbullwin
ms.openlocfilehash: 11e9099ba9008ead6583eaf30292cdec63475f02
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994263"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Instantâneos de depuração com exceções em aplicações .NET

Quando ocorre uma exceção, é possível recolher automaticamente um instantâneo de depuração da sua aplicação web em direto. O instantâneo mostra o estado do código-fonte e variáveis no momento que a exceção foi acionada. Snapshot Debugger (pré-visualização) [do Azure Application Insights](app-insights-overview.md) monitoriza a telemetria de exceção da sua aplicação web. Recolhe os instantâneos em suas exceções jogando em cima, de modo a que tem as informações necessárias diagnosticar problemas na produção. Incluir o [pacote de NuGet do recoletor de instantâneos](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) na sua aplicação e, opcionalmente, configure parâmetros de coleção no [applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md). Os instantâneos são apresentadas na [exceções](app-insights-asp-net-exceptions.md) no portal do Application Insights.

Pode ver instantâneos de depuração no portal para visualizar a pilha de chamadas e inspecionar as variáveis em cada frame de pilha de chamadas. Para obter uma experiência de depuração mais poderosa com o código de origem, abra instantâneos com Visual Studio 2017 Enterprise. No Visual Studio, pode também [definir Snappoints interativamente tirar instantâneos](https://aka.ms/snappoint) sem esperar por uma exceção.

Depure instantâneos são armazenados durante sete dias. Esta política de retenção está definida numa base por aplicação. Se precisar de aumentar este valor, pode pedir um aumento ao abrir um incidente de suporte no portal do Azure.

Coleção de instantâneos está disponível para:
* Aplicações de .NET framework e o ASP.NET com o .NET Framework 4.5 ou posterior.
* Aplicações de .NET core 2.0 e ASP.NET Core 2.0 em execução no Windows.

São suportados os seguintes ambientes:
* Serviço de aplicações do Azure.
* Serviço Cloud do Azure com a família de SO 4 ou posterior.
* Serviços do Service Fabric do Azure em execução no Windows Server 2012 R2 ou posterior.
* Máquinas virtuais do Azure com o Windows Server 2012 R2 ou posterior.
* Máquinas virtuais ou físicas, o Windows Server 2012 R2 ou posterior.

> [!NOTE]
> Não são suportadas aplicações de cliente (por exemplo, WPF, formulários Windows ou UWP).

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Configurar a recolha de instantâneos para aplicativos ASP.NET

1. [Ativar o Application Insights na sua aplicação web](app-insights-asp-net.md), se ainda não fez isso ainda.

2. Incluir o [snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pacote NuGet na sua aplicação.

3. Reveja as opções predefinidas que o pacote adicionado à [applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>1</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Os instantâneos são coletados apenas em exceções que são enviadas para o Application Insights. Em alguns casos (por exemplo, as versões mais antigas da plataforma .NET), talvez seja preciso [configurar a recolha de exceção](app-insights-asp-net-exceptions.md#exceptions) para ver as exceções com instantâneos no portal.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Configurar a recolha de instantâneos para aplicativos do ASP.NET Core 2.0

1. [Ativar o Application Insights na sua aplicação web do ASP.NET Core](app-insights-asp-net-core.md), se ainda não fez isso ainda.

    > [!NOTE]
    > Ser-se de que seu aplicativo faz referência a versão 2.1.1 ou mais recente do pacote Microsoft.ApplicationInsights.AspNetCore.

2. Incluir o [snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pacote NuGet na sua aplicação.

3. Modificar a sua aplicação `Startup` classe para adicionar e configurar o processador de telemetria do Recoletor de instantâneos.

    Adicione o seguinte utilizar as instruções `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Adicione as seguintes `SnapshotCollectorTelemetryProcessorFactory` classe `Startup` classe.

   ```csharp
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }
       ...
    ```
    Adicionar a `SnapshotCollectorConfiguration` e `SnapshotCollectorTelemetryProcessorFactory` serviços para o pipeline de arranque:

    ```csharp
       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Configure o Recoletor de instantâneos, adicionando uma seção de SnapshotCollectorConfiguration ao appSettings. Por exemplo:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Configurar a recolha de instantâneos para outras aplicações de .NET

1. Se a sua aplicação já não é instrumentada com o Application Insights, comece por [ativar o Application Insights e definir a chave de instrumentação](app-insights-windows-desktop.md).

2. Adicionar a [snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pacote NuGet na sua aplicação.

3. Os instantâneos são coletados apenas em exceções que são enviadas para o Application Insights. Terá de modificar o código para relatá-los. A código de manipulação de exceção depende da estrutura do seu aplicativo, mas um exemplo é abaixo:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="grant-permissions"></a>Conceder permissões

Acesso a instantâneos está protegido pelo controlo de acesso baseado em funções (RBAC). Para inspecionar um instantâneo, deve primeiro ser adicionado à função necessária por um proprietário da subscrição.

> [!NOTE]
> Os proprietários e contribuintes não têm automaticamente esta função. Se pretende ver instantâneos, têm de adicionar-se para a função.

Proprietários de subscrições devem atribuir a `Application Insights Snapshot Debugger` função aos utilizadores que fará uma checagem instantâneos. Esta função pode ser atribuída a utilizadores individuais ou grupos por proprietários de subscrições para o destino do recurso do Application Insights ou o grupo de recursos ou subscrição.

1. Navegue para o recurso do Application Insights no portal do Azure.
1. Clique em **controlo de acesso (IAM)**.
1. Clique nas **+ adicionar atribuição de função** botão.
1. Selecione **Snapshot Debugger do Application Insights** partir do **funções** na lista pendente.
1. Procure e introduza um nome para o utilizador adicione.
1. Clique nas **guardar** botão para adicionar o utilizador à função.


> [!IMPORTANT]
> Instantâneos potencialmente podem conter informações confidenciais pessoais e outras valores de variável e o parâmetro.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Depure instantâneos no portal do Application Insights

Se um instantâneo está disponível para uma determinada exceção ou um ID de problema, uma **aberto depurar instantâneo** botão aparece no [exceção](app-insights-asp-net-exceptions.md) no portal do Application Insights.

![Botão de abrir instantâneo de depuração na exceção](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

Na vista de instantâneo de depuração, verá uma pilha de chamadas e um painel de variáveis. Quando seleciona quadros de pilha de chamadas no painel de pilha de chamada, pode ver variáveis locais e parâmetros para essa função chamam-se no painel de variáveis.

![Ver instantâneo de depuração no portal](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Instantâneos podem incluir informações confidenciais e, por predefinição não estão visíveis. Para ver os instantâneos, tem de ter o `Application Insights Snapshot Debugger` função atribuída a.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Depure instantâneos com o Visual Studio 2017 Enterprise
1. Clique nas **transferir instantâneo** botão para transferir um `.diagsession` arquivo, que pode ser aberto pelo Visual Studio 2017 Enterprise.

2. Para abrir o `.diagsession` ficheiro, tem de ter o componente de VS de depurador de instantâneos instalado. O componente de depurador de instantâneos é um componente necessário da carga de trabalho do ASP.net no VS e pode ser selecionado na lista de componentes individuais no instalador do VS. Se estiver a utilizar uma versão do Visual Studio antes de 15,5 terá de instalar a extensão a partir da [marketplace do VS](https://aka.ms/snapshotdebugger).

3. Depois de abrir o ficheiro de instantâneo, é apresentada a página de Minidespejo depuração no Visual Studio. Clique em **depurar código gerenciado** para iniciar a depuração do instantâneo. O instantâneo é aberta a linha de código em que a exceção foi acionada, para que pode depurar o estado atual do processo.

    ![Ver instantâneo de depuração no Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

O instantâneo transferido inclui os arquivos de símbolo que foram encontrados no seu servidor de aplicativos web. Esses arquivos de símbolo são necessárias para associar dados de instantâneos de código-fonte. Para aplicações de serviço de aplicações, certifique-se ativar a implementação de símbolo quando publicar as suas aplicações web.

## <a name="how-snapshots-work"></a>Como funcionam os instantâneos

O Recoletor de instantâneos é implementado como um [processador de telemetria do Application Insights](app-insights-configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Quando executa a aplicação, o processador de telemetria do Recoletor de instantâneos é adicionado ao pipeline de telemetria do seu aplicativo.
Sempre que suas chamadas de aplicativo [TrackException](app-insights-asp-net-exceptions.md#exceptions), o Recoletor de instantâneos computa um ID de problema do tipo de exceção que está a ser emitida e o método throwing.
Sempre que seu aplicativo chama TrackException, um contador é incrementado para o ID de problema apropriado. Quando o contador de atinge o `ThresholdForSnapshotting` valor, o ID do problema é adicionado a um plano de coleção.

O Recoletor de instantâneos também monitora exceções, como eles são acionados por subscrever a [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) eventos. Quando esse evento é acionado, o ID do problema da exceção é calculado e comparado com os IDs de problema no plano de coleção.
Se existir uma correspondência, em seguida, é criado um instantâneo do processo em execução. O instantâneo é atribuído um identificador exclusivo e a exceção está marcada com esse identificador. Depois do manipulador de FirstChanceException retorna, a exceção lançada é processada como normal. Eventualmente, a exceção atinge novamente o método de TrackException onde, juntamente com o identificador de instantâneos, será reportado para o Application Insights.

O processo principal continua a executar e a servir o tráfego para os utilizadores com pouca interrupção. Enquanto isso, o instantâneo é transferido para o processo de carregador de instantâneo. O carregador de instantâneo cria um minidespejo e carrega-o para o Application Insights, juntamente com quaisquer arquivos de símbolo relevantes (. pdb).

> [!TIP]
> - Um instantâneo de processo é um clone suspenso do processo em execução.
> - Criar o instantâneo demora cerca de 10 a 20 milissegundos.
> - O valor predefinido para `ThresholdForSnapshotting` é 1. Isso também é o valor mínimo. Por conseguinte, a aplicação tem de acionar a mesma exceção **duas vezes** antes da criação de um instantâneo.
> - Definir `IsEnabledInDeveloperMode` como verdadeira se pretender gerar instantâneos durante a depuração no Visual Studio.
> - A taxa de criação de instantâneos está limitada pelo `SnapshotsPerTenMinutesLimit` definição. Por predefinição, o limite é um instantâneo de cada dez minutos.
> - Podem ser carregados não mais de 50 instantâneos por dia.

## <a name="current-limitations"></a>Limitações atuais

### <a name="publish-symbols"></a>Publicar símbolos
O Snapshot Debugger requer arquivos de símbolo no servidor de produção para descodificar variáveis e fornecer uma experiência de depuração no Visual Studio.
Versão 15.2 (ou superior) do Visual Studio 2017 publica símbolos para as compilações de versão por predefinição quando publica no serviço de aplicações. Nas versões anteriores, terá de adicionar a seguinte linha ao seu perfil de publicação `.pubxml` para que os símbolos são publicados no modo de versão do ficheiro:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Para a computação do Azure e outros tipos, certifique-se de que os arquivos de símbolo estão na mesma pasta do ficheiro. dll do aplicativo principal (normalmente, `wwwroot/bin`) ou estão disponíveis no caminho atual.

### <a name="optimized-builds"></a>Compilações otimizadas
Em alguns casos, não não possível ver variáveis locais, nas compilações de versão, devido a otimizações que são aplicadas pelo compilador JIT.
No entanto, nos serviços de aplicações do Azure, o Recoletor de instantâneos pode deoptimize lançando métodos que fazem parte do seu plano de coleção.

> [!TIP]
> Instale a extensão de Site do Application Insights no seu serviço de aplicações para obter suporte deoptimization.

## <a name="troubleshooting"></a>Resolução de problemas

Estas sugestões ajudam a resolver problemas relacionados com o depurador de instantâneos.

### <a name="use-the-snapshot-health-check"></a>Utilizar a verificação de estado de funcionamento de instantâneo
Vários problemas comuns resultam no instantâneo de depuração aberto não aparecer. Usando um desatualizado Recoletor de instantâneos, por exemplo; atingir o limite de carregamento diário; ou talvez o instantâneo estiver apenas a demorar muito tempo a carregar. Utilize a verificação de estado de funcionamento de instantâneo para resolver problemas comuns.

Existe uma ligação no painel de exceção da vista de rastreamento de ponta a ponta que leva-o para a verificação de estado de funcionamento de instantâneo.

![Introduza a verificação de estado de funcionamento de instantâneo](./media/app-insights-snapshot-debugger/enter-snapshot-health-check.png)

A interface interativa, como bate-papo procura de problemas comuns e orienta-o para corrigi-los.

![Verificação de estado de funcionamento](./media/app-insights-snapshot-debugger/healthcheck.png)

Se o que não resolve o problema, consulte, em seguida, o manual seguinte passos de resolução de problemas.

### <a name="verify-the-instrumentation-key"></a>Certifique-se a chave de instrumentação

Certifique-se de que está a utilizar a chave de instrumentação correto no seu aplicativo publicado. Normalmente, a chave de instrumentação é lidos a partir do ficheiro Applicationinsights Config. Certifique-se de que o valor é o mesmo que a chave de instrumentação para o recurso do Application Insights que vê no portal.

### <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>Atualizar para a versão mais recente do pacote NuGet

Utilize o Gestor de pacotes de NuGet do Visual Studio para se certificar de que está a utilizar a versão mais recente do snapshotcollector. Notas de versão podem ser encontradas em https://github.com/Microsoft/ApplicationInsights-Home/issues/167

### <a name="check-the-uploader-logs"></a>Verifique os registos de carregador

Depois de um instantâneo é criado, é criado um ficheiro de minidespejo (. dmp) no disco. Um processo separado carregador cria esse arquivo de minidespejo e carrega-, juntamente com quaisquer PDBs associados, para o armazenamento do Snapshot Debugger do Application Insights. Depois do minidespejo tem carregados com êxito, este é eliminado do disco. Os ficheiros de registo para o processo de carregador são mantidos no disco. Num ambiente de serviço de aplicações, pode encontrar estes registos no `D:\Home\LogFiles`. Utilize o site de gestão de Kudu do serviço de aplicações para localizar estes ficheiros de registo.

1. Abra a aplicação do serviço de aplicações no portal do Azure.
2. Clique em **ferramentas avançadas**, ou procure **Kudu**.
3. Clique em **ir**.
4. Na **consola de depuração** caixa de lista pendente, selecione **CMD**.
5. Clique em **LogFiles**.

Deverá ver pelo menos um ficheiro com um nome que começa com `Uploader_` ou `SnapshotUploader_` e um `.log` extensão. Clique no item apropriado para transferir os ficheiros de registo ou abri-los num browser.
O nome de ficheiro inclui um sufixo exclusivo que identifica a instância de serviço de aplicações. Se a sua instância de serviço de aplicações é alojada em mais de uma máquina, existem ficheiros de registo separados para cada máquina. Quando o carregador Deteta um novo arquivo de minidespejo, ele será gravado no arquivo de log. Eis um exemplo de um instantâneo com êxito e o carregamento:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> O exemplo acima é da versão 1.2.0 do pacote NuGet snapshotcollector. Nas versões anteriores, o processo de carregador é chamado `MinidumpUploader.exe` e o registo é menos detalhado.

No exemplo anterior, a chave de instrumentação é `c12a605e73c44346a984e00000000000`. Este valor deve corresponder a chave de instrumentação para a sua aplicação.
O minidespejo está associado um instantâneo com o ID de `139e411a23934dc0b9ea08a626db16c5`. Pode utilizar este ID mais tarde para localizar a telemetria de exceção associada no Application Insights Analytics.

A carregar verifica a existência de novos PDBs sobre uma vez a cada 15 minutos. Segue-se um exemplo:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

Para aplicativos que _não estão_ alojado no serviço de aplicações, os registos de carregador estão na mesma pasta que os minidespejos: `%TEMP%\Dumps\<ikey>` (onde `<ikey>` é a chave de instrumentação).

### <a name="troubleshooting-cloud-services"></a>Resolução de problemas de serviços Cloud
Para funções nos serviços Cloud, a pasta temporária do padrão pode ser demasiado pequena para manter os ficheiros de mini-cópia de segurança, que leva a instantâneos perdidos.
O espaço necessário depende do conjunto de trabalho total da sua aplicação e o número de instantâneos em simultâneo.
O conjunto de trabalho de uma função de web ASP.NET de 32 bits é normalmente entre 200 MB e 500 MB.
Permita a, pelo menos, dois instantâneos em simultâneo.
Por exemplo, se a sua aplicação utilizar 1 GB do conjunto de trabalho total, deve Certifique-se de que existe pelo menos 2 GB de espaço em disco para armazenar instantâneos.
Siga estes passos para configurar a sua função de serviço em nuvem com um recurso local dedicado para instantâneos.

1. Adicione um novo recurso local, ao seu serviço Cloud, ao editar o ficheiro de definição (. csdef) do serviço em nuvem. O exemplo seguinte define um recurso chamado `SnapshotStore` com um tamanho de 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. Modificar o código de inicialização de sua função para adicionar uma variável de ambiente que aponta para o `SnapshotStore` recursos locais. Para funções de trabalho, o código deve ser adicionado à sua função `OnStart` método:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Para funções de Web (ASP.NET), o código deve ser adicionado ao seu aplicativo web `Application_Start` método:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Atualizar ficheiro applicationinsights. config de sua função para substituir a localização da pasta temporário utilizada por `SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

### <a name="overriding-the-shadow-copy-folder"></a>Substituir a pasta de cópia de sombra

Quando o Recoletor de instantâneos é iniciado, ele tenta encontrar uma pasta no disco que é adequado para a execução do processo de carregador de instantâneo. A pasta que escolheu é conhecida como a pasta de cópia de sombra.

O Recoletor de instantâneos verifica alguns locais bem conhecidos, certificar-se de que tem permissões para copiar os binários do carregador de instantâneo. As seguintes variáveis de ambiente são utilizadas:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

Se não é possível encontrar uma pasta adequada, o Recoletor de instantâneos relatórios a indicar um erro _"Não foi possível localizar uma pasta de cópia de sombra adequado."_

Se falha a cópia, relatórios de Recoletor de instantâneos um `ShadowCopyFailed` erro.

Se o carregador não pode ser inicializado, relatórios de Recoletor de instantâneos um `UploaderCannotStartFromShadowCopy` erro. O corpo da mensagem, muitas vezes, contém `System.UnauthorizedAccessException`. Este erro ocorre normalmente porque o aplicativo for executado sob uma conta com permissões reduzidas. A conta tem permissão para escrever na pasta de cópia de sombra, mas ele não tem permissão para executar o código.

Uma vez que estes erros normalmente ocorrem durante o arranque,, normalmente, irá ser seguidos por um `ExceptionDuringConnect` indicação do erro _"Carregador Falha ao iniciar."_

Para contornar esses erros, pode especificar a pasta de cópia de sombra manualmente por meio do `ShadowCopyFolder` opção de configuração. Por exemplo, usando o applicationinsights. config:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Em alternativa, se estiver a utilizar appSettings com uma aplicação .NET Core:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Utilize o Application Insights pesquisa para encontrar exceções com instantâneos

Quando um instantâneo é criado, a exceção throwing é marcada com um ID de instantâneo. Esse ID de instantâneo é incluído como uma propriedade personalizada quando a telemetria de exceção é reportada para o Application Insights. Usando **pesquisa** no Application Insights, pode encontrar toda a telemetria com o `ai.snapshot.id` propriedade personalizada.

1. Navegue para o recurso do Application Insights no portal do Azure.
2. Clique em **pesquisa**.
3. Tipo de `ai.snapshot.id` na caixa de texto de pesquisa e prima Enter.

![Procure telemetria com um ID de instantâneo no portal](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Se esta pesquisa devolve os resultados não, não existem instantâneos foram comunicados ao Application Insights para a sua aplicação no intervalo de tempo selecionado.

Para procurar um ID específico de instantâneo dos logs de carregador, escreva esse ID na caixa de pesquisa. Se não conseguir encontrar a telemetria para um instantâneo que sabe que foi carregado, siga estes passos:

1. Verifique novamente que estiver olhando para o recurso do Application Insights correto, verificando a chave de instrumentação.

2. Utilizar o carimbo de hora a partir do registo de carregador, ajuste o filtro de intervalo de tempo da pesquisa para abordar esse intervalo de tempo.

Se ainda não vê uma exceção com esse ID de instantâneo, a telemetria de exceção não foi reportada para o Application Insights. Esta situação pode acontecer se a sua aplicação falhou após levou o instantâneo, mas antes de ele relatado a telemetria de exceção. Neste caso, verifique os registos de serviço de aplicações em `Diagnose and solve problems` para ver se houve reinícios inesperados ou exceções não processadas.

### <a name="edit-network-proxy-or-firewall-rules"></a>Editar regras de firewall ou um proxy de rede

Se seu aplicativo se conecta à Internet através de um proxy ou de uma firewall, terá de editar as regras para permitir que a aplicação comunicar com o serviço de depurador de instantâneos. Eis [uma lista de endereços IP e portas utilizadas pelo Snapshot Debugger](app-insights-ip-addresses.md#snapshot-debugger).

## <a name="next-steps"></a>Passos Seguintes

* [Definir snappoints em seu código](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) obter instantâneos sem esperar por uma exceção.
* [Diagnosticar exceções nas suas aplicações web](app-insights-asp-net-exceptions.md) explica como tornar as exceções mais visíveis para o Application Insights.
* [Deteção inteligente](app-insights-proactive-diagnostics.md) Deteta automaticamente anomalias de desempenho.
