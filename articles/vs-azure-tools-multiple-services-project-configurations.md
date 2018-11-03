---
title: Configurar seu projeto do Azure através de várias configurações de serviço | Documentos da Microsoft
description: Saiba como configurar um projeto do serviço cloud do Azure, alterando os ficheiros servicedefinition. Csdef e Serviceconfiguration ServiceConfiguration.Local.cscfg.
services: visual-studio-online
author: ghogen
manager: douge
assetId: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 1ec587f4a4519f86efceb7cefa0acb372035a9a9
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969714"
---
# <a name="configuring-your-azure-project-in-visual-studio-to-use-multiple-service-configurations"></a>Configurar o seu projeto do Azure no Visual Studio para utilizar várias configurações de serviço

Um projeto do serviço cloud do Azure no Visual Studio inclui três ficheiros de configuração: `ServiceDefinition.csdef`, `ServiceConfiguration.Local.cscfg`, e `ServiceConfiguration.Cloud.cscfg`:

- `ServiceDefinition.csdef` for implementada no Azure para descrever os requisitos de serviço em nuvem e respetivas funções e para fornecer definições que se aplicam a todas as instâncias. As definições podem ser lidos em tempo de execução utilizando a API de tempo de execução de alojamento do Azure Service. Este ficheiro pode ser atualizado no Azure apenas quando o serviço em nuvem está parado.
- `ServiceConfiguration.Local.cscfg` e `ServiceConfiguration.Cloud.cscfg` fornecer valores para as definições na definição do ficheiro e especifique o número de instâncias para ser executada para cada função. O ficheiro "Local" contém valores utilizados na depuração local; o ficheiro de "Nuvem" é implementado para o Azure como `ServiceConfiguration.cscfg` e fornece as definições para o ambiente de servidor. Este ficheiro pode ser atualizado durante a execução de seu serviço cloud no Azure.

Definições de configuração são geridas e modificadas no Visual Studio com páginas de propriedades para a função aplicável (a função com o botão direito e selecione **propriedades**, ou clique duas vezes a função). As alterações podem ser confinadas para qualquer configuração é escolhida no **configuração do serviço** pendente. As propriedades para as funções web e de trabalho são semelhantes, exceto onde descrito nas seções a seguir.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Para obter informações sobre os esquemas subjacentes para a definição de serviço e os ficheiros de configuração de serviço, consulte a [. csdef o esquema XML](cloud-services/schema-csdef-file.md) e [o. cscfg o esquema XML](cloud-services/schema-cscfg-file.md) artigos. Para obter mais informações sobre a configuração do serviço, consulte [como configurar os serviços Cloud](cloud-services/cloud-services-how-to-configure-portal.md).


## <a name="configuration-page"></a>Página de configuração

### <a name="service-configuration"></a>Configuração do serviço

Seleciona qual `ServiceConfiguration.*.cscfg` ficheiro é afetado pelas alterações. Por predefinição, são variantes de Local e na Cloud, e pode usar o **gerir...**  comandos para copiar, mudar o nome e remover ficheiros de configuração. Estes ficheiros são adicionados ao seu projeto de serviço de nuvem e constar **Explorador de soluções**. No entanto, mudar o nome ou remover configurações pode ser feito apenas a partir deste controlo.

### <a name="instances"></a>Instâncias

Definir o **instância** contagem de propriedade para o número de instâncias, o serviço deve ser executado para esta função.

Definir o **tamanho VM** propriedade **muito pequenas**, **pequeno**, **médio**, **grande**, ou **Extra grande**.  Para obter mais informações, consulte [tamanhos para os serviços Cloud](cloud-services/cloud-services-sizes-specs.md).

### <a name="startup-action-web-role-only"></a>Ação de arranque (apenas a função Web)

Defina esta propriedade para especificar que o Visual Studio deve iniciar um navegador da web para pontos de extremidade HTTP ou pontos finais de HTTPS ou ambos quando começa a depuração.

O **ponto final de HTTPS** opção está disponível apenas se já definiu um ponto final HTTPS para a sua função. Pode definir um ponto final HTTPS no **pontos de extremidade** página de propriedades.

Se já tiver adicionado um ponto final HTTPS, a opção de ponto final HTTPS está ativada por predefinição e o Visual Studio inicia um browser para este ponto final, quando iniciar a depuração, além de um browser para o ponto final HTTP, partindo do princípio de que ambas as opções de arranque estão ativadas.

### <a name="diagnostics"></a>Diagnóstico

Por predefinição, os diagnósticos estão ativados para a função da Web. A conta de armazenamento e de projeto de serviço cloud do Azure são definidos para utilizar o emulador de armazenamento local. Quando estiver pronto para implementar no Azure, pode selecionar o botão de builder (**...** ) para utilizar o armazenamento do Azure em vez disso. Pode transferir os dados de diagnóstico para a conta de armazenamento a pedido ou em intervalos agendados automaticamente. Para obter mais informações sobre o diagnóstico do Azure, consulte [ativar diagnósticos nos serviços Cloud do Azure e máquinas virtuais](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Página de definições

Sobre o **definições** página, pode adicionar as definições para uma configuração como pares nome-valor. Código em execução na função pode ler os valores das definições de configuração em tempo de execução usando classes fornecidas pela [biblioteca gerida do Azure](http://go.microsoft.com/fwlink?LinkID=171026), especificamente, o [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) método.

### <a name="configuring-a-connection-string-for-a-storage-account"></a>Configurar uma cadeia de ligação para uma conta de armazenamento

Uma cadeia de ligação é uma definição que fornece informações de ligação e autenticação para o emulador de armazenamento ou para uma conta de armazenamento do Azure. Sempre que o código numa função acede ao armazenamento do Azure (blobs, filas ou tabelas), ele precisa de uma cadeia de ligação.

> [!Note]
> Uma cadeia de ligação para a conta de armazenamento do Azure tem de utilizar um formato definido (consulte [configurar cadeias de ligação de armazenamento do Azure](storage/common/storage-configure-connection-string.md)).

Pode definir a cadeia de ligação para utilizar armazenamento local, conforme necessário, em seguida, defina a uma conta de armazenamento do Azure ao implementar a aplicação do serviço em nuvem. Falha ao definir a cadeia de ligação corretamente pode fazer com que a sua função, não para começar, ou para percorrer os Estados de inicializar, ocupados e interromper.

Para criar uma cadeia de ligação, selecione **Adicionar definição** e defina **tipo** para "Cadeia de ligação".

Para cadeias de ligação nova ou existente, selecione **...** * no lado direito do **valor** campo para abrir o **criar cadeia de ligação de armazenamento** caixa de diálogo:

1. Sob **ligar através de**, escolha a **sua assinatura** opção de selecionar uma conta de armazenamento de uma subscrição. Visual Studio, em seguida, obtém as credenciais da conta de armazenamento automaticamente a partir do `.publishsettings` ficheiro.
1. Selecionando **introduzido manualmente as credenciais** permite-lhe Especifica o nome da conta e chave diretamente usando as informações a partir do portal do Azure. Para copiar a chave de conta:
    1. Navegue para a conta de armazenamento no portal do Azure e selecione **gerir chaves**.
    1. Para copiar a chave da conta, navegue para a conta de armazenamento no portal do Azure, selecione **definições > chaves de acesso**, em seguida, utilize o botão de cópia para copiar a chave de acesso primária para a área de transferência.
1. Selecione uma das opções de ligação. **Especifique os pontos finais personalizados** pede que especifique URLs específicos para blobs, tabelas e coloca em fila. Os pontos finais personalizados permitem-lhe utilizar [domínios personalizados](storage/blobs/storage-custom-domain-name.md) e para controlar o acesso mais exatamente. Ver [configurar cadeias de ligação de armazenamento do Azure](./storage/common/storage-configure-connection-string.md).
1. Selecione **OK**, em seguida, **ficheiro > guardar** para atualizar a configuração com a nova cadeia de ligação.

Novamente, quando publica a aplicação no Azure, escolha a configuração de serviço que contenha a conta de armazenamento do Azure para a cadeia de ligação. Após a publicação da sua aplicação, certifique-se de que o aplicativo funciona corretamente com os serviços de armazenamento do Azure.

Para obter mais informações sobre como atualizar as configurações de serviço, consulte a secção [gerir cadeias de ligação para as contas de armazenamento](vs-azure-tools-configure-roles-for-cloud-service.md#manage-connection-strings-for-storage-accounts).

## <a name="endpoints-page"></a>Página de pontos finais

Uma função da web normalmente possui um único ponto de final HTTP na porta 80. Uma função de trabalho, por outro lado, pode ter qualquer número de pontos finais HTTP, HTTPS ou TCP. Pontos de extremidade podem ser pontos finais de entrada, que estão disponíveis para clientes externos, ou pontos finais internos, estão disponíveis para outras funções que estão em execução no serviço.

- Para disponibilizar um ponto final HTTP para clientes externos e navegadores da Web, altere o tipo de ponto final de entrada e especifique um nome e um número de porta.
- Para disponibilizar um ponto final HTTPS para clientes externos e navegadores da Web, altere o tipo de ponto final para **entrada**e especifique um nome, um número de porta e um nome de certificado de gestão. Também tem de definir o certificado no **certificados** página de propriedades antes de especificar um certificado de gestão. 
- Para disponibilizar um ponto final para o acesso interno por outras funções no serviço cloud, altere o tipo de ponto final para interno e especifique um nome e portas privadas possíveis para este ponto final.

## <a name="local-storage-page"></a>Página de armazenamento local

Pode utilizar o **armazenamento Local** página de propriedades para reservar um ou mais recursos de armazenamento local para uma função. Um recurso de armazenamento local é um diretório reservado no sistema de ficheiros da máquina virtual do Azure em que uma instância de uma função está em execução.

## <a name="certificates-page"></a>Página Certificados

O **certificados** página de propriedades adiciona informações sobre os certificados à sua configuração de serviço. Tenha em atenção que os certificados não são empacotados com o seu serviço; tem de carregar os certificados em separado para o Azure através da [portal do Azure](http://portal.azure.com).

Adicionar um certificado aqui adiciona informações sobre os certificados à sua configuração de serviço. Certificados não são empacotados com o serviço; tem de carregar os certificados em separado através do portal do Azure.

Para associar um certificado com a sua função, forneça um nome para o certificado. Utilizar este nome para fazer referência ao certificado quando configurar um ponto final HTTPS no **pontos de extremidade** página. Em seguida, especifique se o arquivo de certificados **computador Local** ou **utilizador atual** e o nome do arquivo. Por fim, introduza o thumbprint do certificado. Se o certificado está no User\Personal atual arquivo (meu), pode introduzir o thumbprint do certificado ao selecionar o certificado a partir de uma lista preenchida. Se ele reside em qualquer outro local, introduza o valor do thumbprint manualmente.

Quando adicionar um certificado do arquivo de certificados, todos os certificados intermédios são adicionados automaticamente para as definições de configuração para. Além disso, estes certificados intermediários têm de ser carregados para o Azure para configurar corretamente o seu serviço para SSL.

Quaisquer certificados de gestão associado a seu serviço aplicam-se ao seu serviço apenas quando está em execução na cloud. Quando seu serviço está em execução no ambiente de desenvolvimento local, ele usa um certificado padrão que é gerido pelo emulador de computação.
