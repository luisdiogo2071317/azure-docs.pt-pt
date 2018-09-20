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
ms.openlocfilehash: adcc0b741cac0ac5038243c80b3da2e0ff872c95
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466103"
---
# <a name="how-to-administer-your-application"></a>Como administrar a sua aplicação

Depois de criar uma aplicação do Microsoft Azure IoT Central, pode utilizar o **administração** secção da interface do usuário do Azure IoT Central para administrá-lo. Para navegar para o **Administration** secção, escolha **administração** no menu de navegação esquerdo.

O **administração** secção permite-lhe:

- Gerir utilizadores

- Gerir funções

- Ver informações de faturação

- Gerir definições da aplicação

- Expanda uma versão de avaliação gratuita

Na **administração** secção, existe um menu de navegação secundária com ligações para as diversas tarefas de administração.

Aceder e utilizar o **Administration** seção, deve estar no **administrador** função na aplicação do Azure IoT Central. Se criar uma aplicação do Azure IoT Central, é atribuídos automaticamente para o **administrador** função para essa aplicação. O *gerir utilizadores* secção deste artigo explica mais sobre como atribuir o **administrador** função a outros utilizadores.

## <a name="change-application-name"></a>Alterar o nome de aplicação

Para alterar o nome da sua aplicação, utilize o menu de navegação secundária para navegar para o **as configurações do aplicativo** página no **administração** secção.

Na **as configurações do aplicativo** página, introduza um nome à sua escolha no **nome da aplicação** campo e, em seguida, escolha **guardar**.

## <a name="change-the-application-url"></a>Alterar o URL da aplicação

Para alterar o URL para a sua aplicação, utilize o menu de navegação secundária para navegar para o **as configurações do aplicativo** página no **administração** secção.

![Página de definições de aplicação](media\howto-administer\image0-a.png)

Na **as configurações do aplicativo** página, introduza o URL da sua preferência no **URL** campo e, em seguida, escolha **guardar**. O URL pode ser no máximo, 200 carateres de comprimento. Se o URL não estiver disponível, verá um erro de validação

> [!Note]
> Se alterar o URL, o seu URL antigo pode ser realizado por outro cliente do Azure IoT Central. Nesse caso, já não está disponível para utilização. Quando altera o URL, o antigo URL já não funciona e tem de notificar os seus utilizadores sobre o novo URL a utilizar.

## <a name="change-the-application-image"></a>Alterar a imagem de aplicação

Para obter mais informações sobre a utilização de imagens num aplicativo do Azure IoT Central, consulte [preparar e carregar imagens para a sua aplicação do Azure IoT Central](howto-prepare-images.md).

## <a name="copy-an-application"></a>Copiar uma aplicação

Pode criar uma cópia de qualquer aplicativo, menos quaisquer instâncias de dispositivo, o histórico de dados do dispositivo e dados de utilizador. A cópia será uma aplicação paga que lhe será cobrado. Não é possível criar uma aplicação de avaliação, copiando outra aplicação.

Para copiar uma aplicação, navegue para o **as configurações do aplicativo** página e clique nas **cópia** botão.

![Página de definições de aplicação](media\howto-administer\appCopy1.png)

Ao clicar o **cópia** botão abrirá uma caixa de diálogo na qual pode selecionar um nome, URL, AAD diretório, subscrição e região do Azure para a nova aplicação que será criada ao copiar o seu aplicativo. Selecione os valores para cada um desses campos e, em seguida, clique nas **cópia** botão para confirmar que pretende prosseguir. Pode saber mais sobre o que introduzir para esses valores no artigo sobre [como criar uma aplicação](howto-create-application.md).

![Página de definições de aplicação](media\howto-administer\appCopy2.png)

Assim que a operação de cópia da aplicação for bem-sucedida, poderá navegar para a nova aplicação que foi criada ao copiar a sua aplicação ao clicar na ligação que aparece no **as definições da aplicação** página.

![Página de definições de aplicação](media\howto-administer\appCopy3.png)

> [!Note]
> A copiar de um aplicativo copiará a definição de regras ou ações. No entanto, uma vez que os utilizadores que têm acesso à sua aplicação original não são copiados para a aplicação de copiado, terá de adicionar manualmente os utilizadores a ações como o e-mail para o qual os usuários são um pré-requisito.

## <a name="delete-an-application"></a>Eliminar uma aplicação

A eliminar a aplicação, utilize o menu de navegação secundária para navegar para o **as configurações do aplicativo** página no **administração** secção.

Escolher **eliminar**.

> [!Note]
> A eliminar uma aplicação de forma irrevogável elimina todos os dados associados à aplicação. Para eliminar uma aplicação, também tem de ter os direitos para eliminar recursos na subscrição do Azure que escolheu quando criou a aplicação. Para obter mais informações, consulte [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Funções no Azure IoT Central

As funções permitem controlar quem, dentro da sua organização, pode realizar várias tarefas do Azure IoT Central. O Azure IoT Central tem três funções, que pode atribuir aos utilizadores da sua aplicação. Funções são atribuídas por cada aplicação. O mesmo utilizador pode ter diferentes funções em aplicativos diferentes. Pode atribuir o mesmo utilizador podem a várias funções dentro de um aplicativo.

### <a name="administrator"></a>Administrador

Os utilizadores a **administrador** função tem acesso a todas as funcionalidades numa aplicação do Azure IoT Central.

O utilizador criar uma aplicação é atribuído automaticamente para o **administrador** função. Deve sempre haver, pelo menos, um utilizador no **administrador** função.

### <a name="application-builder"></a>Application Builder

Os utilizadores a **Application Builder** função pode fazer tudo num aplicativo do Azure IoT Central, exceto a administrar o aplicativo.

### <a name="application-operator"></a>Operador de aplicações

Os utilizadores a **operador de aplicações** função não tem acesso ao **Application Builder** página. Eles não podem administrar o aplicativo.

## <a name="manage-users"></a>Gerir utilizadores

Os administradores do aplicativo podem atribuir utilizadores às funções na aplicação.

### <a name="add-users"></a>Adicionar utilizadores

Cada utilizador tem de ter uma conta de utilizador antes de poderem iniciar sessão e aceder a uma aplicação do Azure IoT Central. Accounts da Microsoft (Msa) e contas do Azure Active Directory (AAD) são suportadas no Azure IoT Central. Os grupos do Active Directory do Azure não são atualmente suportados no Azure IoT Central.

Para obter mais informações, consulte [ajuda de conta da Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) e [guia de início rápido: adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para adicionar uma conta de utilizador a uma aplicação do Azure IoT Central, utilize o menu de navegação secundária para navegar para o **usuários** página no **administração** secção.

    ![Lista de utilizadores](media\howto-administer\image1.png)

1. Sobre o **usuários** página, selecione **+ adicionar utilizador** para adicionar um utilizador.

    ![Adicionar Utilizador](media\howto-administer\image2.png)

1. Quando adicionar um utilizador à sua aplicação do Azure IoT Central, escolha uma função para o utilizador a partir da **função** pendente. Saiba mais sobre as funções no *funções no Azure IoT Central* seção deste artigo.

    ![Seleção da função](media\howto-administer\image3.png)

    > [!NOTE]
    >  Para adicionar utilizadores em volume, introduza os IDs de utilizador de todos os utilizadores que pretende adicionar separados por ponto e vírgula. Escolher uma função a partir do **função** pendente e escolha **guardar**.

1. Depois de adicionar um utilizador, uma entrada é apresentada para esse utilizador sobre o **utilizadores** página.

    ![Lista de utilizadores](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>Editar as funções atribuídas a utilizadores

Funções não não possível alterar uma vez assinged. Para alterar a função atribuída a um utilizador, elimine o utilizador e adicionar o utilizador novamente com uma função diferente.

### <a name="delete-users"></a>Eliminar utilizadores

Para eliminar os utilizadores, verificar uma ou mais caixas de verificação a **usuários** página e, em seguida, escolha **eliminar**.

## <a name="view-your-bill"></a>Veja a sua fatura

Para ver a sua fatura, navegue para o **faturação** página no **administração** secção e escolha **faturação**. A página de faturação do Azure é aberto num novo separador e pode ver a fatura para cada uma das suas aplicações do Azure IoT Central.

## <a name="convert-your-trial-to-a-paid-application"></a>Converter a versão de avaliação para uma aplicação paga

Depois de ter avaliado o IoT Central, pode converter a versão de avaliação para uma aplicação paga. Para concluir este processo self-service, siga estes passos:

1. Utilize o menu de navegação secundária para navegar para o **faturação** página no **administração** secção. Se ainda não tiver expandido sua versão de avaliação, a página é semelhante ao seguinte:

    ![Estado de avaliação gratuito](media/howto-administer/freetrial.png)

2. Clique em **converter à subscrição paga**. Se ainda não tiver expandido sua versão de avaliação, o pop-up é semelhante ao seguinte:

    No pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure que pretende utilizar para a sua aplicação do Centro de IoT.

    ![Expandir versão de avaliação gratuita](media/howto-administer/extend.png)

3. Depois de clicar em **converter**, a versão de avaliação é convertida para uma aplicação paga e iniciar a faturadas.

## <a name="extend-your-free-trial"></a>Expandir a sua avaliação gratuita

Por predefinição, todas as avaliações gratuitas estão disponíveis durante sete dias. Se gostaria de aumentar a sua versão de avaliação para 30 dias, siga estes passos:

1. Utilize o menu de navegação secundária para navegar para o **faturação** página no **administração** secção:

1. Clique em **expandir versão de avaliação**. No pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure para utilizar para a sua aplicação do Centro de IoT:

1. Em seguida, clique em **expandir**. Sua avaliação agora é válida por 30 dias.

## <a name="utilize-the-azure-sdks-to-do-control-plane-operations"></a>Utilizar os SDKs do Azure para controlar as operações de plano

Pacotes do SDK do IoT Central do Azure Resource Manager estão disponíveis para o nó, Python, c#, Ruby, Java e Go. Estas bibliotecas suporte controle operações do plano para IoT Central, permitindo-lhe criar, listar, atualizar ou eliminar aplicações de IoT Central. Eles também fornecem auxiliares para lidar com a autenticação e manipulação de erros são específico para cada idioma. 

Exemplos de como utilizar os SDKs do Azure Resource Manager podem ser encontrados em [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Para saber que mais examinar esses pacotes no GitHub.

| Idioma | Repositório | Pacote |
| ---------| ---------- | ------- |
| Nó | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Ir | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como administrar a sua aplicação do Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Configurar o modelo de dispositivo](howto-set-up-template.md)