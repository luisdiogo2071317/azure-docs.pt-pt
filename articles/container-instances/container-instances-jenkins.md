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
ms.openlocfilehash: dbe418db8fb3d73739a30b60703f15b3dc47b291
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Utilize instâncias de contentor do Azure como um Jenkins Criar agente

Instâncias de contentor do Azure fornecem um ambiente a pedido, burstable e isolado para execução de carga de trabalho. Devido a estes atributos, instâncias de contentor do Azure, certifique-uma plataforma excelente para executar tarefas de compilação Jenkins em grande escala. Este documento explica implementar e utilizar um servidor de Jenkins está pré-configurada com ACI como um destino de compilação.

Para obter mais informações sobre as instâncias de contentor do Azure, consulte [sobre instâncias de contentor do Azure][about-aci].

## <a name="deploy-jenkins-server"></a>Implementar o servidor de Jenkins

No portal do Azure, selecione **crie um recurso** e procure **Jenkins**. Selecione a oferta Jenkins com um editor de **Microsoft** e selecione **criar**.

Introduza as seguintes informações no formulário Noções básicas e clique em **OK** quando terminar.

- **Nome** - nome para a implementação de Jenkins.
- **Nome de utilizador** -este nome de utilizador é utilizado como o utilizador de administrador para a máquina virtual de Jenkins.
- **Tipo de autenticação** - SSH recomenda-se a chave pública. Se selecionado, copie uma chave pública SSH para ser utilizado quando iniciar sessão na máquina virtual Jenkins.
- **Subscrição** -Selecione uma subscrição do Azure.
- **Grupo de recursos** - criar um novo ou selecione um grupo de recursos existente.
- **Localização** -Selecione uma localização para o servidor de Jenkins.

![Definições básicas de implementação no portal de Jenkins](./media/container-instances-jenkins/jenkins-portal-01.png)

No formulário definições adicionais, conclua os seguintes itens:

- **Tamanho** -selecionar a opção de dimensionamento adequado para a máquina virtual de Jenkins.
- **Tipo de disco da VM** -especificar HDD (unidade de disco rígido) ou SSD (unidade de estado sólido) para o servidor de Jenkins.
- **Rede virtual** -rede (opcional) selecione Virtual para modificar as predefinições.
- **Sub-redes** - selecionar sub-redes, verifique as informações e selecione **OK**.
- **Endereço IP público** -selecionar o endereço IP público permite-lhe atribuir-lhe um nome personalizado, configure o SKU e o método de atribuição.
- **Etiqueta de nome de domínio** -Especifique um valor para criar um URL para a máquina virtual de Jenkins completamente qualificado.
- **Tipo de versão Jenkins** -selecione o tipo de versão pretendida entre as opções: LTS, compilação semanal ou verificar o Azure.

![Definições adicionais do Jenkins implementação no portal](./media/container-instances-jenkins/jenkins-portal-02.png)

Integração de principal de serviço, selecione **Auto(MSI)** ter [identidade de serviço gerida do Azure] [ managed-service-identity] automática criar uma identidade de autenticação para a instância de Jenkins. Selecione o Manual ao fornecedor as suas próprias credenciais de principal de serviço.

Agentes de nuvem configurar uma plataforma baseado na nuvem para as tarefas de compilação Jenkins. Com vista à, este documento, selecione ACI. Com o agente de cloud ACI, cada tarefa de compilação Jenkins é executada numa instância de contentor do Azure.

![Definições de integração de nuvem de implementação no portal de Jenkins](./media/container-instances-jenkins/jenkins-portal-03.png)

Quando tiver terminado com as definições de integração, clique em **OK**e, em seguida, **OK** novamente no resumo de validação. Clique em **criar** em termos de resumo de utilização. O servidor de Jenkins demora alguns minutos a implementar.

## <a name="configure-jenkins"></a>Configurar o Jenkins

No portal do Azure, navegue para o grupo de recursos Jenkins, selecione a máquina virtual de Jenkins e tome nota do nome DNS.

![Instruções de início de sessão Jenkins](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

Browser para o nome DNS da Jenkins VM e copiar a cadeia devolvida do SSH.

![Instruções de início de sessão Jenkins](./media/container-instances-jenkins/jenkins-portal-04.png)

Abrir uma sessão de terminal no sistema de desenvolvimento e cole a cadeia SSH no último passo. O nome de utilizador especificado ao implementar o servidor de Jenkins de atualização 'de username'.

Assim que estiver ligado, execute o seguinte comando para obter a palavra-passe de administrador inicial.

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Deixe a sessão SSH e túnel em execução e navegue para http://localhost:8080 num browser. Cole a palavra-passe de administrador inicial para o campo como mostrado na imagem seguinte. Selecione **continuar** quando terminar.

![Desbloquear Jenkins](./media/container-instances-jenkins/jenkins-portal-05.png)

Selecione **instalar plug-ins sugeridos** instalar todos os plug-ins de Jenkins de recomendado.

![Instalar o plug-in do Jenkins](./media/container-instances-jenkins/jenkins-portal-06.png)

Crie uma nova conta de utilizador de admin. Esta conta é utilizada para iniciar sessão no e trabalhar com a instância de Jenkins.

![Criar utilizador Jenkins](./media/container-instances-jenkins/jenkins-portal-07.png)

Selecione **guardar e concluir** quando terminar e, em seguida, **começar a utilizar Jenkins** para concluir a configuração.

Jenkins está agora configurado e pronto para criar e implementar código. Neste exemplo, uma aplicação de Java simples é utilizada para demonstrar uma compilação Jenkins em instâncias de contentor do Azure.

## <a name="create-build-job"></a>Criar tarefa de compilação

Quando utilizar uma imagem do contentor como um Jenkins criar destino, terá de especificar uma imagem que inclui todas as ferramentas necessárias para uma compilação efetuada com êxito. Para especificar a imagem, selecione **gerir Jenkins** > **Configurar sistema** e desloque para baixo até o **nuvem** secção. Neste exemplo, atualize o valor de imagem de Docker para `microsoft/java-on-azure-jenkins-slave`.

Quando tiver terminado, clique em **guardar** para regressar ao Jenkins dashboard.

![Configuração da nuvem Jenkins](./media/container-instances-jenkins/jenkins-aci-image.png)

Crie uma tarefa de compilação Jenkins. Selecione **Novo Item**, dê um nome de projeto de compilação como `aci-java-demo`, selecione **projeto Freestyle**e clique em **OK**.

![Criar tarefa do Jenkins](./media/container-instances-jenkins/jenkins-new-job.png)

Em **geral**, certifique-se de que **restringir onde é possível executar este projeto** está selecionada. Introduza `linux` para a expressão da etiqueta. Esta configuração assegura que esta tarefa de compilação é executado na nuvem ACI.

![Criar tarefa do Jenkins](./media/container-instances-jenkins/jenkins-job-01.png)

Em gestão de código de origem, selecione `Git` e introduza `https://github.com/spring-projects/spring-petclinic.git` para o URL do repositório. Este repositório do GitHub contém o código de aplicação de exemplo.

![Adicione o código de origem para a tarefa de Jenkins](./media/container-instances-jenkins/jenkins-job-02.png)

Em compilação, selecione **adicione um passo de compilação** e selecione `Invoke top-level Maven targets`. Introduza `package` como o objetivo do passo de compilação.

![Adicione o que passo de compilação Jenkins](./media/container-instances-jenkins/jenkins-job-03.png)

Selecione **guardar** quando terminar.

## <a name="run-the-build-job"></a>Executar a tarefa de compilação

Para testar a tarefa de compilação e observar instâncias de contentor do Azure como a plataforma de compilação, é iniciada manualmente a compilação.

Selecione **compilar agora** para iniciar uma tarefa de compilação. Demora alguns minutos para que a tarefa iniciar, quando em execução, deverá ver o estado semelhante às seguintes imagens.

![Adicione o que passo de compilação Jenkins](./media/container-instances-jenkins/jenkins-job-status.png)

Enquanto a tarefa está em execução, abra o portal do Azure e observe o grupo de recursos Jenkins. Deverá ver que foi criada uma instância de contentor do Azure. É dentro desta instância que está a executar a tarefa de Jenkins.

![Jenkins baseia-se no ACI](./media/container-instances-jenkins/jenkins-aci.png)

Jenkins executa tarefas mais do que o número de executor Jenkins (predefinição 2) configurado, são criadas várias instâncias de contentor do Azure.

![Jenkins baseia-se no ACI](./media/container-instances-jenkins/jenkins-aci-multi.png)

Depois de todas as tarefas de compilação foram concluídas, são removidas as instâncias de contentor do Azure.

![Jenkins baseia-se no ACI](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre Jenkins no Azure, consulte [Azure e Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md