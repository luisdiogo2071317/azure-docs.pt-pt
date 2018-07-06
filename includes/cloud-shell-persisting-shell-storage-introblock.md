# <a name="persist-files-in-azure-cloud-shell"></a>Manter os ficheiros no Azure Cloud Shell
Cloud Shell utiliza o armazenamento de ficheiros do Azure para manter os ficheiros em sessões.

## <a name="set-up-a-clouddrive-file-share"></a>Configurar uma partilha de ficheiros clouddrive
No início inicial, o Cloud Shell pede-lhe para associar uma partilha de ficheiros novos ou existentes para manter os ficheiros em sessões.

> [!NOTE]
> Bash e o PowerShell partilham a mesma partilha de ficheiros. Apenas uma partilha de ficheiros pode ser associada com a montagem automática no Cloud Shell.

### <a name="create-new-storage"></a>Criar novo armazenamento

Quando utilizar as definições básicas e selecionar apenas uma subscrição, o Cloud Shell cria três recursos em seu nome na região suportada mais próxima:
* Grupo de recursos: `cloud-shell-storage-<region>`
* Conta de armazenamento: `cs<uniqueGuid>`
* Partilha de ficheiros: `cs-<user>-<domain>-com-<uniqueGuid>`

![A definição de subscrição](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

A partilha de ficheiros monta como `clouddrive` no seu `$Home` diretório. Esta é uma ação única e a partilha de ficheiros monta automaticamente nas sessões subsequentes. 

> [!NOTE]
> Para segurança, cada utilizador deve aprovisionar a sua própria conta de armazenamento.  Para controlo de acesso baseado em funções (RBAC), os utilizadores tem de ter acesso de contribuinte ou acima no armazenamento de conta de nível.

No Bash, a partilha de ficheiros também contém uma imagem de 5 GB, que é criada para si que automaticamente mantém os dados no seu `$Home` diretório. 

### <a name="use-existing-resources"></a>Utilizar recursos existentes

Ao utilizar a opção avançada, pode associar recursos existentes. Quando for apresentada a linha de comandos do programa de configuração da armazenamento, selecione **Mostrar definições avançadas** para ver opções adicionais. Os menus de lista pendente são filtrados para a sua região do Cloud Shell atribuído e o armazenamento localmente redundante e contas de armazenamento georredundante.

Partilhas de ficheiros recebem uma imagem de 5 GB criada por si manter sua `$Home` diretório.

![A definição de grupo de recursos](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Restringir a criação de recursos com uma política de recursos do Azure
Contas de armazenamento que criou no Cloud Shell são marcadas com `ms-resource-usage:azure-cloud-shell`. Se quiser não que os utilizadores de criar contas de armazenamento no Cloud Shell, crie uma [política de recursos do Azure para etiquetas](../articles/azure-policy/json-samples.md) que são acionados por esta etiqueta específica.

## <a name="supported-storage-regions"></a>Regiões de armazenamento suportadas
Associados a contas têm de residir na mesma região que a máquina do Cloud Shell que está a montagem-los para o armazenamento do Azure.

Para encontrar a sua região atribuído pode:
* Veja a nota na caixa de diálogo de "Definições avançadas de armazenamento"
* Consulte o nome da conta de armazenamento criada por si (ex: `cloud-shell-storage-westus`)
* Executar `env` e localize a variável `ACC_LOCATION`

Máquinas do cloud Shell existem nas seguintes regiões:
|Área|Região|
|---|---|
|Américas|Este dos E.U.A., Centro-Sul dos E.U.A., oeste dos E.U.A.|
|Europa|Europa do Norte, Europa Ocidental|
|Ásia-Pacífico|Ásia Central, Sudeste do Índia|

