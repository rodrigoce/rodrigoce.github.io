[Página Pai](./indexWebApp.md)

## Tratando Exceções

Tramento de erros é um requisito natural, além disso na ocorrencia de um erro, sempre o usuário tem que ser notificado e os datelhes dos erros devem ser persitidos para futura consulta Técnica.

### Olhando para o manual do Asp.net Core.

Neste [link](https://docs.microsoft.com/pt-br/aspnet/core/fundamentals/error-handling?view=aspnetcore-3.1) informações sobre o Middleware para tratamento de exceções disponibilizado pela Microsoft e configurado no WebApp. Mas resumidamente, o Middleware tem uma página para mostrar detalhes de uma exception para o desenvolvedor e outra, foi desenvolvida, para ambiente de produção com dados básicos e não sensíveis a segurança do aplicativo para o usuário.

Neste outro [link](https://docs.microsoft.com/pt-br/dotnet/standard/exceptions/best-practices-for-exceptions) as melhores práticas para exceções.

Dois pontos em destaque nas boas práticas: 1. "Use blocos try/catch ao redor de código que pode potencialmente gerar uma exceção **e** seu código pode se recuperar dessa exceção". 2. Não fazer uso de funções que retornam código de erros ou boolean na intenção de denotar sucesso ou falha.

### Exemplos de exceções que não serão tratadas

Definiu-se aqui que exceptions vão ocorrer e serão tratadas pelo Middleware e serão mostradas ao usuário como mensagens de erro. Isso valerá pelo menos para a maioria dos casos.

Exemplos:

1. Tentar excluir uma entidade de que tem filhos. Neste caso ocorerrá uma exception de banco.
2. 

### Exemplos de exceções que serão tratadas

Quando uma exception **pode ser recuperada** para que o erro deixe de existir.

1. Quando uma exception não tratada ocorrer deve-se gravar em log de banco de dados os detalhes da exception. Mas se o erro anterior tiver ocorrido por causa de indisponibilidade de banco de dados não será possível gravar em banco de dados e neste caso o try/catch pode ser usado, ou seja ao tentar gravar o log em banco, vai ocorrer uma exception, a recuperação do erro em catch seria gravar o log em arquivo para depois importar para o banco de dados.
2. 

### Exemplos de exceções que podem ser tratadas e transformadas em erro de validação

### Melhorar as mensagens de erro de exceptions para o usuário

As menagens cruas de exceptions podem não ser inteligíveis ao usuário, mas elas podems ser traduzidas e transformadas em uma linguagem mais explicativa para o usuário, para isso use a classe auxiliar de tratamento de exceções PlannedException. Nela você pode fazer essa transformação da mensagem.