


## <a name="attach-an-empty-disk"></a>Anexar um disco vazio
Anexar um disco vazio é uma forma simples de adicionar um disco de dados, porque o Azure cria o ficheiro. vhd para e armazena-a na conta de armazenamento.

1. Clique em **máquinas virtuais (clássicas)** e, em seguida, selecione a VM adequada.

2. No menu de definições, clique em **discos**.

   ![Anexar um disco novo vazio](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Na barra de comandos, clique em **anexar novo**.  
    O **anexar novo disco** é apresentada a caixa de diálogo.

    ![Anexar um disco novo](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Preencha as seguintes informações:
    - Na **nome de ficheiro**, aceite o nome predefinido ou insira um outro para o ficheiro. vhd. O disco de dados utiliza um nome gerado automaticamente, mesmo que escreva outro nome para o ficheiro. vhd.
    - Selecione o **tipo** do disco de dados. Todas as máquinas virtuais suportam os discos standard. Número de máquinas virtuais também suporta discos premium.
    - Selecione o **tamanho (GB)** do disco de dados.
    - Para **colocação em cache do anfitrião**, escolha none ou só de leitura.
    - Clique em OK para concluir.

4. Depois do disco de dados é criado e ligado, é listado na seção discos da VM.

   ![Disco de dados novo e vazio ligado com êxito](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Depois de adicionar um disco de dados, terá de iniciar sessão na VM e inicializar o disco para que possa ser utilizado.

## <a name="how-to-attach-an-existing-disk"></a>Como: anexar um disco existente
Para anexar um disco existente, tem de ter um .vhd disponível numa conta de armazenamento. Utilize o [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet para carregar o ficheiro. vhd para a conta de armazenamento. Depois de ter criado e carregado o ficheiro. vhd, pode anexar a uma VM.

1. Clique em **máquinas virtuais (clássicas)** e, em seguida, selecione a máquina virtual adequado.

2. No menu de definições, clique em **discos**.

3. Na barra de comandos, clique em **anexar existente**.

    ![Anexar disco de dados](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Clique em **localização**. Apresentam as contas de armazenamento disponível. Em seguida, selecione uma conta de armazenamento adequado aos listados.

    ![Forneça a conta de armazenamento de disco](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. R **conta de armazenamento** contém um ou mais contentores que contêm as unidades de disco (vhds). Selecione o contentor adequado aos listados.

    ![Fornecer o contentor do windows de máquinas virtuais](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. O **vhds** painel lista as unidades de disco mantidas no contêiner. Clique em um dos discos e, em seguida, clique em selecionar.

    ![Fornecer a imagem de disco para o windows de máquinas virtuais](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. O **anexar disco existente** painel apresenta mais uma vez, com a localização que contém a conta de armazenamento, o contentor e o disco rígido selecionado (vhd) para adicionar a máquina virtual.

  Definir **colocação em cache do anfitrião** como none ou leitura apenas, em seguida, clique em OK.

    ![Disco de dados anexado com êxito](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
