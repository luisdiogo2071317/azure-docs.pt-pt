---
title: Utilizar Importar/exportar do Azure para transferir dados para e do armazenamento do Azure | Microsoft Docs
description: Saiba como criar importar e exportar tarefas no portal do Azure para transferir dados para e do armazenamento do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 83ba437e699eb150e86e6c89e478377394966419
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809547"
---
# <a name="what-is-azure-importexport-service"></a>O que é o serviço importar/exportar do Azure?

Serviço de importação/exportação do Azure é utilizado em segurança importar grandes quantidades de dados para o Blob storage do Azure e ficheiros do Azure através do envio unidades de disco para um datacenter do Azure. Este serviço também pode ser utilizado para transferir dados do Blob storage do Azure em unidades de disco e são enviados para os sites no local. Dados a partir de um ou mais discos podem ser importados o Blob storage do Azure ou os ficheiros do Azure. 

## <a name="azure-importexport-usecases"></a>Usecases de importação/exportação do Azure

Considere utilizar o serviço importar/exportar do Azure quando carregamento ou transferência de dados através da rede é demasiado lenta ou obter largura de banda de rede adicional é proibitivos em termos de custo. Utilize este serviço nos seguintes cenários:

* **Migração de dados para a nuvem**: mover rapidamente grandes quantidades de dados para o Azure e o custo de forma eficaz.
* **Distribuição de conteúdo**: rapidamente enviar dados para os sites de cliente.
* **Cópia de segurança**: efetuar cópias de segurança dos seus dados no local para armazenar no armazenamento do Azure.
* **Recuperação de dados**: recuperar grande quantidade de dados armazenados no armazenamento e tiver entregues à sua localização no local.

## <a name="importexport-components"></a>Componentes para importar/exportar

Serviço de importação/exportação utiliza os seguintes componentes:

- **Importar/exportar**serviço: este serviço disponível no portal do Azure ajuda-o utilizador criar e controlar a importar e exportar tarefas.  

- **Ferramenta de WAImportExport**: Esta é uma ferramenta da linha de comandos que faz o seguinte: 
    - Prepara a unidades que é fornecida para importação.
    - Facilita a copiar os dados para a unidade.
    - Encripta os dados na unidade com o BitLocker.
    - Gera os ficheiros do diário de alterações de unidade utilizados durante a criação de importação.
    - Ajuda a identifica os números de unidades necessários para as tarefas de exportação.

    Esta ferramenta está disponível em duas versões, versão 1 e 2. Recomendamos que utilize:

    - Versão 1 para importar/exportar para o armazenamento de Blobs do Azure. 
    - Versão 2, para importar dados para os ficheiros do Azure.

    A ferramenta de WAImportExport só é compatível com o sistema de operativo do Windows de 64 bits. Para versões de SO específicas suportadas, aceda a [requisitos do Azure para importar/exportar](storage-import-export-requirements.md#supported-operating-systems).

- **Discos**: pode enviar unidades de estado sólido (SSDs) ou unidades de disco rígido (HDDs) para o datacenter do Azure. Ao criar uma tarefa de importação, são enviados unidades de disco que contém os dados. Ao criar uma tarefa de exportação, são enviados unidades vazias para o datacenter do Azure. Para tipos de disco específico, aceda a [tipos de disco suportados](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>Como funciona a importação/exportação?

Serviço de importação/exportação do Azure permite a transferência de dados em Blobs do Azure e ficheiros do Azure através da criação de tarefas. Utilize o portal do Azure ou a API de REST do Azure Resource Manager para criar tarefas. Cada tarefa está associada uma única conta de armazenamento. 

As tarefas podem ser importar ou exportar tarefas. Uma tarefa de importação permite-lhe importar dados para o Blobs do Azure ou ficheiros do Azure, enquanto que a tarefa de exportação permite dados exportados de Blobs do Azure. Para uma tarefa de importação, são enviados unidades que contêm os dados. Quando cria uma tarefa de exportação, são enviados unidades vazias para um datacenter do Azure. Em cada caso, pode enviar até 10 unidades de disco por tarefa.

> [!IMPORTANT]
> Exportar dados para ficheiros do Azure não é suportada.

Nesta secção, os passos de nível elevados envolvido na importação e exportação tarefas descritas. 


### <a name="inside-an-import-job"></a>Dentro de uma tarefa de importação

Um nível elevado, uma tarefa de importação envolve os seguintes passos:

1. Determine os dados podem ser importados, número de unidades que precisa, localização de blob de destino para os seus dados no armazenamento do Azure.
2. Utilize a ferramenta de WAImportExport para copiar dados para unidades de disco. Encriptar os discos com o BitLocker.
3. Crie uma tarefa de importação na sua conta de armazenamento de destino no portal do Azure. Carregar os ficheiros do diário de alterações de unidade.
2. Forneça o endereço do remetente e o número de conta de carrier para envio as unidades de volta para o utilizador.
3. Envie as unidades de disco para o endereço de envio fornecido durante a criação da tarefa.
4. Atualize a entrega controlo número nos detalhes da tarefa de importação e submeter a tarefa de importação.
5. Unidades são recebidas e processadas no Centro de dados do Azure.
6. Unidades são fornecidas com a sua conta operadora para o endereço do remetente fornecido na tarefa de importação.
  
    ![Figura 1:Import fluxo de trabalho](./media/storage-import-export-service/importjob.png)

Para obter instruções passo a passo sobre dados importar, aceda a:

- [Importar dados para o Blobs do Azure](storage-import-export-data-to-blobs.md)
- [Importar dados para ficheiros do Azure](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>Dentro de uma tarefa de exportação

> [!IMPORTANT]
> O serviço suporta apenas a exportação do Blobs do Azure. Exportação de ficheiros do Azure não é suportada.

Um nível elevado, uma tarefa de exportação envolve os seguintes passos:

1. Determinar os dados a ser exportado, número de unidades é necessário, de blobs de origem ou de caminhos do contentor dos seus dados no Blob storage.
3. Crie uma tarefa de exportação na sua conta de armazenamento de origem no portal do Azure.
4. Especifique os blobs de origem ou de caminhos do contentor para os dados ser exportada.
5. Forneça o endereço do remetente e o número de conta de carrier para envio as unidades de volta para o utilizador.
6. Envie as unidades de disco para o endereço de envio fornecido durante a criação da tarefa.
7. Atualize a entrega controlo número nos detalhes da tarefa de exportação e submeter a tarefa de exportação.
8. As unidades são recebidas e processadas no Centro de dados do Azure.
9. As unidades são encriptadas com BitLocker e as chaves estão disponíveis através do portal do Azure.  
10. As unidades são fornecidas com a sua conta operadora para o endereço do remetente fornecido na tarefa de importação.
  
    ![Figura 2:Export fluxo de trabalho](./media/storage-import-export-service/exportjob.png)

Para obter instruções passo a passo sobre a exportação de dados, vá para [exportar dados de Blobs do Azure](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Disponibilidade de região 

O serviço de importação/exportação do Azure suporta a cópia de dados de e para todas as contas de armazenamento do Azure. Pode enviar unidades de disco para uma das localizações indicadas. Se a conta de armazenamento numa localização do Azure que não for especificada aqui, uma localização alternativa de envio é fornecida quando criar a tarefa.

### <a name="supported-shipping-locations"></a>Suportado localizações de envio


|País  |País  |País  |País  |
|---------|---------|---------|---------|
|EUA Leste    | Europa do Norte        | Índia Central        |Gov (US) - Iowa         |
|EUA Oeste     |Europa Ocidental         | Sul da Índia        | US DoD Leste        |
|EUA Leste 2    | Ásia Oriental        |  Índia Ocidental        | US DoD Centro        |
|EUA Oeste 2     | Sudeste Asiático        | Canadá Central        | Leste da China         |
|EUA Central     | Leste da Austrália        | Leste do Canadá        | China Norte        |
|EUA Centro-Norte     |  Sudeste da Austrália       | Sul do Brasil        | Reino Unido Sul        |
|EUA Centro-Sul     | Oeste do Japão        |Coreia Central         | Alemanha Central        |
|EUA Centro-Oeste     |  Leste do Japão       | Gov (US) - Virginia        | Alemanha Nordeste        |


## <a name="security-considerations"></a>Considerações de segurança

Os dados na unidade são encriptados utilizando a encriptação de unidade BitLocker. Esta encriptação protege os dados enquanto esta se encontra em trânsito.

Para tarefas de importação, unidades são encriptadas de duas formas.  


- Especifique a opção ao utilizar *dataset.csv* ficheiro ao executar a ferramenta de WAImportExport durante a preparação de unidade. 

- Ative a encriptação BitLocker manualmente na unidade. Especifique a chave de encriptação no *driveset.csv* quando executar linha de comandos de ferramenta WAImportExport durante a preparação de unidade.


Para as tarefas de exportação, depois dos dados são copiados para as unidades, o serviço encripta a unidade de utilizar o BitLocker antes de envio-lo novamente para si. A chave de encriptação é fornecida através do portal do Azure.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Preços

**Taxa de processamento de unidade**

Há uma taxa de processamento de unidade para cada unidade processada como parte da sua importação ou exportação tarefa. Consulte os detalhes sobre o [preços do Azure para importar/exportar](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Os custos de envio**

Quando são enviados unidades para o Azure, paga um custo o envio para o operador de envio. Quando a Microsoft devolve as unidades para si, o custo de envio é cobrado a conta de carrier que fornecido no momento da criação da tarefa.

**Custos de transação**

Não existem sem custos de transação para além dos custos de transação de armazenamento standard ao importar dados para armazenamento do Azure. Os encargos de saída padrão são aplicáveis quando os dados são exportados do armazenamento de Blobs. Para obter mais informações sobre os custos de transação, consulte [preços de transferência de dados.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Passos Seguintes

Saiba como utilizar o serviço de importação/exportação para:
* [Importar dados de Blobs do Azure](storage-import-export-data-to-blobs.md)
* [Exportar dados de Blobs do Azure](storage-import-export-data-from-blobs.md)
* [Importar dados para ficheiros do Azure](storage-import-export-data-to-files.md)

