---
title: Como gerir configurações de serviço e perfis | Documentos da Microsoft
description: Saiba como trabalhar com arquivos de configuração de perfis e configurações de serviço | que armazenar as definições para os ambientes de implantação e definições para os serviços cloud de publicação.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/11/2017
ms.author: ghogen
ms.openlocfilehash: 1acb13f8bdcb7f5a6f214a7425e13c72e21bd29d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059596"
---
# <a name="how-to-manage-service-configurations-and-profiles"></a>Como gerir configurações de serviço e perfis
## <a name="overview"></a>Descrição geral
Quando publica um serviço em nuvem, o Visual Studio armazena informações de configuração em dois tipos de ficheiros de configuração: configurações e perfis de serviço. Configurações de serviço (ficheiros. cscfg) armazenam as definições para os ambientes de implantação para um serviço cloud do Azure. Quando gere os serviços cloud, o Azure utiliza estes ficheiros de configuração. Por outro lado, o arquivo de perfis (ficheiros .azurePubxml) definições de publicação para os serviços cloud. Estas definições são um registo de aquilo que escolher quando utilizar o Assistente de publicação e são usadas localmente pelo Visual Studio. Este tópico explica como trabalhar com os dois tipos de ficheiros de configuração.

## <a name="service-configurations"></a>Configurações de serviço
Pode criar várias configurações de serviço a utilizar para cada um dos seus ambientes de implantação. Por exemplo, poderá criar uma configuração de serviço para o ambiente local que utiliza para executar e testar uma aplicação do Azure e a outra configuração de serviço para o seu ambiente de produção.

Pode adicionar, eliminar, mudar o nome e modificar essas configurações de serviço com base nos seus requisitos. Pode gerir estas configurações de serviço no Visual Studio, conforme mostrado na ilustração seguinte.

![Gerir configurações de serviço](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Também pode abrir o **gerir configurações** caixa de diálogo de páginas de propriedades da função. Para abrir as propriedades para uma função em seu projeto do Azure, abra o menu de atalho para essa função e, em seguida, escolha **propriedades**. Na **definições** separador, expanda o **configuração de serviço** lista e, em seguida, selecione **gerir** para abrir o **gerir configurações** caixa de diálogo.

### <a name="to-add-a-service-configuration"></a>Para adicionar uma configuração de serviço
1. No Explorador de soluções, abra o menu de atalho para o projeto do Azure e, em seguida, selecione **gerir configurações**.
   
    O **gerir configurações de serviço** é apresentada a caixa de diálogo.
2. Para adicionar uma configuração de serviço, tem de criar uma cópia de uma configuração existente. Para tal, escolha a configuração que pretende copiar a partir da lista de nomes e, em seguida, selecione **criar cópia**.
3. (Opcional) Para dar um nome diferente da configuração do serviço, escolha a nova configuração de serviço na lista nome e, em seguida, selecione **mudar o nome**. Na **Name** texto, escreva o nome que pretende utilizar para esta configuração de serviço e, em seguida, selecione **OK**.
   
    Um novo ficheiro de configuração do serviço, com o nome ServiceConfiguration. [Nome do novo]. cscfg é adicionado ao projeto no Explorador de soluções do Azure.

### <a name="to-delete-a-service-configuration"></a>Para eliminar uma configuração de serviço
1. No Explorador de soluções, abra o menu de atalho para o projeto do Azure e, em seguida, selecione **gerir configurações**.
   
    O **gerir configurações de serviço** é apresentada a caixa de diálogo.
2. Para eliminar uma configuração de serviço, escolha a configuração que pretende eliminar a partir da **Name** lista e, em seguida, selecione **remover**. É apresentada uma caixa de diálogo Confirmar que pretende eliminar esta configuração.
3. Selecione **Eliminar**.
   
     O ficheiro de configuração do serviço é removido do projeto no Explorador de soluções do Azure.

### <a name="to-rename-a-service-configuration"></a>Para mudar o nome de uma configuração de serviço
1. No Solution Explorer, abra o menu de atalho para o projeto do Azure e, em seguida, selecione **gerir configurações**.
   
    O **gerir configurações de serviço** é apresentada a caixa de diálogo.
2. Para mudar o nome de uma configuração de serviço, escolha a nova configuração de serviço a partir da **nome** lista e, em seguida, selecione **mudar o nome**. Na **Name** texto, escreva o nome que pretende utilizar para esta configuração de serviço e, em seguida, selecione **OK**.
   
    O nome do ficheiro de configuração do serviço é alterado no projeto no Explorador de soluções do Azure.

### <a name="to-change-a-service-configuration"></a>Para alterar uma configuração de serviço
* Se quiser alterar uma configuração de serviço, abra o menu de atalho para a função específica que pretende alterar no projeto do Azure e, em seguida, selecione **propriedades**. Ver [como: configurar as funções para um serviço Cloud do Azure com o Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) para obter mais informações.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Tornar combinações de configuração diferente, através de perfis
Ao utilizar um perfil, pode preencher automaticamente o **Assistente de publicação** com diferentes combinações de definições para diferentes fins. Por exemplo, pode ter um perfil para depuração e outro para a versão baseia-se. Nesse caso, sua **depurar** perfil teria **IntelliTrace** ativada e o **depurar** configuração selecionada e seu **versão** perfil teria **IntelliTrace** desativada e o **versão** configuração selecionada. Também poderia usar perfis diferentes para implementar um serviço com uma conta de armazenamento diferente.

Quando executa o assistente pela primeira vez, é criado um perfil predefinido. Visual Studio armazena o perfil num arquivo que possui uma extensão de .azurePubXml, que é adicionada ao seu projeto do Azure sob o **perfis** pasta. Se especificar manualmente diferentes opções quando executa o assistente mais tarde, o ficheiro é atualizada automaticamente. Antes de executar o procedimento a seguir, deve já publicou seu serviço cloud, pelo menos, uma vez.

### <a name="to-add-a-profile"></a>Para adicionar um perfil
1. Abra o menu de atalho para o seu projeto do Azure e, em seguida, selecione **publicar**.
2. Junto a **perfil de destino** lista, selecione o **Guardar perfil** botão, como mostra a ilustração seguinte. Esta ação cria um perfil para.
   
    ![Criar um novo perfil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Depois do perfil é criado, selecione **< gerir... >** no **perfil de destino** lista.
   
    O **Spravovat Profily** caixa de diálogo for exibida, como mostra a ilustração seguinte.
   
    ![Gerir a caixa de diálogo de perfis](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. Na **Name** lista, escolha um perfil e, em seguida, selecione **criar cópia**.
5. Escolha o **fechar** botão.
   
    O novo perfil aparece na lista de perfil de destino.
6. Na **perfil de destino** lista, selecione o perfil que acabou de criar. As definições do Assistente de publicação são preenchidas com as opções do perfil que selecionou.
7. Selecione o **Previous** e **próxima** botões para exibir cada página do Assistente para publicar e, em seguida, personalizar as definições para este perfil. Ver [publicar Assistente da aplicação Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) para obter informações.
8. Depois de concluir a personalizar as definições, selecione **seguinte** voltar para a página de definições. O perfil é guardado quando publica o serviço ao utilizar estas definições, ou se selecionar **guardar** junto à lista de perfis.

### <a name="to-rename-or-delete-a-profile"></a>Para mudar o nome ou eliminar um perfil
1. Abra o menu de atalho para o seu projeto do Azure e, em seguida, selecione **publicar**.
2. Na **perfil de destino** lista, selecione **gerir**.
3. Na **Spravovat Profily** caixa de diálogo, selecione o perfil que pretende eliminar e, em seguida, selecione **remover**.
4. Na caixa de diálogo de confirmação que aparece, selecione **OK**.
5. Selecione **fechar**.

### <a name="to-change-a-profile"></a>Para alterar um perfil
1. Abra o menu de atalho para o seu projeto do Azure e, em seguida, selecione **publicar**.
2. Na **perfil de destino** lista, selecione o perfil que pretende alterar.
3. Selecione o **Previous** e **próxima** botões para exibir cada página do Assistente para publicar e, em seguida, altere as definições que pretende. Ver [publicar Assistente da aplicação Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) para obter informações.
4. Depois de concluir a alteração das definições, selecione **próxima** voltar para o **definições** página.
5. (Opcional) selecione **publicar** para publicar o serviço em nuvem com as novas definições. Se não deseja publicar o seu serviço cloud neste momento, e fechar o Assistente de publicação, o Visual Studio solicita que se pretender guardar as alterações ao perfil.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre como configurar outras partes do seu projeto do Azure a partir do Visual Studio, veja [configurando um projeto do Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)

