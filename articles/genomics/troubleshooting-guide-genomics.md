---
title: 'Microsoft Genomics: Guia de resolução de problemas | Documentos da Microsoft'
titleSuffix: Azure
description: Saiba mais sobre estratégias de resolução de problemas
keywords: resolução de problemas, erro, depuração
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 07/18/2018
ms.openlocfilehash: 9bd1690003fd37b6c2edd0f0421cf8d0e74f8cb5
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144181"
---
# <a name="troubleshooting-guide"></a>Guia de resolução de problemas
Esta descrição geral descreve estratégias para resolver problemas comuns ao utilizar o serviço Microsoft Genomics. Para perguntas freqüentes gerais sobre, consulte [perguntas comuns](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>Como posso verificar o meu estado de tarefa?
Pode verificar o estado do fluxo de trabalho ao chamar `msgen status` da linha de comando, conforme mostrado. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Existem três argumentos necessários:
* Adresa URL – o URI de base para a API
* CHAVE - a chave de acesso para a sua conta do Genomics. 
* ID - o ID de fluxo de trabalho

Para localizar o URL e a chave, aceda ao portal do Azure e abrir a página da sua conta do Genomics. Sob o **gerenciamento** cabeçalho, escolha **chaves de acesso**. Aí, encontrará o URL da API e as chaves de acesso.

Em alternativa, pode incluir o caminho para o ficheiro de configuração em vez de introduzir diretamente o URL e a chave. Tenha em atenção que, se incluir estes argumentos da linha de comandos, bem como o ficheiro de configuração, os argumentos da linha de comandos irão ter precedência. 

Após chamar `msgen status`, será apresentada uma mensagem amigável, que descreve se o fluxo de trabalho foi concluída com êxito ou fornecendo um motivo para a falha da tarefa. 


## <a name="get-more-information-about-my-workflow-status"></a>Obter mais informações sobre o meu estado de fluxo de trabalho

Para obter mais informações sobre por que uma tarefa pode não ter foi concluída com êxito, pode explorar os ficheiros de registo produzidos durante o fluxo de trabalho. No seu contentor de saída, deverá ver um `[youroutputfilename].logs.zip` pasta.  Descomprimir nesta pasta, verá os seguintes itens:

* outputFileList.txt - uma lista dos ficheiros de saída produzidos durante o fluxo de trabalho
* StandardError.txt - Este ficheiro está em branco.
* StandardOutput.txt - contém o registo de nível superior do fluxo de trabalho. 
* Ficheiros - todos os outros ficheiros de registo de GATK o `logs` pasta

O `standardoutput.txt` ficheiro é um bom lugar para começar a determinar o motivo pelo qual o fluxo de trabalho não foi bem-sucedida, pois inclui mais informações de baixo nível do fluxo de trabalho. 

## <a name="common-issues-and-how-to-resolve-them"></a>Problemas comuns e como resolvê-los
Esta secção destaca rapidamente problemas comuns e como resolvê-los.

### <a name="fastq-files-are-unmatched"></a>Ficheiros Fastq são sem correspondência
Ficheiros Fastq apenas devem ser diferente pelo /1 à direita ou /2 no identificador de exemplo. Se tiver submetido acidentalmente arquivos de ficheiros FASTQ sem correspondência, poderá ver as seguintes mensagens de erro ao chamar `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

Para resolver este problema, reveja se os ficheiros fastq submetidos para o fluxo de trabalho são, na verdade, um conjunto correspondente. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>Erro ao carregar o ficheiro do ficheiro. BAM. Blob de saída já existe e se a opção de substituição foi definida como False.
Se vir a seguinte mensagem de erro, `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, a pasta de saída já contém um ficheiro de saída com o mesmo nome.  A eliminar o ficheiro de saída existentes ou ative a opção de substituição no ficheiro de configuração. Em seguida, volte a submeter o fluxo de trabalho.

### <a name="when-to-contact-microsoft-genomics-support"></a>Quando contactar o suporte do Microsoft Genomics
Se vir as seguintes mensagens de erro, Ocorreu um erro interno. 

* `Error locating input files on worker machine`
* `Process management failure`

Tente voltar a submeter o seu fluxo de trabalho. Se continuar com falhas de tarefas, ou se tiver outras dúvidas, contacte o suporte da Microsoft Genomics no portal do Azure. Pode encontrar informações adicionais sobre como submeter um pedido de suporte [aqui](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a resolver problemas comuns com o serviço Microsoft Genomics. Para obter mais informações e FAQ mais gerais, veja [perguntas comuns](frequently-asked-questions-genomics.md). 
