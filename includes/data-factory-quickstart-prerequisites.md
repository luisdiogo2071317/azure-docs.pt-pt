## <a name="prerequisites"></a>Pré-requisitos

### <a name="azure-subscription"></a>Subscrição do Azure
Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

### <a name="azure-roles"></a>Funções do Azure
Para criar instâncias do Data Factory, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ser um membro das funções **contribuidor** ou **proprietário**, ou um **administrador** da subscrição do Azure. No portal do Azure, clique no seu **nome de utilizador** no canto superior direito e selecione **Permissões** para ver as permissões que tem na subscrição. Se tiver acesso a várias subscrições, selecione a subscrição apropriada. Para obter instruções de exemplo sobre como adicionar um utilizador a uma função, veja o artigo [Add roles](../articles/billing/billing-add-change-azure-subscription-administrator.md) (Adicionar funções).

### <a name="azure-storage-account"></a>Conta de Armazenamento do Azure
Neste início rápido, vai utilizar uma Conta de Armazenamento do Azure para fins gerais (especificamente o Armazenamento de Blobs) como arquivo de dados de **origem** e de **destino**. Se não tiver uma conta de armazenamento do Azure para fins gerais, veja [Criar uma conta de armazenamento](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account), que explica como criar uma. 

#### <a name="get-storage-account-name-and-account-key"></a>Obter o nome e a chave da conta de armazenamento
Utilize o nome e a chave da sua conta de armazenamento do Azure neste início rápido. O procedimento seguinte disponibiliza os passos para obter o nome e a chave da conta de armazenamento. 

1. Abra um browser e navegue para o [portal do Azure](https://portal.azure.com). Inicie sessão com o seu nome de utilizador e a sua palavra-passe do Azure. 
2. Clique em **Mais serviços >**, no menu do lado esquerdo, filtre com a palavra-chave **Armazenamento** e selecione **Contas de armazenamento**.

    ![Procurar contas de armazenamento](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. Na lista das contas de armazenamento, filtre para encontrar a sua conta de armazenamento (se necessário) e selecione **a sua conta de armazenamento**. 
4. Na página **Conta de armazenamento**, selecione **Chaves de acesso**, no menu.

    ![Obter o nome e a chave da conta de armazenamento](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. Copie os valores dos campos **Nome da conta de armazenamento** e **Key1** para a área de transferência. Cole-os num bloco de notas ou noutro editor e guarde-os. Vai utilizá-los mais tarde neste início rápido.   

#### <a name="create-input-folder-and-files"></a>Criar pasta e ficheiros de entrada
Nesta secção, vai criar um contentor de blobs com o nome **adftutorial** no seu armazenamento de blobs do Azure. Depois, vai criar uma pasta chamada **entrada** no contentor e, em seguida, vai carregar um ficheiro de exemplo para essa pasta. 

1. Na página **Conta de armazenamento**, mude para a **Descrição Geral** e, em seguida, clique em **Blobs**. 

    ![Selecionar a opção Blobs](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. Na página **Serviço Blob** página, clique em **+ Contentor** na barra de ferramentas. 

    ![Adicionar o botão de contentor](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. Na caixa de diálogo **Novo contentor**, introduza **adftutorial** para o nome e clique em **OK**. 

    ![Introduzir o nome do contentor](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. Clique em **adftutorial** na lista de contentores. 

    ![Selecionar o contentor](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. Na página **Contentor**, clique em **Carregar** na barra de ferramentas.  

    ![Botão Carregar](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. Na página **Carregar blob**, clique em **Avançadas**.

    ![Clicar na ligação Avançadas](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. Inicie o **Bloco de Notas** e crie um ficheiro chamado **emp.txt** com o seguinte conteúdo: Guarde-o na pasta **c:\ADFv2QuickStartPSH**: Crie a pasta **ADFv2QuickStartPSH** caso esta ainda não exista.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. No portal do Azure, na página **Carregar blob**, procure e selecione o ficheiro **emp.txt** para o campo **Ficheiros**. 
9. Introduza **entrada** como um valor no campo **Carregar para a pasta**. 

    ![Carregar definições de blob](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Confirme se a pasta é **entrada** e se o ficheiro é **emp.txt**, e clique em **Carregar**.
11. Deverá ver o ficheiro **emp.txt** e o estado do carregamento na lista. 
12. Feche a página **Carregar blob** clicando no **X** no canto. 

    ![Fechar a página Carregar blob](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. Mantenha a página do **contentor** aberta. Vai utilizá-la para verificar a saída no final deste início rápido.