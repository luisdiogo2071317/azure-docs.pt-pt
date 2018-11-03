---
title: Configurar diagnósticos para máquinas virtuais e serviços Cloud do Azure | Documentos da Microsoft
description: Saiba como configurar diagnósticos para depuração de serviços de cloude do Azure e máquinas virtuais (VMs) no Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: douge
editor: ''
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/28/2018
ms.author: mikejo
ms.openlocfilehash: 8f32165a7a54ead06d57a3d8d1b4282498dca635
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969616"
---
# <a name="set-up-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais
Quando tiver de resolver problemas de um serviço cloud do Azure ou a máquina virtual, pode utilizar o Visual Studio para configurar mais facilmente o diagnóstico do Azure. Diagnóstico de captura de dados do sistema e dados de registo nas máquinas virtuais e instâncias de máquinas virtuais que executam o seu serviço cloud. Dados de diagnóstico são transferidos para uma conta de armazenamento que escolher. Para obter mais informações sobre o diagnóstico de iniciar sessão no Azure, consulte [ativar o registo de diagnóstico para aplicações Web no App Service do Azure](app-service/web-sites-enable-diagnostic-log.md).

Neste artigo, mostraremos como usar o Visual Studio para ativar e configurar o diagnóstico do Azure, antes e após a implementação. Saiba como configurar diagnósticos em máquinas virtuais do Azure, como selecionar os tipos de informações de diagnóstico para coletar e como visualizar as informações depois de serem recolhido.

Pode utilizar uma das seguintes opções para configurar o diagnóstico do Azure:

* Alterar as definições de diagnóstico no **configuração de diagnósticos** caixa de diálogo no Visual Studio. As definições são guardadas num ficheiro denominado diagnostics.wadcfgx (no Azure SDK 2.4 e anteriores, o arquivo é chamado diagnostics.wadcfg). Também pode modificar diretamente o ficheiro de configuração. Caso atualize manualmente o ficheiro, o efeito de take de alterações de configuração da próxima vez que implantar a nuvem de serviço para o Azure ou executar o serviço no emulador.
* Utilize o Cloud Explorer ou o Explorador de servidores no Visual Studio para alterar as definições de diagnóstico para um serviço cloud ou máquina virtual que está em execução.

## <a name="azure-sdk-26-diagnostics-changes"></a>Alterações de diagnóstico do Azure SDK 2.6
As alterações seguintes aplicam-se para o Azure SDK 2.6 e posteriores projetos no Visual Studio:

* O emulador local agora suporta diagnósticos. Isso significa que pode recolher dados de diagnóstico e certifique-se de que a sua aplicação cria os rastreios certos enquanto desenvolve e testar no Visual Studio. A cadeia de ligação `UseDevelopmentStorage=true` ativa a recolha de dados de diagnóstico enquanto estiver a executar o seu projeto de serviço em nuvem no Visual Studio, utilizando o emulador de armazenamento do Azure. Todos os dados de diagnóstico são recolhidos na conta de armazenamento de armazenamento de desenvolvimento.
* A cadeia de ligação de conta de armazenamento de diagnóstico `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` é armazenado no ficheiro de configuração (. cscfg) do serviço. No Azure SDK 2.5, a conta de armazenamento de diagnóstico é especificada no ficheiro diagnostics.wadcfgx.

A cadeia de ligação funciona de forma diferente de algumas maneiras principais no Azure SDK 2.6 e mais tarde, em comparação com o Azure SDK 2.4 e versões anteriores:

* No Azure SDK 2.4 e anteriores, a cadeia de ligação é utilizada como um tempo de execução, o plug-in de diagnóstico para obter as informações de conta de armazenamento para transferir os registos de diagnóstico.
* No Azure SDK 2.6 ou posterior, o Visual Studio utiliza a cadeia de ligação de diagnóstico para configurar a extensão de diagnóstico do Azure com as informações de conta de armazenamento adequado durante a publicação. Pode utilizar a cadeia de ligação para definir as contas de armazenamento diferentes para as configurações de serviço diferentes que o Visual Studio utiliza durante a publicação. No entanto, uma vez que o plug-in de diagnóstico não está disponível após o Azure SDK 2.5, o ficheiro. cscfg por si só, não é possível configurar a extensão de diagnóstico. Tem de configurar a extensão separadamente usando ferramentas como o Visual Studio ou o PowerShell.
* Para simplificar o processo de configurar a extensão de diagnóstico com o PowerShell, a saída de pacote do Visual Studio inclui o XML de configuração pública para a extensão de diagnóstico para cada função. Visual Studio utiliza a cadeia de ligação de diagnóstico para preencher as informações de conta de armazenamento na configuração do pública. Os ficheiros de configuração públicos são criados na pasta de extensões. Os ficheiros de configuração pública utilizam o padrão de nomenclatura PaaSDiagnostics. &lt;nome da função\>. PubConfig.xml. Todas as implementações baseadas no PowerShell podem utilizar este padrão para mapear cada configuração a uma função.
* O [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) utiliza a cadeia de ligação no ficheiro. cscfg para acessar os dados de diagnóstico. São apresentados os dados sobre o **monitorização** separador. A cadeia de ligação é necessária para definir o serviço para mostrar dados de monitorização verbosos no portal.

## <a name="migrate-projects-to-azure-sdk-26-and-later"></a>Migrar projetos ao Azure SDK 2.6 ou posterior
Quando migra do Azure SDK 2.5 para o Azure SDK 2.6 ou posterior, se tiver uma conta de armazenamento de diagnóstico especificada no ficheiro .wadcfgx, a conta de armazenamento permanece nesse ficheiro. Para tirar partido da flexibilidade de usar contas de armazenamento diferentes para configurações de armazenamento diferentes, adicione manualmente a cadeia de ligação ao seu projeto. Se estiver a migrar um projeto do Azure SDK 2.4 ou anteriormente para o Azure SDK 2.6, as cadeias de ligação de diagnóstico são preservadas. No entanto, tenha em atenção as alterações no como cadeias de ligação são processadas no Azure SDK 2.6, descrito na secção anterior.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Como o Visual Studio determina a conta de armazenamento de diagnóstico
* Se uma cadeia de ligação de diagnóstico é especificada no ficheiro. cscfg, o Visual Studio utiliza-o para configurar a extensão de diagnóstico durante a publicação e ao gerar os arquivos XML de configuração pública durante empacotamento.
* Se uma cadeia de ligação de diagnóstico não está especificada no ficheiro. cscfg, Visual Studio retrocede para utilizando a conta de armazenamento que é especificada no ficheiro .wadcfgx para configurar a extensão de diagnóstico para a publicação e para gerar o XML de configuração pública arquivos durante o empacotamento.
* A cadeia de ligação de diagnóstico no ficheiro. cscfg tem precedência sobre a conta de armazenamento no ficheiro .wadcfgx. Se uma cadeia de ligação de diagnóstico é especificado no ficheiro. cscfg, o Visual Studio utiliza essa cadeia de ligação e ignora a conta de armazenamento no .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-check-box-do"></a>O que faz a caixa de verificação "Atualizar cadeias de ligação de armazenamento de desenvolvimento..."?
O **atualizar cadeias de ligação de armazenamento de desenvolvimento para diagnóstico e de colocação em cache com as credenciais de conta de armazenamento do Microsoft Azure durante a publicação para o Microsoft Azure** caixa de verificação é uma forma conveniente para atualizar qualquer armazenamento de desenvolvimento ligação da conta de cadeias de caracteres com a conta de armazenamento do Azure que especificou durante a publicação.

Por exemplo, se selecionar esta caixa de verificação e a cadeia de ligação de diagnóstico especifica `UseDevelopmentStorage=true`, quando publica o projeto no Azure, Visual Studio atualiza automaticamente a cadeia de ligação de diagnóstico com a conta de armazenamento que especificou no o Assistente para publicação. No entanto, se uma conta de armazenamento real foi especificada como a cadeia de ligação de diagnóstico, essa conta é utilizada em vez disso.

## <a name="diagnostics-functionality-differences-in-azure-sdk-24-and-earlier-vs-azure-sdk-25-and-later"></a>Diferenças de funcionalidade de diagnóstico no Azure SDK 2.4 e vs anteriores. Azure SDK 2.5 e posterior
Se estiver a atualizar o seu projeto do Azure SDK 2.4 e anteriormente para o Azure SDK 2.5 ou posterior, tenha em atenção as seguintes diferenças de funcionalidade de diagnóstico:

* **APIs de configuração são preteridas**. A configuração programática de diagnóstico está disponível no Azure SDK 2.4 e anteriores, mas foi preterida no Azure SDK 2.5 e versões posteriores. Se a configuração de diagnóstico está atualmente definida no código, tem de reconfigurar essas definições a partir do zero no projeto migrado para obter um diagnóstico para continuar a trabalhar. O ficheiro de configuração de diagnósticos para o Azure SDK 2.4 é diagnostics.wadcfg. O ficheiro de configuração de diagnósticos para o Azure SDK 2.5 e posterior é diagnostics.wadcfgx.
* **Diagnósticos para aplicações de serviço em nuvem podem ser configurados apenas ao nível da função**. No Azure SDK 2.5 e posterior, não é possível configurar diagnósticos para aplicações de serviço na cloud ao nível da instância.
* **Sempre que implemente a sua aplicação, a configuração de diagnósticos é atualizada**. Isso pode causar problemas de paridade, se alterar a configuração de diagnósticos do Visual Studio Server Explorer e, em seguida, volte a implementar a sua aplicação.
* **No Azure SDK 2.5 e posterior, despejos de memória estão configurados no arquivo de configuração de diagnósticos e não no código**. Se tiver despejos de memória configurados no código, deve transferir manualmente a configuração a partir do código ao arquivo de configuração. Despejos de memória não são transferidos durante a migração para o Azure SDK 2.6.

## <a name="turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them"></a>Ativar diagnósticos nos projetos de serviço de cloud antes de implementá-las
No Visual Studio, pode recolher dados de diagnóstico para funções que são executadas no Azure ao executar o serviço no emulador antes da implantação. Todas as alterações às definições de diagnóstico no Visual Studio são guardadas no ficheiro de configuração de diagnostics.wadcfgx. Estas definições especificam a conta de armazenamento onde os dados de diagnóstico são guardados quando implementar o serviço de nuvem.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="to-turn-on-diagnostics-in-visual-studio-before-deployment"></a>Para ativar os diagnósticos no Visual Studio antes da implantação

1. No menu de atalho para a função, selecione **propriedades**. A função **propriedades** caixa de diálogo, selecione a **configuração** separador.
2. Na **diagnóstico** secção, certifique-se de que o **ativar diagnósticos** caixa de verificação está selecionada.
   
    ![A opção de ativar o diagnóstico de acesso](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Para especificar a conta de armazenamento para os dados de diagnóstico, selecione o botão de reticências (...).
   
    ![Especifique a conta de armazenamento a utilizar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. Na **criar cadeia de ligação de armazenamento** diálogo caixa, especifique se pretende ligar-se ao utilizar o emulador de armazenamento do Azure, uma subscrição do Azure, ou introduzir manualmente as credenciais.
   
    ![Caixa de diálogo de conta de armazenamento](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Se selecionou **emulador de armazenamento do Microsoft Azure**, a cadeia de ligação é definida como `UseDevelopmentStorage=true`.
   * Se selecionou **sua assinatura**, pode selecionar a subscrição do Azure que pretende utilizar e introduza um nome de conta. Para gerir as suas subscrições do Azure, selecione **gerir contas**.
   * Se selecionou **introduzido manualmente as credenciais**, introduza o nome e chave da conta do Azure que pretende utilizar.
5. Para ver os **configuração de diagnósticos** caixa de diálogo, selecione **configurar**. Exceto para **gerais** e **diretórios de registo**, cada separador Representa uma origem de dados de diagnóstico que pode recolher. A predefinição **gerais** separador oferece o diagnóstico seguinte opções de recolha de dados: **apenas erros**, **todas as informações**, e **plano de personalizado**. A predefinição **apenas erros** opção utiliza a menor quantidade de armazenamento, porque ele não transferência avisos ou mensagens de rastreio. O **todas as informações** opção transfere mais informações, utiliza o armazenamento de mais e, portanto, é a opção mais cara.

   > [!NOTE]
   > Tamanho mínimo suportado de "Disco Quota em MB" é 4GB. No entanto, se está a recolher despejos de memória, aumente esse tamanho para um valor mais alto, como 10GB.
   >
  
    ![Ativar o diagnóstico do Azure e a configuração](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Para este exemplo, selecione o **plano personalizado** opção, pelo que pode personalizar os dados recolhidos.
7. Na **Disková Kvóta v MB** caixa, pode definir a quantidade de espaço para alocar na sua conta de armazenamento para dados de diagnóstico. Pode alterar ou aceite o valor predefinido.
8. Em cada separador de dados de diagnóstico que pretende recolher, selecione o **ativar a transferência de \<tipo de registo\>**  caixa de verificação. Por exemplo, se pretender recolher registos de aplicações, na **registos de aplicações** separador, selecione a **ativar a transferência de registos de aplicações** caixa de verificação. Além disso, especifique quaisquer outras informações que sejam necessários para cada tipo de dados de diagnóstico. Para informações de configuração para cada guia, consulte a secção **configurar origens de dados de diagnóstico** mais adiante neste artigo. 
9. Depois de ativar a coleção de todos os dados de diagnóstico que pretende, selecione **OK**.
10. Execute seu projeto de serviço cloud do Azure no Visual Studio como de costume. Como utilizar a aplicação, as informações de registo que ativou são guardadas para a conta de armazenamento do Azure que especificou.

## <a name="turn-on-diagnostics-on-azure-virtual-machines"></a>Ativar diagnósticos em máquinas virtuais do Azure
No Visual Studio, pode recolher dados de diagnóstico para máquinas virtuais do Azure.

### <a name="to-turn-on-diagnostics-on-azure-virtual-machines"></a>Para ativar os diagnósticos em máquinas virtuais do Azure

1. No Explorador de servidores, selecione o nó do Azure e, em seguida, ligar à sua subscrição do Azure, se ainda não estiver ligado.
2. Expanda a **máquinas virtuais** nó. Pode criar uma nova máquina virtual ou selecione um nó existente.
3. No menu de atalho para a máquina virtual que pretende, selecione **configurar**. É apresentada a caixa de diálogo de configuração de máquina virtual.
   
    ![Configurar uma máquina virtual do Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Se ainda não estiver instalado, adicione a extensão de diagnóstico de agente de monitorização Microsoft. Com essa extensão, pode recolher dados de diagnóstico para a máquina virtual do Azure. Sob **extensões instaladas**, na **Selecione uma extensão disponível** caixa de lista pendente, selecione **diagnóstico de agente de monitorização Microsoft**.
   
    ![Instalar uma extensão de máquina virtual do Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
    > [!NOTE]
   > Outras extensões de diagnóstico estão disponíveis para as suas máquinas virtuais. Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades para o Windows](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).
   > 
   > 
5. Para adicionar a extensão e a vista de sua **configuração de diagnósticos** caixa de diálogo, selecione **Add**.
6. Para especificar uma conta de armazenamento, selecione **configurar**e, em seguida, selecione **OK**.
   
    Cada separador (exceto para **gerais** e **diretórios de registo**) representa uma origem de dados de diagnóstico que pode recolher.
   
    ![Ativar o diagnóstico do Azure e a configuração](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    A guia padrão, **gerais**, oferece as seguintes opções de recolha de dados de diagnóstico: **apenas erros**, **todas as informações**, e **doplanodepersonalizado**. A opção predefinida, **apenas erros**, leva a menor quantidade de armazenamento porque ele não transferência avisos ou mensagens de rastreio. O **todas as informações** opção transfere o máximo de informações e é, portanto, a opção mais cara em termos de armazenamento.
7. Para este exemplo, selecione o **plano personalizado** opção, pelo que pode personalizar os dados recolhidos.
8. O **Disková Kvóta v MB** caixa especifica a quantidade de espaço que pretende alocar na sua conta de armazenamento para dados de diagnóstico. Pode alterar o valor predefinido se desejar.
9. Em cada separador de dados de diagnóstico que pretende recolher, selecione seu **ativar a transferência de \<tipo de registo\>**  caixa de verificação.
   
    Por exemplo, se pretender recolher registos de aplicações, selecione o **ativar a transferência de registos de aplicações** caixa de verificação a **registos das aplicações** separador. Além disso, especifique quaisquer outras informações necessárias para cada tipo de dados de diagnóstico. Para informações de configuração para cada guia, consulte a secção **configurar origens de dados de diagnóstico** mais adiante neste artigo.
10. Depois de ativar a coleção de todos os dados de diagnóstico que pretende, selecione **OK**.
11. Guarde o projeto atualizado.
    
    Uma mensagem na **registo de atividades do Microsoft Azure** janela indica que a máquina virtual foi atualizada.

## <a name="set-up-diagnostics-data-sources"></a>Configurar origens de dados de diagnóstico
Depois de ativar a recolha de dados de diagnóstico, pode escolher exatamente que origens de dados que pretende recolher e as informações são recolhidas. As secções seguintes descrevem os separadores na **configuração de diagnósticos** caixa de diálogo e significa que cada opção de configuração.

### <a name="application-logs"></a>Registos de aplicações
Registos de aplicações tem informações de diagnóstico que são produzidas por um aplicativo web. Se quiser capturar os registos da aplicação, selecione o **ativar a transferência de registos de aplicações** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de registos de aplicações para a sua conta de armazenamento, altere a **período de transferência (min)** valor. Também pode alterar a quantidade de informações capturadas no registo de definindo a **nível de registo** valor. Por exemplo, seleccione **verboso** para obter mais informações ou selecione **crítico** para capturar apenas erros críticos. Se tiver um provedor de diagnóstico específico que emite os registos da aplicação, pode capturar os registos ao Adicionar GUID o fornecedor no **GUID de provedor** caixa.

  ![Registos de aplicações](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

Para obter mais informações sobre os registos de aplicação, consulte [ativar o registo de diagnóstico para aplicações Web no App Service do Azure](app-service/web-sites-enable-diagnostic-log.md).

### <a name="windows-event-logs"></a>Registos de eventos do Windows
Para capturar os logs de eventos do Windows, selecione o **ativar a transferência de registos de eventos do Windows** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de registos de eventos para a sua conta de armazenamento, altere a **período de transferência (min)** valor. Selecione as caixas de verificação para os tipos de eventos que pretende controlar.

![Registos de eventos](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Se estiver a utilizar o Azure SDK 2.6 ou posterior e pretender especificar uma origem de dados personalizada, introduza-a na **\<nome da origem de dados\>** caixa de texto e, em seguida, selecione **Add**. A origem de dados é adicionada ao arquivo diagnostics.cfcfg.

Se estiver a utilizar o Azure SDK 2.5 e pretender especificar uma origem de dados personalizada, pode adicioná-lo para o `WindowsEventLog` seção o diagnostics.wadcfgx de ficheiros, como no exemplo a seguir:

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Contadores de desempenho
Informações do contador de desempenho podem ajudá-lo a localizar constrangimentos no sistema e otimizar o desempenho do sistema e da aplicação. Para obter mais informações, consulte [criar e utilizar contadores de desempenho numa aplicação Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx). Para capturar os contadores de desempenho, selecione o **ativar a transferência de contadores de desempenho** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de registos de eventos para a sua conta de armazenamento, altere a **período de transferência (min)** valor. Selecione as caixas de verificação para os contadores de desempenho que pretende controlar.

![Contadores de desempenho](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Para monitorizar um contador de desempenho que não estiver listado, introduza o contador de desempenho utilizando a sintaxe sugerida. e, em seguida, selecione **adicionar**. O sistema operativo na máquina virtual determina que contadores de desempenho pode controlar. Para obter mais informações sobre a sintaxe, consulte [especificar um caminho de contador](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Registos de infraestrutura
Registos de infraestrutura têm informações sobre a infra-estrutura de diagnóstico do Azure, o módulo de RemoteAccess e RemoteForwarder módulo. Para recolher informações sobre os registos de infraestrutura, selecione o **ativar a transferência de Logs de infraestrutura** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de logs de infraestrutura para a sua conta de armazenamento, altere a **período de transferência (min)** valor.

![Registos de infraestrutura de diagnóstico](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

Para obter mais informações, consulte [recolher dados de registo com o diagnóstico do Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="log-directories"></a>Diretórios de registo
Diretórios de registo tem dados recolhidos a partir de diretórios de registo para pedidos de serviços de informação Internet (IIS), pedidos falhados ou pastas que escolher. Para capturar os diretórios de registo, selecione o **ativar a transferência de diretórios de registo** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de registos para a sua conta de armazenamento, altere a **período de transferência (min)** valor.

Selecione as caixas de verificação dos registos que pretende recolher, tal como **registos do IIS** e **pedidos falhados** registos. São fornecidos nomes de contentor de armazenamento padrão, mas pode alterar os nomes.

Pode capturar os registos de qualquer pasta. Especifique o caminho no **Log do Directory absoluto** secção e, em seguida, selecione **Adicionar diretório**. Os registos são capturados nos contentores especificados.

![Diretórios de registo](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Registos ETW
Se usar [rastreio de eventos para o Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) e pretende capturar os registos ETW, selecione a **ativar a transferência de registos ETW** caixa de verificação. Para aumentar ou diminuir o intervalo entre a transferência de registos para a sua conta de armazenamento, altere a **período de transferência (min)** valor.

Os eventos são capturados a partir de origens de eventos e manifestos de eventos que especificar. Para especificar uma origem de evento, o **origens de eventos** secção, introduza um nome e, em seguida, selecione **Adicionar origem de evento**. Da mesma forma, pode especificar um manifesto de eventos no **manifestos de eventos** secção e, em seguida, selecione **adicionar manifesto de eventos**.

![Registos ETW](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

A estrutura ETW é suportada no ASP.NET por meio de classes no [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) espaço de nomes. O espaço de nomes Microsoft.WindowsAzure.Diagnostics, que herda do e estende standard [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) classes, permite a utilização de [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) como um registro em log estrutura no ambiente do Azure. Para obter mais informações, consulte [assuma o controle de Registro em log e rastreamento de eventos no Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) e [ativar diagnósticos nos serviços Cloud do Azure e máquinas virtuais](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Informações de falha de sistema
Para capturar informações sobre o quando uma instância de função falha, selecione o **ativar a transferência de informações do Estado de falha de sistema** caixa de verificação. (Uma vez que o ASP.NET gerencia a maioria das exceções, é geralmente útil apenas para funções de trabalho.) Para aumentar ou reduzir a percentagem de espaço de armazenamento dedicado para os despejos de memória, altere a **Quota de diretório (%)** valor. Pode alterar o contentor de armazenamento onde os despejos de memória são armazenados e selecione se pretende capturar um **completo** ou **Mini** informação.

Os processos sendo acompanhados no momento estão listados na seguinte captura de ecrã. Selecione as caixas de verificação para os processos que deseja capturar. Para adicionar outro processo à lista, introduza o nome do processo e, em seguida, selecione **processo de adicionar**.

![Informações de falha de sistema](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

Para obter mais informações, consulte [assuma o controle de Registro em log e rastreamento de eventos no Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) e [Microsoft Azure Diagnostics parte 4: componentes de registo personalizado e alterações de Azure diagnóstico 1.3](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/).

## <a name="view-the-diagnostics-data"></a>Ver os dados de diagnóstico
Depois de já recolhidos os dados de diagnóstico para um serviço cloud ou máquina virtual, pode vê-lo.

### <a name="to-view-cloud-service-diagnostics-data"></a>Para ver dados de diagnóstico do serviço de nuvem
1. Implementar o seu serviço cloud como é habitual e, em seguida, executá-lo.
2. Pode ver os dados de diagnóstico num relatório que o Visual Studio gera ou nas tabelas na sua conta de armazenamento. Para ver os dados num relatório, aberto Cloud Explorer ou o Explorador de servidores, abra o menu de atalho do nó para a função que pretende e, em seguida, selecione **vista de dados de diagnóstico**.
   
    ![Zobrazit diagnostická data](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    É apresentado um relatório que mostra os dados disponíveis.
   
    ![Relatório do Microsoft Azure Diagnostics no Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Se os dados mais recentes não são mostrados, poderá ter de aguardar o tempo de transferência termine.
   
    Para atualizar imediatamente os dados, selecione o **atualizar** ligação. Para que os dados atualizados automaticamente, selecione um intervalo na **atualização automática** caixa de lista pendente. Para exportar os dados de erro, selecione o **exportar para CSV** botão para criar um ficheiro de valores separados por vírgulas que pode abrir numa planilha do Excel.
   
    No Cloud Explorer ou no Explorador de servidores, abra a conta de armazenamento que está associada a implementação.
3. Abra as tabelas de diagnóstico no Visualizador de tabela e, em seguida, reveja os dados que recolheu. Para registos do IIS e registos personalizados, pode abrir um contentor de Blobs. A tabela seguinte lista as tabelas ou contentores de BLOBs que contêm os dados para os ficheiros de registo diferente. Além dos dados para esse ficheiro de registo, as entradas de tabela contêm **EventTickCount**, **DeploymentId**, **função**, e **RoleInstance** , para ajudar a identificar quais máquina virtual e a função gerados os dados e quando. 
   
   | Dados de diagnóstico | Descrição | Localização |
   | --- | --- | --- |
   | Registos de aplicações |Os registos que seu código gera chamando métodos do **Trace** classe. |WADLogsTable |
   | Registos de eventos |Dados dos logs de eventos do Windows nas máquinas virtuais. Windows armazena informações nestes registos, mas aplicativos e serviços também utilizam os registos de erros de relatório ou registar informações. |WADWindowsEventLogsTable |
   | Contadores de desempenho |Pode recolher dados em qualquer contador de desempenho que está disponível na máquina virtual. O sistema operativo fornece os contadores de desempenho, que incluem estatísticas de muitos, como o tempo de processador e utilização de memória. |WADPerformanceCountersTable |
   | Registos de infraestrutura |Registos que são gerados a partir da própria infra-estrutura de diagnóstico. |WADDiagnosticInfrastructureLogsTable |
   | Registos do IIS |Registos que registam as solicitações da web. Se o seu serviço cloud obtém uma quantidade significativa de tráfego, estes registos podem ser demorados. É uma boa idéia para recolher e armazenar esses dados apenas quando precisa dela. |Pode encontrar registos de solicitação com falha no contentor de BLOBs em wad-iis-failedreqlogs, num caminho para essa implantação, a função e a instância. Pode encontrar registos completos em wad-iis-logfiles. As entradas para cada ficheiro são feitas na tabela WADDirectories. |
   | Informações de falha de sistema |Fornece imagens binárias de processo do seu serviço cloud (normalmente uma função de trabalho). |contentor de BLOBs de capturas de crush WAD |
   | Ficheiros de registo personalizado |Registos de dados que predefinidas. |Pode especificar no código a localização dos ficheiros de registo personalizado na sua conta de armazenamento. Por exemplo, pode especificar um contentor de BLOBs personalizado. |
4. Se os dados de qualquer tipo serão truncados, pode experimentar aumentar a memória intermédia para que os dados de tipo ou reduzindo o intervalo entre as transferências de dados da máquina virtual para a sua conta de armazenamento.
5. (Opcional) Remover dados da conta de armazenamento, ocasionalmente, para reduzir os custos gerais de armazenamento.
6. Quando efetuar uma implementação completa, o arquivo de diagnostics.cscfg (.wadcfgx para o Azure SDK 2.5) é atualizado no Azure e o seu serviço cloud seleciona todas as alterações à sua configuração de diagnósticos. Se atualizar uma implementação existente em vez disso, o ficheiro. cscfg não é atualizado no Azure. Ainda pode alterar as definições de diagnóstico, no entanto, ao seguir os passos na secção seguinte. Para obter mais informações sobre como efetuar uma implementação completa e atualizar uma implementação existente, consulte [publicar Assistente da aplicação Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Para ver os dados de diagnóstico de máquina virtual
1. No menu de atalho para a máquina virtual, selecione **Zobrazit Diagnostická Data**.
   
    ![Zobrazit diagnostická data numa máquina virtual do Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    O **souhrn Diagnostiky** é apresentada a caixa de diálogo.
   
    ![Souhrn diagnostiky de máquina virtual do Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Se os dados mais recentes não são mostrados, poderá ter de aguardar o tempo de transferência termine.
   
    Para atualizar imediatamente os dados, selecione o **atualizar** ligação. Para que os dados atualizados automaticamente, selecione um intervalo na **atualização automática** caixa de lista pendente. Para exportar os dados de erro, selecione o **exportar para CSV** botão para criar um ficheiro de valores separados por vírgulas que pode abrir numa planilha do Excel.

## <a name="set-up-cloud-service-diagnostics-after-deployment"></a>Configurar o diagnóstico do serviço cloud após a implementação
Se estiver investigando um problema com um serviço cloud que já está em execução, pode querer recolher dados que não especificou antes que originalmente implementado a função. Neste caso, pode começar a coletar esses dados ao alterar as definições no Explorador de servidores. Pode configurar diagnósticos para uma única instância ou para todas as instâncias numa função, dependendo se abrir o **configuração de diagnósticos** caixa de diálogo no menu de atalho para a instância ou para a função. Se configurar o nó de função, quaisquer alterações que fizer aplicam-se a todas as instâncias. Se configurar o nó de instância, todas as alterações que fizer aplicam-se apenas a essa instância.

### <a name="to-set-up-diagnostics-for-a-running-cloud-service"></a>Para configurar diagnósticos para um serviço cloud em execução
1. No Explorador de servidores, expanda o **serviços Cloud** nó e, em seguida, expanda a lista de nós para localizar a função ou instância (ou ambos) que pretende investigar.
   
    ![Configurar o diagnóstico](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. No menu de atalho para uma instância de nó ou função, selecione **as definições de diagnóstico de atualização**e, em seguida, selecione as definições de diagnóstico que pretende recolher.
   
    Para obter informações sobre as definições de configuração, consulte a secção **configurar origens de dados de diagnóstico** neste artigo. Para obter informações sobre como ver os dados de diagnóstico, consulte a secção **ver os dados de diagnóstico** neste artigo.
   
    Se alterar a recolha de dados no Explorador de servidores, as alterações permanecem em efeito até que Reimplementar totalmente o seu serviço cloud. Se utilizar a predefinição definições de publicação, as alterações não são substituídas. Definição de publicar a predefinição é para atualizar a implementação existente, em vez de fazer uma reimplantação completa. Para garantir que as definições de limpar no momento da implementação, vá para o **definições avançadas** separador no Assistente de publicação e, em seguida, desmarque a **atualização da implementação** caixa de verificação. Quando implementar novamente com a caixa de verificação desmarcada, as definições de reverter para aqueles no ficheiro .wadcfgx (ou .wadcfg) conforme definido por meio da **propriedades** editor para a função. Se atualizar a implementação, o Azure mantém as definições anteriores.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Resolver problemas de serviço cloud do Azure
Se tiver problemas com seus projetos de serviço na cloud, como uma função que fica preso num estado "ocupado", repetidamente recicla ou emite um erro de servidor interno, há ferramentas e técnicas que pode usar para diagnosticar e corrigir o problema. Para obter exemplos específicos de problemas e soluções comuns e uma descrição geral dos conceitos e ferramentas que pode usar para diagnosticar e corrigir esses erros, veja [dados de diagnóstico de computação da Azure PaaS](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>P&R
**O que é o tamanho do buffer, e qual o tamanho máximo deve ser?**

Em cada instância de máquina virtual, as quotas limitam a quantidade de dados diagnóstico pode ser armazenado no sistema de ficheiros local. Além disso, especifica um tamanho de buffer para cada tipo de dados de diagnóstico que estão disponíveis. Este tamanho de memória intermédia funciona como uma quota individual para esse tipo de dados. Para determinar a quota global e a quantidade de memória que permanece, ver a parte inferior da caixa de diálogo para o tipo de dados de diagnóstico. Se especificar buffers maiores ou mais tipos de dados, irá abordar a quota global. Pode alterar a quota global ao modificar o ficheiro de configuração diagnostics.wadcfg ou .wadcfgx. Os dados de diagnóstico são armazenados no mesmo sistema de ficheiros, como os dados da aplicação. Se a sua aplicação utiliza uma grande quantidade de espaço em disco, não deve aumentar a quota de diagnóstico geral.

**O que é o período de transferência, e quanto deve ser?**

O período de transferência é a quantidade de tempo que decorrer entre dados de captura. Após cada período de transferência, dados são movidos do sistema de arquivos local numa máquina virtual para tabelas na conta de armazenamento. Se a quantidade de dados que são recolhidos exceder a quota antes do final de um período de transferência, os dados mais antigos são descartados. Se está perdendo dados uma vez que os seus dados excedem o tamanho da memória intermédia ou a quota global, pode querer diminua o período de transferência.

**O fuso horário são os carimbos de data / hora no?**

É carimbos de data / hora no fuso horário local do datacenter que aloja o seu serviço cloud. As seguintes colunas de carimbo de hora três nas tabelas de log são utilizadas:

* **PreciseTimeStamp**: timestamp o ETW do evento. Ou seja, a hora do evento é registado do cliente.
* **TIMESTAMP**: O valor para **PreciseTimeStamp** arredondado para baixo até o limite da frequência de carregamento. Por exemplo, se a frequência de carregamento é 5 minutos e o evento de tempo 00:17:12, TIMESTAMP é 00:00 15:.
* **Timestamp**: O carimbo de hora em que a entidade foi criada na tabela do Azure.

**Como posso gerir os custos aquando da recolha de informações de diagnóstico?**

As predefinições (**nível de registo** definida como **erro**, e **período de transferência** definido como **1 minuto**) foram concebidos para minimizar os custos. Custos de computação aumentam quando recolher mais dados de diagnóstico ou se diminua o período de transferência. Não recolha mais dados do que precisa e não se esqueça de desativar a recolha de dados quando já não precisam dele. Pode sempre ativá-la novamente, até mesmo no tempo de execução, conforme descrito anteriormente neste artigo.

**Como recolher registos de solicitação com falha a partir do IIS?**

Por predefinição, o IIS não recolhe registos de solicitação com falha. Pode configurar o IIS para recolher registos de solicitação com falha ao editar o ficheiro Web. config para a função da web.

**Não estou a obter informações de rastreio de métodos de RoleEntryPoint como OnStart. O que há de errado?**

Os métodos de **RoleEntryPoint** são chamados no contexto de WAIISHost.exe, não no IIS. As informações de configuração Web. config que normalmente permite rastreio não se aplica. Para resolver este problema, adicione um arquivo. config ao seu projeto de função da web e nomeie o arquivo de acordo com o assembly de saída que contém o **RoleEntryPoint** código. No projeto de função da web de padrão, o nome do arquivo. config deve ser WAIISHost.exe.config. Adicione as seguintes linhas para este ficheiro:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Na **propriedades** janela, definida a **Copy to Output Directory** propriedade **Copiar sempre**.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o diagnóstico de registo no Azure, veja [ativar diagnósticos nos serviços Cloud do Azure e máquinas virtuais](cloud-services/cloud-services-dotnet-diagnostics.md) e [ativar o registo de diagnóstico para aplicações Web no App Service do Azure](app-service/web-sites-enable-diagnostic-log.md).

