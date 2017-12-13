Agora, pode utilizar a ferramenta Data Explorer no portal do Azure para criar uma base de dados e uma tabela. 

1. Clique em **Data Explorer** > **Nova Tabela**. 
    
    A área **Adicionar Tabela** é apresentada na extremidade direita, pelo que poderá ter de se deslocar para a direita para vê-la.

    ![Data Explorer no portal do Azure](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. Na página **Adicionar Tabela**, introduza as definições para a nova tabela.

    Definição|Valor sugerido|Descrição
    ---|---|---
    ID da tabela|tabela de exemplo|O ID da sua nova tabela. Os nomes das tabelas têm os mesmos requisitos de carateres que os IDs das bases de dados. Os nomes das bases de dados têm de ter entre um e 255 carateres e não podem conter `/ \ # ?` nem espaços à direita.
    Capacidade de armazenamento| Fixa (10 GB)|Altere o valor para **Fixo (10 GB)**. Este valor é a capacidade de armazenamento da base de dados.
    Débito|400 RUs|Altere o débito para 400 unidades de pedido por segundo (RU/s). Se pretender reduzir a latência, pode aumentar o débito mais tarde.

    Clique em **OK**.

    O Data Explorer mostra a base de dados e a tabela novas.

    ![O Data Explorer do portal do Azure a mostrar a base de dados e a coleção novas](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)