---
title: Ligar a uma conta Amazon Web Services ao Azure custo Management | Microsoft Docs
description: Ligar a uma conta Amazon Web Services para ver o custo e dados de utilização nos relatórios de gestão de custo.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 0c18fc065ae4d9a9401a8d603f051e9d6236c538
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Ligar a uma conta Amazon Web Services

Tem duas opções para ligar a sua conta Amazon Web Services (AWS) para gestão de custo do Azure. Pode ligar com uma função IAM ou com uma conta de utilizador IAM só de leitura. A função IAM é recomendada porque permite-lhe delegar o acesso com permissões definidas para entidades fidedignas. A função IAM não requerem a partilhar chaves de acesso de longo prazo. Depois de ligar uma conta AWS para gestão de custo, custo e dados de utilização estão disponíveis em relatórios de gestão de custo. Este documento orienta-o ambas as opções.

Para obter mais informações sobre as identidades de AWS IAM, consulte [identidades (utilizadores, grupos e funções)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Além disso, ative o AWS detalhadas relatórios de faturação e armazenar as informações no registo de serviço (S3) de armazenamento simples AWS. Relatórios detalhados de faturação incluem taxas com informações de tag e recursos numa base horária. Armazenar os relatórios permite a gestão de custo, obtê-los a partir do seu registo e apresentar as informações no seus relatórios.


## <a name="aws-role-based-access"></a>Acesso baseado em funções do AWS

As secções seguintes explica como criar uma função IAM só de leitura para fornecer acesso à gestão de custo.

### <a name="get-your-cost-management-account-external-id"></a>Obter o seu ID de externo de conta de gestão de custos

O primeiro passo é obter o frase de acesso de ligação exclusiva do portal de gestão de custo do Azure. É utilizado no AWS como o **ID externo**.

1. Abra o portal de Cloudyn do portal do Azure ou navegue até ao [ https://azure.cloudyn.com ](https://azure.cloudyn.com) e iniciar sessão.
2. Clique no símbolo roda dentada por e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. No **adicionar conta AWS** caixa de diálogo, copie o **ID externo** e guarde o valor para a função de AWS criação os passos na secção seguinte. O ID externo é exclusivo para a sua conta. ID externo na imagem seguinte, o exemplo é _Contoso_ seguido por um número. O ID é diferente.  
    ![ID externo](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>Adicionar AWS acesso só de leitura baseada em funções

1. Inicie sessão na consola do AWS no https://console.aws.amazon.com/iam/home e selecione **funções**.
2. Clique em **criar função** e, em seguida, selecione **conta AWS outro**.
3. No **ID da conta** caixa, cole `432263259397`. Este ID de conta é a conta de recoletor de dados de gestão de custos atribuída pelo AWS para o serviço de Cloudyn. Utilize o ID de conta exato apresentado.
4. Junto a **opções**, selecione **requerem ID externo**. Colar o valor exclusivo que copiou anteriormente do **ID externo** campo na gestão de custo. Em seguida, clique em **seguinte: permissões**.  
    ![Criar função](./media/connect-aws-account/create-role01.png)
5. Em **ligar políticas de permissões**, no **tipo de política** pesquisa de caixa do filtro, escreva `ReadOnlyAccess`, selecione **ReadOnlyAccess**, em seguida, clique em **seguinte: Reveja**.  
    ![Acesso só de leitura](./media/connect-aws-account/readonlyaccess.png)
6. Na página revisão, certifique-se as suas seleções estão corretas e escrevem um **nome da função**. Por exemplo, *Mgt de custo de Azure*. Introduza um **descrição da função**. Por exemplo, _atribuição de função para a gestão de custo de Azure_, em seguida, clique em **criar função**.
7. No **funções** lista, clique na função de que criou e copie o **função ARN** valor a partir da página de resumo. Utilize o valor de função ARN (nome de recurso do Amazon) mais tarde ao registar a configuração na gestão de custo do Azure.  
    ![Função ARN](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Configurar o acesso do AWS IAM funções na gestão de custos

1. Abra o portal de Cloudyn do portal do Azure ou navegue até ao https://azure.cloudyn.com/ e iniciar sessão.
2. Clique no símbolo roda dentada por e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. No **nome da conta**, escreva um nome para a conta.
5. Junto a **acesso tipo**, selecione **IAM função**.
6. No **função ARN** campo, cole o valor que copiou anteriormente e, em seguida, clique em **guardar**.  
    ![Adicionar caixa conta AWS](./media/connect-aws-account/add-aws-account-box.png)


Sua conta AWS aparece na lista de contas. O **ID do proprietário** listados corresponde ao seu valor ARN de função. O **estado da conta** deve ter um símbolo de marca de verificação verde a indicar que a gestão de custo podem aceder a sua conta AWS. Até que tenha ativado a faturação detalhada do AWS, o estado de consolidação é apresentado como **autónomo**.

![Estado da conta AWS](./media/connect-aws-account/aws-account-status01.png)

Custo de gestão inicia a recolha de dados e para preencher relatórios. Em seguida, [ativar detalhada faturação do AWS](#enable-detailed-aws-billing).


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

1. Abra o portal da Cloudyn a partir do portal do Azure ou navegue para https://azure.cloudyn.com/ e inicie sessão.
2. Clique no símbolo roda dentada por e, em seguida, selecione **contas na nuvem**.
3. Na gestão de contas, selecione o **AWS contas** separador e, em seguida, clique em **adicionar novo +**.
4. Para **nome da conta**, escreva um nome de conta.
5. Junto a **acesso tipo**, selecione **IAM utilizador**.
6. No **chave de acesso**, cole o **acesso ID chave** valor a partir do ficheiro credentials.csv.
7. No **chave secreta**, cole o **chave de acesso secreta** valor a partir do ficheiro credentials.csv e, em seguida, clique em **guardar**.  

Sua conta AWS aparece na lista de contas. O **estado da conta** deve ter um símbolo de marca de verificação verde.

Custo de gestão inicia a recolha de dados e para preencher relatórios. Em seguida, [ativar detalhada faturação do AWS](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Ativar a faturação do AWS detalhada

Utilize os seguintes passos para obter o ARN de função AWS. Utilize o ARN de função para conceder permissões de leitura para um registo de faturação.

1. Inicie sessão na consola do AWS no https://console.aws.amazon.com e selecione **serviços**.
2. Na pesquisa de serviço caixa tipo *IAM*e selecionar essa opção.
3. Selecione **funções** no menu esquerdo.
4. Na lista de funções, selecione a função que criou para Cloudyn acesso.
5. Na página de resumo de funções, clique para copiar o **função ARN**. Manter a função ARN útil para os passos seguintes.

### <a name="create-an-s3-bucket"></a>Criar um registo de S3

Criar um registo de S3 para armazenar informações de faturação detalhadas.

1. Inicie sessão na consola do AWS no https://console.aws.amazon.com e selecione **serviços**.
2. Na pesquisa de serviço caixa tipo *S3*e selecione **S3**.
3. Na página Amazon S3, clique em **criar registo**.
4. No Assistente de registo de criar, escolha um nome de registo e a região e, em seguida, clique em **seguinte**.  
    ![Criar registo](./media/connect-aws-account/create-bucket.png)
5. No **definir propriedades** página, mantenha os valores predefinidos e, em seguida, clique em **seguinte**.
6. Na página revisão, clique em **criar registo**. É apresentada a lista de registo.
7. Clique o registo que criou e selecione o **permissões** separador e, em seguida, selecione **registo política**. Abre o editor de políticas de registo.
8. Copie o seguinte exemplo JSON e colá-lo no editor de políticas de registo.
  - Substitua `<BillingBucketName>` com o nome do seu registo de S3.
  - Substitua `<ReadOnlyUserOrRole>` com a função ou utilizador ARN que tinha copiou anteriormente.

  ```
  {
    "Version": "2012-10-17",
    "Id": "Policy1426774604000",
    "Statement": [
        {
            "Sid": "Stmt1426774604000",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": [
                "s3:GetBucketAcl",
                "s3:GetBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>"
        },
        {
            "Sid": "Stmt1426774604001",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        },
        {
            "Sid": "Stmt1426774604002",
            "Effect": "Allow",
            "Principal": {
                "AWS": "<ReadOnlyUserOrRole>"
            },
            "Action": [
                "s3:List*",
                "s3:Get*"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        }
    ]
  }
  ```

9. Clique em **Guardar**.  
    ![Editor de políticas de registo](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>Ativar AWS relatórios de faturação

Depois de criar e configurar o registo de S3, navegue para [preferências de faturação](https://console.aws.amazon.com/billing/home?#/preference) na consola do AWS.

1. Na página de preferências, selecione **receber relatórios de faturação**.
2. Em **receber relatórios de faturação**, introduza o nome do registo que criou e, em seguida, clique em **verifique**.  
3. Selecione todos os quatro opções de granularidade de relatório e, em seguida, clique em **guardar preferências**.  
    ![Ative relatórios](./media/connect-aws-account/enable-reports.png)

Custo de gestão obtém informações detalhadas de faturação do seu registo de S3 e preenche relatórios depois de faturação detalhada está ativada. Pode demorar até 24 horas até que os dados detalhados de faturação é apresentado na consola do Cloudyn. Quando estão disponíveis dados de faturação de detalhado, o estado de consolidação de conta é apresentado como **consolidado**. Estado da conta é apresentado como **concluído**.

![Conta consolidados Estado](./media/connect-aws-account/consolidated-status.png)

Alguns dos relatórios de otimização podem necessitar de alguns dias de dados para obter um tamanho de amostra de dados adequada para recomendações precisos.

## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre a gestão de custo do Azure, avance para o [rever os custos de utilização e](tutorial-review-usage.md) tutorial para a gestão de custo.
