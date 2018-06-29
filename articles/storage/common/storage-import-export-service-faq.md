---
title: Perguntas mais frequentes sobre serviço importar/exportar do Azure | Microsoft Docs
description: Leitura respostas a perguntas mais frequentes sobre serviço exportar de importação do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: alkohli
ms.openlocfilehash: d324025a50346415de8232af4adc487a5d907331
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031401"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Serviço de importação/exportação do Azure: Perguntas mais frequentes 
Seguem-se perguntas e respostas que possam ter quando utiliza o serviço importar/exportar do Azure para transferir dados para o armazenamento do Azure. Perguntas e respostas são dispostas nas seguintes categorias:

- Sobre o serviço importar/exportar
- Preparar os discos para importar/exportar
- Importar/exportar tarefas
- Discos de envio
- Diversos 

## <a name="about-importexport-service"></a>Sobre o serviço importar/exportar

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Pode copiar File storage do Azure utilizando o serviço importar/exportar do Azure?

Sim. O suporta de serviço do Azure para importar/exportar importar para o File Storage do Azure. Não suporta a exportação de ficheiros do Azure neste momento.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>O serviço importar/exportar do Azure está disponível para subscrições de CSP?

Sim. Serviço de importação/exportação do Azure suporta subscrições de fornecedores de soluções de nuvem (CSP).

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>Pode utilizar o serviço de importação/exportação do Azure para copiar as caixas de correio PST e dados do SharePoint para o Office 365?

Sim. Para obter mais informações, aceda a [ficheiros de importação PST ou dados do SharePoint para o Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Pode utilizar o serviço de importação/exportação do Azure para copiar os meus as cópias de segurança offline para o serviço de cópia de segurança do Azure?

Sim. Para obter mais informações, aceda a [fluxo de trabalho de cópia de segurança Offline no Backup do Azure](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Pode comprar unidades de tarefas de importação/exportação do Microsoft?

Não. Terá de enviar as seus próprios unidades para importar e exportar tarefas.


## <a name="preparing-disks-for-importexport"></a>Preparar os discos para importar/exportar

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>Pode ignorar o passo de preparação de unidade para uma tarefa de importação? Pode preparar uma unidade sem copiar?

Não. Qualquer unidade utilizada para importar dados deve estar preparada utilizando a ferramenta de WAImportExport do Azure. Utilize a ferramenta também copiar dados para a unidade.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>É necessário efetuar qualquer preparação do disco durante a criação de uma tarefa de exportação?

Não. Alguns prechecks são recomendadas. Para verificar o número de discos necessários, utilize o comando de PreviewExport a ferramenta WAImportExport. Para obter mais informações, consulte [pré-visualização utilização da unidade para uma tarefa de exportação](https://msdn.microsoft.com/library/azure/dn722414.aspx). O comando ajuda-o a pré-visualize a utilização da unidade para os blobs selecionados, com base no tamanho das unidades que pretende utilizar. Verifique também que pode ser lida e escrita no disco rígido que é enviada para a tarefa de exportação.

## <a name="importexport-jobs"></a>Importar/exportar tarefas

### <a name="can-i-cancel-my-job"></a>Pode cancelar a minha tarefa?
Sim. Pode cancelar uma tarefa quando o respetivo estado é **criar** ou **envio**. Para além destas fases, não é possível cancelar a tarefa e continua até que a fase final.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Quanto pode ver o estado de tarefas foi concluídas no portal do Azure?
Pode ver o estado para as tarefas concluídas até 90 dias. As tarefas concluídas são eliminadas passados 90 dias.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>Se pretender importar ou exportar mais de 10 unidades, o que devo fazer?
Uma importação ou exportação tarefa pode referenciar apenas 10 unidades numa única tarefa. Para enviar mais de 10 unidades, deve criar várias tarefas. Unidades associadas com a mesma tarefa tem de ser incluídas em conjunto no mesmo pacote. Para obter mais informações e orientações quando a capacidade de dados abrange várias disco importar tarefas, contacte a Microsoft em bulkimport@microsoft.com.                                                              

## <a name="shipping-disks"></a>Discos de envio

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>O que é o número máximo de HDD no um shipment?
Não há nenhum limite para o número de HDDs no um shipment. Se os discos pertencem a várias tarefas, recomendamos que tem: 
- etiqueta de discos com os nomes de tarefa correspondente.
- atualizar as tarefas com um número de controlo de com -1, o sufixo-2 etc.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>Deve posso incluir qualquer coisa que não o HDD na minha pacote?
São enviados apenas as unidades de disco rígido no pacote de envio. Não inclua itens, como os cabos de alimentação de energia ou cabos USB.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>É necessário que enviar as minhas unidades utilizando FedEx ou DHL?
Pode enviar unidades para o datacenter do Azure, utilizando qualquer operadora conhecida como FedEx, DHL, UPS ou nos serviço Postal. No entanto, para retorno shipment de unidades para a partir do Centro de dados, tem de fornecer:

- Um número de conta FedEx nos E.U.A. e EU, ou
- Um número de conta DHL nas regiões Ásia e da Austrália.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Existem restrições com envio internationally minha unidade?
Tenha em atenção que o suporte de dados físicos que são envio poderá ter de cruzada internacionais limites. É responsável por assegurar que os dados e suportes de dados físicos são importados e/ou exportados de acordo com as leis aplicáveis. Antes do envio do suporte de dados físico, consulte o seu consultores, para verificar que os dados e suportes de dados podem legalmente enviados para o Centro de dados identificados. Isto irá ajudar a garantir que atinge Microsoft atempadamente.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Ao criar uma tarefa, o endereço de envio é uma localização diferente do meu localização da conta de armazenamento. O que devo fazer?

Algumas localizações de conta de armazenamento estão mapeadas para as localizações de envio alternativo. Anteriormente as localizações de envio disponíveis podem também ser temporariamente mapeadas para localizações alternativas. Verifique sempre o endereço de envio fornecido durante a criação da tarefa antes de envio suas unidades.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Quando a minha unidade do envio, operadora pede-lhe o data center endereço e o telefone número de contacto. O que posso fornece?

O número de telefone e o DC endereço é fornecido como parte da criação da tarefa.


## <a name="miscellaneous"></a>Diversos

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>O que acontece se enviar acidentalmente um HDD que não está em conformidade com os requisitos suportados?

O Centro de dados do Azure irá devolver a unidade que não está em conformidade com os requisitos suportados para si. Se apenas algumas das unidades no pacote de satisfazem os requisitos de suporte, esses unidades serão processadas e as unidades que não cumprem os requisitos serão devolvidas para si.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>O serviço formatar as unidades antes de a devolvê-las?

Não. Todas as unidades estão encriptadas com o BitLocker.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Como aceder a dados que são importados por este serviço?

Utilizar o Portal do Azure ou [Explorador de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) para aceder aos dados na sua conta de armazenamento do Azure.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Após a importação estiver concluída, o que os meus aspeto de dados, como na conta de armazenamento? A minha hierarquia de diretório é mantida?

Quando preparar um disco rígido para uma tarefa de importação, o destino é especificado pelo campo de DstBlobPathOrPrefix no conjunto de dados CSV. Este é o contentor de destino na conta de armazenamento ao qual os dados da unidade de disco rígido são copiados. Dentro deste contentor de destino, diretórios virtuais são criados para as pastas no disco rígido e blobs são criados para ficheiros. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Se uma unidade tiver os ficheiros que já existem na minha conta de armazenamento, o serviço de substituir blobs existentes ou ficheiros?

Depende. Quando preparar a unidade, pode especificar se os ficheiros de destino devem ser substituídos ou ignoradas utilizar o campo no ficheiro CSV de conjunto de dados chamado disposição: < mudar o nome | substituir o não | substituir >. Por predefinição, o serviço muda o nome de novos ficheiros em vez de substituir blobs existentes ou ficheiros.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>A ferramenta de WAImportExport é compatível com os sistemas operativos de 32 bits?
Não. A ferramenta de WAImportExport só é compatível com sistemas de operativos do Windows de 64 bits. Para obter uma lista completa de SO suportado, aceda a [sistemas operativos suportados pelo](). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>O que é o máximo BLOBs de blocos e o tamanho do Blob de página suportados pelo Azure importação/exportação?

Tamanho do Blob de bloco máximo é aproximadamente 4.768TB ou 5,000,000 MB.
Tamanho do Blob de página máximo é 1TB.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>Importar/exportar do Azure suporta a encriptação AES 256?
Serviço de importação/exportação do Azure utiliza a encriptação bitlocker AES-128, por predefinição. Pode alterar isto para AES 256 ao encriptar manualmente com o bitlocker antes dos dados são copiados. 

- Se utilizar [WAImportExport V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), segue-se um comando de exemplo
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Se utilizar [WAImportExport V2](https://www.microsoft.com/en-us/download/details.aspx?id=55280) especificar "AlreadyEncrypted" e fornecer a chave no driveset CSV.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Passos Seguintes

* [O que é para importar/exportar do Azure?](storage-import-export-service.md)


