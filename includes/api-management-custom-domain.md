## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Como o servidor de Proxy APIM responde com certificados SSL no handshake TLS

### <a name="clients-calling-with-sni-header"></a>Clientes chamar com o cabeçalho SNI
Se o cliente tem um ou vários domínios personalizados configurados para Proxy, APIM pode responder a pedidos HTTPS de domínios personalizados (por exemplo, contoso.com), bem como o domínio predefinido (por exemplo, apim-service-name.azure-api.net). Com base nas informações do cabeçalho a indicação de nome de servidor (SNI), APIM responde com o certificado de servidor adequado.

### <a name="clients-calling-without-sni-header"></a>Clientes chamar sem cabeçalho SNI
Se o cliente está a utilizar um cliente, que não envia o [SNI](https://tools.ietf.org/html/rfc6066#section-3) cabeçalho, APIM cria respostas com base na lógica seguinte:

* Se o serviço tem apenas um domínio personalizado configurado para Proxy, o certificado predefinido é o certificado que foi emitido para o domínio personalizado de Proxy.
* Se o serviço configurado vários domínios personalizados para o Proxy (apenas suportado no **Premium** camada), o cliente pode designar que certificado deve ser o certificado predefinido. Para definir o certificado predefinido, o [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#hostnameconfiguration) propriedade deve ser definida como true ("defaultSslBinding": "true"). Se o cliente não defina a propriedade, o certificado predefinido é o certificado emitido para o domínio de Proxy predefinido alojado em *.azure api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Suporte para o pedido PUT/POST com grande payload

Servidor APIM Proxy suporta pedido com payload grande ao utilizar o lado do cliente certificados em HTTPS (por exemplo, payload > entre 40 KB). Para impedir que o pedido do servidor freezing, os clientes podem definir a propriedade ["negotiateClientCertificate": "true"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#hostnameconfiguration) no nome do anfitrião de Proxy. Se a propriedade está definida como VERDADEIRO, o cliente do certificado é solicitado ao tempo de ligação SSL/TLS, antes de qualquer exchange de pedido HTTP. Uma vez que a definição aplica-se no **Proxy Hostname** nível, todos os pedidos de ligação pedir o certificado de cliente. Os clientes podem configurar até 20 domínios personalizados para o Proxy (apenas suportado no **Premium** camada) e de trabalho em torno esta limitação.

