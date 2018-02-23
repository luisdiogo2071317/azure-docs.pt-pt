---
title: "Gerir a expiração de conteúdo web numa rede de entrega de conteúdo do Azure | Microsoft Docs"
description: "Saiba como gerir a expiração de conteúdo de serviços de aplicações/nuvem de Web do Azure, o ASP.NET nem o IIS na CDN do Azure."
services: cdn
documentationcenter: .NET
author: dksimpson
manager: akucer
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2018
ms.author: mazha
ms.openlocfilehash: db7b5053cb926d2ec86c7feea4ac411acbeb1ae2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Gerir a expiração de conteúdo web numa rede de entrega de conteúdo do Azure
> [!div class="op_single_selector"]
> * [Conteúdo Web do Azure](cdn-manage-expiration-of-cloud-service-content.md)
> * [Armazenamento de Blobs do Azure](cdn-manage-expiration-of-blob-content.md)
> 

Ficheiros a partir de servidores de web de origem acessível publicamente podem ser colocadas em cache na rede de entrega de conteúdos (CDN) do Azure até as respetivas time-to-live (TTL) decorrida. O valor de TTL é determinado pelo `Cache-Control` cabeçalho de resposta HTTP do servidor de origem. Este artigo descreve como definir `Cache-Control` cabeçalhos para a funcionalidade Web Apps do App Service do Microsoft Azure, Cloud Services do Azure, as aplicações ASP.NET e sites de serviços de informação Internet (IIS), todos os que estão configurados da mesma forma. Pode definir o `Cache-Control` cabeçalho utilizando ficheiros de configuração ou através de programação. 

Também pode controlar as definições de cache do portal do Azure através da definição [CDN regras a colocação em cache](cdn-caching-rules.md). Se criar um ou mais a colocação em cache as regras e definir o respetivo comportamento de colocação em cache para **substituir** ou **ignorar a cache**, as definições de colocação em cache origem fornecidos pelo abordadas neste artigo são ignoradas. Para obter informações sobre os conceitos gerais de colocação em cache, consulte [funciona como colocação em cache](cdn-how-caching-works.md).

> [!TIP]
> Pode optar por não definir nenhum valor de TTL num ficheiro. Neste caso, o CDN do Azure aplica automaticamente uma predefinição TTL de sete dias, a menos que tiver configurado a colocação em cache regras no portal do Azure. Esta predefinição TTL só se aplica a otimizações de entrega web geral. Para otimizações de ficheiros grandes, o TTL predefinido é um dia e para o suporte de dados otimizações de transmissão em fluxo, o TTL predefinido é de um ano.
> 
> Para obter mais informações sobre como funciona o CDN do Azure para acelerar o acesso a ficheiros e outros recursos, consulte [descrição geral da rede de entrega de conteúdos de Azure](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Definir cabeçalhos Cache-Control ao utilizar a CDN regras a colocação em cache
O método preferencial de definição de um servidor web `Cache-Control` cabeçalho consiste em utilizar regras de colocação em cache no portal do Azure. Para obter mais informações sobre a CDN regras a colocação em cache, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

> [!NOTE] 
> Regras de colocação em cache só estão disponíveis para **CDN do Azure da Verizon padrão** e **CDN do Azure da Akamai padrão** perfis. Para **CDN do Azure da Verizon Premium** perfis, tem de utilizar o [motor de regras da CDN do Azure](cdn-rules-engine.md) no **gerir** portal para uma funcionalidade semelhante.

**Para navegar para a página regras de colocação em cache CDN**:

1. No portal do Azure, selecione um perfil CDN, em seguida, selecione o ponto final para o servidor web.

2. No painel esquerdo em definições, selecione **regras a colocação em cache**.

   ![Botão de regras de colocação em cache CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   O **regras a colocação em cache** é apresentada a página.

   ![Página de colocação em cache de CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Para definir uma web cabeçalhos Cache-Control do servidor através de regras de colocação em cache global:**

1. Em **Global regras a colocação em cache**, defina **comportamento de colocação em cache de cadeia de consulta** para **ignorar cadeias de consulta** e defina **comportamento de colocação em cache** para  **Substituir**.
      
2. Para **duração de expiração da Cache**, introduza 3600 no **segundos** caixa ou 1 no **horas** caixa. 

   ![Exemplo de regras de colocação em cache global de CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Esta regra de colocação em cache global define uma duração da cache de uma hora e afeta todos os pedidos para o ponto final. Substitui qualquer `Cache-Control` ou `Expires` cabeçalhos de HTTP são enviados pelo servidor de origem especificado pelo ponto final.   

3. Selecione **Guardar**.

**Para definir cabeçalhos de Cache-Control do ficheiro de um servidor web utilizando regras personalizadas de colocação em cache:**

1. Em **personalizada de colocação em cache regras**, crie duas condições de correspondência:

     a. Para a primeira condição de correspondência, defina **correspondem à condição** para **caminho** e introduza `/webfolder1/*` para **corresponde ao valor**. Definir **comportamento de colocação em cache** para **substituir** e introduza 4 no **horas** caixa.

     b. Para a segunda condição de correspondência, defina **correspondem à condição** para **caminho** e introduza `/webfolder1/file1.txt` para **corresponde ao valor**. Definir **comportamento de colocação em cache** para **substituir** e introduza 2 no **horas** caixa.

    ![Exemplo de regras de colocação em cache personalizado de CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    A primeira regra de colocação em cache personalizada define uma duração da cache de quatro horas para os ficheiros de `/webfolder1` pasta no servidor de origem especificado pelo seu ponto final. A segunda regra substitui a primeira regra para o `file1.txt` apenas de ficheiros e define uma duração da cache de duas horas para a mesma.

2. Selecione **Guardar**.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Definir cabeçalhos Cache-Control utilizando ficheiros de configuração
Para o conteúdo estático, como imagens e folhas de estilo, pode controlar a frequência de atualização ao modificar o **applicationHost. config** ou **Web. config** ficheiros de configuração da sua aplicação web. Para definir o `Cache-Control` cabeçalho para o conteúdo, utilize o `<system.webServer>/<staticContent>/<clientCache>` elemento em qualquer um dos ficheiros.

### <a name="using-applicationhostconfig-files"></a>A utilizar ficheiros de no applicationHost. config
O **applicationHost. config** ficheiro é o ficheiro de raiz do sistema de configuração do IIS. As definições de configuração num **applicationHost. config** ficheiro afetar todas as aplicações no site, mas são substituídas por definições de qualquer **Web. config** ficheiros existentes para uma aplicação web.

### <a name="using-webconfig-files"></a>Utilizar ficheiros Web. config
Com um **Web. config** ficheiro, pode personalizar a forma como se comporta a sua aplicação web todo ou um diretório específico na sua aplicação web. Normalmente, tem, pelo menos, um **Web. config** ficheiro na pasta raiz da aplicação web. Para cada **Web. config** ficheiros numa pasta específica, as definições de configuração afetam tudo nessa pasta e correspondentes subpastas, a menos que estes são substituídos ao nível da subpasta por outro **Web. config** ficheiro. 

Por exemplo, pode definir um `<clientCache>` elemento um **Web. config** ficheiro na pasta raiz da aplicação web para colocar em cache todo o conteúdo estático na sua aplicação web de três dias. Também pode adicionar um **Web. config** ficheiro numa subpasta com o conteúdo mais variável (por exemplo, `\frequent`) e defina o `<clientCache>` elemento para colocar em cache o conteúdo a subpasta para seis horas. O resultado net é esse conteúdo em toda a web site é colocado em cache para três dias, exceto para qualquer conteúdo a `\frequent` diretório, que é colocado em cache para apenas seis horas.  

O exemplo de ficheiro de configuração XML seguinte mostra como definir o `<clientCache>` elemento para especificar uma duração máxima de três dias:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Para utilizar o **cacheControlMaxAge** atributo, tem de definir o valor da **cacheControlMode** atributo para `UseMaxAge`. Esta definição causou o cabeçalho HTTP e a diretiva, `Cache-Control: max-age=<nnn>`, para ser adicionado à resposta. O formato do valor timespan para o **cacheControlMaxAge** atributo `<days>.<hours>:<min>:<sec>`. O valor é convertido em segundos e é utilizado como o valor da `Cache-Control` `max-age` diretiva. Para obter mais informações sobre o `<clientCache>` elemento, consulte [Cache do cliente <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Definir cabeçalhos Cache-Control através de programação
Para aplicações ASP.NET, pode controlar a CDN permitir a colocação em forma programática definindo a **HttpResponse.Cache** propriedade da .NET API. Para obter informações sobre o **HttpResponse.Cache** propriedade, consulte [HttpResponse.Cache propriedade](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) e [HttpCachePolicy classe](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Para através de programação aplicação conteúdo em cache no ASP.NET, siga estes passos:
   1. Certifique-se de que o conteúdo está marcado como colocáveis definindo `HttpCacheability` para `Public`. 
   2. Definir um validador cache chamando um dos seguintes `HttpCachePolicy` métodos:
      - Chamar `SetLastModified` para definir um valor de carimbo para o `Last-Modified` cabeçalho.
      - Chamar `SetETag` para definir um valor para o `ETag` cabeçalho.
   3. Opcionalmente, especifique uma hora de expiração de cache ao chamar `SetExpires` para definir um valor para o `Expires` cabeçalho. Caso contrário, a heurística de cache predefinido descrita anteriormente neste documento aplicam-se.

Por exemplo, a cache de conteúdo para uma hora e adicione o seguinte código c#:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>O cabeçalho de Cache-Control de teste
Pode facilmente verificar as definições de TTL do seu conteúdo web. Com o seu browser [ferramentas de programador](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), teste o conteúdo da web inclui o `Cache-Control` cabeçalho de resposta. Também pode utilizar uma ferramenta como **wget**, [Postman](https://www.getpostman.com/), ou [Fiddler](http://www.telerik.com/fiddler) para examinar os cabeçalhos de resposta.

## <a name="next-steps"></a>Próximos Passos
* [Lê os detalhes sobre o **clientCache** elemento](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Leia a documentação para o **HttpResponse.Cache** propriedade](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Leia a documentação para o **HttpCachePolicy classe**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)  
* [Saiba mais sobre conceitos a colocação em cache](cdn-how-caching-works.md)
