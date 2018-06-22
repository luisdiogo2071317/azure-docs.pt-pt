## <a name="deploy-template-from-cloud-shell"></a>Implementar o modelo a partir do Cloud Shell

Pode utilizar o [Cloud Shell](../articles/cloud-shell/overview.md) para implementar o modelo. No entanto, deve primeiro carregar o modelo para a conta de armazenamento para a Shell de nuvem. Se não utilizou o Cloud Shell, consulte a [Descrição Geral do Azure Cloud Shell](../articles/cloud-shell/overview.md) para obter informações sobre como o configurar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione o grupo de recursos do Cloud Shell. O padrão de nome é `cloud-shell-storage-<region>`.

   ![Selecionar grupo de recursos](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Selecione a conta de armazenamento para o Cloud Shell.

   ![Selecionar a conta de armazenamento](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Selecione **Blobs**.

   ![Selecione os blobs](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Selecione **+ contentor**.

   ![Adicionar contentor](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Atribua o contentor de um nome e um nível de acesso. O modelo de exemplo neste artigo contém não existem informações confidenciais, por isso, permitem acesso de leitura anónimo. Selecione **OK**.

   ![Indique os valores de contentor](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Selecione o contentor que criou.

   ![Selecione o novo contentor](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. Selecione **Upload**.

   ![Carregar blob](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Localize e carregue o modelo.

   ![Carregar o ficheiro](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Depois de terem carregados, selecione o modelo.

   ![Selecione o novo modelo](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. Copie o URL.

   ![Copie o URL](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. Abra o prompt.

   ![Abrir o Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
