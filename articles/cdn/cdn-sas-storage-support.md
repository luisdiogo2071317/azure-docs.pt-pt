---
title: Utilizar a CDN do Azure com SAS | Microsoft Docs
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Utilizar a CDN do Azure com SAS

Ao servir conteúdo a partir do contentor de armazenamento da sua conta de armazenamento, poderá querer proteger a forma como os utilizadores podem aceder aos seus ficheiros, concedendo acesso privado para o contentor de armazenamento. Caso contrário, um contentor de armazenamento para o qual foi concedido acesso público pode ser acedido por qualquer pessoa que sabe o URL. Para proteger uma conta de armazenamento que tenha a permissão da rede de entrega de conteúdos (CDN) para aceder, pode utilizar a funcionalidade de assinatura de acesso partilhado (SAS) de armazenamento do Azure para conceder acesso limitado a contentores de armazenamento privada.

Uma SAS é um URI que concede restrito direitos de acesso aos seus recursos de armazenamento do Azure sem a chave de conta a exposição. Pode fornecer uma SAS para clientes que não seja fidedigna com a sua chave de conta de armazenamento, mas para quem pretende delegar o acesso a alguns recursos da conta de armazenamento. Por distribuição de uma assinatura de acesso partilhado URI para estes clientes, pode conceder acesso a um recurso durante um período de tempo especificado.
 
SAS permite-lhe definir vários parâmetros de acesso para um blob, como os tempos de início e de expiração, permissões (leitura/escrita) e os intervalos de IP. Este artigo descreve como utilizar SAS em conjunto com a CDN do Azure. Para obter mais informações sobre SAS, incluindo como criar e as opções de parâmetros, consulte [Using partilhado assinaturas de acesso (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Ao configurar a CDN do Azure para trabalhar com o armazenamento SAS
As três opções seguintes são recomendadas para através da SAS com CDN do Azure. Todas as opções de partem do princípio de que já criou um trabalho SAS (consulte pré-requisitos). 
 
### <a name="prerequisites"></a>Pré-requisitos
Para começar, criar uma conta de armazenamento e, em seguida, gerar uma SAS para o seu elemento. Pode gerar dois tipos de assinaturas de acesso armazenada: um serviço SAS ou uma conta SAS. Para obter mais informações, consulte [tipos de assinaturas de acesso partilhado](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Depois de ter gerado uma SAS, pode aceder ao seu ficheiro de armazenamento de Blobs com um URL que tem o seguinte formato:`https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Por exemplo:
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Para obter mais informações sobre os parâmetros de definição, consulte [considerações de parâmetro SAS](#sas-parameter-considerations) e [parâmetros de assinatura de acesso partilhado](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Definições de SAS do CDN](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Opção 1: Através da SAS com pass-through para o blob storage da CDN

Esta opção é a mais simples e utiliza apenas um único token SAS, que é transmitido da CDN para o servidor de origem. É suportado pelo **CDN do Azure da Verizon**, perfis Standard e Premium, e **CDN do Azure da Akamai**. 
 
1. Selecione um ponto final, clique em **regras a colocação em cache**, em seguida, selecione **colocar em Cache todos os URLs únicos** do **colocação em cache de cadeia de consulta** lista.

    ![Regras de colocação em cache de CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depois de configurar SAS na sua conta de armazenamento, utilize o token SAS com o URL da CDN para aceder ao ficheiro. 
   
   O URL resultante tem o seguinte formato:`https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Por exemplo:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Otimizar a duração da cache utilizando regras de colocação em cache ou adicionando `Cache-Control` cabeçalhos na origem. Porque a CDN trata o token SAS como uma cadeia de consulta simples, como melhor prática, deve configurar uma duração de colocação em cache expira no ou antes da data de expiração de SAS. Caso contrário, se um ficheiro é colocado em cache durante um período de tempo que as SAS está ativa, o ficheiro poderá estar acessível a partir do servidor de origem de CDN, depois de decorrido o tempo de expiração de SAS. Se isto ocorrer e pretender que os ficheiros na cache inacessível, tem de efetuar uma operação de remoção do ficheiro para desmarcá-la a partir da cache. Para obter informações sobre como definir a duração da cache em CDN, consulte [controlo Azure conteúdo rede de entrega de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Opção 2: Oculta utilizando a regra de reescrever token de segurança CDN
 
Com esta opção, pode proteger o armazenamento de BLOBs de origem sem necessidade de um token SAS para o utilizador da CDN. Poderá utilizar esta opção se não precisa de restrições de acesso específicas para o ficheiro, mas pretende impedir que os utilizadores acedam a origem de armazenamento diretamente para melhorar os tempos de descarga da CDN. Esta opção só está disponível para **CDN do Azure Premium da Verizon** perfis. 
 
1. Utilize o [motor de regras](cdn-rules-engine.md) para criar uma regra de URL Rewrite. Novas regras demorar cerca de 90 minutos para propagar.

   ![Botão de gerir a CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Botão de motor de regras CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Esta regra de exemplo URL Rewrite tem os seguintes padrões:
   
   Origem:   
   `/test/demo.jpg`
   
   Destino:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Regra de CDN URL Rewrite](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Aceder ao ficheiro no seu CDN sem token SAS, no seguinte formato:`https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Por exemplo:   
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Tenha em atenção que qualquer pessoa, independentemente se estão a utilizar um token SAS, pode aceder a um ponto final da CDN. 

3. Otimizar a duração da cache utilizando regras de colocação em cache ou adicionando `Cache-Control` cabeçalhos na origem. Porque a CDN trata o token SAS como uma cadeia de consulta simples, como melhor prática, deve configurar uma duração de colocação em cache expira no ou antes da data de expiração de SAS. Caso contrário, se um ficheiro é colocado em cache durante um período de tempo que as SAS está ativa, o ficheiro poderá estar acessível a partir do servidor de origem de CDN, depois de decorrido o tempo de expiração de SAS. Se isto ocorrer e pretender que os ficheiros na cache inacessível, tem de efetuar uma operação de remoção do ficheiro para desmarcá-la a partir da cache. Para obter informações sobre como definir a duração da cache em CDN, consulte [controlo Azure conteúdo rede de entrega de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Opção 3: Utilizar a autenticação de token de segurança CDN com uma regra de reescrever

Esta opção é a mais segura e personalizáveis. Para utilizar a autenticação de token de segurança CDN, tem de ter um **CDN do Azure Premium da Verizon** perfil. Acesso de cliente é baseado nos parâmetros de segurança definida no token de segurança de CDN. No entanto, se a SAS fica inválida, a CDN não conseguir revalidate o conteúdo do servidor de origem.

1. [Criar um token de segurança CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) e ative-a utilizando o motor de regras para o ponto final de CDN e o caminho onde os utilizadores podem aceder ao ficheiro.

   Um URL SAS tem o seguinte formato:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Por exemplo:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   As opções de parâmetros para uma autenticação de token de segurança CDN são diferentes as opções de parâmetros para um token SAS. Se optar por utilizar um período de tempo de expiração quando cria um token de segurança CDN, defini-lo para o mesmo valor que a hora de expiração para o token SAS. Se o fizer, garante que a hora de expiração é previsível. 
 
2. Utilize o [motor de regras](cdn-rules-engine.md) para criar uma regra de URL Rewrite para ativar o acesso de token para todos os blobs no contentor. Novas regras demorar cerca de 90 minutos para propagar.

   Esta regra de exemplo URL Rewrite tem os seguintes padrões:
   
   Origem:   
   `/test/demo.jpg`
   
   Destino:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Regra de CDN URL Rewrite](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. Quando renovar a SAS, atualize a regra Url Rewrite para utilizar o novo token SAS. 

## <a name="sas-parameter-considerations"></a>Considerações de parâmetro SAS

Porque os parâmetros SAS não estão visíveis para a CDN, a CDN não é possível alterar o comportamento de entrega com base nos mesmos. As restrições de parâmetro definido aplicam-se apenas nos pedidos que a CDN torna-se para o servidor de origem, não para pedidos de cliente para a CDN. Este distinção é importante a considerar quando definir os parâmetros SAS. Se estas capacidades avançadas são necessárias e estiver a utilizar [opção 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), defina as restrições adequadas no token de segurança de CDN.

| Nome do parâmetro SAS | Descrição |
| --- | --- |
| Iniciar | O tempo que pode começar a CDN para aceder ao ficheiro de blob. Devido a relógio desfasamento (quando chega relógio um sinal de em alturas diferentes para diferentes componentes), escolha uma hora de 15 minutos anteriormente se pretender que o elemento estar imediatamente disponível. |
| Terminar | O tempo após o qual a CDN já não pode aceder ao ficheiro de blob. Anteriormente, os ficheiros em cache na CDN são continuarão acessíveis. Para controlar a hora de expiração de ficheiros, defina a hora de expiração adequado no token de segurança de CDN ou remover o elemento. |
| Endereços IP permitidos | Opcional. Se estiver a utilizar **CDN do Azure da Verizon**, pode definir este parâmetro para os intervalos definidos no [CDN do Azure da Verizon Edge intervalos de IP servidor](https://msdn.microsoft.com/library/mt757330.aspx). Se estiver a utilizar **CDN do Azure da Akamai**, não é possível definir o parâmetro de intervalos IP, porque os endereços IP não são estáticos.|
| Protocolos permitidos | Os protocolos permitidos para um pedido efetuado com a conta SAS. Recomenda-se a definição de HTTPS.|

## <a name="see-also"></a>Consulte também
- [Utilizar assinaturas de acesso partilhado (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Partilhado assinaturas de acesso, parte 2: Criar e utilizar um SAS com o Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Proteger recursos de rede de entrega de conteúdos do Azure com a autenticação de token](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
