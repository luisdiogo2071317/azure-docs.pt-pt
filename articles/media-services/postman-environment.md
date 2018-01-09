 ---
Título: importar o ambiente do Postman para REST de serviços de suporte de dados do Azure chama Descrição: Este tópico fornece uma definição do ambiente Postman para chamadas de REST de serviços de suporte de dados do Azure.
serviços: documentationcenter de serviços de suporte de dados: ' autor: Gestor Juliako: cfowler editor: '

MS. Service: dos media services workload: MS. tgt_pltfrm do suporte de dados: na devlang: MS. topic de na: article MS. Date: 04/01/2017 Author: juliako

---

# <a name="import-the-postman-environment"></a>Importar o ambiente do Postman 

Este artigo contém uma definição do **Postman** variáveis de ambiente que são utilizadas o [coleção Postman](postman-collection.md) que contenha agrupados pedidos de HTTP chamar APIs REST do suporte de dados de serviços. Os ficheiros de ambiente e a coleção são utilizados pelo [chama de configurar o Postman para API de REST dos serviços de suporte de dados](media-rest-apis-with-postman.md) tutorial.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
