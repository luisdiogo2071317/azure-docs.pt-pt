---
title: "Armazenamento do Azure da pilha: Diferenças e as considerações"
description: "Compreenda as diferenças entre o armazenamento de pilha do Azure e Storage do Azure, juntamente com considerações de implementação de pilha do Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/21/2017
ms.author: jeffgilb
ms.openlocfilehash: 7c4f030018f388302c3b60a41086bbd97c86513d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Armazenamento do Azure da pilha: Diferenças e as considerações

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Armazenamento de pilha do Azure é um conjunto de serviços de cloud de armazenamento na pilha do Microsoft Azure. Armazenamento de pilha do Azure fornece blob, tabela, fila e funcionalidade de gestão de conta com semântica consistentes com o Azure.

Este artigo resume as diferenças de armazenamento do Azure pilha conhecidas do armazenamento do Azure. Também resume outras considerações a lembrar quando implementa a pilha do Azure. Para saber mais sobre das principais diferenças entre a pilha do Azure e do Azure, consulte o [chave considerações](azure-stack-considerations.md) tópico.

## <a name="cheat-sheet-storage-differences"></a>Cheat folha: diferenças de armazenamento

| Funcionalidade | Azure (global) | Azure Stack |
| --- | --- | --- |
|Armazenamento de ficheiros|Baseado na nuvem partilhas de ficheiros SMB suportadas|Ainda não suportado
|Encriptação do Serviço de Armazenamento do Azure para Dados Inativos|encriptação AES de 256 bits|Encriptação AES de 128 bits BitLocker
|Tipo de conta de armazenamento|Contas do Blob storage do Azure e para fins gerais|Para fins gerais apenas.
|Opções de replicação|Armazenamento localmente redundante, o armazenamento georredundante, o armazenamento georredundante com acesso de leitura e o armazenamento com redundância de zona|Armazenamento localmente redundante.
|Armazenamento Premium|Totalmente suportado|Pode ser aprovisionado, mas sem limite de desempenho ou garantir.
|Managed disks|Premium e padrão suportados|Ainda não suportado.
|Nome do blob|1024 caracteres (2.048 bytes)|880 carateres (1,760 bytes)
|Tamanho máximo do blob de bloco|4.75 TB (100 MB X 50 000 blocos)|4.75 TB (100 MB x 50 000 blocos) para a atualização de 1802 ou a versão mais recente. 50 000 4 MB (aprox 195 GB) para versões anteriores.
|Cópia de instantâneos do blob de página|Cópia de segurança do Azure não geridos VM discos anexados a uma VM em execução suportada|Ainda não suportado.
|Cópia de instantâneo incremental de blob de página|Suportado de blobs de página do Azure standard e Premium|Ainda não suportado.
|Camadas de armazenamento para o blob storage|Hot, esporádico e as camadas de armazenamento de arquivo.|Ainda não suportado.
Eliminação de forma recuperável para o blob storage|Pré-visualização|Ainda não suportado.
|Tamanho máximo do blob de página|8 TB|1 TB
|Tamanho de página do blob de página|512 bytes|4 KB
|Tamanho da chave de linha e chave de partição da tabela|1024 caracteres (2.048 bytes)|400 carateres (800 bytes)

### <a name="metrics"></a>Métricas
Também existem algumas diferenças com as métricas do storage:
* Os dados de transação nas métricas do storage não distingue largura de banda de rede internos ou externos.
* Os dados de transação nas métricas do storage não incluem acesso a máquina virtual para os discos montados.

## <a name="api-version"></a>Versão de API
São suportadas as seguintes versões com armazenamento de pilha do Azure:

APIs de serviços de armazenamento do Azure:

Atualizar 1802 ou mais recente:
 - [2017-04-17](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-12-11)
 - [2015-07-08 ](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-04-05)

Versões anteriores:
 - [2015-04-05](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-04-05)


APIs de gestão de serviços de armazenamento do Azure:

 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>Versões do SDK

As seguintes bibliotecas de cliente são suportadas com o Azure pilha de armazenamento:

| Biblioteca de cliente | Versão suportada de pilha do Azure | Ligação                                                                                                                                                                                                                                                                                                                                     | Especificação de ponto final       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | De 6.2.0 para 8.7.0.          | Pacote Nuget:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | ficheiro App. config              |
| Java           | De 4.1.0 para 6.1.0           | Pacote maven:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Configuração de cadeia de ligação      |
| Node.js        | De 1.1.0 para 2.7.0           | Ligação NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Por exemplo: executar "instalação de npm azure-storage@2.7.0")<br> <br>Versão do Github:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Declaração de instância de serviço |
| C++            | De 2.4.0 para 3.1.0           | Pacote Nuget:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Configuração de cadeia de ligação      |
| PHP            | De 0.15.0 para 1.0.0          | Versão do GitHub:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Instalar através do compositor (consulte os detalhes abaixo)                                                                                                                                                                                                                  | Configuração de cadeia de ligação      |
| Python         | De 0.30.0 para 1.0.0          | Versão do GitHub:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Declaração de instância de serviço |
| Ruby           | De 0.12.1 para 1.0.1          | Pacote de RubyGems:<br>Comuns:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Queue: https://rubygems.org/gems/azure-storage-queue/<br>Table: https://rubygems.org/gems/azure-storage-table/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-ruby/releases | Configuração de cadeia de ligação      |

## <a name="next-steps"></a>Passos Seguintes

* [Começar a utilizar as ferramentas de desenvolvimento do armazenamento de pilha do Azure](azure-stack-storage-dev.md)
* [Introdução ao armazenamento de pilha do Azure](azure-stack-storage-overview.md)

