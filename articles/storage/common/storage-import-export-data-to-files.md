---
title: Importar/exportar do Azure a utilizar para transferir dados para ficheiros do Azure | Microsoft Docs
description: Saiba como criar tarefas de importação no portal do Azure para transferir dados para ficheiros do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 4349b471f960e7844511c473bffcd2177a34e055
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661021"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Utilize o serviço importar/exportar do Azure para importar dados para ficheiros do Azure

Este artigo fornece instruções passo a passo sobre como utilizar o serviço importar/exportar do Azure para importar com segurança grandes quantidades de dados para ficheiros do Azure. Para importar dados, o serviço requer que são enviados suportadas unidades de disco que contém os dados para um datacenter do Azure.  

O suporta de serviço de importação/exportação importa apenas dos ficheiros do Azure para armazenamento do Azure. Não é suportada a exportar os ficheiros do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Antes de criar uma tarefa de importação para transferir dados para ficheiros do Azure, cuidadosamente reveja e conclua a lista seguinte de pré-requisitos. Tem de:

- Ter uma subscrição do Azure Active Directory para utilizar com o serviço de importação/exportação.
- Ter, pelo menos, uma conta de armazenamento do Azure. Ver a lista de [suportadas contas de armazenamento e os tipos de armazenamento para o serviço de importação/exportação](storage-import-export-requirements.md). Para obter informações sobre como criar uma nova conta de armazenamento, consulte [como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account).
- Ter um número suficiente de discos de [tipos suportados](storage-import-export-requirements.md#supported-disks). 
- Tem um sistema do Windows em execução um [versão do SO suportado](storage-import-export-requirements.md#supported-operating-systems).
- [Transferir o versão 2 do WAImportExport](https://www.microsoft.com/download/details.aspx?id=55280) no sistema Windows. Deszipe para a pasta predefinida `waimportexport`. Por exemplo, `C:\WaImportExport`.


## <a name="step-1-prepare-the-drives"></a>Passo 1: Preparar as unidades

Este passo gera um ficheiro do diário de alterações. O ficheiro de diário armazena informações básicas, tais como o número de série da unidade, chave de encriptação e detalhes da conta de armazenamento.

Execute os seguintes passos para preparar as unidades.

1. Ligar o nosso unidades de disco para o sistema do Windows através de conectores SATA.
2. Crie um único volume NTFS em cada unidade. Atribua uma letra de unidade ao volume. Não utilize mountpoints.
3. Modificar o *dataset.csv* ficheiro na pasta raiz onde a ferramenta reside. Consoante pretenda importar um ficheiro ou pasta ou ambos, adicionar as entradas no *dataset.csv* ficheiro semelhante para os exemplos seguintes.  

    - **Para importar um ficheiro**: no exemplo seguinte, os dados ao copiar residem na unidade c:. O ficheiro *MyFile1.txt* é copiado para a raiz do *MyAzureFileshare1*. Se o *MyAzureFileshare1* não existir, é criado na conta do Storage do Azure. Estrutura de pastas é mantida.

        ```
            BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
        ```
    - **Para importar uma pasta**: todos os ficheiros e pastas sob *MyFolder2* são recursivamente copiado para a partilha de ficheiros. Estrutura de pastas é mantida.

        ```
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
        ```
    Existem várias entradas podem ser efectuadas no mesmo ficheiro correspondente a pastas ou ficheiros que são importados. 

        ```
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            "F:\MyFolder3\MyFile3.txt","MyAzureFileshare2/",file,rename,"None",None 
            
        ```
    Saiba mais sobre [preparar o ficheiro CSV de conjunto de dados](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Modificar o *driveset.csv* ficheiro na pasta raiz onde a ferramenta reside. Adicionar entradas no *driveset.csv* ficheiro semelhante para os exemplos seguintes. O ficheiro de driveset tem a lista de discos e correspondente letras de unidade para que a ferramenta corretamente pode escolher a lista de discos para estar preparado.

    Este exemplo assume que os dois discos estão anexados e volumes NTFS básicos G:\ e H:\ são criados. H:\is não encriptados enquanto g já estiver encriptado. A ferramenta de formatos e encripta o disco que aloja H:\ apenas (e não g\).

    - **Para um disco que não está encriptado**: especificar *encriptar* para ativar a encriptação BitLocker no disco.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        H,Format,SilentMode,Encrypt,
        ```
    
    - **Para um disco que já estiver encriptado**: especificar *AlreadyEncrypted* e fornecer a chave do BitLocker.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
        ```

    Existem várias entradas podem ser efectuadas no mesmo ficheiro correspondente a várias unidades. Saiba mais sobre [preparar ficheiro CSV driveset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5.  Utilize o `PrepImport` opção para copiar e preparar dados para a unidade de disco. Para a primeira sessão de cópia copiar diretórios e/ou os ficheiros com uma nova sessão de cópia, execute o seguinte comando:

        ```
        .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
        ```

    Um exemplo de importação é mostrado abaixo.
  
        ```
        .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
        ```
 
6. Um ficheiro do diário de alterações com o nome fornecido com `/j:` parâmetro, é criada para cada execução da linha de comandos. Cada unidade que preparar tem um ficheiro de diário de alterações que tem de ser carregado quando criar a tarefa de importação. Unidades sem diário ficheiros não são processados.

    > [!IMPORTANT]
    > - Não modifique os dados em unidades de disco ou o ficheiro de diário depois de concluir a preparação de disco.

Para obter exemplos adicionais, aceda a [amostras para ficheiros do diário](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>Passo 2: Criar uma tarefa de importação 

Execute os seguintes passos para criar uma tarefa de importação no portal do Azure.
1. Inicie sessão no https://portal.azure.com/.
2. Aceda a **todos os serviços > armazenamento > importar/exportar tarefas**. 

    ![Ir para importar/exportar](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Clique em **criar tarefa de importação/exportação**.

    ![Clique em tarefa importar/exportar](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. No **Noções básicas**:

    - Selecione **importar para o Azure**.
    - Introduza um nome descritivo para a tarefa de importação. Utilize este nome para controlar as tarefas enquanto estiverem em curso e depois de serem concluídas.
        -  Este nome pode conter apenas letras minúsculas, números, hífenes e carateres de sublinhado.
        -  O nome tem de começar com uma letra e não pode conter espaços. 
    - Selecione uma subscrição.
    - Selecione um grupo de recursos. 

        ![Criar tarefa de importação - passo 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. No **detalhes da tarefa**:
    
    - Carregar os ficheiros do diário de alterações que criou durante precedente [passo 1: preparar as unidades](#step-1-prepare-the-drives). 
    - Selecione a conta de armazenamento que os dados serão importados para. 
    - A localização de drop-off é preenchida automaticamente com base na região da conta de armazenamento selecionada.
   
       ![Criar tarefa de importação - passo 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. No **devolver informações de envio**:

    - Selecione o carrier da lista pendente.
    - Introduza um número de conta operadora válida que tenha criado com esse operadora. A Microsoft utiliza esta conta para enviar as unidades para, assim que a tarefa de importação estiver concluída. 
    - Forneça um nome do contacto completo e válido, telefone, e-mail, morada, cidade, zip, distrito e país/região.

       ![Criar tarefa de importação - passo 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. No **resumo**:

    - Forneça o endereço de envio de discos para o Azure de envio do datacenter do Azure. Certifique-se de que o nome da tarefa e o endereço completo são mencionadas na etiqueta do envio.
    - Clique em **OK** para concluir a criação da tarefa de importação.

        ![Criar tarefa de importação - passo 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Passo 3: Enviar as unidades para o datacenter do Azure 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Passo 4: Atualizar a tarefa com informações de registo

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="samples-for-journal-files"></a>Exemplos de ficheiros do diário de alterações

Para **adicionar mais unidades**, crie um novo ficheiro de driveset e execute o comando, tal como indicado abaixo. 

Para as sessões de cópia subsequentes para as diferentes unidades de disco superior ao especificado no *. csv de InitialDriveset* de ficheiros, especifique um novo driveset *. csv* de ficheiros e fornecê-lo como um valor para o parâmetro `AdditionalDriveSet`. Utilize o **mesmo ficheiro de diário** nome e fornecem um **novo ID de sessão**. O formato do ficheiro AdditionalDriveset CSV é igual ao formato InitialDriveSet.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

Um exemplo de importação é mostrado abaixo.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Para adicionar dados adicionais para o mesmo driveset, utilize o comando de PrepImport sessões subsequentes cópia para copiar os ficheiros/diretório adicional.

Para sessões de cópia subsequentes para as unidades de disco rígido mesmas especificadas no *InitialDriveset.csv* de ficheiros, especifique o **mesmo ficheiro de diário** nome e fornecem um **novo ID de sessão**; Não é necessário para fornecer a chave de conta de armazenamento.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

Um exemplo de importação é mostrado abaixo.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Passos Seguintes

* [Ver o estado de tarefa e unidade](storage-import-export-view-drive-status.md)
* [Reveja os requisitos de importação/exportação](storage-import-export-requirements.md)


