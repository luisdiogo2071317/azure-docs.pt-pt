---
title: Copiar dados para o Microsoft Azure Data Box através do SMB | Documentos da Microsoft
description: Saiba como copiar dados para o Azure Data Box através do serviço de cópia de dados
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: alkohli
ms.openlocfilehash: 9d271642a432d8a149fbe468087a0598c91e7c36
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902384"
---
# <a name="tutorial-use-data-copy-service-to-directly-ingest-data-into-azure-data-box-preview"></a>Tutorial: Utilizar o serviço de cópia de dados para diretamente ingerir dados no Azure Data Box (pré-visualização)

Este tutorial descreve como para ingestão de dados com o serviço de cópia de dados sem a necessidade de um anfitrião intermediário. O serviço de cópia de dados é executada localmente no Data Box, liga para o seu dispositivo NAS através de SMB e copia os dados para o Data Box.

Utilize o serviço de cópia de dados:

- Nos ambientes de armazenamento ligados à rede (NAS) em que os anfitriões intermédios podem não estar disponíveis.
- Com arquivos pequenos que levam semanas para ingestão e carregamento de dados. Este serviço melhora significativamente o tempo de carregamento e da ingestão.

Neste tutorial, ficará a saber mais sobre:

> [!div class="checklist"]
> * Pré-requisitos
> * Copiar dados para o Data Box


## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que:

1. Concluiu o [Tutorial: Configurar o Azure Data Box](data-box-deploy-set-up.md).
2. Que recebeu o Data Box e é o estado da encomenda no portal **entregues**.
3. Tem as credenciais de origem NAS dispositivo que ligar para cópia de dados.
4. Está ligado a uma rede de alta velocidade. Recomendamos vivamente que tenha, pelo menos, uma ligação de 10 GbE. Se uma ligação de 10 GbE não estiver disponível, utilize uma ligação de dados de 1 GbE, mas as velocidades de cópia serão afetadas.

## <a name="copy-data-to-data-box"></a>Copiar dados para o Data Box

Assim que estiver ligado ao NAS, a próxima etapa é copiar dados. Antes de iniciar a cópia de dados, reveja as seguintes considerações:

- Ao copiar dados, certifique-se de que o tamanho dos dados está em conformidade com os limites de tamanho descritos a [armazenamento do Azure e limites do Data Box](data-box-limits.md).
- Se os dados carregados por Data Box, é carregado em simultâneo por outras aplicações fora do Data Box, isto pode resultar no carregamento de Corrupção de dados e de falhas de tarefa.
- Se estão sendo formados os dados, como o serviço de cópia de dados é lê-lo, pode ver falhas ou corrupção de dados.

Para copiar dados com o serviço de cópia de dados, terá de criar uma tarefa. Siga estes passos para criar uma tarefa para copiar dados.

1. Na IU da web local de sua caixa de dados, aceda a **gerir > copiar dados**.
2. Na **copiar dados** página, clique em **criar**.

    ![Clique em * * criar * * em * * dados * * a página de cópia](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. Na **configurar e iniciar** diálogo caixa, forneça as seguintes entradas.
    
    |Campo                          |Valor    |
    |-------------------------------|---------|
    |Nome da tarefa                       |Um único nome de menos de 230 carateres para a tarefa. Estes carateres não são permitidos no nome da tarefa - \<, \>, \|, \?, \*, \\, \:, \/, e \\\.         |
    |Localização de origem                |Forneça o caminho SMB para a origem de dados no formato: `\\<ServerIPAddress>\<ShareName>` ou `\\<ServerName>\<ShareName>`.        |
    |Nome de utilizador                       |Nome de utilizador no `\\<DomainName><UserName>` formato para aceder à origem de dados.        |
    |Palavra-passe                       |Palavra-passe para aceder à origem de dados.           |
    |Conta de armazenamento de destino    |Selecione a conta de armazenamento de destino para carregar dados na lista pendente.         |
    |Tipo de armazenamento de destino       |Selecione o tipo de armazenamento de destino a partir do blob de blocos, BLOBs de páginas ou ficheiros do Azure.        |
    |Contentor/partilha de destino    |Introduza o nome do contentor ou partilhar para carregar dados na sua conta de armazenamento de destino. O nome pode ser um nome de partilha ou um nome de contentor. Por exemplo, `myshare` ou `mycontainer`. Também pode introduzir no formato `sharename\directory_name` ou `containername\virtual_directory_name` na cloud.        |
    |Copiar ficheiros correspondentes ao padrão    | Introduza o padrão de correspondência do nome de ficheiro das seguintes duas formas.<ul><li>**Utilize expressões com carateres universais** apenas `*` e `?` são suportados em expressões de caráter universal. Por exemplo, esta expressão `*.vhd` corresponde a todos os ficheiros que tenham a extensão. vhd. Da mesma forma, `*.dl?` corresponde a todos os ficheiros cuja extensão seja `.dl` ou `.dll`. Além disso, `*foo` irá corresponder a todos os arquivos cujos nomes terminam com `foo`.<br>Pode introduzir diretamente expressão com carateres universais no campo. Por predefinição, o valor inserido no campo é tratado como expressão com carateres universais.</li><li>**Usar expressões regulares** -baseadas em POSIX expressões regulares são suportadas. Por exemplo, uma expressão regular `.*\.vhd` irá corresponder a todos os ficheiros que tenham `.vhd` extensão. Para a expressão regular, forneça o `<pattern>` diretamente como `regex(<pattern>)`. <li>Para obter mais informações sobre expressões regulares, aceda a [linguagem de expressão Regular - uma referência rápida](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).</li><ul>|
    |Otimização de ficheiro              |Quando ativada, os ficheiros são incluídos inferior a 1 MB na ingestão. Isso acelera a cópia de dados para ficheiros pequenos. Economia de tempo significativa é vista quando o número de ficheiros muito excede o número de diretórios.        |
 
4. Clique em **Iniciar**. As entradas são validadas e se a validação for bem-sucedida, em seguida, uma tarefa é iniciada. Pode demorar alguns minutos para que a tarefa iniciar.

    ![Iniciar uma tarefa da tarefa de configurar e iniciar a caixa de diálogo](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. É criada uma tarefa com as definições especificadas. Selecione a caixa de verificação e, em seguida, pode colocar em pausa e retomar, cancelar ou reiniciar uma tarefa.

    ![Gerir uma tarefa por meio da página de dados de cópia](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Pode interromper esta tarefa se está a afetar os recursos NAS durante as horas de pico.

        ![Colocar em pausa uma tarefa](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Pode retomar a tarefa mais tarde durante o pico off horas.

        ![Retomar uma tarefa](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Pode cancelar uma tarefa em qualquer altura.

        ![Cancelar uma tarefa](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png) uma confirmação é necessária quando cancelar uma tarefa.

        ![Confirmar cancelamento da tarefa](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Se optar por cancelar uma tarefa, os dados copiados já não são eliminados. Para eliminar todos os dados que tenha copiado em sua caixa de dados, repor o dispositivo.

        ![Repor dispositivo](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Se cancela ou interromper um trabalho, podem ser deixados grandes ficheiros que estão a ser copiados copiados de metade. Estes ficheiros são carregados no mesmo Estado para o Azure. Ao tentar cancelar ou colocar em pausa, validar os ficheiros corretamente são copiados. Para validar os ficheiros, observar as partilhas SMB ou transfira o ficheiro BOM.

    - Pode reiniciar uma tarefa que ela falhe abruptamente devido a um erro transitório, como uma falha de rede. Não é possível reiniciar uma tarefa, se ele alcançou um Estado terminal como foi concluída com êxito ou foi concluída com erros. As falhas podem ser devido a problemas de tamanho de ficheiro ou de nomes de arquivos. Estes erros são registados, mas não é possível reiniciar a tarefa depois de concluída a ele.

        ![Reiniciar uma tarefa falhada](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Se ocorrer uma falha e não é possível reiniciar a tarefa, transfira os registos de erros e examinar a falha nos ficheiros de registo. Depois de ter corrigido o problema, pode criar uma nova tarefa para copiar os ficheiros. Também pode [copie os ficheiros através de SMB](data-box-deploy-copy-data.md).
    
    - Nesta versão, não é possível eliminar uma tarefa.
    
    - Pode criar tarefas ilimitadas mas executar um máximo de 10 tarefas em paralelo num dado momento.
    - Se a otimização de ficheiros está ativado, os arquivos pequenos são incluídos na ingestão para melhorar o desempenho de cópia. Nesses casos, verá um arquivo compactado (GUID como nome). Não elimine este ficheiro, este ficheiro será descompactado durante o carregamento.

6. Enquanto a tarefa está em curso, a ser o **copiar dados** página:

    - Na **estado** coluna, pode ver o estado da tarefa de cópia. O estado pode ser:
        - **Em execução**
        - **Falhou**
        - **Foi efetuada com êxito**
        - **Colocar em pausa**
        - **Em pausa**
        - **A cancelar**
        - **Foi cancelada**
        - **Concluído com erros**
    - Na **ficheiros** coluna, pode ver o número e o tamanho do total de ficheiros que está a ser copiados.
    - Na **processados** coluna, pode ver o número e o tamanho dos ficheiros que são processados.
    - Na **detalhes** coluna, clique em **vista** para ver os detalhes da tarefa.
    - Se tem erros durante o processo de cópia, como mostra a **# erros** coluna, aceda a **registo de erros** coluna e a transferência para resolução de problemas nos registos de erros.

Aguarde que as tarefas de cópia concluir. Como alguns erros só são registados no **Connect e a cópia** página, certifique-se de que as tarefas de cópia tem concluído sem erros antes de ir para o passo seguinte.

![Não existem erros em * * página ligar e copiar * *](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Para garantir a integridade dos dados, a soma de verificação é calculada inline à medida que os dados são copiados. Quando a cópia estiver concluída, verifique o espaço utilizado e o espaço livre no seu dispositivo.
    
![Verificar o espaço livre e utilizado no dashboard](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Quando a tarefa de cópia for concluída, pode aceder à **preparação para envio**.

>[!NOTE]
> Preparação para envio não é possível executar enquanto as tarefas de cópia estão em curso.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box, como:

> [!div class="checklist"]
> * Pré-requisitos
> * Copiar dados para o Data Box


Avance para o próximo tutorial para saber como enviar o Data Box para a Microsoft.

> [!div class="nextstepaction"]
> [Enviar o Azure Data Box para a Microsoft](./data-box-deploy-picked-up.md)

