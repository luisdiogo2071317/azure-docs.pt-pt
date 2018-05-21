---
title: Utilize instâncias de contentor do Azure como um Jenkins Criar agente
description: Saiba como utilizar as instâncias de contentor do Azure como um Jenkins Criar agente.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/20/2018
ms.author: nepeters
ms.openlocfilehash: 4df230c8306a3876e94a5e9ada5e7408f134ba26
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Utilize instâncias de contentor do Azure como um Jenkins Criar agente

Instâncias de contentor do Azure (ACI) fornece um ambiente a pedido, burstable e isolado para a execução de cargas de trabalho. Devido a estes atributos ACI faz com que uma plataforma excelente para executar tarefas de compilação Jenkins em grande escala. Este artigo explica implementar e utilizar um servidor de Jenkins pré-configurado com ACI como um destino de compilação.

Para obter mais informações sobre as instâncias de contentor do Azure, consulte [sobre instâncias de contentor do Azure][about-aci].

## <a name="deploy-a-jenkins-server"></a>Implementar um servidor de Jenkins

1. No portal do Azure, selecione **crie um recurso** e procure **Jenkins**. Selecione a oferta Jenkins com um editor de **Microsoft**e, em seguida, selecione **criar**.

2. Introduza as seguintes informações o **Noções básicas** formam e, em seguida, selecione **OK**.

   - **Nome**: introduza um nome para a implementação de Jenkins.
   - **Nome de utilizador**: introduza um nome para o utilizador de admin da máquina virtual Jenkins.
   - **Tipo de autenticação**: Recomendamos uma chave pública SSH para a autenticação. Se selecionar esta opção, cole uma chave pública SSH a ser utilizada para iniciar sessão para a máquina virtual de Jenkins.
   - **Subscrição**: selecione uma subscrição do Azure.
   - **Grupo de recursos**: crie um grupo de recursos ou selecione um grupo existente.
   - **Localização**: selecione uma localização para o servidor de Jenkins.

   ![Definições básicas para implementação no portal Jenkins](./media/container-instances-jenkins/jenkins-portal-01.png)

3. No **definições adicionais** formulário, conclua os seguintes itens:

   - **Tamanho**: selecione a opção de dimensionamento adequado para a máquina virtual de Jenkins.
   - **Tipo de disco da VM**: especifique **HDD** (unidade de disco rígido) ou **SSD** (unidade de estado sólido) para o servidor de Jenkins.
   - **Rede virtual**: selecione a seta para a se pretender modificar as predefinições.
   - **Sub-redes**: selecione a seta, verifique as informações e selecione **OK**.
   - **Endereço IP público**: selecione a seta para atribuir o endereço IP público um nome personalizado, configure o SKU e definir o método de atribuição.
   - **Etiqueta de nome de domínio**: Especifique um valor para criar um URL para a máquina virtual de Jenkins completamente qualificado.
   - **Jenkins versão tipo**: selecione o tipo de versão pretendida entre as opções: **LTS**, **semanalmente criar**, ou **Azure verificado**.

   ![Definições adicionais para implementação no portal Jenkins](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Integração de principal de serviço, selecione **Auto(MSI)** ter [identidade de serviço gerida do Azure] [ managed-service-identity] criar automaticamente uma identidade de autenticação para o Jenkins instância. Selecione **Manual** para fornecer as suas próprias credenciais de principal de serviço.

5. Agentes de nuvem configurar uma plataforma baseado na nuvem para as tarefas de compilação Jenkins. Com vista à, neste artigo, selecione **ACI**. Com o agente de cloud ACI, cada tarefa de compilação Jenkins é executada numa instância de contentor.

   ![Definições de integração em nuvem para implementação no portal Jenkins](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Quando tiver terminado com as definições de integração, selecione **OK**e, em seguida, selecione **OK** novamente no resumo de validação. Selecione **criar** no **termos de utilização** resumo. O servidor de Jenkins demora alguns minutos a implementar.

## <a name="configure-jenkins"></a>Configurar o Jenkins

1. No portal do Azure, navegue para o grupo de recursos Jenkins, selecione a máquina virtual de Jenkins e tome nota do nome DNS.

   ![Nome DNS nos detalhes sobre a máquina virtual de Jenkins](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Navegue para o nome DNS da Jenkins VM e copie a cadeia devolvida do SSH.

   ![Instruções de início de sessão Jenkins com a cadeia de SSH](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Abra uma sessão de terminal no sistema de desenvolvimento e cole a cadeia SSH no último passo. Atualização `username` para o nome de utilizador que especificou quando implementou o servidor de Jenkins.

4. Após a sessão está ligada, execute o seguinte comando para obter a palavra-passe de administrador inicial:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. Deixe a sessão SSH e túnel em execução e aceda a http://localhost:8080 num browser. Cole a palavra-passe de administrador inicial para a caixa e, em seguida, selecione **continuar**.

   !["Desbloquear Jenkins" ecrã com a caixa da palavra-passe de administrador](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Selecione **instalar plug-ins sugeridos** instalar todos os plug-ins de Jenkins de recomendado.

   ![Ecrã "Personalizar Jenkins" com "Instalar o plug-ins sugeridos" selecionado](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Crie uma conta de utilizador administrador. Esta conta é utilizada para iniciar sessão e trabalhar com a instância de Jenkins.

   ![Ecrã "Criar primeiro utilizador de Admin", com credenciais preenchidos](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Selecione **guardar e concluir**e, em seguida, selecione **começar a utilizar Jenkins** para concluir a configuração.

Jenkins está agora configurado e pronto para criar e implementar código. Neste exemplo, uma aplicação de Java simples é utilizada para demonstrar uma compilação Jenkins em instâncias de contentor do Azure.

## <a name="create-a-build-job"></a>Criar uma tarefa de compilação

Quando estiver a utilizar uma imagem do contentor como um Jenkins criar destino, tem de especificar uma imagem que inclui todas as ferramentas necessárias para uma compilação efetuada com êxito. Para especificar a imagem:

1. Selecione **gerir Jenkins** > **Configurar sistema** e desloque para baixo até o **nuvem** secção. Neste exemplo, atualize o valor de imagem de Docker para **microsoft/java-em-azure-jenkins-multisservidor**.

   Quando estiver pronto, selecione **guardar** para regressar ao Jenkins dashboard.

   ![Configuração da nuvem Jenkins](./media/container-instances-jenkins/jenkins-aci-image.png)

2. Crie uma tarefa de compilação Jenkins. Selecione **Novo Item**, dê um nome de projeto de compilação como **aci-java-demonstração**, selecione **projeto Freestyle**e selecione **OK**.

   ![Caixa para o nome da tarefa de compilação e a lista de tipos de projeto](./media/container-instances-jenkins/jenkins-new-job.png)

3. Em **geral**, certifique-se de que **restringir onde é possível executar este projeto** está selecionada. Introduza **linux** para a expressão da etiqueta. Esta configuração assegura que esta tarefa de compilação é executado na nuvem ACI.

   ![Separador "Geral" com os detalhes de configuração](./media/container-instances-jenkins/jenkins-job-01.png)

4. Em **gestão da origem de código**, selecione **Git** e introduza **https://github.com/spring-projects/spring-petclinic.git** para o URL do repositório. Este repositório do GitHub contém o código de aplicação de exemplo.

   ![Separador "Gestão de código de origem" com as informações de código de origem](./media/container-instances-jenkins/jenkins-job-02.png)

5. Em **criar**, selecione **Adicionar passo de compilação** e selecione **invocar destinos nível superior do Maven**. Introduza **pacote** como o objetivo do passo de compilação.

   ![Separador "Criar" com as seleções para o passo de compilação](./media/container-instances-jenkins/jenkins-job-03.png)

6. Selecione **Guardar**.

## <a name="run-the-build-job"></a>Executar a tarefa de compilação

Para testar a tarefa de compilação e observar instâncias de contentor do Azure como a plataforma de compilação, é iniciada manualmente a compilação.

1. Selecione **compilar agora** para iniciar uma tarefa de compilação. Demora alguns minutos para que a tarefa iniciar. Deverá ver um Estado que é semelhante a imagem seguinte:

   ![Informações de "Histórico de compilação" com o estado da tarefa](./media/container-instances-jenkins/jenkins-job-status.png)

2. Enquanto a tarefa está em execução, abra o portal do Azure e observe o grupo de recursos Jenkins. Deverá ver que foi criada uma instância de contentor. A tarefa de Jenkins está em execução dentro desta instância.

   ![Instância de contentor do grupo de recursos](./media/container-instances-jenkins/jenkins-aci.png)

3. Jenkins executa tarefas mais do que o número de executor Jenkins (predefinição 2) configurado, são criadas várias instâncias de contentor.

   ![Recém-criado instâncias de contentor](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Depois de terminar todas as tarefas de compilação, são removidas as instâncias de contentor.

   ![Grupo de recursos com instâncias de contentor removido](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre Jenkins no Azure, consulte o artigo [Azure e Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md