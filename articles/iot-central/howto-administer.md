---
title: Administrar uma aplicação do Azure IoT Central | Microsoft Docs
description: Como um administrador, como administrar a sua aplicação do Azure IoT Central
services: iot-central
author: TanmayBhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: b60b9e851a3b6612964e67e7764ad8d43d606b4e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-administer-your-application"></a>Como administrar a sua aplicação

Depois de criar uma aplicação do Microsoft Azure IoT Central, pode utilizar o **administração** secção da interface de utilizador do Azure IoT Central para administrar. Para navegar para o **administração** secção, escolha **administração** no menu de navegação esquerdo.

O **administração** secção permite-lhe:

- Gerir utilizadores

- Gerir funções

- Ver as informações de faturação

- Gerir definições da aplicação

- Expandir uma avaliação gratuita

No **administração** secção, existe um menu de navegação secundário com ligações para as várias tarefas de administração.

Aceder e utilizar o **administração** secção, é necessário o **administrador** função na aplicação do Azure IoT Central. Se criar uma aplicação do Azure IoT Central, são automaticamente atribuídos ao **administrador** função para essa aplicação. O *gerir utilizadores* secção este artigo explica mais sobre como atribuir o **administrador** função a outros utilizadores.

## <a name="change-application-name"></a>Nome da aplicação de alteração

Para alterar o nome da sua aplicação, utilize o menu de navegação secundário para navegar para o **definições da aplicação** página no **administração** secção.

No **definições da aplicação** página, introduza um nome à sua escolha no **nome da aplicação** campo e, em seguida, escolha **guardar**.

## <a name="change-the-application-url"></a>Altere o URL da aplicação

Para alterar o URL para a sua aplicação, utilize o menu de navegação secundário para navegar para o **definições da aplicação** página no **administração** secção.

![Página de definições de aplicação](media\howto-administer\image0-a.png)

No **definições da aplicação** página, introduza o URL da sua preferência no **URL** campo e, em seguida, escolha **guardar**. O URL pode ser, no máximo, 200 carateres de comprimento. Se o URL não estiver disponível, verá um erro de validação

> [!Note]
> Se alterar o URL, o URL antigo pode ser tomado por outro cliente do Azure IoT Central. Nesse caso, já não está disponível para utilização. Quando altera o URL, o antigo URL já não funciona e tem de notificar os utilizadores sobre o novo URL a utilizar.

## <a name="change-the-application-image"></a>Alterar a imagem de aplicação

Para obter mais informações sobre como utilizar imagens de uma aplicação do Azure IoT Central, consulte [preparar e carregar imagens para a aplicação do Azure IoT Central](howto-prepare-images.md).

## <a name="delete-an-application"></a>Eliminar uma aplicação

A eliminar a aplicação, utilize o menu de navegação secundário para navegar para o **definições da aplicação** página no **administração** secção.

Escolha **eliminar**.

> [!Note]
> Eliminar uma aplicação irrevogavelmente elimina todos os dados associados à aplicação. Para eliminar uma aplicação, terá também de ter os direitos para eliminar os recursos na subscrição do Azure que selecionou quando criou a aplicação. Para obter mais informações, consulte [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Funções no Centro de IoT do Azure

As funções permitem controlar quem, dentro da sua organização, pode efetuar várias tarefas do Azure IoT Central. Centro de IoT do Azure tem três funções que pode atribuir aos utilizadores da sua aplicação. Funções são atribuídas por cada aplicação. O mesmo utilizador pode ter diferentes funções na diferentes aplicações. Pode atribuir o mesmo utilizador podem a várias funções dentro de uma aplicação.

### <a name="administrator"></a>Administrador

Os utilizadores a **administrador** função tem acesso a todas as funcionalidades numa aplicação do Azure IoT Central.

O utilizador criar uma aplicação é atribuído automaticamente para o **administrador** função. Deve sempre ser, pelo menos, um utilizador a **administrador** função.

### <a name="application-builder"></a>Construtor de aplicação

Os utilizadores a **aplicação Builder** função pode fazer tudo numa aplicação do Azure IoT Central exceto administrar a aplicação.

### <a name="application-operator"></a>Operador de aplicações

Os utilizadores a **operador aplicações** função não tem acesso ao **Builder de aplicação** página. Estes não podem administrar a aplicação.

## <a name="manage-users"></a>Gerir utilizadores

Os administradores de aplicação podem atribuir utilizadores a funções na aplicação.

### <a name="add-users"></a>Adicionar utilizadores

Cada utilizador tem de ter uma conta de utilizador para que poderem iniciar sessão e aceder a uma aplicação do Azure IoT Central. Microsoft Accounts (MSAs) e Azure Active Directory (AAD) são suportados no Azure IoT Central. Grupos do Active Directory do Azure não são atualmente suportados no Azure IoT Central.

Para obter mais informações, consulte [ajuda de conta Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) e [guia de introdução: adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para adicionar uma conta de utilizador para uma aplicação do Azure IoT Central, utilize o menu de navegação secundário para navegar para o **utilizadores** página no **administração** secção.

    ![Lista de utilizadores](media\howto-administer\image1.png)

1. No **utilizadores** página, escolha **+ adicionar utilizador** para adicionar um utilizador.

    ![Adicionar Utilizador](media\howto-administer\image2.png)

1. Quando adiciona um utilizador à sua aplicação do Azure IoT Central, selecione uma função de utilizador a partir do **função** pendente. Saiba mais sobre as funções no *funções no Azure IoT Central* secção deste artigo.

    ![Seleção da função](media\howto-administer\image3.png)

    > [!NOTE]
    >  Para adicionar utilizadores em volume, introduza os IDs de utilizador de todos os utilizadores que pretende adicionar, separados por ponto e vírgula. Escolher uma função do **função** pendente e escolha **guardar**.

1. Depois de adicionar um utilizador, uma entrada aparece para esse utilizador o **utilizadores** página.

    ![Lista de utilizadores](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>Editar funções atribuídas a utilizadores

Funções não podem ser alteradas uma vez assinged. Para alterar a função atribuída a um utilizador, elimine o utilizador e adicionar o utilizador novamente com uma outra função.

### <a name="delete-users"></a>Eliminar utilizadores

Para eliminar os utilizadores, verificar uma ou mais caixas de verificação a **utilizadores** página e, em seguida, escolha **eliminar**.

## <a name="view-your-bill"></a>Ver a fatura

Para ver a fatura, navegue para o **faturação** página no **administração** secção e escolha **faturação**. Abre a página de faturação do Azure num novo separador e pode ver a fatura para cada uma das suas aplicações do Azure IoT Central.

## <a name="convert-your-trial-to-a-paid-application"></a>Converter a versão de avaliação para uma aplicação paga

Assim que tiver avaliado IoT Central, pode converter a versão de avaliação para uma aplicação paga. Para concluir este processo self-service, siga estes passos:

1. Utilize o menu de navegação secundário para navegar para o **faturação** página no **administração** secção. Se ainda não expandido a versão de avaliação, a página o seguinte aspeto:

    ![Estado de avaliação gratuito](media/howto-administer/freetrial.png)

2. Clique em **converter a paga**. Se ainda não expandido a versão de avaliação, o pop-up o seguinte aspeto:

    No pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure que pretende utilizar para a sua aplicação do Centro de IoT.

    ![Expandir a avaliação gratuita](media/howto-administer/extend.png)

3. Depois de clicar em **converter**, a versão de avaliação é convertida para uma aplicação paga e iniciar a obtenção cobrados.

## <a name="extend-your-free-trial"></a>Expandir a sua avaliação gratuita

Por predefinição, todas as avaliações gratuitas estão disponíveis para 7 dias. Se gostaria de aumentar a sua versão de avaliação de 30 dias, siga estes passos:

1. Utilize o menu de navegação secundário para navegar para o **faturação** página no **administração** secção:

1. Clique em **expandir a avaliação**. No pop-up, selecione o inquilino do Azure Active Directory adequado e, em seguida, a subscrição do Azure a utilizar para a sua aplicação do Centro de IoT:

1. Em seguida, clique em **expanda**. A versão de avaliação já é válida durante 30 dias.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como administrar a sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Configurar o modelo de dispositivo](howto-set-up-template.md)