---
title: Mapeamento de transformação de Sink de fluxo de dados de fábrica de dados do Azure
description: Mapeamento de transformação de Sink de fluxo de dados de fábrica de dados do Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 795b8072bbd9b248f982d061d699f490b1b63b17
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272115"
---
# <a name="azure-data-factory-mapping-data-flow-sink-transformation"></a>Mapeamento de transformação de Sink de fluxo de dados de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Opções de sink](media/data-flow/windows1.png "sink 1")

Após a conclusão da sua transformação de fluxo de dados, pode sink os dados transformados num conjunto de dados de destino. A transformação de Sink, pode escolher a definição de conjunto de dados que deseja usar para os dados de saída de destino.

É uma prática comum para levar em conta a alteração de dados de entrada e levar em conta os descompassos de esquema para os dados de saída de sink para uma pasta sem um esquema definido no conjunto de dados de saída. Pode além em conta todas as alterações de coluna em suas origens ao selecionar "Permitir Descompassos de esquema" na origem e, em seguida, automap todos os campos no coletor.

Pode optar por substituir, acrescentar ou falhar o fluxo de dados quando afundando até um conjunto de dados.

Também pode escolher "automap" para todos os campos de entrada de sink. Se desejar escolhe os campos que pretende para o destino de sink, ou se gostaria de alterar os nomes dos campos no destino, escolha "Desativado" para "automap" e, em seguida, clique no separador de mapeamento para mapear campos de saída:

![Opções de sink](media/data-flow/sink2.png "sink 2")

## <a name="output-to-one-file"></a>Saída para um ficheiro
Para tipos de sink do Blob de armazenamento do Azure ou o Data Lake, produzirá os dados transformados para uma pasta. Spark irá gerar arquivos de dados de saída particionada com base no esquema de particionamento a ser utilizado na transformação de Sink. Pode definir o esquema de partição ao clicar na guia "Otimizar". Se desejar ADF para intercalar o resultado num único arquivo, clique no botão de opção "Partição única".

![Opções de sink](media/data-flow/opt001.png "opções de sink")

## <a name="blob-storage-folder"></a>Pasta de armazenamento de BLOBs
Quando Afundando até as transformações de dados para BLOBs Store, escolha um blob *pasta* como seu caminho de pasta de destino, não é um ficheiro. O fluxo de dados do ADF irá gerar os ficheiros de saída para nessa pasta.

![Caminho da pasta](media/data-flow/folderpath.png "caminho da pasta")

## <a name="optional-azure-sql-data-warehouse-sink"></a>Sink de armazém de dados SQL do Azure opcional

Vamos lançar um versão beta anterior do ADW Sink conjunto de dados para o fluxo de dados. Isso permitirá que parar os dados transformados diretamente para o Azure SQL DW dentro do fluxo de dados sem a necessidade de adicionar uma atividade de cópia no seu pipeline.

Comece por criar um conjunto de dados ADW, tal como faria para qualquer outro pipeline do ADF, com um serviço ligado que inclua as suas credenciais ADW e selecione a base de dados que deseja se conectar. O nome de tabela, selecione uma tabela existente ou digite o nome da tabela que gostaria de ter o fluxo de dados para criar automaticamente para si a partir nos campos de entrada.

![Opções de sink](media/data-flow/adw3.png "sink 3")

Novamente no tranformation Sink (ADW está atualmente apenas suportado como Sink) irá escolher o conjunto de dados de ADW que criou, bem como a conta de armazenamento que pretende utilizar para os dados para o Polybase carregar para ADW de preparação. O campo de caminho é o formato: "containername/foldername".

![Opções de sink](media/data-flow/adw1.png "sink 4")

### <a name="save-policy"></a>Guardar política

Substituir irá truncar a tabela se existir, em seguida, recriá-lo e carregar os dados. Acrescentar irá inserir novas linhas. Se a tabela a partir do nome de tabela do conjunto de dados não existe nenhuma no destino ADW, fluxo de dados irá criar a tabela, em seguida, carregue os dados.

Se desmarcar "Mapear automática", pode mapear os campos à sua tabela de destino manualmente.

![Opções de ADW de sink](media/data-flow/adw2.png "adw sink")

### <a name="max-concurrent-connections"></a>Ligações simultâneas máx.

Pode definir o número máximo de ligações simultâneo a transformação de Sink ao escrever os dados para uma ligação de base de dados do Azure.

![Opções de ligação](media/data-flow/maxcon.png "ligações")
