---
title: 'Início Rápido: Consultar dados através da biblioteca Python do Azure Data Explorer'
description: Neste início rápido, irá aprender a consultar dados a partir do Azure Data Explorer com Python.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: fee982812456548ed6d1e15d86151df88532389f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956103"
---
# <a name="quickstart-query-data-using-the-azure-data-explorer-python-library"></a>Início Rápido: Consultar dados através da biblioteca Python do Azure Data Explorer

O Azure Data Explorer é um serviço de exploração de dados rápido e altamente dimensionável para dados telemétricos e de registo. O Azure Data Explorer fornece uma [biblioteca de cliente de dados ao Python](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Esta biblioteca permite-lhe consultar dados a partir do código. Neste início rápido, vai estabelecer ligação a uma tabela no *cluster de ajuda* que configurámos para ajudar na aprendizagem. Em seguida, pode consultar uma tabela nesse cluster e devolver os resultados.

Este início rápido também está disponível como um [Bloco de Notas do Azure](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueryKusto.ipynb).

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta de e-mail organizacional que seja membro do Azure Active Directory (AAD)

* [Python](https://www.python.org/downloads/) instalado no seu computador de desenvolvimento

## <a name="install-the-data-library"></a>Instalar a biblioteca de dados

Instale *azure-kusto-data*.

```python
pip install azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Adicionar declarações e constantes de importação

Importe classes de biblioteca, bem como o *pandas*, uma biblioteca de análise de dados.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
import pandas as pd
```

Para autenticar uma aplicação, o Azure Data Explorer utiliza o ID de inquilino do AAD. Para localizar o ID de inquilino, utilize o seguinte URL, substituindo o domínio pelo *SeuDomínio*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Por exemplo, se o seu domínio for *contoso.com*, o URL é: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Clique neste URL para ver os resultados; a primeira linha é igual à seguinte. 

```
`"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"`
```

Neste caso, o ID de inquilino é `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. Defina o valor para AAD_TENANT_ID antes de executar este código.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_CLUSTER = "https://help.kusto.windows.net/"
KUSTO_DATABASE  = "Samples"
```

Agora construa a cadeia de ligação. Este exemplo utiliza a autenticação do dispositivo para aceder ao cluster. Também pode utilizar o certificado de aplicação do AAD, a chave de aplicação do AAD e o utilizador e a palavra-passe do AAD.

```python
KCSB = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_CLUSTER)
KCSB.authority_id = AAD_TENANT_ID
```

## <a name="connect-to-azure-data-explorer-and-execute-a-query"></a>Ligar ao Azure Data Explorer e executar uma consulta

Executar uma consulta contra o cluster e armazenar o resultado num pacote de dados. Quando este código é executado, devolve uma mensagem semelhante à seguinte: *Para iniciar sessão, utilize um browser para abrir a página https://microsoft.com/devicelogin e introduza o código F3W4VWZDM para autenticar*. Siga os passos para iniciar sessão e volte para executar o bloco de código seguinte.

```python
KUSTO_CLIENT  = KustoClient(KCSB)
KUSTO_QUERY  = "StormEvents | sort by StartTime desc | take 10"

df = KUSTO_CLIENT.execute_query(KUSTO_DATABASE, KUSTO_QUERY).primary_results[0].to_dataframe()
```

## <a name="explore-data-in-dataframe"></a>Explorar dados em DataFrame

Depois de introduzir um início de sessão, a consulta devolve resultados e estes são armazenados num pacote de dados. Pode trabalhar com os resultados, tal como faz com qualquer outro pacote de dados.

```python
df
```

Deve ver os dez principais resultados da tabela StormEvents.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Início Rápido: ingerir dados através da biblioteca Python do Azure Data Explorer](python-ingest-data.md)