---
title: Configurar contas de armazenamento para a gestão de custo do Azure | Microsoft Docs
description: Este artigo descreve como configurar contas do storage do Azure e registos de armazenamento do AWS para a gestão de custo do Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: e37604e5cd36cfed016ef596060459011ec32d35
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297840"
---
# <a name="configure-storage-accounts-for-cost-management"></a>Configurar contas de armazenamento para a gestão de custos

<!--- intent: As a Cost Management user, I want to configure Cost Management to use my cloud service provider storage account to store my reports. -->

Pode guardar relatórios de gestão de custos no portal de Cloudyn, armazenamento do Azure ou AWS registos de armazenamento. A guardar os seus relatórios do portal do Cloudyn é gratuitamente. No entanto, guardar os seus relatórios para o armazenamento do seu fornecedor de serviços de nuvem é opcional e implica um custo adicional. Este artigo ajuda-o a configurar contas do storage do Azure e registos de armazenamento do Amazon Web Services (AWS) para armazenar os seus relatórios.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma conta de armazenamento do Azure ou um registo de armazenamento da Amazon.

Se não tiver uma conta de armazenamento do Azure, terá de criar um. Para obter mais informações sobre como criar uma conta de armazenamento do Azure, consulte [criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Se não tiver um AWS registo do serviço (S3) de armazenamento simples, terá de criar um. Para obter mais informações sobre como criar um registo de S3, consulte [criar um registo](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Configurar a conta de armazenamento do Azure

Armazenamento do Azure para utilização por gestão de custo de configuração é simples. Recolher detalhes sobre a conta de armazenamento e copie-os no portal do Cloudyn.

1. Inicie sessão no portal do Azure em http://portal.azure.com.
2. Clique em **todos os serviços**, selecione **contas do Storage**, desloque-se para a conta de armazenamento que pretende utilizar e, em seguida, selecione a conta.
3. Na sua página de conta de armazenamento em **definições**, clique em **chaves de acesso**.
4. Copiar o **nome da conta de armazenamento** e **cadeia de ligação** em chave1.  
![Chaves de acesso de armazenamento do Azure](./media/storage-accounts/azure-storage-access-keys.png)  
5. Abra o portal de Cloudyn do portal do Azure ou navegue até ao https://azure.cloudyn.com e iniciar sessão.
6. Clique no símbolo roda dentada por e, em seguida, selecione **relatórios de gestão de armazenamento**.
7. Clique em **adicionar novo +** e certifique-se de que o Microsoft Azure está selecionado. Cole o seu nome de conta de armazenamento do Azure no **nome** área. Colar o **cadeia de ligação** na área de correspondente. Introduza um nome de contentor e, em seguida, clique em **guardar**.  
![Armazenamento de Cloudyn configurado para o Azure](./media/storage-accounts/azure-cloudyn-storage.png)

  A nova entrada de armazenamento do Azure de relatório é apresentado na lista de conta de armazenamento.  
    ![Novo relatório do Azure armazenamento na lista](./media/storage-accounts/azure-storage-entry.png)


Agora pode guardar relatórios de armazenamento do Azure. Qualquer relatório, clique em **ações** e, em seguida, selecione **agendar relatórios**. Nome do relatório e adicionar o seu próprio URL ou utilize o URL criado automaticamente. Selecione **guardar para o armazenamento** e, em seguida, selecione a conta de armazenamento. Introduza um prefixo que obtém acrescentado ao nome do ficheiro de relatório. Selecione o formato de ficheiro CSV ou JSON e, em seguida, guardar o relatório.

## <a name="configure-an-aws-storage-bucket"></a>Configurar um registo de armazenamento do AWS

O Cloudyn utiliza credenciais AWS existentes: utilizador ou função, para guardar os relatórios para o registo. Para testar o acesso, Cloudyn tentar guardar um pequeno ficheiro de texto para o registo com o nome de ficheiro _permission.txt de registo de verificação_.

Forneça o utilizador ou função Cloudyn com a permissão de PutObject para o registo. Em seguida, utilizar um registo existente ou crie um novo para guardar relatórios. Por fim, decida como gerir a classe de armazenamento, definir regras de ciclo de vida ou remover quaisquer ficheiros desnecessários.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>Atribuir permissões ao utilizador AWS ou função

Ao criar uma nova política, está a fornecer as permissões exatas necessárias para guardar um relatório para um registo de S3.

1. Iniciar sessão consola do AWS e selecionar **serviços**.
2. Selecione **IAM** da lista de serviços.
3. Selecione **políticas** no lado esquerdo da consola e, em seguida, clique em **criar política**.
4. Clique em de **JSON** separador.
5. A seguinte política permite-lhe guardar um relatório para um registo de S3. Copie e cole o seguinte exemplo de política para o **JSON** separador. Substitua &lt;bucketname&gt; com o nome do registo.

  ```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
}
```

6. Clique em **leia a política**.  
    ![Leia a política](./media/storage-accounts/aws-policy.png)  
7. Na página de política de revisão, escreva um nome para a sua política. Por exemplo, _CloudynSaveReport2S3_.
8. Clique em **criar política**.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>Anexar a política a uma função de Cloudyn ou o utilizador na sua conta

Para ligar a nova política, abra a consola AWS e editar a função de Cloudyn ou o utilizador.

1. Iniciar sessão consola do AWS e selecionar **serviços**, em seguida, selecione **IAM** da lista de serviços.
2. Selecione **funções** ou **utilizadores** do lado esquerdo da consola.

**Para funções:**

  1. Clique no seu nome de função Cloudyn.
  2. No **permissões** separador, clique em **anexar política**.
  3. Procure a política que criou e selecione-o e clique em **anexar política**.
    ![AWS - anexar política para uma função](./media/storage-accounts/aws-attach-policy-role.png)

**Para os utilizadores:**

1. Selecione o utilizador Cloudyn.
2. No **permissões** separador, clique em **adicionar permissões**.
3. No **conceder permissão** secção, selecione **anexar diretamente as políticas existentes**.
4. Procure a política que criou e selecione-o e clique em **seguinte: Reveja**.
5. As permissões de adicionar à página de nome de função, clique em **adicionar permissões**.  
    ![AWS - anexar política para um utilizador](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>Opcional: Definir permissões com a política de registo

Também é possível definir permissão para criar relatórios sobre o registo de S3 através de uma política de registo. Na vista de S3 clássica:

1. Crie ou selecione um registo existente.
2. Selecione o **permissões** separador e, em seguida, clique em **registo política**.
3. Copie e cole o seguinte exemplo de política. Substitua &lt;registo\_nome&gt; e &lt;Cloudyn\_princípio&gt; com ARN do seu registo. Substitua o ARN da função ou utilizador utilizado pelo Cloudyn.

  ```
{
  "Id": "Policy1485775646248",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
  ]
}
```

4. No editor de política de registo, clique em **guardar**.

### <a name="add-aws-report-storage-to-cloudyn"></a>Adicionar o armazenamento de relatório do AWS para Cloudyn

1. Abra o portal de Cloudyn do portal do Azure ou navegue até ao https://azure.cloudyn.com e iniciar sessão.
2. Clique no símbolo roda dentada por e, em seguida, selecione **relatórios de gestão de armazenamento**.
3. Clique em **adicionar novo +** e certifique-se de que o AWS está selecionado.
4. Selecione um registo de conta e de armazenamento. O nome do registo de armazenamento AWS é automaticamente preenchida-in.  
    ![Adicionar armazenamento de relatório para o registo do AWS](./media/storage-accounts/aws-cloudyn-storage.png)  
5. Clique em **guardar** e, em seguida, clique em **Ok**.

    A nova entrada de armazenamento de relatório do AWS aparece na lista de conta de armazenamento.  
    ![AWS novo armazenamento na lista de relatórios](./media/storage-accounts/aws-storage-entry.png)


Agora pode guardar relatórios de armazenamento do Azure. Qualquer relatório, clique em **ações** e, em seguida, selecione **agendar relatórios**. Nome do relatório e adicionar o seu próprio URL ou utilize o URL criado automaticamente. Selecione **guardar para o armazenamento** e, em seguida, selecione a conta de armazenamento. Introduza um prefixo que obtém acrescentado ao nome do ficheiro de relatório. Selecione o formato de ficheiro CSV ou JSON e, em seguida, guardar o relatório.

## <a name="next-steps"></a>Passos Seguintes

- Reveja [relatórios de gestão de custos de compreender](understanding-cost-reports.md) para saber mais sobre a estrutura básica e as funções de relatórios de gestão de custo.
