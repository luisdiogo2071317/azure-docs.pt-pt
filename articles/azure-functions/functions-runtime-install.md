---
title: "Instalação de tempo de execução das funções do Azure | Microsoft Docs"
description: "Como instalar a pré-visualização de Runtime de funções do Azure 2"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: f8ce27bf28f73818932f2ac9056d4fdd573679e8
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Instalar a pré-visualização de Runtime de funções do Azure 2

Se gostaria de instalar a pré-visualização de Runtime de funções do Azure 2, siga estes passos:

1. Certifique-se de que a máquina transmite os requisitos mínimos.
1. Transferir o [instalador de pré-visualização do tempo de execução das funções do Azure](https://aka.ms/azafrv2).
1. Desinstale a pré-visualização de tempo de execução de funções de Azure 1.
1. Instale a pré-visualização de Runtime de funções do Azure 2.
1. Conclua a configuração do tempo de execução de funções de Azure preview 2.
1. Criar a sua primeira função em pré-visualização de tempo de execução de funções do Azure

## <a name="prerequisites"></a>Pré-requisitos

Antes de instalar a pré-visualização de Runtime de funções do Azure, tem de ter os seguintes recursos disponíveis:

1. Uma máquina com o Microsoft Windows Server 2016 ou o Microsoft Windows 10 criadores Update (Professional ou Enterprise Edition).
1. Uma instância do SQL Server em execução dentro da sua rede.  Edição mínima necessária é o SQL Server Express.

## <a name="uninstall-previous-version"></a>Desinstale a versão anterior

Se tiver instalado anteriormente a pré-visualização de Runtime de funções do Azure, terá de desinstalar antes de instalar a versão mais recente.  Desinstale a pré-visualização de Runtime de funções do Azure ao remover o programa em Adicionar/remover programas no Windows.

## <a name="install-the-azure-functions-runtime-preview"></a>Instalar a pré-visualização de tempo de execução de funções do Azure

O instalador de pré-visualização do tempo de execução de funções do Azure orienta-o a instalação da pré-visualização do tempo de execução de funções de Azure gestão e as funções de trabalho.  É possível instalar a função de gestão e de trabalho no mesmo computador.  No entanto, à medida que adiciona mais aplicações de função, tem de implementar mais funções de trabalho em máquinas adicionais para poder dimensionar as suas funções em vários workers.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Instalar a função de trabalho e de gestão no mesmo computador

1. Execute o instalador de pré-visualização do tempo de execução de funções do Azure.

    ![Instalador de pré-visualização de tempo de execução de funções do Azure][1]

1. Clique em **Seguinte**.
1. Depois de ter leia os termos do **EULA**, **a caixa de verificação** para aceitar os termos de licenciamento e clique em **seguinte** para avançar.
1. Selecione as funções que pretende instalar neste computador **função de gestão de funções** e/ou **função de trabalho de funções** e clique em **seguinte**.

    ![Instalador de pré-visualização de tempo de execução de funções do Azure - seleção da função][3]

    > [!NOTE]
    > Pode instalar o **função de trabalho de funções** em muitas outras máquinas. Para tal, siga estas instruções e selecionar apenas **função de trabalho de funções** no instalador do.

1. Clique em **seguinte** ter o **Assistente de configuração de tempo de execução de funções do Azure** iniciar o processo de instalação no seu computador.
1. Depois de concluído, o Assistente de configuração inicia o **Runtime de funções do Azure** ferramenta de configuração.

    ![Instalador de pré-visualização de tempo de execução de funções do Azure concluir][6]

    > [!NOTE]
    > Se estiver a instalar num **Windows 10** e **contentor** funcionalidade não foi ativada anteriormente, o **configuração de tempo de execução de funções do Azure** pede-lhe reiniciar a máquina para concluir a instalação.

## <a name="configure-the-azure-functions-runtime"></a>Configurar o tempo de execução de funções do Azure

Para concluir a instalação de Runtime de funções do Azure, tem de concluir a configuração.

1. O **Runtime de funções do Azure** ferramenta de configuração mostra que funções são instaladas no seu computador.

    ![Ferramenta de configuração da pré-visualização de tempo de execução de funções do Azure][7]

1. Clique em de **base de dados** separador, introduza os detalhes de ligação para a instância do SQL Server, incluindo a especificação de um [chave mestra de base de dados](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine)e clique em **aplicar**.  Conectividade a uma instância do SQL Server é necessário para o tempo de execução de funções do Azure criar uma base de dados para suportar o tempo de execução.

    ![Configuração de base de dados de pré-visualização de tempo de execução de funções do Azure][8]

1. Clique em de **credenciais** separador.  Aqui, tem de criar duas novas credenciais para utilização com uma partilha de ficheiros para o alojamento de todas as suas aplicações de função.  Especifique **nome de utilizador** e **palavra-passe** combinações para o **proprietário da partilha de ficheiros** e para o **utilizador de partilha de ficheiros**, em seguida, clique em **Aplicar**.

    ![Credenciais de pré-visualização de tempo de execução de funções do Azure][9]

1. Clique em de **partilha de ficheiros** separador.  Aqui tem de especificar os detalhes da localização de partilha de ficheiros.  A partilha de ficheiros pode ser criada por si ou pode utilizar uma partilha de ficheiros existente e clique em **aplicar**.  Se selecionar uma nova localização de partilha de ficheiros, tem de especificar um diretório para utilização pelo Runtime de funções do Azure.

    ![Partilha de ficheiros de pré-visualização de tempo de execução de funções do Azure][10]

1. Clique em de **IIS** separador.  Este separador mostra os detalhes dos sites no IIS, que cria a ferramenta de configuração do Runtime de funções do Azure.  Pode especificar um DNS nome personalizado aqui para o portal de pré-visualização de Runtime de funções do Azure.  Clique em **aplicar** para concluir.

    ![Azure funções tempo de execução pré-visualização do IIS][11]

1. Clique em de **serviços** separador.  Este separador mostra o estado dos serviços na sua ferramenta de configuração do Runtime de funções do Azure.  Se o **serviço de ativação de anfitrião de funções do Azure** é não em execução após a configuração inicial, clique em **iniciar serviço**.

    ![Configuração do Azure funções tempo de execução pré-visualização concluída][12]

1. Navegue para o **Runtime Portal das funções do Azure** como `https://<machinename>.<domain>/`.

    ![Portal de pré-visualização de tempo de execução de funções do Azure][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Criar a sua primeira função em pré-visualização de Runtime de funções do Azure

Para criar a sua primeira função em pré-visualização de Runtime de funções do Azure

1. Navegue para o **Runtime Portal das funções do Azure** como https://<machinename>.<domain> Por exemplo https://mycomputer.mydomain.com
1. Lhe for pedido para **início de sessão**, se implementada uma utilização de domínio, o nome de utilizador de conta de domínio e palavra-passe, caso contrário, utilizam o seu nome de utilizador da conta local e a palavra-passe para iniciar sessão no portal.

![Azure funções tempo de execução pré-visualização portal início de sessão][14]

1. Para criar aplicações de função, tem de criar uma subscrição.  No canto superior esquerdo do portal, clique em de  **+**  opção junto as subscrições

![As funções tempo de execução pré-visualização portais as subscrições do Azure][15]

1. Escolha **DefaultPlan**, introduza um nome para a sua subscrição e clique em **criar**.

![Plano de subscrição de portal de pré-visualização de tempo de execução de funções do Azure e o nome][16]

1. Todas as suas aplicações de função estão listadas no painel da esquerda do portal.  Para criar uma nova aplicação de função, selecione o título **aplicações de função** e clique em de  **+**  opção.

1. Introduza um nome para a sua aplicação de função, selecione a subscrição correta, escolher qual a versão do tempo de execução das funções do Azure que pretende programa contra e clique em **criar**

![Azure funções tempo de execução pré-visualização portal nova aplicação de função][17]

1. A nova aplicação de função é listada no painel da esquerda do portal.  Selecione as funções e, em seguida, clique em **nova função** na parte superior do painel no portal do System center.

![Modelos de pré-visualização de tempo de execução de funções do Azure][18]

1. Selecione a função de Acionador de temporizador, o direito flyout designar a função e alterar a agenda para `*/5 * * * * *` (esta expressão de cron faz com que a sua função de temporizador a cada cinco segundos de execução) e clique em **criar**

![Configuração do Azure funções tempo de execução pré-visualização novo temporizador função][19]

1. A função agora foi criada.  Pode ver o registo de execução da sua aplicação de função, expandindo o **registo** painel na parte inferior do portal.

![Execução de função de pré-visualização de tempo de execução de funções do Azure][20]

<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-Progress.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer6-InstallComplete.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[13]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
[14]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Login.png
[15]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions.png
[16]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions1.png
[17]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewFunctionApp.png
[18]: ./media/functions-runtime-install/AzureFunctionsRuntime_v1FunctionsTemplates.png
[19]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewTimerFunction.png
[20]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_RunningV2Function.png
