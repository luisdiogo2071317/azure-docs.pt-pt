---
title: "Início Rápido: Submeter um fluxo de trabalho através de várias entradas | Microsoft Docs"
titleSuffix: Azure
description: "O início rápido pressupõe que tem o cliente msgen instalado e os dados de exemplo foram executados com êxito através do serviço."
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: quickstart
ms.date: 12/07/2017
ms.openlocfilehash: d410516f807b7914e15bed1fb93ee58d3e340d1e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="submit-a-workflow-using-multiple-inputs-from-the-same-sample"></a>Submeter um fluxo de trabalho através de várias entradas do mesmo exemplo

Este início rápido demonstra como submeter um fluxo de trabalho para o serviço Microsoft Genomics se o ficheiro de entrada tiver múltiplos ficheiros FASTQ ou BAM **provenientes do mesmo exemplo**. No entanto, tenha em atenção que **não pode** misturar ficheiros FASTQ e BAM na mesma submissão.

Este tópico pressupõe que já instalou e executou o cliente `msgen` e está familiarizado com a utilização do Armazenamento do Azure. Se tiver submetido um fluxo de trabalho com êxito com os dados de exemplo fornecidos, está pronto para continuar este início rápido. 


## <a name="multiple-bam-files"></a>Múltiplos ficheiros BAM

### <a name="upload-your-input-files-to-azure-storage"></a>Carregar os ficheiros de entrada para o armazenamento do Azure
Vamos supor que tem múltiplos ficheiros BAM como entrada, *reads.bam*, *additional_reads.bam* e *yet_more_reads.bam*, e os carregou para a sua conta de armazenamento *myaccount* no Azure. Tem o URL da API e a chave de acesso. Quer ter saídas em **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>**.


### <a name="submit-your-job-to-the-msgen-client"></a>Submeter a tarefa para o cliente `msgen` 

Pode submeter múltiplos ficheiros BAM ao transmitir todos os respetivos nomes para o argumento --input-blob-name-1. Tenha em atenção que todos os ficheiros devem vir do mesmo exemplo, mas a ordem não é importante. Seguem-se submissões de exemplo de uma linha de comandos no Windows, no Unix e através de um ficheiro de configuração. As quebras de linha são adicionadas para maior clareza:


Para Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Para Unix:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Se preferir utilizar um ficheiro de configuração, eis o que deverá conter:

``` config.txt
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads.bam additional_reads.bam yet_more_reads.bam
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Submeta o ficheiro `config.txt` com esta invocação: `msgen submit -f config.txt`


## <a name="multiple-paired-fastq-files"></a>Múltiplos ficheiros FASTQ emparelhados

### <a name="upload-your-input-files-to-azure-storage"></a>Carregar os ficheiros de entrada para o armazenamento do Azure
Vamos supor que tem múltiplos ficheiros FASTQ emparelhados como entrada, *reads_1.fq.gz* e *reads_2.fq.gz*, *additional_reads_1.fq.gz* e *additional_reads_2.fq.gz* e *yet_more_reads_1.fq.gz* e *yet_more_reads_2.fq.gz*. Carregou-os para a sua conta de armazenamento *myaccount* no Azure e tem o URL da API e a chave de acesso. Quer ter saídas em **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>**.


### <a name="submit-your-job-to-the-msgen-client"></a>Submeter a tarefa para o cliente `msgen` 

Os ficheiros FASTQ emparelhados não só têm de ser provenientes do mesmo exemplo, mas também têm de ser processados em conjunto.  A ordem dos nomes de ficheiro é importante quando são transmitidos como argumentos para --input-blob-name-1 e --input-blob-name-2. 

Seguem-se submissões de exemplo de uma linha de comandos no Windows, no Unix e através de um ficheiro de configuração. As quebras de linha são adicionadas para maior clareza:


Para Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz ^
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Para Unix:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz \
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Se preferir utilizar um ficheiro de configuração, eis o que deverá conter:

```
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads_1.fq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz
input_blob_name_2:                reads_2.fq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Submeta o ficheiro `config.txt` com esta invocação: `msgen submit -f config.txt`

## <a name="next-steps"></a>Passos seguintes
Neste artigo, carregou múltiplos ficheiros BAM ou ficheiros FASTQ emparelhados para o Armazenamento do Azure e submeteu um fluxo de trabalho para o serviço Microsoft Genomics através do cliente python `msgen`. Para obter informações adicionais sobre a submissão de fluxos de trabalho e outros comandos que pode utilizar com o serviço Microsoft Genomics, veja as nossas [FAQ](frequently-asked-questions-genomics.md). 