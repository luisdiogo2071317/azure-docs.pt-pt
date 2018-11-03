---
title: Testes de desempenho de um serviço em nuvem | Documentos da Microsoft
description: Testar o desempenho de um serviço de nuvem usando o criador de perfil do Visual Studio
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: aa8416bb6883a1aa76ddd370bf7061d7013904eb
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969483"
---
# <a name="testing-the-performance-of-a-cloud-service"></a>Testes de desempenho de um serviço cloud
## <a name="overview"></a>Descrição geral
Pode testar o desempenho de um serviço em nuvem das seguintes formas:

* Utilize o diagnóstico do Azure para recolher informações sobre ligações e pedidos de e para rever as estatísticas de site que mostram como o serviço executa da perspectiva do cliente. Para começar, veja [Konfiguruje se diagnostika para serviços Cloud do Azure e máquinas virtuais](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* Use o criador de perfil do Visual Studio para obter uma análise aprofundada dos aspectos de computacionais do como o serviço é executado. Conforme descrito neste tópico, pode usar o criador de perfil para medir o desempenho como um serviço é executado no Azure. Para obter informações sobre como usar o criador de perfil para medir o desempenho como um serviço é executado localmente no emulador de computação, consulte [testar o desempenho de um serviço de nuvem do Azure localmente do emulador de computação com o Profiler Studio Visual](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Escolher um método de teste de desempenho
### <a name="use-azure-diagnostics-to-collect"></a>Utilize o diagnóstico do Azure para recolher:
* Estatísticas em páginas da web ou serviços, como ligações e pedidos.
* Estatísticas em funções, tais como a frequência com que uma função é reiniciada.
* Em geral, informações sobre a utilização de memória, como a percentagem de tempo que demora o coletor de lixo ou a memória definida de uma função em execução.

### <a name="use-the-visual-studio-profiler-to"></a>Utilize o criador de perfil do Visual Studio para:
* Determine as funções que demoram mais tempo.
* Meça quanto tempo leva cada parte de um programa intensos de computação.
* Compare relatórios de desempenho detalhadas para duas versões de um serviço.
* Analise a alocação de memória em mais detalhes que o nível de alocações de memória individuais.
* Analise problemas de simultaneidade em código multithread.

Quando utiliza o criador de perfil, pode recolher dados quando um serviço em nuvem é executada localmente ou no Azure.

### <a name="collect-profiling-data-locally-to"></a>Recolha dados de perfil localmente para:
* Teste o desempenho de uma parte de um serviço de nuvem, como a execução de função de trabalho específica, que não necessita de uma carga simulada realista.
* Teste o desempenho de um serviço cloud de forma isolada, sob condições controladas.
* Teste o desempenho de um serviço em nuvem antes de o implementar no Azure.
* Teste o desempenho de um serviço em nuvem privada, sem perturbar as implementações existentes.
* Teste o desempenho do serviço sem incorrer em custos para executar no Azure.

### <a name="collect-profiling-data-in-azure-to"></a>Recolha dados de criação de perfis no Azure para:
* Teste o desempenho de um serviço em nuvem com uma carga simulado ou real.
* Utilize o método de instrumentação de recolha de dados de criação de perfis, como este tópico descreve mais tarde.
* Teste o desempenho do serviço no mesmo ambiente que quando o serviço é executado na produção.

Normalmente, a simular uma carga para serviços de cloud de teste em normal ou condições de estresse.

## <a name="profiling-a-cloud-service-in-azure"></a>A criação de perfis de um serviço cloud no Azure
Quando publica o seu serviço de cloud a partir do Visual Studio, pode o serviço de perfil e especificar as definições de criação de perfis que dão-lhe as informações que pretende. Uma sessão de criação de perfis é iniciada para cada instância de uma função. Para obter mais informações sobre como publicar o seu serviço a partir do Visual Studio, consulte [publicar num serviço Cloud do Azure a partir do Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Para saber mais sobre o perfil de desempenho no Visual Studio, veja [manual de principiantes para a criação de perfis de desempenho](https://msdn.microsoft.com/library/azure/ms182372.aspx) e [análise de desempenho de aplicações através de ferramentas de criação de perfis](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> Pode ativar o IntelliTrace ou criação de perfis quando publica o seu serviço cloud. Não é possível ativar ambos.
> 
> 

### <a name="profiler-collection-methods"></a>Métodos de coleção do Profiler
Pode usar métodos de coleção diferente para a criação de perfis, com base nos seus problemas de desempenho:

* **Amostragem de CPU** -neste método recolhe estatísticas da aplicação que são úteis para análise inicial dos problemas de utilização da CPU. Amostragem de CPU é o método sugerido para iniciar a maioria das investigações de desempenho. Há um impacto baixo sobre a aplicação que se está criando quando recolher os dados de amostragem de CPU.
* **Instrumentação** -esse método recolhe dados de tempo detalhadas que é útil para uma análise focada e para analisar problemas de desempenho de entrada/saída. O método de instrumentação regista cada entrada, saída e chamada de função das funções num módulo durante uma geração de perfil. Esse método é útil para a recolha de informações de temporização detalhadas sobre uma seção do código e para compreender o impacto das operações de entrada e saídas no desempenho da aplicação. Este método está desativado para um computador com um sistema operativo de 32 bits. Esta opção só está disponível quando executa o serviço em nuvem no Azure, não localmente no emulador de computação.
* **Alocação de memória de .NET** -neste método recolhe dados de alocação de memória do .NET Framework usando a método de criação de perfis de amostragem. Os dados recolhidos incluem o número e tamanho dos objetos alocados.
* **Simultaneidade** -neste método recolhe dados de contenção de recursos e processos e threads dados de execução que é útil na análise de aplicações com múltiplos threads e com vários processos. O método de simultaneidade recolhe dados de cada evento que a execução de blocos do seu código, por exemplo, quando um thread aguarda bloqueado acesso a um recurso de aplicação para ser liberado. Este método é útil para a análise de aplicativos multithread.
* Também é possível habilitar **Tier Interaction Profiling**, que fornece informações adicionais sobre os tempos de execução do ADO.NET síncrono chama nas funções de aplicativos de várias camadas de mensagens em fila que comunicam com um ou mais bases de dados. Pode recolher dados de interação de camada com qualquer um dos métodos de criação de perfis. Para obter mais informações sobre a criação de perfil de interação de camada, consulte [vista de interações da camada](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Configurar definições de criação de perfis
A ilustração seguinte mostra como configurar as definições de criação de perfis da caixa de diálogo Publicar a aplicação do Azure.

![Configurar definições de criação de perfis](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> Para ativar a **ativar a criação de perfis** caixa de verificação, tem de ter o criador de perfil instalado no computador local que está a utilizar para publicar o seu serviço cloud. Por predefinição, o criador de perfil é instalado quando instalar o Visual Studio.
> 
> 

### <a name="to-configure-profiling-settings"></a>Para configurar as definições de criação de perfis
1. No Solution Explorer, abra o menu de atalho para o seu projeto do Azure e, em seguida, escolha **publicar**. Para obter passos detalhados sobre como publicar um serviço em nuvem, consulte [publicação de um serviço de nuvem usando as ferramentas do Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. Na **publicar a aplicação do Azure** caixa de diálogo, escolheu o **definições avançadas** separador.
3. Para ativar a criação de perfis, selecione o **ativar a criação de perfis** caixa de verificação.
4. Para configurar as definições de criação de perfis, escolha o **definições** hyperlink. É apresentada a caixa de diálogo de definições de criação de perfis.
5. Partir do **qual método de criação de perfil pretende utilizar** botões de opção, selecione o tipo de criação de perfis que precisa.
6. Para recolher dados de criação de perfil de interação de camada, selecione o **ativar Tier Interaction Profiling** caixa de verificação.
7. Para guardar as definições, escolha o **OK** botão.
   
    Quando publicar esta aplicação, estas definições são utilizadas para criar a sessão de criação de perfil para cada função.

## <a name="viewing-profiling-reports"></a>Ver relatórios de criação de perfis
Uma sessão de criação de perfil é criada para cada instância de uma função no seu serviço cloud. Para ver os relatórios de criação de perfil de cada sessão a partir do Visual Studio, pode exibir a janela do Explorador de servidores e, em seguida, selecione o nó de computação do Azure para selecionar uma instância de uma função. Em seguida, pode ver o relatório de criação de perfis, conforme mostrado na ilustração seguinte.

![Zobrazit Sestavu Profilace do Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Para ver relatórios de criação de perfis
1. Para exibir a janela do Explorador de servidores no Visual Studio, na barra de menus escolha exibição, Explorador de servidores.
2. Escolha o nó de computação do Azure e, em seguida, selecione o nó de implementação do Azure para o serviço cloud que selecionou para o perfil quando publicados a partir do Visual Studio.
3. Para ver relatórios de criação de perfis para uma instância, escolha a função no serviço, abra o menu de atalho para uma instância específica e, em seguida, escolha **Ver relatório de criação de perfis**.
   
    O relatório, um arquivo de Vsp, agora é transferido a partir do Azure e o estado da transferência aparece no registo de atividades do Azure. Quando o download for concluído, o relatório de criação de perfis é apresentado num separador do Editor para Visual Studio com o nome <Role name> *<Instance Number>* <identifier>Vsp. Dados de resumo para o relatório é apresentada.
4. Para apresentar exibições diferentes do relatório, na lista de vista atual, escolha o tipo de vista que pretende. Para obter mais informações, consulte [vistas de relatório de ferramentas de criação de perfis](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Passos Seguintes
[Depuração de serviços Cloud](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publicar um serviço Cloud do Azure a partir do Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

