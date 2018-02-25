---
title: "Como adicionar um conjunto de dados de referência para o seu ambiente de informações de séries de tempo do Azure"
description: "Este artigo descreve como adicionar um conjunto de dados de referência para aumentar os dados no seu ambiente de informações de séries de tempo do Azure."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/15/2018
ms.openlocfilehash: e0d11f253d5aa143ff636c4dc8dff7665a80360e
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Criar um conjunto de dados de referência para o seu ambiente de informações de séries de tempo no portal do Azure

Este artigo descreve como adicionar um conjunto de dados de referência para o seu ambiente de informações de séries de tempo do Azure. Dados de referência são útil para associar a sua origem de dados para aumentar os valores.

Um conjunto de dados de referência é uma coleção de itens que aumentar os eventos a partir da sua origem de evento. Motor de entrada de informações de séries de tempo associa cada evento da sua origem de evento com a linha de dados correspondente no seu conjunto de dados de referência. Esse evento aumentado fica então disponível para consulta. Esta associação é baseada em colunas de chave primária definidas no seu conjunto de dados de referência.

Dados de referência não estão associados retroactively. Isto significa que apenas os dados de entrada atuais e futuras são correspondidos e associados ao conjunto de datas de referência, uma vez que foi configurado e carregado.

## <a name="add-a-reference-data-set"></a>Adicionar um conjunto de dados de referência

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Localize o seu ambiente existente do Insights de séries de tempo. Clique em **todos os recursos** no menu no lado esquerdo do portal do Azure. Selecione o seu ambiente do Time Series Insights.

3. Selecione o **descrição geral** página. Localize o **URL do Explorador do Insights de séries de tempo** e abrir a ligação.  

   Ver o Explorador para o seu ambiente TSI.

4. Expanda o Seletor de ambiente no Explorador de TSI. Escolha o ambiente do Active Directory. Selecione o ícone de dados de referência no canto superior direito da página do Explorador.

   ![Adicionar dados de referência](media/add-reference-data-set/add_reference_data.png)

5. Selecione o **+ adicionar um conjunto de dados** botão para começar a adicionar um novo conjunto de dados.

   ![Adicionar o conjunto de dados](media/add-reference-data-set/add_data_set.png)

6. No **novo conjunto de dados de referência** página, escolha o formato dos dados: 
   - Escolha **CSV** para dados delimitada por vírgulas. A primeira linha é tratada como uma linha de cabeçalho. 
   - Escolha **matriz JSON** para javascript notação de objeto (JSON) formatado dados.

   ![Escolha o formato de dados.](media/add-reference-data-set/add_data.png)

7. Fornece os dados, utilizando um dos dois métodos:
   - Cole os dados para o editor de texto. Em seguida, selecione **dados de referência de análise** botão.
   - Selecione **Escolher ficheiro** botão para adicionar dados a partir de um ficheiro de texto local. 

   Por exemplo, colar os dados CSV: ![dados colados CSV](media/add-reference-data-set/csv_data_pasted.png)

   Por exemplo, cole os dados de matriz JSON: ![dados colar JSON](media/add-reference-data-set/json_data_pasted.png)

   Se existir um erro ao analisar os valores de dados, aparece o erro vermelho na parte inferior da página, tal como `CSV parsing error, no rows extracted`.

8. Depois dos dados com êxito são analisados, uma grelha de dados é apresentada a apresentar as colunas e linhas que representam os dados.  Reveja a grelha de dados para se certificar de que está correto.

   ![Adicionar dados de referência](media/add-reference-data-set/parse_data.png)

9. Reveja cada coluna para o tipo de dados, pressupõe-se de ver e alterar o tipo de dados, se necessário.  Selecione o símbolo de tipo de dados no cabeçalho da coluna:  **#**  para duplo (dados numéricos), **T | F** para boolean, ou **Abc** cadeia.

   ![Escolha tipos de dados no cabeçalho da coluna.](media/add-reference-data-set/choose_datatypes.png)

10. Se for necessário, mude o nome os cabeçalhos de coluna. O nome da coluna chave é necessário para efetuar a adesão à propriedade correspondente na sua origem de evento. Certifique-se de que os nomes de coluna chave de dados de referência correspondem exatamente ao nome do evento aos seus dados de entrada, incluindo sensibilidade. Os nomes de coluna sem chave são utilizados para melhorar os dados de entrada com valores de dados de referência correspondentes.

11. Clique em **adicionar uma linha** ou **adicionar uma coluna** para adicionar mais valores de dados de referência, conforme necessário.

12. Escreva um valor no **filtrar as linhas...**  campo para rever linhas específicas, conforme necessário. O filtro é útil para rever dados, mas não se aplica ao carregar os dados.
 
13. Nome do conjunto de dados, ao preencher a **o nome do conjunto de dados** campo acima grelha de dados.

   ![Nome do conjunto de dados.](media/add-reference-data-set/name_reference_dataset.png)

14. Forneça o **chave primária** colunas no conjunto de dados, selecionando a lista pendente acima grelha de dados.

   ![Selecione as colunas chaves.](media/add-reference-data-set/set_primary_key.png)

   Opcionalmente, selecione o  **+**  botão para adicionar uma coluna chave secundária, como uma chave primária composta. Se precisar de anular a seleção, escolha o valor vazio na lista pendente para remover a chave secundária.

15.  Para carregar os dados, selecione o **carregar linhas** botão.

   ![Carregar](media/add-reference-data-set/upload_rows.png)

   A página confirma o concluída carregar e apresenta a mensagem **carregado com êxito o conjunto de dados**.

## <a name="next-steps"></a>Passos Seguintes
* [Gerir dados de referência](time-series-insights-manage-reference-data-csharp.md) através de programação.
* Para obter a referência completa de API, consulte o documento [API de dados de referência](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
