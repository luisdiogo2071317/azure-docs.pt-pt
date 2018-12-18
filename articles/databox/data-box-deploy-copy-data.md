---
title: Copiar dados para o Microsoft Azure Data Box através do SMB | Documentos da Microsoft
description: Saiba como copiar dados para o Azure Data Box através de SMB
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 11/20/2018
ms.author: alkohli
ms.openlocfilehash: e5219a0ade610a41d316970aecda06d4020b37f2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53546186"
---
# <a name="tutorial-copy-data-to-azure-data-box-via-smb"></a>Tutorial: Copiar dados para o Azure Data Box através de SMB

Este tutorial descreve como ligar e copiar dados do computador anfitrião através da IU Web local e preparar para enviar o Data Box.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ligar ao Data Box
> * Copiar dados para o Data Box
> * Preparar para enviar o Data Box.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que:

1. Concluiu o [Tutorial: Configurar o Azure Data Box](data-box-deploy-set-up.md).
2. Recebeu o seu Data Box e o estado da encomenda no portal é **Entregue**.
3. Tem um computador anfitrião com os dados que pretende copiar para o Data Box. O computador anfitrião tem de
    - Executar um [sistema operativo suportado](data-box-system-requirements.md).
    - Estar ligado a uma rede de alta velocidade. Recomendamos vivamente que tenha, pelo menos, uma ligação de 10 GbE. Se uma ligação de 10 GbE não estiver disponível, pode ser utilizada uma ligação de dados de 1 GbE, mas as velocidades de cópia serão afetadas. 

## <a name="connect-to-data-box"></a>Ligar ao Data Box

Com base na conta de armazenamento selecionada, o Data Box cria até:
- Três partilhas para cada conta de armazenamento associada de GPv1 e GPv2.
- Uma partilha para uma conta de armazenamento de blobs ou premium. 

Nas partilhas de blob de blocos e de blob de páginas, as entidades de primeiro nível são os contentores e as de segundo nível são os blobs. Nas partilhas de ficheiros do Azure, as entidades de primeiro nível são as partilhas e as de segundo nível são os ficheiros.

Considere o seguinte exemplo. 

- Conta de armazenamento: *Mystoracct*
- Partilha para o blob de bloco: *Mystoracct_BlockBlob/my-container/blob*
- Partilha para o blob de página: *Mystoracct_PageBlob/my-container/blob*
- Partilha de ficheiros: *Mystoracct_AzFile/my-partilhar*

Se estiver a utilizar um computador anfitrião do Windows Server, execute os seguintes passos para ligar ao Data Box.

1. O primeiro passo é autenticar e iniciar uma sessão. Aceda a **Ligar e copiar**. Clique em **Obter credenciais** para obter as credenciais de acesso para as partilhas associadas à sua conta de armazenamento. 

    ![Obter credenciais de partilhas 1](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. Na caixa de diálogo Aceder à partilha e copiar os dados, copie o **Nome de utilizador** e a **Palavra-passe** correspondentes à partilha. Clique em **OK**.
    
    ![Obter credenciais de partilhas 1](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Aceda às partilhas associadas com a sua conta de armazenamento (Mystoracct no exemplo a seguir). Utilize o caminho `\\<IP of the device>\ShareName` para aceder às partilhas. Consoante o formato de dados, ligue às partilhas (utilize o nome da partilha) no seguinte endereço: 
    - *\\<IP address of the device>\Mystoracct_Blob*
    - *\\<IP address of the device>\Mystoracct_Page*
    - *\\<IP address of the device>\Mystoracct_AzFile*
    
    Para ligar às partilhas a partir do computador anfitrião, abra uma janela de comando. Na linha de comandos, escreva:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Introduza a palavra-passe da partilha quando lhe for pedido. O exemplo seguinte mostra a ligação a uma partilha através do comando anterior.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Prima Windows + R. Na janela **Executar**, especifique o `\\<device IP address>`. Clique em **OK**. Esta ação abre o Explorador de Ficheiros. Agora deve conseguir ver as partilhas como pastas.
    
    ![Ligar à partilha através do Explorador de Ficheiros 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

5.  **Crie sempre uma pasta para os ficheiros que pretende copiar na partilha e, em seguida, copie os ficheiros para essa pasta**. Ocasionalmente, as pastas podem mostrar uma cruz cinzenta. A cruz não denota uma condição de erro. As pastas são sinalizadas pela aplicação para controlar o estado.
    
    ![Ligar à partilha através do Explorador de Ficheiros 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) ![Ligar à partilha através do Explorador de Ficheiros 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) 

## <a name="copy-data-to-data-box"></a>Copiar dados para o Data Box

Assim que estiver ligado às partilhas do Data Box, o passo seguinte é copiar os dados. Antes da cópia de dados, certifique-se de que revê as seguintes considerações:

- Certifique-se de que copia os dados para partilhas que correspondem ao formato de dados apropriado. Por exemplo, copie os dados de blobs de blocos para a partilha de blobs de blocos. Se o formato de dados não corresponder à partilha apropriada (tipo de armazenamento), num passo posterior, o carregamento de dados para o Azure falhará.
-  Ao copiar os dados, certifique-se de que o respetivo tamanho está em conformidade com os limites descritos em [Limites de armazenamento do Azure e do Data Box](data-box-limits.md). 
- Se os dados, que estão a ser carregados pelo Data Box, forem carregados em simultâneo por outras aplicações fora do Data Box, isto pode resultar em falhas da tarefa de carregamento e danos nos dados.
- Recomendamos que não utilize as opções SMB e NFS em simultâneo nem copie os mesmos dados para o mesmo destino final no Azure. Nesses casos, não é possível determinar o resultado final.

Depois de ligar à partilha SMB, inicie uma cópia de dados. 

Pode utilizar qualquer ferramenta de cópia de ficheiros compatível com SMB, como o Robocopy, para copiar os dados. É possível iniciar várias tarefas com o Robocopy. Utilize o seguinte comando:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 Os atributos são descritos na tabela seguinte.
    
|Atributo  |Descrição  |
|---------|---------|
|/e     |Copia os subdiretórios, incluindo diretórios vazios.         |
|/r:     |Especifica o número de repetições nas cópias falhadas.         |
|/w:     |Especifica o tempo de espera entre as repetições, em segundos.         |
|/is     |Inclui os mesmos ficheiros.         |
|/nfl     |Especifica que os nomes de ficheiro não se destinam a ser registados.         |
|/ndl    |Especifica que os nomes de diretório não se destinam a ser registados.        |
|/np     |Especifica que o progresso da operação de cópia (o número de ficheiros ou diretórios copiados até ao momento) não será apresentado. A apresentação do progresso reduz significativamente o desempenho.         |
|/MT     | Utilize multithreading (são recomendados 32 ou 64 threads). Esta opção não é utilizada com ficheiros encriptados. Pode ter de separar os ficheiros encriptados e não encriptados. No entanto, uma cópia de thread único reduz significativamente o desempenho.           |
|/fft     | Utilize para reduzir a granularidade de carimbo de data/hora para qualquer sistema de ficheiros.        |
|/b     | Copia os ficheiros no modo de Cópia de Segurança.        |
|/z    | Copia os ficheiros no modo de Reinício. Utilize se o ambiente for instável. Esta opção reduz o débito devido ao registo adicional.      |
| /zb     | Utiliza o modo de Reinício. Se o acesso for negado, esta opção utiliza o modo de Cópia de Segurança. Esta opção reduz o débito devido ao ponto de verificação.         |
|/efsraw     | Copia todos os ficheiros encriptados no modo EFS não processado. Utilize apenas com ficheiros encriptados.         |
|log+:<LogFile>| Anexa a saída ao ficheiro de registo existente.|    
 
O exemplo seguinte mostra a saída do comando do Robocopy para copiar ficheiros para o Data Box.
    
    C:\Users>robocopy
        -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:53 PM
            Simple Usage :: ROBOCOPY source destination /MIR
    
                    source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                    /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:32
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
            Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
        Options : *.* /DCOPY:DA /COPY:DAT /MT:32 /R:5 /W:60
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                    Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
        Files :        17        17         0         0         0         0
        Bytes :     3.9 k     3.9 k         0         0         0         0          
    C:\Users>
       

Para otimizar o desempenho, utilize os seguintes parâmetros do Robocopy ao copiar os dados.

|    Plataforma    |    Principalmente ficheiros pequenos < 512 KB                           |    Principalmente ficheiros médios 512 KB - 1 MB                      |    Principalmente ficheiros grandes > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 sessões do Robocopy <br> 16 threads por sessões    |    3 sessões do Robocopy <br> 16 threads por sessões    |    2 sessões do Robocopy <br> 24 threads por sessões    |  |


Para obter mais informações sobre o comando do Robocopy, aceda a [Robocopy e alguns exemplos](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

Abra a pasta de destino para ver e verificar os ficheiros copiados. Se ocorrerem erros durante o processo de cópia, transfira os ficheiros de erro para resolução de problemas.

Para garantir a integridade dos dados, a soma de verificação é calculada inline à medida que os dados são copiados. Quando a cópia estiver concluída, verifique o espaço utilizado e o espaço livre no seu dispositivo.
    
   ![Verificar o espaço livre e utilizado no dashboard](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="prepare-to-ship"></a>Preparar para enviar

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box, como:

> [!div class="checklist"]
> * Ligar ao Data Box
> * Copiar dados para o Data Box
> * Preparar para enviar o Data Box

Avance para o próximo tutorial para saber como enviar o Data Box para a Microsoft.

> [!div class="nextstepaction"]
> [Enviar o Azure Data Box para a Microsoft](./data-box-deploy-picked-up.md)

