---
title: Configuração personalizada para o tempo de execução de integração do Azure-SSIS | Microsoft Docs
description: Este artigo descreve como utilizar a interface de configuração personalizada para o tempo de execução de integração do Azure-SSIS
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: ed0f992985b49e4d84ca42a2e9d73121466a2c74
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="custom-setup-for-the-azure-ssis-integration-runtime"></a>Configuração personalizada para o tempo de execução de integração do Azure-SSIS

A interface de configuração personalizada para o tempo de execução de integração de SSIS do Azure permite-lhe alterar a predefinição operativo ambiente (por exemplo, para iniciar serviços do Windows adicionais) ou de configuração ou instale os componentes adicionais (por exemplo, assemblagens, controladores, ou extensões) em cada nó do seu IR. SSIS do Azure Em geral, fornece uma interface para adicionar os seus próprios passos de configuração durante o aprovisionamento ou a reconfiguração da sua IR. SSIS do Azure

Configure a sua configuração personalizada ao preparar um script e os ficheiros associados e carregá-los para um contentor de BLOBs na sua conta do Storage do Azure. Forneça uma assinatura de acesso partilhado (SAS) Uniform Resource Identifier (URI) para o contentor quando aprovisionar ou reconfigurar o IR. SSIS do Azure Cada nó do seu IR SSIS do Azure, em seguida, transfere o script e os ficheiros associados do seu contentor e executa o programa de configuração personalizado com privilégios elevados. Quando a configuração personalizada estiver concluída, cada nó carrega a saída padrão de execução e outros registos para o contentor.

Pode instalar componentes livres ou sem licença e pagos ou licenciados componentes. Se for um ISV, consulte [como desenvolver paga ou licenciados componentes para o Azure SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).


## <a name="current-limitations"></a>Limitações atuais

-   Não é possível diretamente utilizar scripts que chamam xcopy ou robocopy ou ferramentas semelhantes para copiar os ficheiros neste momento. Como solução, crie um `cmd` ficheiro que contém os scripts que chamam ferramentas xcopy ou robocopy (por exemplo, `install.cmd`) e chame este `cmd` em vez disso, o ficheiro. Por exemplo:

    ```
    start /wait cmd /c "call install.cmd > %CUSTOM\_SETUP\_SCRIPT\_LOG\_DIR%\\install.cmd.log"
    ```

-   Não é possível chamar diretamente `gacutil.exe` para instalar as assemblagens na Cache de assemblagem Global (GAC), neste momento. Como solução, utilize `gacinstall.cmd` (fornecido no contentor de pré-visualização pública).

## <a name="prerequisites"></a>Pré-requisitos

Para personalizar a sua resposta a incidentes SSIS do Azure, terá dos seguintes procedimentos:

-   [Subscrição do Azure](https://azure.microsoft.com/)

-   [Um servidor SQL Database do Azure ou de instância geridos](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Aprovisionar a resposta a incidentes SSIS do Azure](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Uma conta de armazenamento do Azure](https://azure.microsoft.com/services/storage/). Para a configuração personalizada, carregar e armazenar o script de configuração personalizada e os ficheiros associados num contentor de blob. O processo de configuração personalizada também carrega os seus registos de execução ao mesmo contentor de blob.

## <a name="instructions"></a>Instruções

2.  Transfira e instale [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) (versão 5.4 ou posterior).

3.  Prepare o script de configuração personalizada e os ficheiros associados (por exemplo, ficheiros. bat,. cmd, .exe,. dll,. msi ou. ps1).

    1.  Tem de ter um ficheiro de script com o nome `main.cmd`, que é o ponto de entrada do seu programa de configuração personalizado.

    2.  Se pretender que os registos adicionais gerados por outras ferramentas (por exemplo, `msiexec.exe`) para ser carregado para o contentor, especifique a variável de ambiente predefinidas, `CUSTOM_SETUP_SCRIPT_LOG_DIR` como a pasta de registo na sua scripts (por exemplo, `msiexec /i xxx.msi /quiet
        /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

4.  Transferir, instalar e iniciar [Explorador de armazenamento do Azure](http://storageexplorer.com/).

    1.  Em **(locais e anexadas)**, selecione de direito **contas do Storage** e selecione **ligar ao armazenamento do Azure**.

       ![Ligar ao armazenamento do Azure](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

    2.  Selecione **utilizar um nome de conta de armazenamento e a chave** e selecione **seguinte**.

       ![Utilizar o nome e a chave de uma conta de armazenamento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

    3.  Introduza o nome de conta do Storage do Azure e a chave, selecione **seguinte**e, em seguida, selecione **Connect**.

       ![Forneça o nome da conta de armazenamento e a chave](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

    4.  Na sua conta do Storage do Azure ligada, clique com botão direito no **contentores de BLOBs**, selecione **criar contentor de Blob**e atribua o nome do contentor de novo.

       ![Criar um contentor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

    5.  Selecione o novo contentor e carregar o script de configuração personalizada e os ficheiros associados. Certifique-se de que carregar `main.cmd` no nível superior do contentor, não se encontra em qualquer pasta. 

       ![Carregar ficheiros para o contentor de blob](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

    6.  O contentor com o botão direito e selecione **obter assinatura de acesso partilhado**.

       ![Obtenha a assinatura de acesso partilhado do contentor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

    7.  Criar o URI de SAS para o contentor com um período de tempo de expiração suficientemente longo e leitura + escrever + lista de permissões. É necessário o URI de SAS para transferir e executar o script de configuração personalizada e os ficheiros associados, sempre que qualquer nó do seu IR SSIS do Azure é recriada. É necessário escrever permissão para carregar os registos de execução do programa de configuração.

       ![Gerar a assinatura de acesso partilhado do contentor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

    8.  Copie e guarde o URI de SAS do seu contentor.

       ![Copie e guarde a assinatura de acesso partilhado](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

    9.  Quando aprovisionar ou reconfigurar a sua resposta a incidentes SSIS do Azure com o PowerShell, antes de iniciar a sua resposta a incidentes SSIS do Azure, execute o `Set-AzureRmDataFactoryV2IntegrationRuntime` cmdlet com o URI de SAS do seu contentor como o valor para o novo `SetupScriptContainerSasUri` parâmetro. Por exemplo:

       ```powershell
       Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName `
                                                  -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

       Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                    -Name $MyAzureSsisIrName `
                                                    -ResourceGroupName $MyResourceGroupName
       ```

    10.  Após a conclusão da sua configuração personalizada e inicia a sua resposta a incidentes SSIS do Azure, pode encontrar a saída padrão do `main.cmd` e outra execução nos registos de `main.cmd.log` na pasta do seu contentor de armazenamento.

2.  Para ver outros exemplos de configuração personalizada, ligue para o contentor de pré-visualização pública com o Explorador de armazenamento do Azure.

    a.  Em **(locais e anexadas)**, faça duplo clique **contas do Storage**, selecione **ligar ao armazenamento do Azure**, selecione **utilizar uma cadeia de ligação ou um acesso partilhado assinatura URI**e, em seguida, selecione **seguinte**.

       ![Ligar ao armazenamento do Azure com a assinatura de acesso partilhado](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

    b.  Selecione **utilizar um URI de SAS** e introduza o URI de SAS seguintes para o contentor de pré-visualização pública. Selecione **seguinte**e selecione **Connect**.

       `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

       ![Forneça a assinatura de acesso partilhado do contentor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

    c. Selecione o contentor de pré-visualização pública ligado e faça duplo clique o `CustomSetupScript` pasta. Nesta pasta são os seguintes itens:

       1. A `Sample` pasta, que contém uma configuração personalizada para uma tarefa básica de instalação em cada nó do seu IR. SSIS do Azure A tarefa não produz qualquer efeito mas suspensão por alguns segundos. Também contém a pasta `gacinstall.cmd` para substituir `gacutil.exe`.

       2. A `UserScenarios` pasta, que contém oito setups personalizados para cenários de utilizador reais.

    ![Conteúdo do contentor de pré-visualização pública](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

    d. Faça duplo clique o `UserScenarios` pasta. Nesta pasta são os seguintes itens:

       1. A `BCP` pasta, que contém uma configuração personalizada para instalar os utilitários da linha de comandos do SQL Server (`MsSqlCmdLnUtils.msi`), incluindo o programa de cópia em massa (`bcp`), em cada nó do seu IR. SSIS do Azure

       2. Um `EXCEL` pasta, que contém uma configuração personalizada para instalar as assemblagens de open source (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, e `ExcelDataReader.dll`) em cada nó do seu IR. SSIS do Azure

       3. Um `MSDTC` pasta, que contém uma configuração personalizada para ativar e iniciar o serviço de coordenador de transações distribuídas ' (MSDTC) da Microsoft em cada nó do seu IR. SSIS do Azure

       4. Um `ORACLE ENTERPRISE` pasta, que contém um script de configuração personalizada (`main.cmd`) e o ficheiro de configuração de instalação automática (`client.rsp`) para instalar o controlador do OCI do Oracle em cada nó do seu Azure SSIS IR Enterprise Edition (pré-visualização privada). Esta configuração permite-lhe utilizar o Gestor de ligações do Oracle, origem e destino. Em primeiro lugar, tem de transferir `winx64_12102_client.zip` de [Oracle](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) e, em seguida, carregá-la em conjunto com `main.cmd` e `client.rsp` para o contentor. Se utilizar TNS para ligar a Oracle, também terá de transferir `tnsnames.ora`, editá-lo e carregue-o para o contentor, para que possam ser copiado para a pasta de instalação do Oracle durante a configuração.

       5. Um `ORACLE STANDARD` pasta, que contém um script de configuração personalizada (`main.cmd`) para instalar o controlador de Oracle ODP.NET em cada nó do seu IR. SSIS do Azure Esta configuração permite-lhe utilizar o Gestor de ligações do ADO.NET, origem e destino. Em primeiro lugar, transferir `ODP.NET_Managed_ODAC122cR1.zip` de [Oracle](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)e, em seguida, carregá-la em conjunto com `main.cmd` para o contentor.

       6. Um `SAP BW` pasta, que contém um script de configuração personalizada (`main.cmd`) para instalar a assemblagem de conector SAP .NET (`librfc32.dll`) em cada nó do seu Azure SSIS IR Enterprise Edition (pré-visualização privada). Esta configuração permite-lhe utilizar o Gestor de ligações do SAP BW, origem e destino. Em primeiro lugar, carregar 64 bits ou a versão de 32 bits do `librfc32.dll` da pasta de instalação SAP para o contentor, em conjunto com `main.cmd`. O script, em seguida, copia a assemblagem SAP para o `%windir%\SysWow64` ou `%windir%\System32` pasta durante a configuração.

       7. A `STORAGE` pasta, que contém uma configuração personalizada para instalar o Azure PowerShell em cada nó do seu IR. SSIS do Azure Esta configuração permite-lhe implementar e executar SSIS pacotes que são executados [scripts do PowerShell para manipular a sua conta do Storage do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Cópia `main.cmd`, uma amostra `AzurePowerShell.msi` (ou instale a versão mais recente) e `storage.ps1` para o contentor. Utilize PowerShell.dtsx como um modelo para os pacotes. O modelo de pacote combina uma [a tarefa de transferir BLOBs do Azure](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), que transfere `storage.ps1` como um script de PowerShell modificável e um [executar tarefa de processo](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) que executa o script em cada nó.

       8. A `TERADATA` pasta, que contém um script de configuração personalizada (`main.cmd)`, o ficheiro associado (`install.cmd`) e os pacotes de instalador (`.msi`). Estes ficheiros instalar Teradata conectores, a API de TPT e o controlador ODBC em cada nó do seu Azure SSIS IR Enterprise Edition (pré-visualização privada). Esta configuração permite-lhe utilizar o Gestor de ligações de Teradata, origem e destino. Em primeiro lugar, transfira o ficheiro Teradata ferramentas e utilitários (TTU) 15.x zip (por exemplo, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) de [Teradata](http://partnerintelligence.teradata.com)e, em seguida, carregue-o em conjunto com o procedimento acima `.cmd` e `.msi` ficheiros para o contentor.

    ![Pastas na pasta de cenários de utilizador](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

    e. Para experimentar estes exemplos de configuração personalizada, copie e cole o conteúdo da pasta selecionada no seu contentor. Quando aprovisionar ou reconfigurar a sua resposta a incidentes SSIS do Azure com o PowerShell, execute o `Set-AzureRmDataFactoryV2IntegrationRuntime` cmdlet com o URI de SAS do seu contentor como o valor para o novo `SetupScriptContainerSasUri` parâmetro.

## <a name="next-steps"></a>Passos Seguintes

-   [Edição Enterprise do tempo de execução da integração do Azure-SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Como desenvolver paga ou está licenciado componentes personalizados para o tempo de execução de integração do Azure-SSIS](how-to-develop-azure-ssis-ir-licensed-components.md)