## <a name="create-a-storage-account-by-using-the-azure-portal"></a>Criar uma conta de armazenamento utilizando o portal do Azure

Em primeiro lugar, crie uma nova conta de armazenamento para fins gerais para utilizar este guia de introdução. 

1. Vá para o [portal do Azure](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) e inicie sessão com a sua conta do Azure. 
2. Introduza um nome único para a conta de armazenamento. Mantenha estas regras em mente para atribuir um nome à sua conta de armazenamento:
    - O nome tem de ser 3 a 24 carateres de comprimento.
    - O nome pode conter apenas letras minúsculas e números.
3. Certifique-se de que os seguintes valores predefinidos estão definidos: 
    - **Modelo de implementação** está definido como **Resource Manager**.
    - O **Tipo de conta** é definido como **Fins gerais**.
    - O **Desempenho** é definido como **Standard**.
    - A **Replicação** é definida como **Armazenamento Localmente Redundante (LRS)**.
4. Selecione a sua subscrição. 
5. Para o **Grupo de recursos**, crie um novo e atribua um nome exclusivo. 
6. Selecione a localização a utilizar para a sua conta de armazenamento.
7. Selecione **afixar ao dashboard** e selecione **criar** para criar a conta de armazenamento. 

Depois de criar a sua conta de armazenamento, é afixado ao dashboard. Selecione-o para abri-lo. Em **definições**, selecione **chaves de acesso**. Selecione a chave primária e copie a cadeia de ligação associado à área de transferência. Em seguida, cole a cadeia para um editor de texto para utilização posterior.
