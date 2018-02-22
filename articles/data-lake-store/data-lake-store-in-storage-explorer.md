---
title: Gerir recursos do Azure Data Lake Store no Explorador de Armazenamento do Azure
description: Permite aos utilizadores aceder e gerir os seus dados e recursos do ADLS no Explorador de Armazenamento do Azure
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Gerir os recursos do Azure Data Lake Store com o Explorador de Armazenamento (Pré-visualização)

O [Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) é um serviço para armazenar grandes quantidades de dados não estruturados, como texto ou dados binários. O utilizador pode obter acesso aos dados a partir de qualquer lugar através de HTTP ou HTTPS. O ADLS no Explorador de Armazenamento do Azure permite aos utilizadores aceder e gerir aos dados e recursos do ADLS, juntamente com outras entidades do Azure, como o blob e a fila. Agora, os utilizadores podem utilizar a mesma ferramenta para gerir as diferentes entidades do Azure num único local.

Outra vantagem é que os utilizadores não precisam de ter permissão de subscrição para gerir dados do ADLS. No Explorador de Armazenamento, os utilizadores podem anexar o caminho do ADLS ao nó “Local e Ligado”, desde que outros concedam a permissão.

## <a name="prerequisites"></a>Pré-requisitos
Para executar os passos descritos neste artigo, os seguintes requisitos são necessários:

*   Uma subscrição do Azure. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial).
*   Uma conta do Azure Data Lake Store. Para obter instruções sobre como criar uma, veja [Introdução ao Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal)

## <a name="installation"></a>Instalação

Instale os bits mais recentes do Explorador de Armazenamento do Azure aqui: [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/). Agora, suportamos a versão do Windows, Linux e MAC.

## <a name="connect-to-an-azure-subscription"></a>Ligar a uma subscrição do Azure

1. Depois de instalar o **Explorador de Armazenamento do Azure**, clique no ícone de **plug-in** à esquerda, conforme mostrado na imagem seguinte.
       
   ![Ícone de plug-in](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Selecione **Adicionar uma Conta do Azure** e clique em **Iniciar sessão**.

   ![Ligar a uma subscrição do Azure](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. Na caixa de diálogo **Início de sessão do Azure**, selecione **Iniciar sessão** e, em seguida, introduza as credenciais do Azure.

    ![Iniciar sessão](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Selecione a sua subscrição na lista e clique em **Aplicar**.

    ![Aplicar](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    O painel do Explorador atualiza e apresenta as contas na subscrição selecionada.

    ![Lista de contas](./media/data-lake-store-in-storage-explorer/account-list.png)

    Ligou com êxito o **Azure Data Lake Store** à subscrição do Azure.

## <a name="connect-to-data-lake-store"></a>Ligar ao Data Lake Store
Se pretender obter acesso a recursos que não existem na sua subscrição. Mas outros concedem-lhe permissão para obter o Uri dos recursos. Neste caso, pode ligar ao Data Lake Store com o Uri, depois de ter sessão iniciada. Consulte os seguintes passos.
1. Abrir o Explorador de Armazenamento (Pré-visualização).
2. No painel esquerdo, expanda **Local e Ligado**.
3. Clique com botão direito do rato em **Data Lake Store** e, no menu de contexto, selecione **Ligar ao Data Lake Store...**.

      ![menu de contexto Ligar ao Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Introduza o Uri e, em seguida, a ferramenta navega para a localização do URL que acabou de introduzir.

      ![caixa de diálogo de contexto Ligar ao Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![resultado de Ligar ao Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Ver os conteúdos de uma conta do Azure Data Lake Store
Os recursos de uma conta do Azure Data Lake Store contêm pastas e ficheiros.

Os passos seguintes mostram como ver os conteúdos de uma conta do ADLS no Explorador de Armazenamento (Pré-visualização):

1. Abrir o Explorador de Armazenamento (Pré-visualização).
2. No painel esquerdo, expanda a subscrição que contém a conta do Azure Data Lake Store que pretende ver.
3. Expanda **Data Lake Store**.
4. Clique com o botão direito do rato no nó do Azure Data Lake Store que quer ver e, no menu de contexto, selecione **Abrir**. Também pode fazer duplo clique na conta do ADLS a abrir. 
5. O painel principal apresenta os conteúdos da conta do ADLS.

     ![painel principal](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Gestão de recursos do Azure Data Lake Store

Pode gerir os recursos do Azure Data Lake Store com as seguintes operações:
*   Navegue por recursos do ADLS em várias contas do ADL.  
*   Utilize a cadeia de ligação para ligar a e gerir o ADLS diretamente. 
*   Veja recursos do ADLS partilhados por outras pessoas através de ACL em Local e Ligado.
*   Execute as operações CRUD de Pasta/Ficheiro: suporta pastas recursivas e seleção de vários ficheiros. 
*   Arraste, largue e adicione pastas para acesso rápido e localizações recentes, o que reflete a experiência do explorador de ficheiros do ambiente de trabalho. 
*   Copie e abra a hiperligação do ADL com o Explorador de Armazenamento, com um só clique. 
*   Apresente o registo de atividades no painel na parte inferior direita para ver o estado das atividades.
*   Apresente as estatísticas de pastas e propriedades de ficheiros.

## <a name="manage-resources-in-azure-storage-explorer"></a>Gerir recursos do Explorador de Armazenamento do Azure
Assim que tiver criado uma conta do Azure Data Lake Store, pode carregar pastas e ficheiros, transferir e abrir recursos no seu computador local. E pode afixar para acesso rápido, nova pasta, criar URL, selecionar Tudo. Além disso, pode copiar, colar, mudar o nome, eliminar, estatísticas de pasta, atualizar.

Os itens seguintes mostram como gerir recursos numa conta do Azure Data Lake Store. Siga estes passos, consoante a tarefa que pretende realizar:
   * **Carregar ficheiros**

     1. Na barra de ferramentas do painel principal, selecione **Carregar** e, em seguida, **Carregar Ficheiros**, no menu pendente.

        ![Menu Carregar ficheiros](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. Na caixa de diálogo **Selecionar ficheiros para carregar**, selecione os ficheiros que pretende carregar.

        ![Caixa de diálogo Carregar pasta](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Selecione **Abrir** para iniciar o carregamento.
   * **Carregar uma pasta**

     1. Na barra de ferramentas do painel principal, selecione **Carregar** e, em seguida, **Carregar Pasta...**, no menu pendente.

        ![Menu Carregar pasta](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. Na caixa de diálogo **Selecionar pasta para Carregar**, selecione a pasta que pretende carregar.

        ![Caixa de diálogo Carregar pasta](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Selecione **Selecionar Pasta** para iniciar o carregamento de pastas.

        ![Caixa de diálogo Carregar pasta](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > Pode arrastar diretamente as pastas e os ficheiros do computador local para implementar o carregamento. 
       
   * **Transferir pastas ou ficheiros para o computador local**

     1. Selecione as pastas ou ficheiros que pretende transferir.
     2. Na barra de ferramentas do painel principal, selecione **Download (Transferir)**.
     3. Na caixa de diálogo **Selecionar uma pasta na qual guardar os ficheiros transferidos**, especifique a localização pretendida para as pastas ou os ficheiros transferidos. E especifique o nome que pretende atribuir-lhe.
     4. Selecione **Guardar**.
   * **Abra o ficheiro ou pasta do computador local**

     1. Selecione a pasta ou o ficheiro que pretende abrir.
     2. Na barra de ferramentas do painel principal, selecione **Abrir** ou clique com o botão direito do rato na pasta ou no ficheiro selecionado, a partir do menu de contexto, clique em **Abrir**.
     3. O ficheiro é transferido e aberto com a aplicação associada ao respetivo tipo de ficheiro subjacente. Ou a pasta é aberta no painel principal.

        ![abrir ficheiro](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Copiar pastas ou ficheiros para a área de transferência**

     1. Selecione as pastas ou os ficheiros que pretende copiar.
     2. Na barra de ferramentas do painel principal, selecione **Copiar** ou clique com o botão direito do rato na pasta ou no ficheiro selecionado, a partir do menu de contexto, clique em **Copiar**.
     3. No painel esquerdo, navegue para outra conta do ADLS e faça duplo clique na mesma para vê-la no painel principal.
     4. Na barra de ferramentas do painel principal, selecione **Colar** para criar uma cópia do ficheiro. Ou clique no menu de contexto **Colar** no destino.

        ![copiar/colar pasta ou ficheiro](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Copiar/colar em tipo de armazenamento **não é** suportado. Pode copiar pastas ou ficheiros do ADLS e colá-los noutra conta do ADLS. Mas **não é possível** colar pastas ou ficheiros do ADLS e colá-los no armazenamento de blobs, ou ao contrário. 
          > + Copiar-colar funciona ao transferir as pastas ou os ficheiros para o local e, em seguida, carregá-los para o destino. A ferramenta **não** executa a ação no back-end. Copiar/colar ficheiros grandes é uma operação lenta. A otimização de copiar/mover ficheiros de elevado desempenho está em curso.
   * **Eliminar pastas ou ficheiros**

     1. Selecione as pastas ou ficheiros que pretende eliminar.
     2. Na barra de ferramentas do painel principal, selecione **Eliminar** ou clique com o botão direito do rato na pasta ou no ficheiro selecionado, a partir do menu de contexto, clique em **Eliminar**.
     3. Selecione **Yes (Sim)** na caixa de diálogo de confirmação.

        ![copiar/colar pasta ou ficheiro](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **Afixar para Acesso rápido**

     1. Selecione a pasta que pretende afixar.
     2. Na barra de ferramentas do painel principal, selecione **Afixar para Acesso rápido**.
     3. No painel esquerdo, verá que a pasta selecionada foi adicionada ao nó **Acesso Rápido**.

        ![afixar para acesso rápido](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Depois de criar o acesso rápido, pode facilmente obter acesso aos recursos.
   * **Ligações Avançadas**
     1. Se tiver um URL, pode introduzir o URL no caminho de endereço do **Explorador de Ficheiros** ou browser.
     2. Em seguida, **Storage Explorer.exe** é iniciado automaticamente para navegar para a localização do URL que acabou de introduzir.

        ![ligação avançada no explorador de ficheiros](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Passos seguintes
* Vejas os [mais recentes vídeos e notas de versão do Storage Explorer (Preview)](http://www.storageexplorer.com).
* Saiba como [Gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer)
* Saiba mais sobre o Explorador de Armazenamento[Introdução ao Explorador de Armazenamento (Pré-visualização)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* Introdução ao Azure Data Lake Store (ADLS)[Descrição Geral do Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview)
* Veja o vídeo seguinte para ver[como utilizar o Azure Cosmos DB no Explorador de armazenamento do Azure](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be)
