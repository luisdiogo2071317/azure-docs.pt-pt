---
title: 'Microsoft Genomics: Perguntas comuns | Microsoft Docs'
titleSuffix: Azure
description: Colocar as respostas aos clientes de perguntas comuns sobre o Microsoft Genomics.
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 2077eeb5177b07c458476ae900f81b72e35f0dc3
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: Perguntas comuns

Este artigo apresenta uma lista de consultas que poderá ter relacionadas com a Microsoft Genomics parte superior. Para obter mais informações sobre o serviço Microsoft Genomics, consulte [Novidades Microsoft Genomics?](overview-what-is-genomics.md) 


## <a name="what-is-the-sla-for-microsoft-genomics"></a>O que é o SLA para a Microsoft Genomics?
Garantimos que 99,9% do serviço Microsoft Genomics tempo estará disponível para receber pedidos de API de fluxo de trabalho. Para obter mais informações, consulte [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Como a utilização do Microsoft Genomics apareçam na minha fatura?
Microsoft Genomics faturas com base no número de gigabases processadas por fluxo de trabalho. Para obter mais informações, consulte [preços](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Onde posso encontrar uma lista de todos os comandos possíveis e argumentos para o `msgen` cliente?
Pode obter uma lista completa dos comandos disponíveis e os argumentos executando `msgen help`. Se não existem argumentos mais forem fornecidos, mostra uma lista de ajuda disponíveis secções, um para cada um dos `submit`, `list`, `cancel`, e `status`. Para obter ajuda para um comando específico, escreva `msgen help command`; por exemplo, `msgen help submit` apresenta uma lista de todas as opções de envio.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>O que são mais frequentemente utilizadas comandos para o `msgen` cliente?
Os comandos mais comuns são argumentos para o `msgen` cliente incluem: 

 |**Comando**          |  **Descrição do campo** |
 |:--------------------|:-------------         |
 |`list`               |Devolve uma lista de tarefas que ter submetido. Para obter argumentos, consulte `msgen help list`.  |
 |`submit`             |Envia um pedido de fluxo de trabalho para o serviço. Para obter argumentos, consulte `msgen help submit`.|
 |`status`             |Devolve o estado do fluxo de trabalho especificado pelo `--workflow-id`. Consulte também `msgen help status`. |
 |`cancel`             |Envia um pedido para cancelar o processamento do fluxo de trabalho especificado pelo `--workflow-id`. Consulte também `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Onde obter o valor para `--api-url-base`?
Aceda ao portal do Azure e abrir a página de conta Genomics. Sob o **gestão** cabeçalho, escolha **chaves de acesso**. Não existe, localizar o URL da API e as chaves de acesso.

## <a name="where-do-i-get-the-value-for---access-key"></a>Onde obter o valor para `--access-key`?
Aceda ao portal do Azure e abrir a página de conta Genomics. Sob o **gestão** cabeçalho, escolha **chaves de acesso**. Não existe, localizar o URL da API e as chaves de acesso.

## <a name="why-do-i-need-two-access-keys"></a>Por que motivo precisa de duas chaves de acesso?
Tem duas chaves de acesso no caso de que pretende atualizar (voltar a gerar)-las sem interromper a utilização do serviço. Por exemplo, pretende atualizar a chave primeiro. Nesse caso, mudar todos os novos fluxos de trabalho para a segunda chave a utilizar. Em seguida, aguarde até que os fluxos de trabalho já em execução com a primeira chave concluídos. Apenas, em seguida, atualize a chave.

## <a name="do-you-save-my-storage-account-keys"></a>Guardar minhas chaves de conta de armazenamento?
A chave de conta de armazenamento é utilizada para criar tokens de acesso de curto prazo para o serviço Microsoft Genomics ler os ficheiros de entrada e escrever ficheiros de saída. A duração do token predefinido é de 48 horas. A duração do token pode ser alterada com o `-sas/--sas-duration` opção do comando submeter; o valor está em horas.

## <a name="what-genome-references-can-i-use"></a>Referencia que genome pode utilizar?

Estas referências são suportadas:
 |Referência              | Valor de`-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (nenhuma análise alt) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Como se formatar os meus argumentos da linha de comandos como um ficheiro de configuração? 

msgen compreende os ficheiros de configuração no seguinte formato:
* Todas as opções são fornecidas como pares chave-valor com valores separados de chaves por ponto e vírgula.
Espaço em branco é ignorado.
* Linhas começam com `#` são ignorados.
* Argumento da linha de comandos no formato longo pode ser convertido para uma chave por stripping respetivos traços à esquerda e de substituir traços entre as palavras com carateres de sublinhado. Seguem-se alguns exemplos de conversão:

 |Argumento da linha de comandos            | Linha do ficheiro de configuração |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://URL*    |
 |`-k/--access-key KEY`            | *access_key:key*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Passos seguintes

Utilize os seguintes recursos para começar a utilizar o Microsoft Genomics:
- Introdução ao executar o fluxo de trabalho primeiro através do serviço Microsoft Genomics. [Executar um fluxo de trabalho através do serviço Microsoft Genomics](quickstart-run-genomics-workflow-portal.md)
- Submeter os seus próprios dados para processamento pelo serviço Microsoft Genomics: [emparelhado FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [FASTQ vários ou BAM](quickstart-input-multiple.md) 

