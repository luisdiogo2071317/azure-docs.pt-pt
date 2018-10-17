---
title: Utilizar identidades geridas para autenticar as tarefas do Azure Stream Analytics à saída de geração 1 de Lake armazenamento de dados do Azure (pré-visualização)
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2018
ms.openlocfilehash: 41b3dcc03f7cfbfee11798738a3b2daaf5e96741
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365293"
---
# <a name="use-managed-identities-to-authenticate-azure-stream-analytics-jobs-to-azure-data-lake-storage-gen1-output-preview"></a>Utilizar identidades geridas para autenticar as tarefas do Azure Stream Analytics à saída de geração 1 de Lake armazenamento de dados do Azure (pré-visualização)

O Azure Stream Analytics suporta a autenticação de identidade gerida com a saída de geração 1 do Azure Data Lake Storage (ADLS). A identidade é um aplicativo gerenciado registado no Azure Active Directory que representa uma determinada tarefa do Stream Analytics e pode ser utilizada para autenticar a um recurso de destino. Identidades geridas eliminam as limitações dos métodos de autenticação baseada no utilizador, como a necessidade de autenticar devido a alterações de palavra-passe ou as Expirações de token de utilizador que ocorrem a cada 90 dias. Além disso, ajudam a identidades geridas com a automatização de implementações de tarefa do Stream Analytics essa saída a geração 1 de armazenamento do Azure Data Lake.

Visite o [oito novos recursos do Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) mensagem de blogue para se inscrever para esta pré-visualização e ler mais sobre as novas funcionalidades.

Este artigo mostra-lhe duas formas de ativar a identidade gerida para uma tarefa do Azure Stream Analytics que gera a saída para um Gen1 de armazenamento do Azure Data Lake: por meio do portal do Azure e implementação do modelo do Azure Resource Manager.

## <a name="enable-managed-identity-with-azure-portal"></a>Ativar a identidade gerido com o portal do Azure

1. Comece por criar uma nova tarefa de Stream Analytics ou ao abrir uma tarefa existente no portal do Azure. Na barra de menus localizada no lado esquerdo do ecrã, selecione **identidade gerida (pré-visualização)** localizada sob **configurar**.

   ![Configurar a identidade gerida de Stream Analytics pré-visualização](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Selecione **atribuído ao sistema de utilização identidade gerida (pré-visualização)** da janela que aparece à direita. Clique em **guardar** para criar um principal de serviço para a identidade da tarefa do Stream Analytics no Azure Active Directory. O ciclo de vida da identidade criado recentemente será gerido pelo Azure. Quando a tarefa do Stream Analytics é eliminada, a identidade associada (ou seja, o principal de serviço) é eliminada automaticamente pelo Azure.

   Quando a configuração é guardada, o ID de objeto (OID) de principal de serviço está listado como ID de Principal, conforme mostrado abaixo:

   ![ID de Principal de análise do Stream](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   O principal de serviço tem o mesmo nome que a tarefa do Stream Analytics. Por exemplo, se o nome da sua tarefa é **MyASAJob**, o nome do principal de serviço criado também está **MyASAJob**.

3. Na janela de propriedades de saída do sink de saída do ADLS Gen1, clique em baixo e selecione o modo de autenticação **identidade gerida (pré-visualização)**.

4. Preencha o resto das propriedades. Para saber mais sobre como criar uma saída do ADLS, veja [criar uma saída de Data lake Store com o stream analytics](../data-lake-store/data-lake-store-stream-analytics.md). Quando tiver terminado, clique em **guardar**.

   ![Configurar o armazenamento do Azure Data Lake](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Navegue para a página de descrição geral do seu Gen1 do ADLS e clique em **Explorador de dados**.

   ![Configurar a descrição geral do Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. No painel do Explorador de dados, selecione **acesso** e clique em **Add** no painel de acesso.

   ![Configurar o acesso do Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Na caixa de texto a **selecionar utilizador ou grupo** painel, escreva o nome do principal de serviço. Lembre-se de que o nome do principal de serviço também é o nome da tarefa de Stream Analytics correspondente. Quando começar a escrever o nome do principal, ele será exibido abaixo da caixa de texto. Escolha o nome principal de serviço desejado e clique em **selecione**.

   ![Selecione um nome principal de serviço](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. Na **permissões** painel, verifique o **escrever** e **Execute** permissões e atribuí-lo ao **esta pasta e todos os filhos**. Em seguida, clique em **Ok**.

   ![Selecione uma permissão](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. O principal de serviço está listado em **permissões atribuídas** sobre o **acesso** painel, conforme mostrado abaixo. Agora pode voltar atrás e iniciar a tarefa de Stream Analytics.

   ![Lista de acesso](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Para saber mais sobre permissões do sistema de ficheiros de geração 1 de armazenamento do Data Lake, veja [controlo de acesso no Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="resource-manager-template-deployment"></a>Implementação de modelo do Resource Manager

1. Pode criar uma *Microsoft.StreamAnalytics/streamingjobs* recursos com uma identidade gerida, incluindo a seguinte propriedade na seção de recurso do modelo do Resource Manager:

   ```json
   "Identity": {
   "Type": "SystemAssigned",
   },
   ```

   Esta propriedade diz ao Azure Resource Manager para criar e gerir a identidade para a sua tarefa do Azure Stream Analytics.

   **Tarefa de exemplo**

   ```json
   { 
   "Name": "AsaJobWithIdentity", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
     "Type": "SystemAssigned", 
     }, 
   "properties": {
      "sku": {
       "name": "standard"
       },
   "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {        
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": “myDataLakeAccountName",
                 "filePathPrefix": “cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
                 }
                 
   }
   ```
  
   **Resposta de tarefa de exemplo**

   ```json
   { 
   "Name": "mySAJob", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
   "Type": "SystemAssigned",
    "principalId": "GUID", 
    "tenantId": "GUID", 
   }, 
   "properties": {
           "sku": {
             "name": "standard"
           },
   }
   ```

   Tome nota do ID Principal da resposta de tarefa para conceder acesso ao recurso do ADLS necessário.

   O **ID de inquilino** é o ID do inquilino do Azure Active Directory em que o principal de serviço é criado. O principal de serviço é criado no inquilino do Azure que seja considerada fidedigna pela subscrição.

   O **tipo** indica o tipo de identidade gerida, conforme explicado em tipos de identidades geridas. Apenas o tipo de sistema atribuído é suportado.

2. Fornece acesso ao principal de serviço com o PowerShell. Para dar acesso ao serviço principal através do PowerShell, execute o seguinte comando:

   ```powershell
   Set-AzureRmDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   O **PrincipalId** é o ID de objeto do principal de serviço e está listado no ecrã do portal, uma vez criado o principal de serviço. Se criou a tarefa através de uma implementação de modelo do Resource Manager, o ID de objeto é listado na propriedade de identidade da resposta de tarefa.

   **Exemplo**

   ```powershell
   PS > Set-AzureRmDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Para saber mais sobre o comando acima do PowerShell, consulte a [Set-AzureRmDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/azurerm.datalakestore/set-azurermdatalakestoreitemaclentry?view=azurermps-6.8.1&viewFallbackFrom=azurermps-4.2.0#optional-parameters) documentação.

## <a name="next-steps"></a>Passos Seguintes

* [Criar uma saída de Data lake Store com o stream analytics](../data-lake-store/data-lake-store-stream-analytics.md)
