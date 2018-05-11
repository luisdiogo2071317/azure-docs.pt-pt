---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/25/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: db241c1a3c8bfd15e13ae0bd9f1cdf4c92c7081d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
>[!NOTE]
> Pode deixar comentários nesta página para comentários ou através de [comentários Azure](https://feedback.azure.com/forums/216843-virtual-machines) #azerrormessage ativado por etiqueta.

## <a name="error-response-format"></a>Formato de resposta de erro 
As VMs do Azure utilizam o seguinte formato JSON da resposta de erro:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Uma resposta de erro inclui sempre um código de estado e um objeto de erro. Cada objeto erro contém sempre um código de erro e uma mensagem. Se a VM é criada com um modelo, o objeto erro também contém uma secção de detalhes que contém um nível interno de códigos de erro e a mensagem. Normalmente, o nível mais interno de mensagem de erro é a falha de raiz.


## <a name="common-virtual-machine-management-errors"></a>Erros comuns de gestão de máquina virtual

Esta secção apresenta as mensagens de erro comuns que poderão surgir quando gerir VMs:

|  Código de Erro  |  Mensagem de Erro  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Falha ao adquirir concessão durante a criação do disco '{0}' utilizando o blob com o URI {1}. BLOB já está em utilização.  |  
|  AllocationFailed  |  Falha na alocação. Tente reduzir o tamanho da VM ou o número de VMs, tente novamente mais tarde ou experimente implementar num conjunto de disponibilidade diferente ou a localização do Azure diferente.  |  
|  AllocationFailed  |  Falha na alocação da VM devido a um erro interno. Volte a tentar mais tarde ou experimente implementar numa localização diferente.  |
|  ArtifactNotFound  |  A extensão da VM com o publicador '{0}'e o tipo'{1}'não foi possível encontrar na localização'{2}'.  |
|  ArtifactNotFound  |  Extensão com o publicador '{0}', tipo de '{1}' e a versão de processador do tipo '{2}' não foi possível encontrar no repositório de extensões.  |
|  ArtifactVersionNotFound  |  Foi encontrada nenhuma versão no repositório de artefactos que coincida com a versão pedida '{0}'.  |
|  ArtifactVersionNotFound  |  Foi encontrada nenhuma versão no repositório de artefactos que coincida com a versão pedida '{0}'para a extensão VM com o publicador'{1}'e o tipo'{2}'.  |
|  AttachDiskWhileBeingDetached  |  Não é possível anexar o disco de dados '{0}'para a VM'{1}' porque o disco está a ser desanexado. Aguarde até que o disco é totalmente desanexado e, em seguida, tente novamente.  |
|  BadRequest  |  Alinhada ' conjuntos de disponibilidade ainda não são suportados nesta região.  |
|  BadRequest  |  Não é suportada a adição de uma VM com discos de gerido para não gerido pelo conjunto de disponibilidade ou a adição de uma VM com discos de baseadas em BLOBs para um conjunto de disponibilidade de gerido. Crie um conjunto de disponibilidade com a propriedade 'geridos' definida para poder adicionar uma VM com discos geridos ao mesmo.  |
|  BadRequest  |  Os Discos Geridos não são suportados nesta região.  |
|  BadRequest  |  Múltiplas VMExtensions por processador que não é suportado para o tipo de SO '{0}'. VMExtension '{1}'com o handler'{2}' já foi adicionada ou especificada na entrada.  |
|  BadRequest  |  Operação '{0}'não é suportada no recurso'{1}' com discos geridos.  |
|  CertificateImproperlyFormatted  |  Representação JSON do segredo obtida {0} tem um campo de dados que não é um ficheiro PFX corretamente formatado ou a palavra-passe fornecida não descodifica corretamente o ficheiro PFX.  |
|  CertificateImproperlyFormatted  |  Os dados obtidos na {0} não é são desserializáveis no JSON.  |
|  Conflito  |  O redimensionamento do disco é permitido apenas quando se cria uma VM ou quando a VM é desalocada.  |
|  ConflictingUserInput  |  Disco '{0}'não pode ser anexada como o disco já é propriedade de VM'{1}'.  |
|  ConflictingUserInput  |  Os grupos de recursos de origem e de destino são os mesmos.  |
|  ConflictingUserInput  |  As contas de armazenamento de origem e de destino do disco {0} são diferentes.  |
|  ContainerAlreadyOnLease  |  Já houver uma concessão no contentor de armazenamento que contém o blob com o URI {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  O pedido de movimentação de recursos contém recursos KeyVault que são referenciados por uma ou mais {0}s no pedido. Isto não é suportado atualmente na subscrição mover cruzada. Verifique os detalhes do erro para o recurso de KeyVault Ids.  |
|  DiagnosticsOperationInternalError  |  Ocorreu um erro interno ao processar o perfil de diagnóstico da VM {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  Blob {0} já está a ser utilizado por outro disco pertencente VM '{1}'. Pode examinar os metadados do blob para as informações de referência do disco.  |
|  DiskBlobNotFound  |  Não é possível localizar o blob VHD com o URI {0} para o disco '{1}'.  |
|  DiskBlobNotFound  |  Não é possível localizar o blob VHD com o URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} segredo não tem o {1} etiquetas. Atualize a versão secreta, adiciona as etiquetas necessárias e tente novamente.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Desenrolar do segredo {0} valor utilizando a chave {1} falhou.  |
|  DiskImageNotReady  |  Imagem de disco {0} está a ser {1} estado. Tente novamente quando a imagem está pronta.  |
|  DiskPreparationError  |  Ocorreram um ou mais erros durante a preparação de discos VM. Consulte a vista de instância de disco para obter mais detalhes.  |
|  DiskProcessingError  |  O processamento do disco foi interrompido uma vez que a VM tem outros discos nos discos com falhas.  |
|  ImageBlobNotFound  |  Não é possível localizar o blob VHD com o URI {0} para o disco '{1}'.  |
|  ImageBlobNotFound  |  Não é possível localizar o blob VHD com o URI {0}.  |
|  IncorrectDiskBlobType  |  Os blobs de disco só podem ser de BLOBs de páginas de tipo. Blob {0} para o disco '{1}' é do tipo blob de blocos.  |
|  IncorrectDiskBlobType  |  Os blobs de disco só podem ser de BLOBs de páginas de tipo. Blob {0} é do tipo '{1}'.  |
|  IncorrectImageBlobType  |  Os blobs de disco só podem ser de BLOBs de páginas de tipo. Blob {0} para o disco '{1}' é do tipo blob de blocos.  |
|  IncorrectImageBlobType  |  Os blobs de disco só podem ser de BLOBs de páginas de tipo. Blob {0} é do tipo '{1}'.  |
|  InternalOperationError  |  Não foi possível resolver a conta de armazenamento {0}. Certifique-se de que foi criada através do fornecedor de recursos de armazenamento na mesma localização que o recurso de computação.  |
|  InternalOperationError  |  {0} Falha de tarefas de atingir o objetivo.  |
|  InternalOperationError  |  Ocorreu um erro na validação do perfil de rede da VM '{0}'.  |
|  InvalidAccountType  |  O AccountType {0} é inválido.  |
|  InvalidParameter  |  O valor do parâmetro {0} é inválido.  |
|  InvalidParameter  |  A palavra-passe de Admin especificada não é permitida.  |
|  InvalidParameter  |  "A palavra-passe fornecida tem de estar entre {0}-{1} carateres de comprimento e deve satisfazer, pelo menos, {2} dos requisitos de complexidade de palavra-passe seguintes: <ol><li> Contém um caráter em maiúsculas</li><li>Contém um caráter em minúsculas</li><li>Contém um dígito numérico</li><li>Contém um caráter especial.</li></ol>  |
|  InvalidParameter  |  O Nome de Utilizador de Admin especificado não é permitido.  |
|  InvalidParameter  |  Não é possível anexar um disco de SO existente quando a VM é criada a partir de uma imagem de plataforma ou de utilizador.  |
|  InvalidParameter  |  Nome do contentor {0} é inválido. Os nomes de contentor tem de ter entre 3 e 63 carateres de comprimento e contenham apenas carateres alfanuméricos minúsculos e hífenes. Hífen tem de ser antecedido e seguido por um caráter alfanumérico.  |
|  InvalidParameter  |  Nome do contentor {0} no URL {1} é inválido. Os nomes de contentor tem de ter entre 3 e 63 carateres de comprimento e contenham apenas carateres alfanuméricos minúsculos e hífenes. Hífen tem de ser antecedido e seguido por um caráter alfanumérico.  |
|  InvalidParameter  |  O nome do blob no URL {0} contém uma barra. Isto atualmente não é suportado para discos.  |
|  InvalidParameter  |  O URI {0} não parece para ser o URI de blob correto.  |
|  InvalidParameter  |  Um disco chamado '{0}' já utiliza o mesmo LUN: {1}.  |
|  InvalidParameter  |  Um disco chamado '{0}' já existe.  |
|  InvalidParameter  |  Não é possível especificar substituições da imagem de utilizador para um disco que já está definido na referência de imagem especificada.  |
|  InvalidParameter  |  Um disco chamado '{0}' já utiliza o mesmo URL de VHD {1}.  |
|  InvalidParameter  |  O número de domínios de falhas especificado {0} tem de estar contido no intervalo {1} para {2}.  |
|  InvalidParameter  |  O tipo de licença {0} é inválido. Tipos de licenças válidos são: Windows_Client ou Windows_Server, sensíveis a maiúsculas e minúsculas.  |
|  InvalidParameter  |  Nome do anfitrião Linux não pode exceder {0} carateres de comprimento ou conter os seguintes carateres: {1}.  |
|  InvalidParameter  |  Caminho de destino para chaves públicas Ssh está limitado ao valor predefinido {0} devido a um problema conhecido no agente de aprovisionamento do Linux.  |
|  InvalidParameter  |  Um disco no LUN {0} já existe.  |
|  InvalidParameter  |  Subscrição {0} do pedido tem de corresponder à subscrição {1} contida no id de disco gerido.  |
|  InvalidParameter  |  Dados personalizados de osprofile têm de ser codificação Base64 e com um comprimento máximo de {0} carateres.  |
|  InvalidParameter  |  Nome do blob no URL {0} tem de terminar com '{1}' extensão.  |
|  InvalidParameter  |  {0}' não é um válido capturado VHD blob prefixo de nome. Um prefixo válido coincide com regex '{1}'.  |
|  InvalidParameter  |  Não não possível adicionar certificados à VM se o agente da VM não está aprovisionado.  |
|  InvalidParameter  |  Um disco no LUN {0} já existe.  |
|  InvalidParameter  |  Não é possível criar a VM, porque o tamanho pedido {0} não está disponível no cluster onde o conjunto de disponibilidade está atribuído. Os tamanhos disponíveis são: {1}. Leia mais VM on redimensionamento estratégia em https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  O tamanho da VM pedido {0} não está disponível na região atual. Os tamanhos disponíveis na região atual são: {1}. Saber mais sobre os tamanhos VM disponíveis em cada região na https://aka.ms/azure-regions.  |
|  InvalidParameter  |  O tamanho da VM pedido {0} não está disponível na região atual. Saber mais sobre os tamanhos VM disponíveis em cada região na https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Nome de utilizador de admin do Windows não pode ser mais do que {0} longos de carateres, terminar com um ponto ou conter os seguintes carateres: {1}.  |
|  InvalidParameter  |  Nome de computador do Windows não pode ser mais do que {0} carateres comprimento, ser inteiramente numérico ou conter os seguintes carateres: {1}.  |
|  MissingMoveDependentResources  |  O pedido de movimentação de recursos não contém todos os recursos dependentes. Verifique os detalhes do erro de falta de ids de recurso.  |
|  MoveResourcesHaveInvalidState  |  O pedido mover recursos contém as VMs que estão associadas a contas de armazenamento inválido. Verifique os detalhes para estes ids de recurso e os nomes das contas de armazenamento referenciado.  |
|  MoveResourcesHavePendingOperations  |  O pedido de movimentação de recursos contém recursos para o qual está pendente uma operação. Verifique os detalhes para estes ids de recurso. Repita a operação depois de concluir as operações pendentes.  |
|  MoveResourcesNotFound  |  O pedido de movimentação de recursos contém recursos que não não possível encontrar. Verifique os detalhes para estes ids de recurso.  |
|  NetworkingInternalOperationError  |  Erro de alocação de rede desconhecido.  |
|  NetworkingInternalOperationError  |  Erro de alocação de rede desconhecido  |
|  NetworkingInternalOperationError  |  Ocorreu um erro interno no processamento do perfil de rede da VM.  |
|  NãoLocalizado  |  O conjunto de disponibilidade {0} não é possível encontrar.  |
|  NãoLocalizado  |  Máquina Virtual de origem '{0}' especificado no pedido não existe nesta localização do Azure.  |
|  NãoLocalizado  |  Inquilino com o id {0} não encontrado.  |
|  NãoLocalizado  |  A imagem {0} não é possível encontrar.  |
|  NotSupported  |  O tipo de licença é {0}, mas o blob de imagem {1} não está no local.  |
|  OperationNotAllowed  |  Conjunto de disponibilidade {0} não pode ser eliminada. Antes de eliminar um conjunto de disponibilidade, certifique-se que não contém nenhuma VM.  |
|  OperationNotAllowed  |  Conjunto de disponibilidade SKU de 'Aligned' para 'Clássico' não é permitido alterar.  |
|  OperationNotAllowed  |  Não é possível modificar as extensões numa VM que não está a ser executada.  |
|  OperationNotAllowed  |  A ação de captura só é suportada numa máquina Virtual com discos baseadas em BLOBs. Utilize o recurso 'Imagem' APIs para criar uma imagem de uma Máquina Virtual gerida.  |
|  OperationNotAllowed  |  O recurso {0} não é possível criar a partir da imagem {1} até que a imagem foi criada com êxito.  |
|  OperationNotAllowed  |  As atualizações de encryptionSettings não é permitido quando a VM está alocada, repita a operação depois de VM é desalocada  |
|  OperationNotAllowed  |  A adição de um disco gerido a uma VM com disco baseado em blob não é suportada.  |
|  OperationNotAllowed  |  O número máximo de discos de dados permitido para anexação a uma VM deste tamanho é {0}.  |
|  OperationNotAllowed  |  A adição de um disco baseado em blob a uma VM com discos geridos não é suportada.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na imagem'{1}' uma vez que a imagem está marcada para eliminação. Só poderá repita a operação de eliminação (ou aguarde um em curso concluir).  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que a VM está generalizada.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitido como coleção de ponto de restauro'{1}' está marcado para eliminação.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na extensão de VM'{1}' porque está marcado para eliminação. Só poderá repita a operação de eliminação (ou aguarde um em curso concluir).  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida dado que as máquinas virtuais'{1}'estão a ser aprovisionados com a imagem de'{2}'.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida dado que a Máquina Virtual ScaleSet'{1}'está atualmente a utilizar a imagem'{2}'.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que a VM está marcada para eliminação. Só poderá repita a operação de eliminação (ou aguarde um em curso concluir).  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que a VM está desatribuída ou marcada como desatribuída.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que a VM está em execução. Volte a energia desativar explicitamente caso encerre a VM a partir do sistema operativo convidado.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que a VM não é desalocada.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}'Uma vez que a VM tem a extensão'{2}' no estado de falha.  |
|  OperationNotAllowed  |  Operação '{0}'não é permitida na VM'{1}' uma vez que está em curso outra operação.  |
|  OperationNotAllowed  |  A operação '{0}'requer a Máquina Virtual'{1}' para ser generalizado.  |
|  OperationNotAllowed  |  A operação requer que a VM esteja em execução (ou definida para ser executada).  |
|  OperationNotAllowed  |  Disco com tamanho {0}GB, o que é inferior ao tamanho {1}GB de disco correspondente na imagem, não é permitida.  |
|  OperationNotAllowed  |  Extensões de conjunto de dimensionamento da VM do processador '{0}' podem ser adicionadas apenas aquando da criação de conjunto de dimensionamento de VM.  |
|  OperationNotAllowed  |  Extensões de conjunto de dimensionamento da VM do processador '{0}' pode ser eliminado apenas no momento da eliminação do conjunto de dimensionamento de VM.  |
|  OperationNotAllowed  |  VM '{0}' já está a utilizar discos geridos.  |
|  OperationNotAllowed  |  VM '{0}'pertence ao conjunto de disponibilidade 'Clássico' '{1}'. Atualize a conjunto de disponibilidade para utilizar o 'Aligned' SKU e, em seguida, repita a conversão.  |
|  OperationNotAllowed  |  VM criada a partir da imagem não pode ter discos baseadas em BLOBs. Todos os discos têm de ser discos geridos.  |
|  OperationNotAllowed  |  A operação de recolher não pode ser concluída porque a VM não está generalizada.  |
|  OperationNotAllowed  |  Operações de gestão na VM '{0}' não são permitidos porque os discos VM estão a ser convertidos em discos geridos.  |
|  OperationNotAllowed  |  Uma operação em curso está a alterar o estado de energia da Máquina Virtual {0} para {1}. Efetuar a operação {2} após algum tempo.  |
|  OperationNotAllowed  |  Não é possível adicionar ou atualizar a VM. O tamanho da VM pedido {0} poderá não estar disponível na unidade de alocação existente. Leia mais VM on redimensionamento estratégia em https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Não é possível redimensionar a VM, porque o tamanho pedido {0} não está disponível no cluster onde o conjunto de disponibilidade está atribuído. Os tamanhos disponíveis são: {1}. Leia mais VM on redimensionamento estratégia em https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Não é possível redimensionar a VM, porque o tamanho pedido {0} não está disponível no cluster onde a VM está atribuída. Redimensionar a VM para {1} desalocar. (esta é uma operação de interrupção no portal do Azure) e repita a operação de redimensionamento. Leia mais VM on redimensionamento estratégia em https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Falha no aprovisionamento de SO para a VM '{0}' porque o SO convidado está atualmente a ser aprovisionado.  |
|  OSProvisioningClientError  |  Aprovisionamento de SO para a VM '{0}' falhou. Detalhes do erro: {1} Certifique-se a imagem foi preparada corretamente (generalizada). <ul><li>Instruções para Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Falha na geração de chaves de anfitrião SSH. Detalhes do erro: {0}. Para resolver este problema Verifique se o agente de Linux está corretamente configurada. <ul><li>Pode verificar as instruções apresentadas em: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Nome de utilizador especificado para a VM é inválido para esta distribuição de Linux. Detalhes do erro: {0}.  |
|  OSProvisioningInternalError  |  Falha no aprovisionamento de SO para a VM '{0}' devido a um erro interno.  |
|  OSProvisioningTimedOut  |  Aprovisionamento de SO para a VM '{0}' não foi concluída no tempo atribuído. A VM ainda poderá concluir o aprovisionamento com êxito. Verifique o estado de aprovisionamento mais tarde.  |
|  OSProvisioningTimedOut  |  Aprovisionamento de SO para a VM '{0}' não foi concluída no tempo atribuído. A VM ainda poderá concluir o aprovisionamento com êxito. Verifique o estado de aprovisionamento mais tarde. Além disso, certifique-se a imagem foi preparada corretamente (generalizada).   <ul><li>Instruções para Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instruções para Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Aprovisionamento de SO para a VM '{0}' não foi concluída no tempo atribuído. No entanto, foi detetado o agente convidado da VM em execução. Isto sugere que o SO convidado não foi corretamente preparado para ser utilizado como uma imagem de VM (com CreateOption = FromImage). Para resolver este problema, utilize o VHD é com CreateOption = Attach ou prepará-la corretamente para utilização como uma imagem:   <ul><li>Instruções para Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instruções para Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  O tamanho de VM necessário não está atualmente disponível na localização selecionada.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Recurso não é possível atualizar neste momento devido a atualização de plataforma em curso. Tente novamente mais tarde.  |
|  StorageAccountLimitation  |  Conta de armazenamento '{0}' não suporta os blobs de páginas que são necessárias para criar discos.  |
|  StorageAccountLimitation  |  Conta de armazenamento '{0}' excedeu a quota alocada.  |
|  StorageAccountLocationMismatch  |  Não foi possível resolver a conta de armazenamento {0}. Certifique-se de que foi criada através do fornecedor de recursos de armazenamento na mesma localização que o recurso de computação.  |
|  StorageAccountNotFound  |  Conta de armazenamento {0} não encontrado. Certifique-se a conta de armazenamento não é eliminada e pertence à mesma localização da VM do Azure.  |
|  StorageAccountNotRecognized  |  Utilize uma conta de armazenamento gerida pelo fornecedor de recursos de armazenamento. A utilização de {0} não é suportada.  |
|  StorageAccountOperationInternalError  |  Ocorreu um erro interno ao aceder à conta de armazenamento {0}.  |
|  StorageAccountSubscriptionMismatch  |  Conta de armazenamento {0} não pertence à subscrição {1}.  |
|  StorageAccountTooBusy  |  Conta de armazenamento '{0}' está atualmente demasiado ocupado. Considere utilizar outra conta.  |
|  StorageAccountTypeNotSupported  |  Disco {0} utiliza {1} que é uma conta do Blob storage. Tente novamente com a conta de armazenamento de caráter geral.  |
|  StorageAccountTypeNotSupported  |  Conta de armazenamento {0} é de {1} tipo. Suporta de diagnóstico de arranque {2} tipos de conta de armazenamento.  <ul><li>Este erro ocorre se utilizar a conta de armazenamento premium para diagnóstico de arranque. Para obter mais informações, consulte [como utilizar o diagnóstico de arranque](../articles/virtual-machines/windows/boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  A subscrição não está autorizada.  |
|  TargetDiskBlobAlreadyExists  |  Blob {0} já existe. Forneça um URI para criar um novo disco de dados em branco de blob diferente '{1}'.  |
|  TargetDiskBlobAlreadyExists  |  Capturar não é possível continuar a operação dado que o blob de imagem de destino {0} já existe e não é possível definir o sinalizador para substituir blobs de VHD. Eliminar o blob ou defina o sinalizador para substituir blobs de VHD e tente novamente.  |
|  TargetDiskBlobAlreadyExists  |  Capturar não é possível continuar a operação dado que o blob de imagem de destino {0} tem uma concessão ativa.   |
|  TargetDiskBlobAlreadyExists  |  Blob {0} já existe. Forneça um URI de blob diferente como destino para o disco '{1}'.  |
|  TooManyVMRedeploymentRequests  |  Foram recebidos demasiados pedidos de reimplementação da VM '{0}' ou o mesmo conjunto de disponibilidades de que as VMs com esta VM. Tente novamente mais tarde.  |
|  VHDSizeInvalid  |  O valor de tamanho de disco especificado de {0} para o disco '{1}' com o blob {2} é inválido. Tamanho do disco tem de estar entre {3} e {4}.  |
|  VMAgentStatusCommunicationError  |  VM '{0}' não comunicou o estado de agente da VM ou extensões. Verifique se a VM tem um agente VM em execução e pode estabelecer ligações de saída para o armazenamento do Azure.  |
|  VMArtifactRepositoryInternalError  |  Ocorreu um erro durante a comunicação com o repositório de artefactos para obtenção dos detalhes de artefacto da VM.  |
|  VMArtifactRepositoryInternalError  |  Ocorreu um erro interno ao obter os dados de artefacto da VM a partir do repositório de artefactos.  |
|  VMExtensionHandlerNonTransientError  |  O processador '{0}'relatou uma falha de extensão da VM'{1}'com o código de erro terminal'{2}' e a mensagem de erro: '{3}'  |
|  VMExtensionManagementInternalError  |  Ocorreu um erro interno ao processar a extensão da VM '{0}'.  |
|  VMExtensionManagementInternalError  |  Ocorreram vários erros durante a preparação as extensões VM. Consulte a vista de instâncias de extensão VM para obter mais detalhes.  |
|  VMExtensionProvisioningError  |  VM reportou uma falha durante o processamento de extensão '{0}'. Mensagem de erro: "{1}".  |
|  VMExtensionProvisioningError  |  Não foi possível ser aprovisionados na VM várias extensões de VM. Consulte a vista de instâncias de extensão VM para obter mais detalhes.  |
|  VMExtensionProvisioningTimeout  |  O aprovisionamento da extensão de VM '{0}' atingiu o limite. Instalação da extensão poderá estar a demorar demasiado tempo ou não foi possível obter o estado da extensão.  |
|  VMMarketplaceInvalidInput  |  Criar uma máquina virtual a partir de uma imagem de tipo não mercado não necessita de informações do plano, remova as informações do plano no pedido. Nome do disco de SO é {0}.  |
|  VMMarketplaceInvalidInput  |  As informações de compra não coincidem. Não é possível implementar a imagem do Marketplace. Nome do disco de SO é {0}.  |
|  VMMarketplaceInvalidInput  |  Criar uma máquina virtual a partir da imagem de mercado necessita de informações do plano no pedido. Nome do disco de SO é {0}.  |
|  VMNotFound  |  A VM '{0}' não é possível encontrar.  |
|  VMRedeploymentFailed  |  VM '{0}' reimplementação falhou devido a um erro interno. Tente novamente mais tarde.  |
|  VMRedeploymentTimedOut  |  A reimplementação da VM '{0}' não foi concluída no tempo atribuído. Poderá ser concluída com êxito no algum tempo. Caso contrário, pode repetir o pedido.  |
|  VMStartTimedOut  |  VM '{0}' não foram iniciados no tempo atribuído. A VM poderá ainda ser iniciada com êxito. Verifique o estado de energia mais tarde.  |


## <a name="next-steps"></a>Passos Seguintes
Se precisar de mais ajuda, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.