## <a name="create-a-storage-account-using-the-azure-portal"></a>Criar uma conta de armazenamento através do portal do Azure

Em primeiro lugar, crie uma nova conta de armazenamento para fins gerais para utilizar este guia de introdução. 

1. Entre no [Portal do Azure](https://portal.azure.com) e inicie sessão com a sua conta do Azure. 
2. No menu Hub, selecione **Novo** > **Armazenamento** > **Conta de armazenamento - blob, ficheiro, tabela, fila**. 
3. Introduza um nome único para a conta de armazenamento. Mantenha estas regras em mente para atribuir um nome à sua conta de armazenamento:
    - O nome tem de ter entre 3 e 24 carateres de comprimento.
    - O nome pode conter apenas letras minúsculas e números.
4. Certifique-se de que os seguintes valores predefinidos estão definidos: 
    - O **Modelo de implementação** é definido como **Resource manager**.
    - O **Tipo de conta** é definido como **Fins gerais**.
    - O **Desempenho** é definido como **Standard**.
    - A **Replicação** é definida como **Armazenamento Localmente Redundante (LRS)**.
5. Selecione a sua subscrição. 
6. Para o **Grupo de recursos**, crie um novo e atribua um nome exclusivo. 
7. Selecione a **Localização** a utilizar na sua conta de armazenamento.
8. Assinale **Afixar ao dashboard** e clique em **Criar** para criar a conta de armazenamento. 

Depois de criar a conta de armazenamento, esta é afixada ao dashboard. Clique nela para abri-la. Em **Definições**, clique em **Chaves de acesso**. Selecione a chave primária e copie a **Cadeia de ligação** associada para a área de transferência e, em seguida, cole-a num editor de texto para utilização posterior.
