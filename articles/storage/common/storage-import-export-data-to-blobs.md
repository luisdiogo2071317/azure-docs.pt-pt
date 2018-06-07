---
title: Importar/exportar do Azure a utilizar para transferir dados para Blobs do Azure | Microsoft Docs
description: Saiba como criar importar e exportar tarefas no portal do Azure para transferir dados e de Blobs do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: fe9292459134972b44037a58235cdd817030a956
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661008"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>Utilizar o serviço de importação/exportação do Azure para importar dados para o Blob Storage do Azure

Este artigo fornece instruções passo a passo sobre como utilizar o serviço importar/exportar do Azure para importar com segurança grandes quantidades de dados para o Blob storage do Azure. Para importar dados para Blobs do Azure, o serviço requer que são enviados encriptadas unidades de disco que contém os dados para um datacenter do Azure.  

## <a name="prerequisites"></a>Pré-requisitos

Antes de criar uma tarefa de importação para transferir dados para o armazenamento de Blobs do Azure, cuidadosamente reveja e conclua a lista seguinte de pré-requisitos para este serviço. Tem de:

- Ter uma subscrição do Azure Active Directory que pode ser utilizada para o serviço de importação/exportação.
- Ter pelo menos uma conta de armazenamento do Azure com um contentor de armazenamento. Ver a lista de [suportadas contas de armazenamento e os tipos de armazenamento para o serviço de importação/exportação](storage-import-export-requirements.md). Para obter informações sobre como criar uma nova conta de armazenamento, consulte [como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account). Para obter informações sobre o contentor de armazenamento, aceda a [criar um contentor de armazenamento](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Ter um número suficiente de discos de [tipos suportados](storage-import-export-requirements.md#supported-disks). 
- Tem um sistema do Windows em execução um [versão do SO suportado](storage-import-export-requirements.md#supported-operating-systems). 
- Ative o BitLocker num sistema Windows. Consulte [como ativar o BitLocker](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [Transferir a versão 1 de WAImportExport](https://www.microsoft.com/en-us/download/details.aspx?id=42659) no sistema Windows. Deszipe para a pasta predefinida `waimportexportv1`. Por exemplo, `C:\WaImportExportV1`.


## <a name="step-1-prepare-the-drives"></a>Passo 1: Preparar as unidades

Este passo gera um ficheiro do diário de alterações. O ficheiro de diário armazena informações básicas, tais como o número de série da unidade, chave de encriptação e detalhes da conta de armazenamento. 

Execute os seguintes passos para preparar as unidades.

1.  Ligar a unidades de disco para o sistema do Windows através de conectores SATA.
1.  Crie um único volume NTFS em cada unidade. Atribua uma letra de unidade ao volume. Não utilize mountpoints.
2.  Ative a encriptação BitLocker no NTFS volume. Se utilizar um sistema do Windows Server, utilize as instruções no [como ativar o BitLocker no Windows Server 2012 R2](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
3.  Copie dados para volume encriptada. Utilize a arrastar e largar ou Robocopy ou tal ferramenta de cópia.
4.  Abra uma janela do PowerShell ou da linha de comandos com privilégios administrativos. Para alterar o diretório para a pasta deszipada, execute o seguinte comando:
    
    `cd C:\WaImportExportV1`
5.  Para obter a chave do BitLocker da unidade, execute o seguinte comando:
    
    ` manage-bde -protectors -get <DriveLetter>: `
6.  Para preparar o disco, execute o seguinte comando. **Dependendo do tamanho de dados, esta operação pode demorar várias horas a dias.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /sk:<Storage account key> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /skipwrite 
    ```
    É criado um ficheiro do diário de alterações na mesma pasta onde que executou a ferramenta. Também são criados dois outros ficheiros - um *. XML* ficheiro (pasta onde a ferramenta é executada) e um *unidade manifest.xml* ficheiro (pasta onde residem a dados).
    
    Os parâmetros utilizados são descritos na tabela seguinte:

    |Opção  |Descrição  |
    |---------|---------|
    |/j:     |O nome do ficheiro diário de alterações, com a extensão de .jrn. Um ficheiro do diário de alterações é gerado por unidade. Recomendamos que utilize o número de série de disco como o nome de ficheiro do diário de alterações.         |
    |formado:     |o ID de sessão. Utilize um número de sessão único para cada instância do comando.      |
    |/SK:     |A chave de conta do Storage do Azure.         |
    |/t:     |A letra de unidade do disco para enviada. Por exemplo, unidade `D`.         |
    |/bk:     |A chave do BitLocker para a unidade. A palavra-passe numérica da saída de ` manage-bde -protectors -get D: `      |
    |/srcdir:     |A letra de unidade do disco para enviada seguido `:\`. Por exemplo, `D:\`.         |
    |/dstdir:     |O nome do contentor de destino no armazenamento do Azure.         |
    |/skipwrite:     |É a opção que não especifica que não existe nenhum novos dados necessários para ser copiado e a dados existentes no disco para estar preparado.          |
7. Repita o passo anterior para cada disco que tem de ser fornecidos. Para cada execução da linha de comandos, é criado um ficheiro de diário de alterações com o nome fornecido.
    
    > [!IMPORTANT]
    > - Juntamente com o ficheiro de diário de alterações, um `<Journal file name>_DriveInfo_<Drive serial ID>.xml` ficheiro também é criado na mesma pasta onde a ferramenta reside. O ficheiro. xml é utilizado em vez do ficheiro do diário de alterações ao criar uma tarefa se o ficheiro do diário de alterações é demasiado grande. 

## <a name="step-2-create-an-import-job"></a>Passo 2: Criar uma tarefa de importação

Execute os seguintes passos para criar uma tarefa de importação no portal do Azure.

1. Inicie sessão no https://portal.azure.com/.
2. Aceda a **todos os serviços > armazenamento > importar/exportar tarefas**. 
    
    ![Vá para as tarefas de importação/exportação](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Clique em **criar tarefa de importação/exportação**.

    ![Clique em tarefa criar importar/exportar](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. No **Noções básicas**:

    - Selecione **importar para o Azure**.
    - Introduza um nome descritivo para a tarefa de importação. Utilize o nome para acompanhar o progresso das suas tarefas.
        - O nome pode conter apenas letras minúsculas, números, hífenes e carateres de sublinhado.
        - O nome tem de começar com uma letra e não pode conter espaços.
    - Selecione uma subscrição.
    - Introduza ou selecione um grupo de recursos.  

    ![Criar tarefa de importação - passo 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. No **detalhes da tarefa**:

    - Carregar os ficheiros do diário de alterações de unidade que obteve durante o passo de preparação de unidade. Se `waimportexport.exe version1` foi utilizado, carregar um ficheiro para cada unidade que preparou. Se o tamanho do diário de alterações do ficheiro excede 2 MB, em seguida, pode utilizar o `<Journal file name>_DriveInfo_<Drive serial ID>.xml` também criado com o ficheiro do diário de alterações. 
    - Selecione a conta de armazenamento de destino onde os dados irão residir. 
    - A localização de drop-off é preenchida automaticamente com base na região da conta de armazenamento selecionada.
   
   ![Criar tarefa de importação - passo 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. No **devolver informações de envio**:

    - Selecione a operadora na lista pendente.
    - Introduza um número de conta operadora válida que tenha criado com esse operadora. A Microsoft utiliza esta conta para enviar as unidades para, assim que a tarefa de importação estiver concluída. Se não tiver um número de conta, crie um [FedEx](http://www.fedex.com/us/oadr/) ou [DHL](http://www.dhl.com/) conta operadora.
    - Forneça um nome do contacto completo e válido, telefone, e-mail, morada, cidade, zip, distrito e país/região.

    ![Criar tarefa de importação - passo 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. No **resumo**:

    - Reveja as informações de tarefa fornecidas no resumo. Anote o nome da tarefa e datacenter do Azure envio endereço para enviar discos para o Azure. Estas informações são utilizadas mais tarde na etiqueta de envio.
    - Clique em **OK** ao criar a tarefa de importação.

    ![Criar tarefa de importação - passo 4](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

## <a name="step-3-ship-the-drives"></a>Passo 3: Enviar as unidades 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>Passo 4: Atualizar a tarefa com informações de registo

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="next-steps"></a>Passos Seguintes

* [Ver o estado de tarefa e unidade](storage-import-export-view-drive-status.md)
* [Reveja os requisitos de importação/exportação](storage-import-export-requirements.md)


