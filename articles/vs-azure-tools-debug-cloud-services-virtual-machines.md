---
title: Depurar um serviço cloud do Azure ou numa máquina virtual no Visual Studio | Documentos da Microsoft
description: Depurar um serviço Cloud ou Máquina Virtual no Visual Studio
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: douge
editor: ''
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: af961533648ce3a939c25fca32173684255a52a8
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444695"
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Depurar um serviço cloud do Azure ou numa máquina virtual no Visual Studio

Visual Studio dá-lhe diferentes opções para a depuração de serviços cloud do Azure e máquinas virtuais.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Depurar o seu serviço cloud no seu computador local

Pode poupar tempo e dinheiro ao utilizar o Azure computação emulador para depurar o seu serviço cloud numa máquina local. Ao depurar um serviço localmente antes de a implementar, pode melhorar a confiabilidade e desempenho sem ter de adquirir tempo de computação. No entanto, poderão ocorrer alguns erros apenas quando executa um serviço em nuvem no Azure em si. Se ativar a depuração remota, quando publicar o seu serviço e, em seguida, anexe o depurador para uma instância de função, pode depurar estes erros.

O emulador simula o serviço de computação do Azure e é executado no seu ambiente local para que possa testar e depurar o seu serviço cloud antes de o implementar. O emulador processa o ciclo de vida das suas instâncias de função e fornece acesso a recursos simulados, como armazenamento local. Para depurar ou executar seu serviço a partir do Visual Studio, ele inicia automaticamente o emulador como um aplicativo em segundo plano e, em seguida, implementa o seu serviço no emulador. Pode utilizar o emulador para ver o seu serviço quando ele é executado no ambiente local. Pode executar a versão completa ou a versão express do emulador. (A partir do Azure 2.3, a versão express do emulador é a predefinição.) Ver [com o Emulator Express para executar e depurar um serviço Cloud localmente](vs-azure-tools-emulator-express-debug-run.md).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Para depurar o seu serviço cloud no seu computador local

1. Na barra de menu, escolha **depurar**, **iniciar depuração** para executar o seu projeto de serviço cloud do Azure. Como alternativa, pode premir F5. Verá uma mensagem que está a iniciar o emulador de computação. Quando o emulador é iniciado, o ícone de tabuleiro de sistema confirma isso.

    ![Emulador do Azure na Bandeja do sistema](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

2. Apresentar a interface do usuário para o emulador de computação ao abrir o menu de atalho para o ícone do Azure na área de notificação e, em seguida, selecione **mostrar IU do emulador de computação**.

    Painel do lado esquerdo da interface do Usuário mostra os serviços que estão atualmente implementados para o emulador de computação e as instâncias de função que cada serviço está em execução. Pode escolher o serviço ou as funções para apresentar o ciclo de vida, registo e informações de diagnóstico no painel da direita. Se colocar o foco na margem superior de uma janela incluída, esta é expandida para preencher o painel da direita.

3. Passo através do aplicativo selecionando comandos no **depurar** menu e a definição de pontos de interrupção em seu código. À medida que avança o aplicativo no depurador, os painéis são atualizados com o estado atual do aplicativo. Quando parar a depuração, a implementação da aplicação é eliminada. Se a aplicação inclui uma função da web e tiver definido a propriedade de ação de arranque para iniciar o navegador da web, o Visual Studio inicia a aplicação web no browser. Se alterar o número de instâncias de uma função na configuração do serviço, tem de parar o serviço em nuvem e, em seguida, reinicie a depuração para que pode depurar estas novas instâncias da função.

    **Nota:** quando parar em execução ou a depurar o seu serviço, o emulador de computação local e o emulador de armazenamento não estão parados. Tem de pará-los explicitamente da área de notificação.

## <a name="debug-a-cloud-service-in-azure"></a>Depurar um serviço cloud no Azure

Para depurar um serviço em nuvem de um computador remoto, tem de ativar essa funcionalidade explicitamente ao implementar o seu serviço cloud para que precisem de serviços (msvsmon.exe, por exemplo) estão instalados nas máquinas virtuais que executam suas instâncias de função. Se não tiver ativado a depuração remota, quando o serviço é publicado, terá de voltar a publicar o serviço com a depuração remota ativada.

Se ativar a depuração remota para um serviço em nuvem, ela não apresentam a degradação do desempenho ou incorrer em custos adicionais. Não utilize a depuração remota num serviço de produção, porque os clientes que utilizam o serviço poderão ser afetados negativamente.

> [!NOTE]
> Quando publica um serviço em nuvem do Visual Studio, pode habilitar **IntelliTrace** para quaisquer funções em que o serviço que visam o .NET Framework 4 ou o .NET Framework 4.5. Usando **IntelliTrace**, pode examinar os eventos que ocorreram numa instância de função no passado e reproduzir o contexto contar dessa hora. Ver [depurar um serviço cloud publicado com o IntelliTrace e o Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) e [usando o IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Para ativar a depuração remota para um serviço cloud

1. Abra o menu de atalho para o projeto do Azure e, em seguida, selecione **publicar**.

2. Selecione o **transição** ambiente e o **depurar** configuração.

    Isso é apenas uma orientação. Pode optar por executar os seus ambientes de teste num ambiente de produção. No entanto, poderá afetar negativamente os utilizadores se ativar a depuração remota no ambiente de produção. Pode escolher a configuração da versão, mas a configuração de depuração torna a depuração mais fácil.

    ![Escolha a configuração de depuração](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

3. Siga as etapas normais, mas selecione os **ativar o depurador remoto para todas as funções** caixa de verificação a **definições avançadas** separador.

    ![Configuração de depuração](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Para anexar o depurador a um serviço cloud no Azure

1. No Server Explorer, expanda o nó do serviço em nuvem.

2. Abra o menu de atalho para a função ou a instância de função para o qual pretende anexar e, em seguida, selecione **anexar o depurador**.

    Se a depurar uma função, o depurador do Visual Studio anexa a cada instância de função. O depurador será interrompido num ponto de interrupção para a primeira instância de função que executa essa linha de código e cumpre as condições desse ponto de interrupção. Se depurar uma instância, o depurador anexa apenas a essa instância e quebras de um ponto de interrupção apenas quando essa instância específica executa essa linha de código e cumpre as condições do ponto de interrupção.

    ![Anexar o depurador](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

3. Após o depurador anexa a uma instância, Depure como de costume. O depurador anexa automaticamente para o processo de anfitrião adequado para a sua função. Dependendo do que a função é, o depurador anexa w3wp.exe, WaWorkerHost.exe ou WaIISHost.exe. Para verificar o processo ao qual o depurador for anexado, expanda o nó de instância no Gerenciador de servidores. Ver [arquitetura de funções do Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) para obter mais informações sobre os processos do Azure.

    ![Selecione a caixa de diálogo do tipo de código](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

4. Para identificar os processos para que o depurador for anexado, abra a caixa de diálogo de processos por, no menu de barra, escolhendo a depuração, Windows, processos. (Teclado: Ctrl + Alt + Z) Para desanexar um processo específico, abra o menu de atalho e, em seguida, selecione **processo de anulação da exposição**. Em alternativa, localize o nó de instância no Gerenciador de servidores, encontrar o processo, abra o menu de atalho e, em seguida, selecione **processo de anulação da exposição**.

    ![Processos de depuração](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Evitar longas paradas em pontos de interrupção quando remoto depuração. O Azure trata um processo que está parado durante mais de alguns minutos como não responsivo e deixará de enviar tráfego para essa instância. Se parar durante muito tempo, msvsmon.exe desliga do processo.

Para desanexar o depurador de todos os processos na sua instância ou a função, abra o menu de atalho para a função ou a instância que estiver depurando e, em seguida, selecione **desanexar depurador**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Limitações de depuração remota no Azure

Do Azure SDK 2.3, a depuração remota tem as seguintes limitações:

* Com a depuração remota ativada, não é possível publicar um serviço em nuvem em que qualquer função tem mais do que 25 instâncias.
* O depurador utiliza as portas 30400 para 30424, 31400 para 31424 e 32400 para 32424. Se tentar usar qualquer uma destas portas, não será possível publicar o seu serviço e uma das seguintes mensagens de erro será apresentada no registo de atividades do Azure:

  * Erro ao validar o ficheiro. cscfg relativamente ao ficheiro. csdef.
    A porta reservado intervalo "range" para o ponto final Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector da função de "função" sobrepõe-se com uma porta já definida ou o intervalo.
  * Falha na atribuição. Tente novamente mais tarde, tente reduzir o tamanho da VM ou o número de instâncias de função ou experimente implementar noutra região.

## <a name="debugging-azure-virtual-machines"></a>Depuração de máquinas virtuais do Azure

Pode depurar programas que são executados em máquinas virtuais do Azure utilizando o Server Explorer no Visual Studio. Quando ativa a depuração remota numa máquina virtual do Azure, Azure instala a extensão de depuração remota na máquina virtual. Em seguida, pode anexar a processos na máquina virtual e depurar como faria normalmente.

> [!NOTE]
> Máquinas virtuais criadas através da pilha do Gestor de recursos do Azure pode ser depuradas remotamente com o Cloud Explorer no Visual Studio 2015. Para obter mais informações, consulte [gerir recursos do Azure com o Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Para depurar uma máquina virtual do Azure

1. No Server Explorer, expanda o nó de máquinas virtuais e selecione o nó da máquina virtual que deseja depurar.

2. Abra o menu de contexto e selecione **ativar a depuração**. Quando lhe for perguntado se tem a certeza se pretender ativar a depuração na máquina virtual, selecione **Sim**.

    Azure instala a extensão de depuração remota na máquina virtual para ativar a depuração.

    ![Máquina virtual ativar comando de depuração](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Registo de atividades do Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

3. Depois da extensão de depuração remota conclui a instalação, abra o menu de contexto da máquina virtual e selecione **anexar o depurador...**

    O Azure obtém uma lista dos processos na máquina virtual e mostra-los em Attach à caixa de diálogo de processo.

    ![Anexar o comando do depurador](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

4. Na **anexar a processo** caixa de diálogo, selecione **selecione** para limitar a lista de resultados para mostrar apenas os tipos de código que deseja depurar. Pode depurar código gerenciado de 32 bits ou 64 bits, o código nativo ou ambos.

    ![Selecione a caixa de diálogo do tipo de código](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

5. Selecione os processos que deseja depurar na máquina virtual e, em seguida, selecione **Attach**. Por exemplo, pode escolher o processo w3wp.exe, se quiser depurar uma aplicação web na máquina virtual. Ver [depurar um ou mais processos no Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) e [arquitetura de funções do Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) para obter mais informações.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Criar um projeto da web e uma máquina virtual para depuração

Antes de publicar seu projeto do Azure, poderá considerar útil para testá-la num ambiente controlado, que oferece suporte a cenários de teste e depuração e, em que pode instalar testando e monitorando programas. É uma forma de executar esses testes depurar remotamente o seu aplicativo numa máquina virtual.

Projetos do Visual Studio ASP.NET oferecem uma opção para criar uma máquina virtual útil que pode utilizar para teste de aplicações. A máquina virtual inclui pontos de extremidade mais necessários, tais como o PowerShell, o ambiente de trabalho remoto e o WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Para criar um projeto da web e uma máquina virtual para depuração

1. No Visual Studio, crie um novo aplicativo de Web do ASP.NET.

2. Na caixa de diálogo novo projeto ASP.NET, na secção do Azure, escolha **Máquina Virtual** na caixa de lista suspensa. Deixe o **criar recursos remotos** caixa de verificação selecionada. Selecione **OK** para continuar.

    O **criar máquina virtual no Azure** é apresentada a caixa de diálogo.

    ![Criar caixa de diálogo de projeto de web ASP.NET](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Nota:** deverá iniciar sessão na sua conta do Azure, se ainda não tiver iniciado sessão.

3. Selecione as várias configurações para a máquina virtual e, em seguida, selecione **OK**. Ver [máquinas virtuais](http://go.microsoft.com/fwlink/?LinkId=623033) para obter mais informações.

    O nome que introduzir para o nome DNS será o nome da máquina virtual.

    ![Criar a máquina virtual na caixa de diálogo do Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    O Azure cria a máquina virtual e, em seguida, Aprovisiona e configura os pontos de extremidade, como o ambiente de trabalho remoto e o Web Deploy

4. Depois da máquina virtual ser totalmente configurada, selecione o nó da máquina virtual no Explorador de servidores.

5. Abra o menu de contexto e selecione **ativar a depuração**. Quando lhe for perguntado se tem a certeza se pretender ativar a depuração na máquina virtual, selecione **Sim**.

    Azure instala a extensão de depuração remota para a máquina virtual para ativar a depuração.

    ![Máquina virtual ativar comando de depuração](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Registo de atividades do Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

6. Publicar o projeto conforme descrito na [como: implementar um projeto usando um só clique publicação na Web no Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Porque deseja depurar na máquina virtual, no **configurações** página do **publicar Web** assistente, selecione **depurar** como a configuração. Isso torna-se de que os símbolos de código estão disponíveis durante a depuração.

    ![Definições de publicação](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

7. Na **opções de publicação do ficheiro**, selecione **remover ficheiros adicionais no destino** se o projeto já foi implementado num horário mais cedo.

8. Depois do projeto publica, no menu de contexto da máquina virtual no Server Explorer, selecione **anexar o depurador...**

    O Azure obtém uma lista dos processos na máquina virtual e mostra-los em Attach à caixa de diálogo de processo.

    ![Anexar o comando do depurador](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

9. Na **anexar a processo** caixa de diálogo, selecione **selecione** para limitar a lista de resultados para mostrar apenas os tipos de código que deseja depurar. Pode depurar código gerenciado de 32 bits ou 64 bits, o código nativo ou ambos.

    ![Selecione a caixa de diálogo do tipo de código](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

10. Selecione os processos que deseja depurar na máquina virtual e, em seguida, selecione **Attach**. Por exemplo, pode escolher o processo w3wp.exe, se quiser depurar uma aplicação web na máquina virtual. Ver [depurar um ou mais processos no Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

* Uso **Intellitrace** para recolher um log de eventos e de chamadas a partir de um servidor de versão. Ver [depurar um serviço Cloud publicado com o IntelliTrace e o Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).

* Uso **diagnóstico do Azure** para registar informações detalhadas a execução de código em funções, independentemente das funções estão a ser executados no ambiente de desenvolvimento ou no Azure. Ver [recolher dados de registo com o diagnóstico do Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).
