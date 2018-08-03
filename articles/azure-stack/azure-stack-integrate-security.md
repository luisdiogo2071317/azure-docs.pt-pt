---
title: Integração de datacenter do Azure Stack - segurança
description: Saiba como integrar a segurança do Azure Stack com a segurança do Centro de dados
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: 9f356b814ac1ac6ca8b6d6efe7cb9f5d9ed66270
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442480"
---
# <a name="azure-stack-datacenter-integration---security"></a>Integração de datacenter do Azure Stack - segurança
O Azure Stack foi concebido e criado tendo em mente a segurança. O Azure Stack é um sistema de bloqueado, pelo que não é suportada a instalação do agente de segurança de software.

Este artigo ajuda-o a integrar as funcionalidades de segurança do Azure Stack com as soluções de segurança já implementadas no seu datacenter.

## <a name="security-logs"></a>Registos de segurança

O Azure Stack recolhe sistema operativo e os eventos de segurança de funções de infraestrutura e os nós de unidade de escala cada dois minutos. Os registos são armazenados em contentores de BLOBs da conta de armazenamento.

Existe uma conta de armazenamento por função de infraestrutura e uma conta de armazenamento geral para todos os eventos de sistema de operativo típico.

O fornecedor de recursos do Estado de funcionamento pode ser chamado através do protocolo REST para obter o URL para o contentor de Blobs. Soluções de segurança de terceiros podem utilizar as contas de armazenamento e de API para obter eventos para processamento.

### <a name="use-azure-storage-explorer-to-view-events"></a>Utilize o Explorador de armazenamento do Azure para ver eventos

Pode recuperar os eventos recolhidos pelo Azure Stack, usando uma ferramenta chamada Explorador de armazenamento do Azure. Pode transferir o Explorador de armazenamento do Azure partir [ http://storageexplorer.com ](http://storageexplorer.com).

O procedimento seguinte é um exemplo que pode utilizar para configurar o Explorador de armazenamento do Azure para o Azure Stack:

1. Inicie sessão no portal de administrador do Azure Stack como um operador.
1. Procure **contas de armazenamento** e procure **frphealthaccount**. O **frphealthaccount** conta é a conta de armazenamento geral utilizada para armazenar todos os eventos de sistema operativo.

   ![Contas de armazenamento](media/azure-stack-integrate-security/storage-accounts.png)

1. Selecione **frphealthaccount**, em seguida, clique em **chaves de acesso**.

   ![Chaves de acesso](media/azure-stack-integrate-security/access-keys.png)

1. Copie a chave de acesso para a área de transferência.
1. Abra o Explorador de armazenamento do Azure.
1. Sobre o **edite** menu, selecione **Azure Stack de destino**.
1. Selecione **adicionar conta**e, em seguida, selecione **utilize um nome de conta de armazenamento e a chave**.

   ![Ligar o armazenamento](media/azure-stack-integrate-security/connect-storage.png)

1. Clique em **Seguinte**.
1. Sobre o **anexar armazenamento externo** página:

   a. Escreva o nome da conta **frphealthaccount**.

   b. Cole a chave de acesso da conta de armazenamento.

   c. Sob **domínio de pontos finais de armazenamento**, selecione **outros**e especifique o ponto final de armazenamento **[Região]. [ DomainName]**.

   d. Selecione o **utilizar HTTP** caixa de verificação.

   ![Anexar armazenamento externo](media/azure-stack-integrate-security/attach-storage.png)

1. Clique em **próxima**, reveja o resumo, e **concluir** o assistente.
1. Agora pode procurar os contentores de BLOBs individuais e transferir os eventos.

   ![Procurar blobs](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Utilize linguagens de programação para eventos de acesso

Pode utilizar várias linguagens de programação para aceder a uma conta de armazenamento. Utilize a seguinte documentação para escolher um exemplo que corresponde ao seu idioma:

[https://azure.microsoft.com/resources/samples/?term=storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Auditoria de acesso do dispositivo

Todos os dispositivos físicos no Azure Stack suportam a utilização de TACACS ou RADIUS. Isto inclui o acesso para o controlador de gestão da placa base (BMC) e comutadores de rede.

Soluções de pilha do Azure não são enviados com RADIUS ou TACACS incorporado. No entanto, as soluções foram validadas para suportar a utilização de RADIUS ou TACACS soluções existentes disponíveis no mercado.

Para RADIUS, MSCHAPv2 foi validada. Isso representa a implementação mais segura com o RADIUS.
Consulte o fornecedor de hardware de OEM para ativar o TACAS ou RADIUS nos dispositivos incluídos com a sua solução do Azure Stack.

## <a name="syslog"></a>Syslog

Todos os dispositivos físicos no Azure Stack podem enviar mensagens do Syslog. Soluções de pilha do Azure não são enviados com um servidor Syslog. No entanto, as soluções foram validadas para suportar o envio de mensagens para soluções de Syslog existentes disponíveis no mercado.

O endereço de destino do Syslog é um parâmetro opcional recolhido para a implementação, mas ele também pode ser adicionado após a implementação.

## <a name="next-steps"></a>Passos Seguintes

[Política de manutenção](azure-stack-servicing-policy.md)
