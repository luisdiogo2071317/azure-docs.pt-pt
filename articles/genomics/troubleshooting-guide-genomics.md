---
title: 'Microsoft Genomics: Guia de resolução de problemas | Microsoft Docs'
titleSuffix: Azure
description: Saiba mais sobre as estratégias de resolução de problemas
keywords: resolução de problemas, erro, a depuração
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/13/2018
ms.openlocfilehash: 18761c02cc423affe7b1050700e560b1f0b0594d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshooting-guide"></a>Guia de resolução de problemas
Esta descrição geral descreve estratégias para resolver problemas comuns quando utilizar o serviço Microsoft Genomics. Para perguntas mais frequentes sobre geral, consulte [perguntas comuns](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>Como verificar a minha estado da tarefa?
Pode verificar o estado do fluxo de trabalho chamando `msgen status` na linha de comandos, como mostrado. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Existem três argumentos necessários:
* URL - o URI base para a API
* CHAVE - a chave de acesso para a sua conta Genomics. 
* ID - o ID do fluxo de trabalho

Para localizar o URL e a chave, aceda ao portal do Azure e abrir a página de conta Genomics. Sob o **gestão** cabeçalho, escolha **chaves de acesso**. Não existe, localizar o URL da API e as chaves de acesso.

Em alternativa, pode incluir o caminho para o ficheiro de configuração em vez de introduzir diretamente o URL e a chave. Tenha em atenção que, se incluir estes argumentos de linha de comandos, bem como o ficheiro de configuração, os argumentos da linha de comandos irão ter precedência. 

Após a chamada `msgen status`, será apresentada uma mensagem amigável de utilizador, que descreve se o fluxo de trabalho foi efetuado com êxito ou fornecer um motivo para a falha da tarefa. 


## <a name="get-more-information-about-my-workflow-status"></a>Obter mais informações sobre o meu estado do fluxo de trabalho

Para obter mais informações sobre o motivo pelo qual uma tarefa poderá não ter foi concluída com êxito, pode explorar os ficheiros de registo produzidos durante o fluxo de trabalho. No contentor de saída, deverá ver uma `[youroutputfilename].logs.zip` pasta.  Unzipping nesta pasta, verá os seguintes itens:

* outputFileList.txt - uma lista dos ficheiros de saída produzidos durante o fluxo de trabalho
* StandardError.txt - Este ficheiro está em branco.
* StandardOutput.txt - contém o registo de nível superior do fluxo de trabalho. 
* Ficheiros - todos os outros ficheiros de registo de GATK o `logs` pasta

O `standardoutput.txt` ficheiro é um bom local para começar a determinar o motivo pelo qual o fluxo de trabalho não foi bem sucedida, que inclui mais informações de baixo nível do fluxo de trabalho. 

## <a name="common-issues-and-how-to-resolve-them"></a>Problemas comuns e como resolvê-los
Esta secção brevemente realça problemas comuns e como resolvê-los.

### <a name="fastq-files-are-unmatched"></a>Ficheiros de Fastq são sem correspondência
Ficheiros de Fastq só devem diferir o /1 à direita ou /2 no identificador de exemplo. Se tiver submetido acidentalmente ficheiros FASTQ sem correspondência, poderá ver as seguintes mensagens de erro ao chamar `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

Para resolver este problema, reveja se os ficheiros de fastq submetidos para o fluxo de trabalho estiverem efetivamente um conjunto correspondente. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>Erro ao carregar o ficheiro .bam. Já existe o blob de saída e a opção de substituição foi definida como False.
Se vir a mensagem de erro seguintes, `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, a pasta de saída já contém um ficheiro de saída com o mesmo nome.  Elimine o ficheiro de saída existente ou ative a opção de substituição no ficheiro de configuração. Em seguida, volte a submeter o fluxo de trabalho.

### <a name="when-to-contact-microsoft-genomics-support"></a>Quando contactar o suporte da Microsoft Genomics
Se vir seguintes mensagens de erro, Ocorreu um erro interno. 

* `Error locating input files on worker machine`
* `Process management failure`

Tente reenviar o fluxo de trabalho. Se continuar com falhas de tarefas, ou se tiver quaisquer outras perguntas, contacte o suporte da Microsoft Genomics do portal do Azure.

![Contacte o suporte no portal do Azure](./media/troubleshooting-guide/genomics-contact-support.png "contacte o suporte no portal do Azure")

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a resolver problemas comuns com o serviço Microsoft Genomics. Para obter mais informações e FAQ mais gerais, consulte [perguntas comuns](frequently-asked-questions-genomics.md). 