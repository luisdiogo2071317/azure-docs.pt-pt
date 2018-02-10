---
title: Ligar a uma conta Amazon Web Services ao Azure custo Management | Microsoft Docs
description: "Ligar a uma conta Amazon Web Services para ver o custo e dados de utilização nos relatórios de gestão de custo."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/08/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: a82d803b51859f809ca5a39ce177697a1f66a008
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Ligar a uma conta Amazon Web Services

Tem duas opções para ligar a sua conta Amazon Web Services (AWS) para gestão de custo do Azure. Pode ligar com uma função IAM ou com uma conta de utilizador IAM só de leitura. A função IAM é recomendada porque permite-lhe delegar o acesso com permissões definidas para entidades fidedignas. A função IAM não requerem a partilhar chaves de acesso de longo prazo. Depois de ligar uma conta AWS para gestão de custo, custo e dados de utilização estão disponíveis em relatórios de gestão de custo. Este documento orienta-o ambas as opções.

Para obter mais informações sobre as identidades de AWS IAM, consulte [identidades (utilizadores, grupos e funções)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

## <a name="aws-role-based-access"></a>Acesso baseado em funções do AWS

As secções seguintes explica como criar uma função IAM só de leitura para fornecer acesso à gestão de custo.

### <a name="get-your-cost-management-account-external-id"></a>Obter o seu ID de externo de conta de gestão de custos

O primeiro passo é obter o frase de acesso de ligação exclusiva do portal de gestão de custo do Azure. É utilizado no AWS como o **ID externo**.

1. Abra o portal de Cloudyn do portal do Azure ou navegue até ao [https://azure.cloudyn.com](https://azure.cloudyn.com) e iniciar sessão.
2. Clique em **definições** (ícone roda dentada) e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. No **adicionar conta AWS** caixa de diálogo, copie o **ID externo** e guarde este valor para a função de AWS criação os passos na secção seguinte. Na imagem seguinte, o ID de exemplo é _Cloudyn_. O ID é diferente.  
    ![ID externo](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>Adicionar AWS acesso só de leitura baseada em funções

1. Inicie sessão na consola do AWS no https://console.aws.amazon.com/iam/home e selecione **funções**.
2. Clique em **criar função** e, em seguida, selecione **conta AWS outro**.
3. Cole a identidade `432263259397` no **ID da conta** campo. Este ID de conta é a conta de recoletor de dados de custo de gestão que tem de utilizar.
4. Junto a **opções**, selecione **requerem ID externo** , em seguida, cole o valor que copiou anteriormente no **ID externo** campo e, em seguida, clique em **seguinte: Permissões**.  
    ![Criar função](./media/connect-aws-account/create-role01.png)
5. Em **ligar políticas de permissões**, no **tipo de política** pesquisa de caixa do filtro, escreva `ReadOnlyAccess`, selecione **ReadOnlyAccess**, em seguida, clique em **seguinte: Reveja**.  
    ![Acesso só de leitura](./media/connect-aws-account/readonlyaccess.png)
6. Na página revisão, certifique-se as suas seleções estão corretas e escrevem um **nome da função**. Por exemplo, *Mgt de custo de Azure*. Introduza um **descrição da função**. Por exemplo, _atribuição de função para a gestão de custo de Azure_, em seguida, clique em **criar função**.
7. No **funções** lista, clique na função de que criou e copie o **função ARN** valor a partir da página de resumo. Utilize o valor de função ARN mais tarde ao registar a configuração na gestão de custo do Azure.  
    ![Função ARN](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Configurar o acesso do AWS IAM funções na gestão de custos

1. Abra o portal de Cloudyn do portal do Azure ou navegue para https://azure.cloudyn.com/ e inicie sessão.
2. Clique em **definições** (ícone roda dentada) e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. No **nome da conta**, escreva um nome para a conta.
5. Junto a **acesso tipo**, selecione **IAM função**.
6. No **função ARN** campo, cole o valor que copiou anteriormente e, em seguida, clique em **guardar**.  
    ![Estado da conta AWS](./media/connect-aws-account/aws-account-status01.png)

Sua conta AWS aparece na lista de contas. O **ID do proprietário** listados corresponde ao seu valor ARN de função. O **estado da conta** deve ter um símbolo de marca de verificação verde.

Custo de gestão inicia a recolha de dados e para preencher relatórios. No entanto, alguns relatórios de otimização podem requerer a dados a partir de alguns dias antes de recomendações exata são geradas.

## <a name="aws-user-based-access"></a>Acesso de utilizador com base AWS

As secções seguintes guiá-lo a criar um utilizador só de leitura para fornecer acesso à gestão de custo.

### <a name="add-aws-read-only-user-based-access"></a>Adicionar AWS acesso só de leitura com base no utilizador

1. Inicie sessão na consola do AWS no https://console.aws.amazon.com/iam/home e selecione **utilizadores**.
2. Clique em **adicionar utilizador**.
3. No **nome de utilizador** campo, escreva um nome de utilizador.
4. Para **aceder tipo**, selecione **acesso programático** e clique em **seguinte: permissões**.  
    ![Adicionar utilizador](./media/connect-aws-account/add-user01.png)
5. Para obter permissões, selecione **anexar diretamente as políticas existentes**.
6. Em **ligar políticas de permissões**, no **tipo de política** pesquisa de caixa do filtro, escreva `ReadOnlyAccess`, selecione **ReadOnlyAccess**e, em seguida, clique em **seguinte : Reveja**.  
    ![Defina as permissões de utilizador](./media/connect-aws-account/set-permission-for-user.png)
7. Na página revisão, certifique-se as suas seleções estão corretas e clique em **criar utilizador**.
8. Na página conclua, a sua chave ID e o segredo acesso chave de acesso são apresentadas. Utilize estas informações para configurar o registo na gestão de custo.
9. Clique em **transferir. csv** e guarde o ficheiro de credentials.csv numa localização segura.  
    ![Transferir as credenciais](./media/connect-aws-account/download-csv.png)


### <a name="configure-aws-iam-user-based-access-in-cost-management"></a>Configurar o acesso de utilizador com base do AWS IAM na gestão de custo

1. Abra o portal de Cloudyn do portal do Azure ou navegue para https://azure.cloudyn.com/ e inicie sessão.
2. Clique em **definições** (ícone roda dentada) e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. Para **nome da conta**, escreva um nome de conta.
5. Junto a **acesso tipo**, selecione **IAM utilizador**.
6. No **chave de acesso**, cole o **acesso ID chave** valor a partir do ficheiro credentials.csv.
7. No **chave secreta**, cole o **chave de acesso secreta** valor a partir do ficheiro credentials.csv e, em seguida, clique em **guardar**.  
    ![Estado da conta de utilizador AWS](./media/connect-aws-account/aws-user-account-status.png)

Sua conta AWS aparece na lista de contas. O **estado da conta** deve ter um símbolo de marca de verificação verde.

Custo de gestão inicia a recolha de dados e para preencher relatórios. No entanto, alguns relatórios de otimização podem requerer a dados a partir de alguns dias antes de recomendações exata são geradas.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre o Azure custo Management Cloudyn, avance para o [rever os custos de utilização e](tutorial-review-usage.md) tutorial para a gestão de custo.
