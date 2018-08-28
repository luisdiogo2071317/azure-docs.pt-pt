O serviço e o seu tipo de subscrição determinam o número de consultas por segundo (QPS) que pode fazer. Certifique-se de que a sua aplicação inclui a lógica necessária para se manter na sua quota. Se o limite de QPS for atingido ou excedido, o pedido falha e é devolvido um código de estado HTTP 429. A resposta inclui o cabeçalho `Retry-After`, que indica o tempo que tem de esperar antes de poder enviar outro pedido.

## <a name="denial-of-service-versus-throttling"></a>Denial of service versus Limitação

O serviço diferencia entre um ataque denial of service (DoS) e uma violação de QPS. Se o serviço suspeitar de um ataque DoS, o pedido é realizado com êxito (o código de estado HTTP é 200 OK). No entanto, o corpo da resposta está vazio.