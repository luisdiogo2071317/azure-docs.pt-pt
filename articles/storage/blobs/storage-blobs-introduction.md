---
title: Introdução ao Armazenamento de blobs - Armazenamento de objetos no Azure | Microsoft Docs
description: O Armazenamento de blobs do Azure foi criado para armazenar grandes quantidades de dados de objetos não estruturados, como dados de texto ou binários. As aplicações podem aceder a objetos no Armazenamento de blobs do PowerShell ou da CLI do Azure, a partir do código através de bibliotecas de cliente do Armazenamento do Azure ou através do REST.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: overview
ms.date: 03/27/2018
ms.author: tamram
ms.openlocfilehash: 0fff0032ec2452413bcd1df3175634b14a64208f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="introduction-to-blob-storage"></a>Introdução ao Armazenamento de blobs

O Armazenamento de blobs do Azure é a solução de armazenamento da cloud da Microsoft para objetos de dados. O Armazenamento de blobs pode armazenar grandes quantidades de dados de objetos não estruturados, como dados de texto ou binários. Os Dados no Armazenamento de blobs podem ser acedidos em qualquer local no mundo através de HTTP ou HTTPS. Pode utilizar o armazenamento de Blobs para expor publicamente os dados ao mundo ou para armazenar dados da aplicação em privado.

Utilizações comuns do armazenamento de Blobs:

* Entrega de imagens ou documentos diretamente a um browser
* Armazenamento de ficheiros para acesso distribuído
* Transmissão de áudio e vídeo
* Armazenamento de cópia de segurança e restauro, recuperação após desastre e arquivo
* Armazenamento de dados para análise por um serviço no local ou alojado no Azure
* Armazenar VHDs para utilização com Máquinas Virtuais do Azure

## <a name="blob-service-concepts"></a>Conceitos do serviço Blob

O serviço Blob contém os seguintes componentes:

![Arquitetura de blob](./media/storage-blobs-introduction/blob1.png)

* **Conta de Armazenamento:** todos os acessos ao Storage do Azure são efetuados através de uma conta de armazenamento. Esta conta de armazenamento pode ser uma **Conta de armazenamento para fins gerais (v1 ou v2)** ou **Contas de armazenamento de Blobs**. Consulte [About Azure storage accounts (Acerca das contas de armazenamento do Azure)](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para obter mais informações.

* **Contentor:** um contentor fornece um agrupamento de um conjunto de blobs. Todos os blobs tem de estar num contentor. Uma conta pode conter um número ilimitado de contentores. Um contentor pode armazenar um número ilimitado de blobs. Tenha em atenção que o nome do contentor tem de ser em minúsculas.

* **Blob:** um ficheiro de qualquer tipo e tamanho. O Armazenamento do Microsoft Azure oferece três tipos de blobs: blobs de blocos, [blobs de páginas](storage-blob-pageblob-overview.md) e blobs de acréscimo.
  
    Os *blobs de blocos* são ideais para armazenar ficheiros de texto ou binários, como documentos e ficheiros multimédia. Os *blobs de acréscimo* são semelhantes aos blobs de blocos na medida em que são constituídos por blocos, mas estão otimizados para operações de acréscimo, sendo pois úteis para cenários de registo. Um único blob de blocos pode conter até 50.000 blocos com um máximo de 100 MB cada, para um tamanho total ligeiramente acima de 4.75 TB (100 MB X 50.000). Um único blob de acréscimo pode conter até 50.000 blocos com um máximo de 4 MB cada, para um tamanho total ligeiramente acima de 195 GB (4 MB X 50.000).
  
    Os *blobs de páginas* podem ter até 8 TB de tamanho e são mais eficientes para operações de leitura/escrita frequentes. As Virtual Machines do Azure utilizam blobs de páginas como discos de dados e SO.
  
    Para obter detalhes sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata (Nomenclatura e Referência de Contentores, Blobs e Metadados)](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Passos seguintes

* [Criar uma conta de armazenamento](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Introdução ao Armazenamento de blobs com o .NET](storage-dotnet-how-to-use-blobs.md)
* [Exemplos de Armazenamento do Microsoft Azure com o .NET](../common/storage-samples-dotnet.md)
* [Exemplos de Armazenamento do Microsoft Azure com o Java](../common/storage-samples-java.md)
