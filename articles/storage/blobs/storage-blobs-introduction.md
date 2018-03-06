---
title: "Introdução ao Armazenamento de blobs do Azure | Microsoft Docs"
description: "Introdução ao Armazenamento de blobs do Azure"
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: overview
ms.date: 09/28/2017
ms.author: tamram
ms.openlocfilehash: aabc10b1c1c501e4fa0eb31835c2818cfb8942c6
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="introduction-to-blob-storage"></a>Introdução ao Armazenamento de blobs

O Blob Storage do Azure é um serviço para armazenar grandes quantidades de dados de objetos não estruturados, como texto ou dados binários, que podem ser acedidos de qualquer local no mundo através de HTTP ou HTTPS. Pode utilizar o armazenamento de Blobs para expor publicamente os dados ao mundo ou para armazenar dados da aplicação em privado.

Utilizações comuns do armazenamento de Blobs:

* Entrega de imagens ou documentos diretamente a um browser
* Armazenamento de ficheiros para acesso distribuído
* Transmissão de áudio e vídeo
* Armazenamento de dados de cópia de segurança e restauro, recuperação após desastre e arquivo
* Armazenamento de dados para análise por um serviço no local ou alojado no Azure

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
