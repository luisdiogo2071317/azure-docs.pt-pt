---
title: Utilizar a CDN do Azure com SAS | Microsoft Docs
description: CDN do Azure suporta a utilização de acesso assinatura partilhado (SAS) para conceder acesso limitado a contentores de armazenamento privada.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: v-deasim
ms.openlocfilehash: 15a4e0a8d62b38fa7aa542d95e53d29621965666
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316573"
---
# <a name="using-azure-cdn-with-sas"></a>Utilizar a CDN do Azure com SAS

Quando configurar uma conta do storage de Azure entrega rede conteúdos (CDN) para utilizar para o conteúdo da cache, por predefinição que qualquer pessoa que conheça os URLs para os contentores de armazenamento pode aceder os ficheiros que carregar. Para proteger os ficheiros na sua conta de armazenamento, pode definir o acesso dos contentores de armazenamento do público para private. No entanto, se o fizer, ninguém irá conseguir aceder aos seus ficheiros. 

Se pretende conceder acesso limitado a contentores de armazenamento privada, pode utilizar a funcionalidade de assinatura de acesso partilhado (SAS) da sua conta de armazenamento do Azure. Uma SAS é um URI que concede restrito direitos de acesso aos seus recursos de armazenamento do Azure sem a chave de conta a exposição. Pode fornecer uma SAS para clientes que não seja fidedigna com a sua chave de conta de armazenamento, mas para quem pretende delegar o acesso a alguns recursos da conta de armazenamento. Por distribuição de uma assinatura de acesso partilhado URI para estes clientes, pode conceder acesso a um recurso durante um período de tempo especificado.
 
Com uma SAS, pode definir vários parâmetros de acesso para um blob, como os tempos de início e de expiração, permissões (leitura/escrita) e os intervalos de IP. Este artigo descreve como utilizar SAS em conjunto com a CDN do Azure. Para obter mais informações sobre SAS, incluindo como criar e as opções de parâmetros, consulte [Using partilhado assinaturas de acesso (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Ao configurar a CDN do Azure para trabalhar com o armazenamento SAS
As três opções seguintes são recomendadas para através da SAS com CDN do Azure. Todas as opções de partem do princípio de que já criou um trabalho SAS (consulte pré-requisitos). 
 
### <a name="prerequisites"></a>Pré-requisitos
Para começar, criar uma conta de armazenamento e, em seguida, gerar uma SAS para o seu elemento. Pode gerar dois tipos de assinaturas de acesso armazenada: um serviço SAS ou uma conta SAS. Para obter mais informações, consulte [tipos de assinaturas de acesso partilhado](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Depois de já gerou um token SAS, pode aceder o ficheiros de armazenamento de BLOBs, acrescentando `?sv=<SAS token>` para o seu URL. Este URL tem o seguinte formato: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Por exemplo:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-07-29&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Para obter mais informações sobre os parâmetros de definição, consulte [considerações de parâmetro SAS](#sas-parameter-considerations) e [parâmetros de assinatura de acesso partilhado](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Definições de SAS do CDN](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Opção 1: Através da SAS com pass-through para o blob storage do Azure CDN

Esta opção é a mais simples e utiliza um único token SAS, o que é transmitido da CDN do Azure para o servidor de origem. É suportado pelo **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai** perfis. 
 
1. Selecione um ponto final, selecione **regras a colocação em cache**, em seguida, selecione **colocar em Cache todos os URLs únicos** do **colocação em cache de cadeia de consulta** lista.

    ![Regras de colocação em cache de CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depois de configurar SAS na sua conta de armazenamento, tem de utilizar o token SAS com os URLs de servidor de ponto final e a origem da CDN para aceder ao ficheiro. 
   
   O URL de ponto final CDN resultante tem o seguinte formato: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Por exemplo:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Otimizar a duração da cache utilizando regras de colocação em cache ou adicionando `Cache-Control` cabeçalhos no servidor de origem. Porque a CDN do Azure trata o token SAS como uma cadeia de consulta simples, como melhor prática, deve configurar uma duração de colocação em cache expira no ou antes da data de expiração de SAS. Caso contrário, se um ficheiro é colocado em cache durante um período de tempo que as SAS está ativa, o ficheiro poderá estar acessível a partir do servidor de origem da CDN do Azure, depois de decorrido o tempo de expiração de SAS. Se esta situação ocorre e pretender que os ficheiros na cache inacessível, tem de efetuar uma operação de remoção do ficheiro para desmarcá-la a partir da cache. Para obter informações sobre como definir a duração da cache na CDN do Azure, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>Opção 2: Oculta token CDN SAS utilizando uma regra de reescrever
 
Esta opção só está disponível para **CDN do Azure Premium da Verizon** perfis. Com esta opção, pode proteger o armazenamento de BLOBs no servidor de origem. Poderá utilizar esta opção se não precisa de restrições de acesso específicas para o ficheiro, mas pretende impedir que os utilizadores acedam a origem de armazenamento diretamente para melhorar os tempos de descarga da CDN do Azure. O token SAS, que é desconhecido para o utilizador, é necessário para que qualquer pessoa que acedem aos ficheiros no contentor especificado do servidor de origem. No entanto, devido a regra, o URL Rewrite o token SAS não é necessário no ponto final de CDN.
 
1. Utilize o [motor de regras](cdn-rules-engine.md) para criar uma regra de URL Rewrite. Novas regras demorar cerca de 10 minutos para propagar.

   ![Botão de gerir a CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Botão de motor de regras CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   A seguinte regra de URL Rewrite de exemplo utiliza um padrão de expressão regular com um grupo de captura e um ponto final com o nome *storagedemo*:
   
   Origem:   
   `(\/container1\/.*)`
   
   Destino:   
   ```
   $1?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Rewrite regra - esquerda](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![CDN URL Rewrite regra - direita](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. Depois da nova regra fica ativa, qualquer pessoa pode aceder a ficheiros no contentor especificado no ponto final de CDN, independentemente se estiver a utilizar um token SAS no URL. Eis o formato: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Por exemplo:   
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       

3. Otimizar a duração da cache utilizando regras de colocação em cache ou adicionando `Cache-Control` cabeçalhos no servidor de origem. Porque a CDN do Azure trata o token SAS como uma cadeia de consulta simples, como melhor prática, deve configurar uma duração de colocação em cache expira no ou antes da data de expiração de SAS. Caso contrário, se um ficheiro é colocado em cache durante um período de tempo que as SAS está ativa, o ficheiro poderá estar acessível a partir do servidor de origem da CDN do Azure, depois de decorrido o tempo de expiração de SAS. Se esta situação ocorre e pretender que os ficheiros na cache inacessível, tem de efetuar uma operação de remoção do ficheiro para desmarcá-la a partir da cache. Para obter informações sobre como definir a duração da cache na CDN do Azure, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Opção 3: Utilizar a autenticação de token de segurança CDN com uma regra de reescrever

Para utilizar a autenticação de token de segurança de CDN do Azure, tem de ter um **CDN do Azure Premium da Verizon** perfil. Esta opção é a mais segura e personalizáveis. Acesso de cliente é baseado nos parâmetros de segurança que definiu no token de segurança. Assim que tiver criado e configurar o token de segurança, será necessário em todos os URLs de ponto final CDN. No entanto, devido a regra, o URL Rewrite o token SAS não é necessário no ponto final de CDN. Se o token SAS mais tarde se torne inválido, o Azure CDN já não poderá revalidate o conteúdo do servidor de origem.

1. [Criar um token de segurança de CDN do Azure](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) e ative-a utilizando o motor de regras para o ponto final de CDN e o caminho onde os utilizadores podem aceder ao ficheiro.

   Um URL de ponto final de tokens de segurança tem o seguinte formato:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Por exemplo:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   As opções de parâmetros para uma autenticação de token de segurança são diferentes as opções de parâmetros para um token SAS. Se optar por utilizar um período de tempo de expiração quando cria um token de segurança, deve defini-lo para o mesmo valor que a hora de expiração para o token SAS. Se o fizer, garante que a hora de expiração é previsível. 
 
2. Utilize o [motor de regras](cdn-rules-engine.md) para criar uma regra de URL Rewrite para ativar o acesso de token SAS para todos os blobs no contentor. Novas regras demorar cerca de 10 minutos para propagar.

   A seguinte regra de URL Rewrite de exemplo utiliza um padrão de expressão regular com um grupo de captura e um ponto final com o nome *storagedemo*:
   
   Origem:   
   `(\/container1\/.*)`
   
   Destino:   
   ```
   $1&sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Rewrite regra - esquerda](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![CDN URL Rewrite regra - direita](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. Se renovar a SAS, certifique-se de que atualiza a regra de Url Rewrite com o novo token SAS. 

## <a name="sas-parameter-considerations"></a>Considerações de parâmetro SAS

Porque os parâmetros SAS não estão visíveis para a CDN do Azure, a CDN do Azure não é possível alterar o comportamento de entrega com base nos mesmos. As restrições de parâmetro definido aplicam-se apenas nos pedidos que faz com que o CDN do Azure para o servidor de origem, não para os pedidos do cliente para a CDN do Azure. Este distinção é importante a considerar quando definir os parâmetros SAS. Se estas capacidades avançadas são necessárias e estiver a utilizar [opção 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), defina as restrições adequadas no token de segurança de CDN do Azure.

| Nome do parâmetro SAS | Descrição |
| --- | --- |
| Iniciar | O tempo que pode começar a CDN do Azure para aceder ao ficheiro de blob. Devido a relógio desfasamento (quando é recebido um sinal de relógio em alturas diferentes para diferentes componentes), escolha uma hora de 15 minutos anteriormente se pretender que o elemento estar imediatamente disponível. |
| Terminar | O tempo após o qual a CDN do Azure já não pode aceder ao ficheiro de blob. Anteriormente, os ficheiros em cache na CDN do Azure são continuarão acessíveis. Para controlar a hora de expiração de ficheiros, defina a hora de expiração adequado no token de segurança de CDN do Azure ou remover o elemento. |
| Endereços IP permitidos | Opcional. Se estiver a utilizar **CDN do Azure da Verizon**, pode definir este parâmetro para os intervalos definidos no [CDN do Azure da Verizon Edge intervalos de IP servidor](https://msdn.microsoft.com/library/mt757330.aspx). Se estiver a utilizar **CDN do Azure da Akamai**, não é possível definir o parâmetro de intervalos IP, porque os endereços IP não são estáticos.|
| Protocolos permitidos | Os protocolos permitidos para um pedido efetuado com a conta SAS. Recomenda-se a definição de HTTPS.|

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre SAS, consulte os artigos seguintes:
- [Utilizar assinaturas de acesso partilhado (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Partilhado assinaturas de acesso, parte 2: Criar e utilizar um SAS com o Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)

Para obter mais informações sobre como configurar a autenticação com token, consulte [ativos de proteger a rede do Azure conteúdo entrega com autenticação de token](https://docs.microsoft.com/azure/cdn/cdn-token-auth).
