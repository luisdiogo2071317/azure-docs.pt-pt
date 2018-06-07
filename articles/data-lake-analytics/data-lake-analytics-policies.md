---
title: Gerir políticas de Azure Data Lake Analytics
description: Saiba como utilizar políticas para controlar a utilização de uma conta de Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 2c074b1a75c5bfef07ffb90e719bd3247a474e63
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623949"
---
# <a name="manage-azure-data-lake-analytics-using-policies"></a>Gerir o Azure Data Lake Analytics através de políticas

Utilizar políticas de conta, pode controlar como recursos de uma conta do Azure Data Lake Analytics são utilizados. Estas políticas permitem-lhe controlar o custo de utilização do Azure Data Lake Analytics. Por exemplo, com estas políticas podem impedir picos de custo inesperado ao limitar AUs quantos a conta pode utilizar em simultâneo.

## <a name="account-level-policies"></a>Políticas ao nível da conta

Estas políticas que se aplicam a todos os trabalhos numa conta do Data Lake Analytics.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Número máximo de AUs numa conta do Data Lake Analytics
Uma política controla o número total de análise de unidades (AUs), pode utilizar a sua conta do Data Lake Analytics. Por predefinição, o valor é definido para 250. Por exemplo, se este valor é definido para 250 AUs, pode fazer com que uma tarefa em execução com 250 AUs atribuídos a ou 10 tarefas em execução com 25 AUs cada. Tarefas adicionais que são submetidas são colocados em fila até terminar as tarefas em execução. Quando terminar tarefas em execução, AUs são libertados para as tarefas em fila para ser executada.

Para alterar o número de AUs para a sua conta do Data Lake Analytics:

1. No portal do Azure, aceda à sua conta do Data Lake Analytics.
2. Clique em **Propriedades**.
3. Em **AUs máximo**, mova o controlo de deslize para selecionar um valor ou introduza o valor na caixa de texto. 
4. Clique em **Guardar**.

> [!NOTE]
> Se precisar de mais do que a predefinição (250) AUs, no portal, clique em **ajuda + suporte** submeter um pedido de suporte. O número de AUs disponíveis na sua conta do Data Lake Analytics pode ser aumentado.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Número máximo de tarefas que podem ser executados em simultâneo
Uma política controla o número de tarefas podem executar em simultâneo. Por predefinição, este valor é definido para 20. Se a Data Lake Analytics tem AUs disponíveis, as novas tarefas agendadas para serem de imediato até que o número total de tarefas em execução atinge o valor desta política. Quando atingir o número máximo de tarefas que podem ser executados em simultâneo, as tarefas subsequentes são colocados em fila na ordem de prioridade até à conclusão de uma ou mais tarefas em execução (dependendo da disponibilidade de AU).

Para alterar o número de tarefas que podem ser executados em simultâneo:

1. No portal do Azure, aceda à sua conta do Data Lake Analytics.
2. Clique em **Propriedades**.
3. Em **máximo número de executar tarefas**, mova o controlo de deslize para selecionar um valor ou introduza o valor na caixa de texto. 
4. Clique em **Guardar**.

> [!NOTE]
> Se precisar de executar mais do que o número predefinido (20) de tarefas, no portal, clique em **ajuda + suporte** submeter um pedido de suporte. O número de tarefas que podem ser executados em simultâneo na sua conta do Data Lake Analytics pode ser aumentado.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>Quanto aos metadados de tarefa de manter e de recursos 
Quando os utilizadores executarem tarefas U-SQL, o serviço do Data Lake Analytics mantém relacionados todos os ficheiros. Ficheiros relacionados incluem o script U-SQL, os ficheiros DLL referenciados o script U-SQL, recursos compilados e estatísticas. Os ficheiros estão na pasta /system/ da conta de armazenamento do Azure Data Lake predefinida. Esta política controla quanto estes recursos são armazenados antes de estes sejam eliminados automaticamente (a predefinição é 30 dias). Pode utilizar estes ficheiros para depuração e para otimização de desempenho de tarefas que irá voltar a executar no futuro.

Para alterar o período de tempo para manter os metadados de tarefa e os recursos:

1. No portal do Azure, aceda à sua conta do Data Lake Analytics.
2. Clique em **Propriedades**.
3. Em **dias a manter a tarefa consulta**, mova o controlo de deslize para selecionar um valor ou introduza o valor na caixa de texto.  
4. Clique em **Guardar**.

## <a name="job-level-policies"></a>Políticas ao nível da tarefa

Com as políticas ao nível da tarefa, pode controlar os AUs máximos e que utilizadores individuais (ou membros dos grupos de segurança específicos), podem definir das tarefas que submetem de prioridade máxima. Esta política permite-lhe controlar os custos tarifas pelos utilizadores. Também permite-lhe controlo pode ter o efeito que tarefas agendadas no tarefas de produção de alta prioridade que estão em execução na mesma conta do Data Lake Analytics.

O Data Lake Analytics tem duas políticas que podem ser definidas ao nível da tarefa:

* **AU valor limite por tarefa**: os utilizadores apenas podem submeter as tarefas que têm cópia de segurança para este número de AUs. Por predefinição, este limite é o mesmo que o limite máximo de AU para a conta.
* **Prioridade**: os utilizadores apenas podem submeter as tarefas com prioridade inferior ou igual a este valor. Um número mais elevado indica uma prioridade mais baixa. Por predefinição, este limite está definido como 1, o que é a prioridade mais elevada possível.

Há uma política predefinida definida em cada conta. A política predefinida é aplicada a todos os utilizadores da conta. Pode definir políticas adicionais de utilizadores e grupos específicos. 

> [!NOTE]
> Políticas ao nível da conta e políticas ao nível da tarefa aplicam-se em simultâneo.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Adicionar uma política para um grupo ou utilizador específico

1. No portal do Azure, aceda à sua conta do Data Lake Analytics.
2. Clique em **Propriedades**.
3. Em **limites de submissão de tarefa**, clique em de **Adicionar política** botão. Em seguida, selecione ou introduza as seguintes definições:
    1. **Nome da política de computação**: introduza um nome de política, para relembrá-do objetivo da política.
    2. **Selecionar utilizador ou grupo**: selecione o utilizador ou grupo esta política aplica-se a.
    3. **Definir o limite de AU tarefa**: definir o limite de AU aplica-se para o utilizador ou grupo selecionado.
    4. **Definir o limite de prioridade**: definir o limite de prioridade que se aplica para o utilizador ou grupo selecionado.

4. Clique em **OK**.

5. A nova política é listada no **predefinido** tabela de política, em **limites de submissão de tarefa**. 

### <a name="delete-or-edit-an-existing-policy"></a>Eliminar ou editar uma política existente

1. No portal do Azure, aceda à sua conta do Data Lake Analytics.
2. Clique em **Propriedades**.
3. Em **limites de submissão de tarefa**, localizar a política que pretende editar.
4.  Para ver o **eliminar** e **editar** opções, na coluna mais à direita da tabela, clique em `...`.

## <a name="additional-resources-for-job-policies"></a>Recursos adicionais para políticas de tarefa
* [Mensagem de blogue de descrição geral de política](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Mensagem de blogue de políticas ao nível da conta](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Mensagem de blogue de políticas ao nível da tarefa](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral do Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Introdução ao Data Lake Analytics utilizando o portal do Azure](data-lake-analytics-get-started-portal.md)
* [Gerir o Azure Data Lake Analytics utilizando o Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

