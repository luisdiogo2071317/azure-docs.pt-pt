---
title: Utilizar Importar/exportar do Azure para exportar dados de Blobs do Azure | Microsoft Docs
description: Saiba como criar tarefas de exportação no portal do Azure para transferir dados de Blobs do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: eb41708c7446b3139758678c9247ffbb11da8b40
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661003"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Utilizar o serviço de importação/exportação do Azure para exportar dados do Blob storage do Azure
Este artigo fornece instruções passo a passo sobre como utilizar o serviço importar/exportar do Azure para exportar com segurança grandes quantidades de dados do Blob storage do Azure. O serviço requer que são enviados unidades vazias para o datacenter do Azure. O serviço exporta os dados da sua conta de armazenamento para as unidades e, em seguida, é fornecido as unidades de volta.

## <a name="prerequisites"></a>Pré-requisitos

Antes de criar uma tarefa de exportação para transferir dados fora do Blob Storage do Azure, cuidadosamente reveja e conclua a lista seguinte de pré-requisitos para este serviço. Tem de:

- Ter uma subscrição do Azure Active Directory que pode ser utilizada para o serviço de importação/exportação.
- Ter, pelo menos, uma conta de armazenamento do Azure. Ver a lista de [suportadas contas de armazenamento e os tipos de armazenamento para o serviço de importação/exportação](storage-import-export-requirements.md). Para obter informações sobre como criar uma nova conta de armazenamento, consulte [como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account).
- Ter um número suficiente de discos de [tipos suportados](storage-import-export-requirements.md#supported-disks).

## <a name="step-1-create-an-export-job"></a>Passo 1: Criar uma tarefa de exportação

Execute os seguintes passos para criar uma tarefa de exportação no portal do Azure.

1. Inicie sessão no https://portal.azure.com/.
2. Aceda a **todos os serviços > armazenamento > importar/exportar tarefas**. 

    ![Vá para as tarefas de importação/exportação](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Clique em **criar tarefa de importação/exportação**.

    ![Clique em tarefa importar/exportar](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. No **Noções básicas**:
    
    - Selecione **exportar do Azure**. 
    - Introduza um nome descritivo para a tarefa de exportação. Utilize o nome que escolher para acompanhar o progresso das suas tarefas. 
        - O nome pode conter apenas letras minúsculas, números, hífenes e carateres de sublinhado.
        - O nome tem de começar com uma letra e não pode conter espaços. 
    - Selecione uma subscrição.
    - Introduza ou selecione um grupo de recursos.

        ![Noções básicas](./media/storage-import-export-data-from-blobs/export-from-blob3.png) 
    
3. No **detalhes da tarefa**:

    - Selecione a conta de armazenamento onde residem a dados a ser exportado. 
    - A localização de drop-off é preenchida automaticamente com base na região da conta de armazenamento selecionada. 
    - Especifique os dados de BLOBs que pretende exportar da sua conta de armazenamento para a unidade em branco ou unidades. 
    - Optar por **exportar todas as** dados na conta de armazenamento de Blobs.
    
         ![Exportar todo](./media/storage-import-export-data-from-blobs/export-from-blob4.png) 

    - Pode especificar que contentores e blobs para exportar.
        - **Para especificar um blob para exportar**: Utilize o **igual a** Seletor. Especifique o caminho relativo para o blob, começando com o nome do contentor. Utilize *$root* para especificar o contentor de raiz.
        - **Para especificar todos os blobs começando com um prefixo**: Utilize o **começa por** Seletor. Especifique o prefixo, começando com uma barra '/'. O prefixo pode ser o prefixo do nome do contentor, o nome do contentor concluída ou o nome do contentor concluída seguido o prefixo do nome do blob. Tem de fornecer os caminhos do blob no formato válido para evitar erros durante o processamento, conforme mostrado nesta captura de ecrã. Para obter mais informações, consulte [exemplos de caminhos de blob válido](#examples-of-valid-blob-paths). 
   
           ![Os blobs e contentores de exportação selecionada](./media/storage-import-export-data-from-blobs/export-from-blob5.png) 

    - Pode exportar a partir do ficheiro de lista de blob.

        ![Exportar a partir do ficheiro de lista de blob](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  
   
   > [!NOTE]
   > Se o blob seja exportado está a ser utilizado durante a cópia de dados, o serviço importar/exportar do Azure tira um instantâneo do blob e copia o instantâneo.
 

4. No **devolver informações de envio**:

    - Selecione a operadora na lista pendente.
    - Introduza um número de conta operadora válida que tenha criado com esse operadora. A Microsoft utiliza esta conta para enviar as unidades para, assim que a tarefa de importação estiver concluída. 
    - Forneça um nome do contacto completo e válido, telefone, e-mail, morada, cidade, zip, distrito e país/região.
   
5. No **resumo**:

    - Reveja os detalhes da tarefa.
    - Tenha em atenção o endereço de envio de nome e fornecido datacenter do Azure de tarefa para envio discos para o Azure. 
    - Clique em **OK** para concluir a criação da tarefa de exportação.

## <a name="step-2-ship-the-drives"></a>Passo 2: Envie as unidades

Se não souber o número de unidades que precisa de, vá para o [Verifique o número de unidades](#check-the-number-of-drives). Se souber o número de unidades, avance para enviar as unidades.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Passo 3: Atualizar a tarefa com informações de registo

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="step-4-receive-the-disks"></a>Passo 4: Receber os discos
Quando o dashboard comunica que a tarefa estiver concluída, os discos são fornecidos para si e o número de controlo para o shipment está disponível no portal.

1. Depois de receber as unidades com dados exportados, terá de obter as chaves do BitLocker para desbloquear as unidades. Vá para a tarefa de exportação no portal do Azure. Clique em **importar/exportar** separador. 
2. Selecione e cick a exportação da tarefa na lista. Aceda a **chaves do BitLocker** e copie as chaves.
   
   ![Ver as chaves de BitLocker para a tarefa de exportação](./media/storage-import-export-service/export-job-bitlocker-keys.png)

3. Utilize as chaves do BitLocker para desbloquear os discos.

A exportação for concluída. Neste momento, pode eliminar a tarefa ou eliminá-lo automaticamente obtém após 90 dias.


## <a name="check-the-number-of-drives"></a>Verifique o número de unidades

Isto *opcional* passo ajuda a determina o número de unidades necessárias para a tarefa de exportação. Execute este passo num sistema Windows em execução um [versão do SO suportado](storage-import-export-requirements.md#supported-operating-systems).

1. [Transferir a versão 1 de WAImportExport](https://www.microsoft.com/en-us/download/details.aspx?id=42659) no sistema Windows. 
2. Deszipe para a pasta predefinida `waimportexportv1`. Por exemplo, `C:\WaImportExportV1`.
3. Abra uma janela do PowerShell ou da linha de comandos com privilégios administrativos. Para alterar o diretório para a pasta deszipada, execute o seguinte comando:
    
    `cd C:\WaImportExportV1`

4. Para verificar o número de discos necessários para os blobs selecionados, execute o seguinte comando:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Os parâmetros são descritos na tabela seguinte:
    
    |Parâmetro da linha de comandos|Descrição|  
    |--------------------------|-----------------|  
    |**/LOGDIR:**|Opcional. O diretório de registo. Ficheiros de registo verboso são escritos para este diretório. Se não for especificado, o atual diretório é utilizado como o diretório de registo.|  
    |**/SN:**|Necessário. O nome da conta de armazenamento para a tarefa de exportação.|  
    |**/SK:**|Obrigatório apenas se não for especificado um contentor SAS. A chave de conta para a conta de armazenamento para a tarefa de exportação.|  
    |**/csas:**|Obrigatório apenas se não for especificada uma chave de conta do storage. O contentor de SAS para listar os blobs para ser exportado na tarefa de exportação.|  
    |**/ ExportBlobListFile:**|Necessário. Caminho para o XML do ficheiro que contém lista de caminhos de blob ou blob prefixos de caminho para os blobs ser exportada. O formato de ficheiro utilizado no `BlobListBlobPath` elemento o [colocar tarefa](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operação da REST API do serviço de importação/exportação.|  
    |**/ DriveSize:**|Necessário. O tamanho das unidades a utilizar para uma tarefa de exportação, *por exemplo,*, 500 GB, 1,5 TB.|  

    Consulte uma [exemplo do comando PreviewExport](#example-of-previewexport-command).
 
5. Verifique se o pode leitura/escrita para as unidades que serão enviadas para a tarefa de exportação.

### <a name="example-of-previewexport-command"></a>Exemplo de comando PreviewExport

O exemplo seguinte demonstra a `PreviewExport` comando:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
O ficheiro de lista de BLOBs de exportação pode conter nomes de BLOBs e BLOBs prefixos, conforme mostrado aqui:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

A ferramenta de importação/exportação do Azure apresenta uma lista de todos os blobs para ser exportada e calcula como pacote-los para unidades de tamanho especificado, tendo em conta quaisquer necessário overhead, em seguida, calcula o número de unidades necessário para armazenar os blobs e informações de utilização da unidade.  
  
Eis um exemplo de saída, com os registos informativas omitido:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```

## <a name="examples-of-valid-blob-paths"></a>Exemplos de caminhos de blob válido

A tabela seguinte mostra exemplos de caminhos de blob válido:
   
   | Seletor | Caminho do blob | Descrição |
   | --- | --- | --- |
   | Começa por |/ |Exporta todos os blobs na conta de armazenamento |
   | Começa por |/$root / |Exporta todos os blobs no contentor de raiz |
   | Começa por |/Book |Exporta todos os blobs em qualquer contentor que começa com o prefixo **livro** |
   | Começa por |/Music/ |Exporta todos os blobs no contentor **música** |
   | Começa por |música/love |Exporta todos os blobs no contentor **música** que começam com prefixo **adoram** |
   | Igual a |$root/logo.bmp |Blob de exportações **logo.bmp** no contentor de raiz |
   | Igual a |videos/story.mp4 |Blob de exportações **story.mp4** no contentor **vídeos** |

## <a name="next-steps"></a>Passos Seguintes

* [Ver o estado de tarefa e unidade](storage-import-export-view-drive-status.md)
* [Reveja os requisitos de importação/exportação](storage-import-export-requirements.md)


