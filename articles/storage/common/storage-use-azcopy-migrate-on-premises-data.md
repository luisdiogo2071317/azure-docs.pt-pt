---
title: Migrar os dados no local ao Storage do Azure utilizando o AzCopy | Microsoft Docs
description: "Utilize o AzCopy para migrar dados nem copiar dados de ou para o blob, tabela e o conteúdo do ficheiro. Migre facilmente os dados do seu armazenamento local para o armazenamento do Azure."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: 3dbfb935ac0b134e127a5dccb7bc76716c688e8a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Migrar os dados no local para armazenamento na nuvem utilizando o AzCopy

O AzCopy é uma ferramenta da linha de comandos para copiar dados para ou do Blob storage do Azure, ficheiros do Azure e Table storage do Azure, utilizando os comandos simples. Os comandos foram concebidos para um desempenho ideal. Pode copiar dados entre um sistema de ficheiros e uma conta de armazenamento, ou entre contas de armazenamento.  

Pode transferir duas versões do AzCopy:

* [AzCopy no Linux](storage-use-azcopy.md) baseia-se com o .NET Core Framework. -Destina-se plataformas Linux por opções da linha de comandos do estilo POSIX da oferta. 
* [AzCopy no Windows](../storage-use-azcopy.md) baseia-se com o .NET Framework. Oferece opções da linha de comandos do Windows-estilo. 
 
Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma conta de armazenamento. 
> * Utilize o AzCopy para carregar todos os seus dados.
> * Modificar os dados para fins de teste.
> * Crie uma tarefa agendada de tarefas ou cron para identificar os novos ficheiros ao carregar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, transfira a versão mais recente do AzCopy no [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) ou [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Se pretender ser capaz de transferir blobs a partir de uma região secundária para o armazenamento local e vice-versa, defina **replicação** para **leitura-acesso-armazenamento georredundante**. A seleção desta opção cria um [armazenamento georredundante](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) conta. 
>
>

## <a name="create-a-container"></a>Criar um contentor

Os Blobs são sempre carregados para um contentor. Pode utilizar contentores para organizar grupos de blobs, tal como organizar os ficheiros no seu computador em pastas. 

Siga estes passos para criar um contentor:

1. Selecione o **contas do Storage** botão da página principal e selecione a conta de armazenamento que criou.
2. Selecione **Blobs** em **serviços**e, em seguida, selecione **contentor**. 

   ![Criar um contentor](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Os nomes de contentor tem de começar com uma letra ou número. Estes podem conter apenas letras, números e o caráter de hífen (-). Para ter acesso a outras regras sobre como atribuir nomes a blobs e contentores, consulte [Atribuir nomes e referenciar contentores, blobs e metadados](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Carregar todos os ficheiros numa pasta para o Blob storage

Pode utilizar o AzCopy para carregar todos os ficheiros numa pasta para o Blob storage em [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) ou [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Para carregar todos os blobs numa pasta, introduza o seguinte comando do AzCopy:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Substitua `<key>` e `key` com a sua chave de conta. No portal do Azure, pode obter a sua chave de conta selecionando **chaves de acesso** em **definições** na sua conta de armazenamento. Selecione uma chave e cole o comando do AzCopy. Se o contentor de destino especificado não existir, o AzCopy criá-lo e carrega o ficheiro para a mesma. O caminho de origem de atualização para o diretório de dados e substitua **myaccount** no URL de destino com o nome da sua conta de armazenamento.

Para carregar o conteúdo do diretório especificado para o modo recursivo de armazenamento de BLOBs, especifique o `--recursive` (Linux) ou `/S` opção (Windows). Quando executar o AzCopy com uma destas opções, todas as subpastas e os ficheiros são carregados bem.

## <a name="upload-modified-files-to-blob-storage"></a>Carregar ficheiros modificados para o Blob storage
Pode utilizar o AzCopy para [carregar ficheiros](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) com base na respetiva hora de última modificação. Experimente este, modificar ou criar novos ficheiros no seu diretório de origem para fins de teste. Para carregar ficheiros apenas novos ou atualizados, adicione o `--exclude-older` (Linux) ou `/XO` parâmetro (Windows) para o comando do AzCopy.

Se pretender apenas copiar recursos de origem que não existe no destino, especifique ambos `--exclude-older` e `--exclude-newer` (Linux) ou `/XO` e `/XN` parâmetros (Windows), o comando do AzCopy. AzCopy carrega apenas os dados atualizados, com base no respetivo carimbo de data / hora.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Criar uma tarefa agendada de tarefas ou cron 
Pode criar uma tarefa agendada ou tarefa de cron que executa um script de comandos do AzCopy. O script identifica e carrega novos dados no local para armazenamento na nuvem num intervalo de tempo específico. 

Copie o comando do AzCopy para um editor de texto. Atualize os valores de parâmetro do comando do AzCopy para os valores adequados. Guarde o ficheiro como `script.sh` (Linux) ou `script.bat` (Windows) para AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy é executado com o verboso `--verbose` (Linux) ou `/V` opção (Windows). O resultado é redirecionado para um ficheiro de registo. 

Neste tutorial, [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) é utilizado para criar uma tarefa agendada no Windows. O [Crontab](http://crontab.org/) comando é utilizado para criar uma tarefa de cron no Linux. 
 **SCHTASKS** permite que um administrador criar, eliminar, consultar, alterar, executar e terminar as tarefas agendadas no computador local ou remoto. **Cron** permite aos utilizadores de Linux e Unix executar comandos ou scripts numa data e hora especificadas utilizando [expressões de cron](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Para criar uma tarefa de cron no Linux, introduza o seguinte comando num terminal: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Especificar a expressão de cron `*/5 * * * * ` no comando indica que o script de shell `script.sh` deve ser executada a cada cinco minutos. Pode agendar o script a executar num momento específico diariamente, mensais ou anuais. Para obter mais informações sobre como definir a data e hora de execução da tarefa, consulte [expressões de cron](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Para criar uma tarefa agendada no Windows, introduza o seguinte comando numa linha de comandos ou no PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Utiliza o comando:
- O `/SC` parâmetro para especificar uma agenda minuto.
- O `/MO` parâmetro para especificar um intervalo de cinco minutos.
- O `/TN` parâmetro para especificar o nome da tarefa.
- O `/TR` parâmetro para especificar o caminho para o `script.bat` ficheiro. 

Para obter mais informações sobre como criar uma tarefa agendada no Windows, consulte [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Para validar que a tarefa de tarefa/cron agendada é executada corretamente, criar novos ficheiros na sua `myfolder` diretório. Aguarde cinco minutos para confirmar que os novos ficheiros tiverem sido carregados para a sua conta de armazenamento. Aceda ao seu diretório de registo para ver registos de saída da tarefa agendada ou tarefa de cron. 

Para obter mais informações sobre formas de mover os dados no local ao Storage do Azure e vice-versa, consulte o artigo [mover dados para e do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o Storage do Azure e AzCopy, consulte os seguintes recursos:

* [Introdução ao Storage do Azure](../storage-introduction.md)
* [Transferência de dados com o AzCopy no Windows](storage-use-azcopy.md) 
* [Transferência de dados com o AzCopy no Linux](storage-use-azcopy-linux.md) 
* [Criar uma conta de armazenamento](../storage-create-storage-account.md)
* [Gerir a blobs com o Explorador de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

