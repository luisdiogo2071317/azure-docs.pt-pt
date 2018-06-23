---
title: LogDownloader - serviços cognitivos do Azure | Microsoft Docs
description: Transferir ficheiros de registo que são produzidos pelo serviço de decisão personalizada do Azure.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 783b534b3b3f4bb7f5d9f073f491690759edfea5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354553"
---
# <a name="logdownloader"></a>LogDownloader

Transferir ficheiros de registo que são produzidos pelo serviço de decisão personalizada do Azure e geram o *.gz* ficheiros que são utilizados por experimentação.

## <a name="prerequisites"></a>Pré-requisitos

- Python 3: Instalar e no seu caminho. Recomendamos que a versão de 64 bits para processar ficheiros grandes.
- O *mwt/Microsoft-ds* repositório: [Clone o repositório](https://github.com/Microsoft/mwt-ds).
- O *blob de armazenamento do azure* pacote: para obter detalhes de instalação, aceda a [biblioteca de armazenamento do Microsoft Azure para Python](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Introduza a cadeia de ligação de armazenamento do Azure no *mwt-ds/DataScience/ds.config*: Siga a *my_app_id: my_connectionString* modelo. Pode especificar vários `app_id`. Quando executa `LogDownloader.py`, se a entrada `app_id` não se encontra na `ds.config`, `LogDownloader.py` utiliza o `$Default` cadeia de ligação.

## <a name="usage"></a>Utilização

Aceda a `mwt-ds/DataScience` e execute `LogDownloader.py` com os argumentos relevantes, conforme detalhado no seguinte código:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parâmetros

| Input | Descrição | Predefinição |
| --- | --- | --- |
| `-h`, `--help` | Mostra a mensagem de ajuda e sair. | |
| `-a APP_ID`, `--app_id APP_ID` | O ID da aplicação (ou seja, o Storage do Azure blob nome do contentor). | Necessário |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | O diretório de base para transferência de dados (uma subpasta é criada).  | Necessário |
| `-s START_DATE`, `--start_date START_DATE` | A transferência data (incluída), além de início *aaaa-MM-DD* formato. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | A transferir data de fim (incluída), no *aaaa-MM-DD* formato. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | O modo de substituir a utilizar. | |
| | `0`: Substituir nunca; pedir ao utilizador se blobs estão atualmente a ser utilizados. | Predefinição | |
| | `1`: Pedir ao utilizador como proceder quando os ficheiros têm tamanhos diferentes ou quando os blobs estejam a ser utilizados. | |
| | `2`: Substituir sempre; Transferir blobs atualmente utilizados. | |
| | `3`: Substituir nunca e de acréscimo se o tamanho for superior, sem pedir; Transferir blobs atualmente utilizados. | |
| | `4`: Substituir nunca e de acréscimo se o tamanho for superior, sem pedir; Ignore os blobs em utilização. | |
| `--dry_run` | Imprimir os blobs seriam tiverem sido transferidos, sem a transferir. | `False` |
| `--create_gzip` | Criar um *gzip* ficheiro para o Vowpal Wabbit. | `False` |
| `--delta_mod_t DELTA_MOD_T` | A janela de tempo, em segundos, para detetar se um ficheiro está atualmente em utilização. | `3600` seg (`1` hora) |
| `--verbose` | Impressão mais detalhes. | `False` |
| `-v VERSION`, `--version VERSION` | A versão de transferência de registo a utilizar. | |
| | `1`: Para obter registos uncooked (apenas para compatibilidade com versões anteriores). | Preterido |
| | `2`: Para os registos de cooked. | Predefinição |

### <a name="examples"></a>Exemplos

Para executar um dry de transferir todos os dados no contentor de blob Storage do Azure, utilize o seguinte código:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Para transferir apenas os registos criados desde 1 de Janeiro de 2018 com `overwrite_mode=4`, utilize o seguinte código:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Para criar um *gzip* ficheiro a intercalar todos os ficheiros transferidos, utilize o seguinte código:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```