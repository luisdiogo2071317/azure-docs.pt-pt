---
title: Informações de desempenho de consulta para a base de dados SQL do Azure | Documentos da Microsoft
description: Monitorização do desempenho de consulta identifica a maioria das consultas de consumo de CPU para uma base de dados do SQL do Azure.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 5069b4e69c53ed93e9018cef2517f6125b838d12
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524170"
---
# <a name="azure-sql-database-query-performance-insight"></a>Informações de desempenho de consulta de base de dados SQL do Azure
Gerenciamento e ajuste do desempenho de bases de dados relacionais são uma tarefa desafiadora, que exige especialização significativa e investimento de tempo. O Query Performance Insight permite gastar menos tempo de resolução de problemas de desempenho de base de dados ao fornecer o seguinte:

* Informações mais aprofundadas sobre o consumo de recursos (DTUS) de bases de dados. 
* As consultas principais pela contagem de CPU/duração/execução, que potencialmente podem ser otimizadas para um melhor desempenho.
* A capacidade de desagregar os detalhes de uma consulta, ver o texto e o histórico de utilização de recursos. 
* Anotações que mostram as ações realizadas por ajuste de desempenho [Advisor de base de dados do SQL Azure](sql-database-advisor.md)  



## <a name="prerequisites"></a>Pré-requisitos
* Query Performance Insight requer que [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) está ativo na base de dados. Se não estiver a executar a consulta Store, o portal do pede-lhe para ativá-la.

## <a name="permissions"></a>Permissões
O seguinte procedimento [controlo de acesso baseado em funções](../role-based-access-control/overview.md) são necessárias permissões para utilizar o Query Performance Insight: 

* **Leitor**, **proprietário**, **contribuinte**, **contribuinte da BD SQL**, ou **contribuinte do SQL Server** são necessárias permissões Para ver o principal recurso consumir consultas e gráficos. 
* **Proprietário**, **contribuinte**, **contribuinte da BD SQL**, ou **contribuinte do SQL Server** são necessárias permissões para ver o texto de consulta.

## <a name="using-query-performance-insight"></a>Usando o Query Performance Insight
O Query Performance Insight é fácil de usar:

* Open [portal do Azure](https://portal.azure.com/) e encontrar o banco de dados que pretende examinar. 
  * No menu do lado esquerdo, em suporte e resolução de problemas, selecione "Query Performance Insight".
* No primeiro separador, reveja a lista de consultas de consumo de recursos principais.
* Selecione uma consulta individual para ver os detalhes.
* Open [Assistente de base de dados do SQL Azure](sql-database-advisor.md) e verifique se existem quaisquer recomendações.
* Utilizar controlos de deslize ou ícones para alterar o intervalo de observado de zoom.
  
    ![dashboard de desempenho](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Duas horas de dados tem de ser capturados pelo Store de consulta para a base de dados SQL fornecer informações de desempenho de consulta. Se a base de dados não tem nenhuma atividade ou Store de consulta não esteve ativo durante um determinado período de tempo, os gráficos estará vazios quando se apresenta esse período de tempo. Pode permitir Store de consulta em qualquer altura, se não está em execução.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Rever as principais consultas de consumo da CPU
Na [portal](http://portal.azure.com) faça o seguinte:

1. Navegue para uma base de dados SQL e clique em **todas as definições** > **suporte + resolução de problemas** > **consultar informações de desempenho**. 
   
    ![Query Performance Insight][1]
   
    Abre a vista de principais consultas e as principais consultas de consumo da CPU estão listadas.
2. Clique em todo o gráfico para obter detalhes.<br>A linha superior mostra % geral de DTU para bases de dados, enquanto as barras mostram a % de CPU consumida por consultas selecionadas durante o intervalo selecionado (por exemplo, se **semana passada** está selecionada cada barra representa um dia).
   
    ![consultas principais][2]
   
    A grade inferior representa informações agregadas para as consultas visível.
   
   * ID de consulta - o identificador exclusivo da consulta no interior da base de dados.
   * CPU por consulta durante o intervalo de observável (depende da função de agregação).
   * Duração por consulta (depende da função de agregação).
   * Número total de execuções para uma consulta específica.
     
     Marque ou desmarque as consultas individuais a incluir ou exclui-las no gráfico com caixas de verificação.
3. Se seus dados se torna obsoletos, clique nas **atualizar** botão.
4. Pode utilizar controlos de deslize e botões para alterar o intervalo de observação e investigar a picos de zoom: ![definições](./media/sql-database-query-performance/zoom.png)
5. Opcionalmente, se quiser uma exibição diferente, poderá selecionar **personalizado** separador e defina:
   
   * Métrica (CPU, duração, a contagem de execução)
   * Intervalo de tempo (últimas 24 horas, última semana, último mês). 
   * Número de consultas.
   * Função de agregação.
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Ver os detalhes de consulta individual
Para ver os detalhes de consulta:

1. Clique em qualquer consulta na lista de consultas principais.
   
    ![detalhes](./media/sql-database-query-performance/details.png)
2. Abre a vista de detalhes e a contagem de execução/duração/consumo de CPU de consultas é dividida ao longo do tempo.
3. Clique em todo o gráfico para obter detalhes.
   
   * Gráfico superior mostra a linha com geral % DTU da base de dados e as barras são % de CPU consumida pela consulta selecionada.
   * Segundo gráfico mostra a duração total pela consulta selecionada.
   * Gráfico de na parte inferior mostra o número total de execuções pela consulta selecionada.
     
     ![Detalhes da consulta][3]
4. Opcionalmente, utilizar controlos de deslize, botões de zoom ou clique em **definições** para personalizar a forma como são apresentados os dados de consulta ou escolha um período de tempo diferentes.

## <a name="review-top-queries-per-duration"></a>Rever as consultas principais por duração
Na recente atualização do Query Performance Insight, apresentamos duas novas métricas que podem ajudar a identificar potenciais estrangulamentos: contagem de duração e a execução.<br>

Consultas de execução longa tem o potencial de maior para bloquear os recursos mais tempo, bloqueando outros utilizadores além de limitar a escalabilidade. Também são os melhores candidatos para a otimização.<br>

Para identificar as consultas de execução longa:

1. Open **personalizado** separador no Query Performance Insight para a base de dados selecionada
2. Alterar as métricas para ser **duração**
3. Selecione o número de consultas e o intervalo de observação
4. Selecione a função de agregação
   
   * **Soma** soma todos os tempos de execução da consulta durante o intervalo de observação inteira.
   * **Máx.** localiza consultas que o tempo de execução foi máximo em intervalos de observação inteira.
   * **Média** encontra média de tempo de execução de todas as execuções de consulta e Mostrar parte superior fora essas médias. 
     
     ![Duração de consulta][4]

## <a name="review-top-queries-per-execution-count"></a>Rever as consultas principais por contagem de execução
Alto número de execuções não estar afetando próprio banco de dados e utilização de recursos pode ser baixa, mas poderá obter geral do aplicativo lenta.

Em alguns casos, a contagem de execução muito elevado pode levar a aumentar de ida e volta de rede. Ida e volta afetar significativamente o desempenho. Eles estão sujeitos a latência de rede e a latência de servidor a jusante. 

Por exemplo, muitos sites da Web orientados a dados muito acessar o banco de dados para cada solicitação de usuário. Embora a ligação de pool de ajuda, o tráfego de rede maior e processamento no servidor de base de dados pode afetar negativamente o desempenho.  Conselho Geral é manter ida e volta a um mínimo absoluto.

Para identificar frequentemente executadas consultas de consultas ("conversadoras"):

1. Open **personalizado** separador no Query Performance Insight para a base de dados selecionada
2. Alterar as métricas para ser **contagem de execução**
3. Selecione o número de consultas e o intervalo de observação
   
    ![Contagem de execução de consulta][5]

## <a name="understanding-performance-tuning-annotations"></a>Compreender as anotações de ajuste de desempenho
Ao explorar a carga de trabalho no Query Performance Insight, pode observar ícones com a linha vertical, na parte superior do gráfico.<br>

Estes ícones são anotações; eles representam o desempenho do que afetam a ações realizadas por [Assistente de base de dados do SQL Azure](sql-database-advisor.md). Ao anotação hovering, obtém informações básicas sobre a ação:

![anotação de consulta][6]

Se quiser saber mais ou aplicar a recomendação do assistente, clique no ícone. Ele irá abrir detalhes da ação. Se for uma recomendação Active Directory, que pode aplicá-lo imediatamente usando o comando.

![detalhes de anotação de consulta][7]

### <a name="multiple-annotations"></a>Vários anotações.
É possível que, devido ao nível de zoom, irão obter recolhidas anotações que estão próximos uns aos outros em um. Isso será representado pelo ícone especial, clicar nele irá abrir o painel nova em que a lista de agrupados anotações será mostrado.
Correlacionar consultas e ações de otimização de desempenho pode ajudar a compreender melhor a sua carga de trabalho. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Otimização da configuração de consulta Store para o Query Performance Insight
Durante a utilização do Query Performance Insight, pode encontrar as seguintes mensagens de consulta Store:

* "Store de consulta não está corretamente configurado nesta base de dados. Clique aqui para saber mais."
* "Store de consulta não está corretamente configurado nesta base de dados. Clique aqui para alterar as definições." 

Estas mensagens são apresentados normalmente quando a consulta Store não é possível recolher novos dados. 

Primeiro caso acontece quando consulta Store está no estado só de leitura e os parâmetros estão definidos de forma otimizada. Para corrigir este problema ao aumentar o tamanho de consulta Store ou limpar Store de consulta.

![botão de qds][8]

Segundo caso acontece quando consulta Store está desativada ou parâmetros não se encontram definidos de forma otimizada. <br>Pode alterar a política de retenção e a captura e ativar Store de consulta ao executar os comandos abaixo ou diretamente a partir do portal:

![botão de qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Política de retenção e captura recomendada
Existem dois tipos de políticas de retenção:

* Tamanho com base - se definida como AUTO-lo irá limpar dados automaticamente quando o próximo tamanho máximo é atingido.
* Hora baseada - por predefinição será o definimos como 30 dias, o que significa que, se a consulta Store irá ficar sem espaço, vai eliminar informações de consulta com mais de 30 dias

Captura de política pode ser definida como:

* **Todos os** -captura todas as consultas.
* **Automática** -consultas pouco frequentes e consultas com a duração de compilação e execução insignificante são ignoradas. Os limites de duração de contagem, compilação e tempo de execução de execução internamente são determinados. Esta é a opção predefinida.
* **Nenhum** -Query Store deixa de captura de novas consultas, no entanto, estatísticas de tempo de execução para consultas já capturadas são ainda recolhidas.

Recomendamos que todas as políticas de definição de política limpa a 30 dias e automático:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Aumente o tamanho de consulta Store. Isso pode ser realizado ao ligar a uma base de dados e emitir a seguinte consulta:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Aplicar estas definições, eventualmente, fará Store de consulta recolher consultas novo, no entanto, se não quiser esperar pode desmarcar Store de consulta. 

> [!NOTE]
> Execução de consulta seguinte irá eliminar todas as informações atuais no Store de consulta. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Resumo
O Query Performance Insight ajuda-o a compreender o impacto da sua carga de trabalho de consulta e como se relaciona com consumo de recursos da base de dados. Com esse recurso, irá saber mais sobre as principais consultas de consumo e identificar facilmente os corrigir antes que eles se tornem um problema.

## <a name="next-steps"></a>Passos Seguintes
Para obter recomendações adicionais sobre como melhorar o desempenho da base de dados SQL, clique em [recomendações](sql-database-advisor.md) sobre o **Query Performance Insight** painel.

![Assistente de desempenho](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

