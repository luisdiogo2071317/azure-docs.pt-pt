---
title: Administrar uma aplicação do Azure IoT Central | Documentos da Microsoft
description: Como um administrador, como administrar a sua aplicação do Azure IoT Central
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 25b4777be4257933b84d58d0f10cf12571de9590
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155325"
---
# <a name="administer-your-iot-central-application"></a>Administrar a sua aplicação IOT Central

Depois de criar uma aplicação do Microsoft Azure IoT Central, pode utilizar o **administração** secção da interface do usuário do Azure IoT Central para administrá-lo. Para ir para o **Administration** secção, selecione **administração** no menu de navegação esquerdo.

O **administração** secção permite-lhe:

- Gerir utilizadores

- Gerir funções

- Ver informações de faturação

- Gerir definições da aplicação

- Oferecemos uma avaliação gratuita

Na **administração** secção, um menu de navegação secundária tem ligações para as diversas tarefas de administração.

Aceder e utilizar o **Administration** seção, deve estar no **administrador** função para uma aplicação do Azure IoT Central. Se criar uma aplicação do Azure IoT Central, automaticamente estiver atribuído para o **administrador** função para essa aplicação. O [gerir utilizadores](#manage-users) secção deste artigo explica mais sobre como atribuir o **administrador** função a outros utilizadores.

## <a name="change-application-name"></a>Alterar o nome de aplicação

Para alterar o nome da sua aplicação, utilize o menu de navegação secundária para ir para o **as configurações do aplicativo** página no **administração** secção.

Sobre o **as definições da aplicação** página, introduza um nome à sua escolha no **nome da aplicação** campo. Em seguida, selecione **Guardar**.

## <a name="change-the-application-url"></a>Alterar o URL da aplicação

Para alterar o URL para a sua aplicação, utilize o menu de navegação secundária para navegar para o **as configurações do aplicativo** página no **administração** secção.

![Página de definições de aplicação](media\howto-administer\image0-a.png)

Sobre o **as definições da aplicação** página, introduza o URL da sua preferência no **URL** campo e, em seguida, selecione **guardar**. O URL pode ser no máximo, 200 carateres de comprimento. Se o URL não estiver disponível, verá um erro de validação.

> [!Note]
> Se alterar o URL, o seu URL antigo pode ser realizado por outro cliente do Azure IoT Central. Se isto acontecer, já não está disponível para utilização. Quando altera o URL, o antigo URL já não funciona, e precisa notificar os utilizadores sobre o novo URL a utilizar.

## <a name="change-the-application-image"></a>Alterar a imagem de aplicação

Para obter mais informações sobre a utilização de imagens num aplicativo do Azure IoT Central, consulte [preparar e carregar imagens para a sua aplicação do Azure IoT Central](howto-prepare-images.md).

## <a name="copy-an-application"></a>Copiar uma aplicação

Pode criar uma cópia de qualquer aplicativo, menos quaisquer instâncias de dispositivo, o histórico de dados do dispositivo e dados de utilizador. A cópia será uma aplicação paga que é cobrada. Não é possível criar uma aplicação de avaliação, copiando outra aplicação.

Para copiar uma aplicação, vá para o **as definições da aplicação** página. Em seguida, selecione o **cópia** botão.

![Página de definições de aplicação](media\howto-administer\appCopy1.png)

Selecionar o **cópia** botão abre uma caixa de diálogo na qual pode selecionar um nome, URL, diretório do Azure AD, subscrição e região do Azure para a nova aplicação que será criada ao copiar o seu aplicativo. Selecione os valores para cada um desses campos. Em seguida, selecione o **cópia** botão para confirmar que pretende prosseguir. Pode saber mais sobre o que introduzir para esses valores neste artigo sobre [como criar uma aplicação](howto-create-application.md).

![Página de definições de aplicação](media\howto-administer\appCopy2.png)

Depois da operação de cópia da aplicação for bem-sucedida, pode ir para a nova aplicação que foi criada ao copiar o seu aplicativo. Para aceder à aplicação, selecione a ligação que aparece no **as definições da aplicação** página.

![Página de definições de aplicação](media\howto-administer\appCopy3.png)

> [!Note]
> Também copiar um aplicativo copia a definição de regras ou ações. Mas uma vez que os utilizadores que têm acesso à sua aplicação original não são copiados para a aplicação de copiado, tem de adicionar manualmente os utilizadores a ações como o e-mail para o qual os usuários são um pré-requisito.

## <a name="delete-an-application"></a>Eliminar uma aplicação

A eliminar a aplicação, utilize o menu de navegação secundária para navegar para o **as configurações do aplicativo** página no **administração** secção.

Escolher **eliminar**.

> [!Note]
> A eliminar permanentemente a uma aplicação elimina todos os dados que está associada essa aplicação.  Para eliminar uma aplicação, também tem de ter permissões para eliminar recursos na subscrição do Azure que escolheu quando criou a aplicação. Para obter mais informações, consulte [utilizar o controlo de acesso baseado em funções para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Funções no Azure IoT Central

As funções permitem controlar quem na sua organização pode efetuar várias tarefas do Azure IoT Central. O Azure IoT Central tem três funções, que pode atribuir aos utilizadores da sua aplicação. Funções são atribuídas por cada aplicação. O mesmo utilizador pode ter diferentes funções em aplicativos diferentes. Pode atribuir o mesmo utilizador a várias funções dentro de um aplicativo.

### <a name="administrator"></a>Administrador

Os utilizadores a **administrador** função tem acesso a todas as funcionalidades numa aplicação do Azure IoT Central.

O utilizador que cria uma aplicação é atribuído automaticamente para o **administrador** função. Deve sempre haver, pelo menos, um utilizador no **administrador** função.

### <a name="application-builder"></a>Application Builder

Os utilizadores a **Application Builder** função pode fazer tudo num aplicativo do Azure IoT Central, exceto a administrar o aplicativo.

### <a name="application-operator"></a>Operador de aplicações

Os utilizadores a **operador de aplicações** função não tem acesso ao **Application Builder** página. Eles não podem administrar o aplicativo.

## <a name="manage-users"></a>Gerir utilizadores

Os administradores do aplicativo podem atribuir utilizadores às funções na aplicação.

### <a name="add-users"></a>Adicionar utilizadores

Cada utilizador tem de ter uma conta de utilizador antes de poderem iniciar sessão e aceder a uma aplicação do Azure IoT Central. Accounts da Microsoft (Msa) e contas do Azure Active Directory (Azure AD) são suportadas no Azure IoT Central. Os grupos do Active Directory do Azure não são atualmente suportados no Azure IoT Central.

Para obter mais informações, consulte [ajuda de conta da Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) e [guia de início rápido: adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para adicionar uma conta de utilizador a uma aplicação do Azure IoT Central, utilize o menu de navegação secundária para ir para o **usuários** página no **administração** secção.

    ![Lista de utilizadores](media\howto-administer\image1.png)

1. Para adicionar um utilizador, o **usuários** página, selecione **+ adicionar utilizador**.

    ![Adicionar utilizador](media\howto-administer\image2.png)

1. Escolher uma função para o utilizador a partir da **função** menu pendente. Saiba mais sobre as funções no *funções no Azure IoT Central* seção deste artigo.

    ![Seleção da função](media\howto-administer\image3.png)

    > [!NOTE]
    >  Para adicionar utilizadores em volume, introduza o utilizador IDs de todos os utilizadores que pretende adicionar separados por ponto e vírgula. Escolha uma função a partir da **função** menu pendente. Em seguida, selecione **Guardar**.

1. Depois de adicionar um utilizador, uma entrada é apresentada para esse utilizador sobre o **utilizadores** página.

    ![Lista de utilizadores](media\howto-administer\image4.png)

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Editar as funções que são atribuídas aos utilizadores

Funções não podem ser alteradas depois de estão atribuídos. Para alterar a função que é atribuída a um utilizador, elimine o utilizador e, em seguida, adicione o utilizador novamente com uma função diferente.

### <a name="delete-users"></a>Eliminar utilizadores

Para eliminar os utilizadores, selecione uma ou mais caixas de verificação sobre o **utilizadores** página. Em seguida, selecione **Eliminar**.

## <a name="view-your-bill"></a>Veja a sua fatura

Para ver a sua fatura, vá para o **faturação** página no **administração** secção. Em seguida, selecione **faturação**. A página de faturação do Azure é aberto num novo separador, onde pode ver a fatura para cada uma das suas aplicações do Azure IoT Central.

## <a name="convert-your-trial-to-a-paid-application"></a>Converter a versão de avaliação para uma aplicação paga

Depois de ter avaliado o IoT Central, pode converter a versão de avaliação para uma aplicação paga. Para concluir este processo self-service, siga estes passos:

1. Utilize o menu de navegação secundária para ir para o **faturação** página no **administração** secção. Se ainda não tiver expandido sua versão de avaliação, a página é semelhante a captura de ecrã seguinte:

    ![Estado de avaliação gratuito](media/howto-administer/freetrial.png)

2. Selecione **converter à subscrição paga**. Se ainda não tiver expandido sua versão de avaliação, a janela de pop-up é semelhante a captura de ecrã seguinte:

    ![Expandir versão de avaliação gratuita](media/howto-administer/extend.png)

3. Na janela de pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure para utilizar para a sua aplicação IoT Central.

3. Depois de selecionar **converter**, comece a sua avaliação converte para uma aplicação paga e faturadas.

## <a name="extend-your-free-trial"></a>Expandir a sua avaliação gratuita

Por predefinição, todas as avaliações gratuitas estão disponíveis durante sete dias. Se gostaria de aumentar a sua versão de avaliação para 30 dias, siga estes passos:

1. Utilize o menu de navegação secundária para ir para o **faturação** página no **administração** secção.

1. Selecione **expandir versão de avaliação**. Na janela de pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure para utilizar para a sua aplicação IoT Central.

1. Em seguida, selecione **expandir**. Sua avaliação agora é válida por 30 dias.

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Utilizar os SDKs do Azure para operações do painel de controlo

Pacotes do SDK do IoT Central do Azure Resource Manager estão disponíveis para o nó, Python, c#, Ruby, Java e Go. Estas bibliotecas suporte controle operações do plano para o Centro de IoT, permitindo-lhe criar, listar, atualizar ou eliminar aplicações de IoT Central. Eles também fornecem auxiliares para lidar com a autenticação e manipulação de erros são específico para cada idioma. 

Pode encontrar exemplos de como utilizar os SDKs do Azure Resource Manager no [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Para obter mais informações, veja estes pacotes no GitHub.

| Idioma | Repositório | Pacote |
| ---------| ---------- | ------- |
| Nó | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Ir | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como administrar a sua aplicação do Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Configurar o modelo de dispositivo](howto-set-up-template.md)
