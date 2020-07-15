[Página Pai](./indexWebApp.md)

## Autenticação e Autorização

### Autenticação

Para autenticação (usuário e senha) foi usado AuthenticationCookies, não foi usado o pacote Identity, assim pode-se fazer essa área do sistema toda sob medida.

### Autorização

Para autorização foi criado o conceito de Módulos. Este é uma parte do sistema cadastrada de forma personalizada adicionando Menus. Estes são cadastrados e apontados a um Controller e a uma Action. Depois disso, para o Módulo é marcado quais Controllers e Actions ele concede acesso.

Já que os acessos são dados aos Controllers e Actions, no desenvolvimento não se deve usar nomes repetidos para Actions no mesmo Controller:

> Cuidado: É comum metodos overload de verbo GET e POST terem o mesmo nome em outros projeto.

Ex. fora do padrão do WebApp:

``` C#
public IActionResult EditModulo(string ID) ...

[HttpPost]
public IActionResult EditModuloPost(EditViewModel vm) ...
```

Neste caso, por convenção, usar o sufixo Post para Actions com o atributo [HttpPost].

Ex. dentro do padrão:

``` C#
public IActionResult EditModulo(string ID) ...

[HttpPost]
public IActionResult EditModuloPost(EditViewModel vm) ...
```

É consenso que o verbo GET não deve alterar dados no servidor, ao contrário do verbo POST, que pode alterar. Com isso, metodos GET são para somente leitura de dados e POST para gravação e isso implica em como as permissões devem ser configuradas.