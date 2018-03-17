Este exemplo envolve a utilização de [Twilio](https://www.twilio.com/) serviço para enviar mensagens SMS para um telemóvel. As funções do Azure já tem suporte para Twilio através de [Twilio enlace](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), e este exemplo utiliza essa funcionalidade.

A primeira coisa que precisa é uma conta do Twilio. É possível criar um livre no https://www.twilio.com/try-twilio. Assim que tiver uma conta, adicione os seguintes três **as definições de aplicação** à sua aplicação de função.

| Nome da definição de aplicação | Descrição de valor |
| - | - |
| **TwilioAccountSid**  | O SID para a sua conta do Twilio |
| **TwilioAuthToken**   | O token de autenticação para a sua conta do Twilio |
| **TwilioPhoneNumber** | O número de telefone associado à sua conta do Twilio. Isto é utilizado para enviar mensagens SMS. |