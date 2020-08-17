[Página Pai](./indexWebApp.md)

## Try Vesus Validations

Sendo direto, tratar um erro é inserir um try catch no código.

Validação é verificar se alguma regra de negócio foi violada. Essa regra pode ser simples, como digitar pelo menos 3 caracteres em algum campo ou uma regra de nogócio mais elaborada como se um cliente for pessoa física, não poder finalizar uma compra com o opção de parcelamento se ele possuir débitos pendentes.

Os dois são parecidos porque devem apresentar uma notificação para o usuário, mas o primeiro é um erro que ocorreu como código fonte do sistema, o segundo é uma violação de alguma regra do sistema, não um erro. Já viu-se violação de regras de negócio gerarem exceptions mais não é a ideia aplicada aqui.
