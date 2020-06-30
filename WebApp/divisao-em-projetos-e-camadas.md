[Página Pai](./indexWebApp.md)

# Divisão em projetos e camadas

## Quanto ao projeto e suas categorias de classes e arquivos internos

O WebApp é dividido em 3 projetos:

1. ApplicationCore - com as entidades, dts (views models), interfaces, specifications, etc.
2. Infrastructure - com os repositórios, serviços, etc.
3. Web - Com os Controllers, Views, TS, JS, etc.

Algumas categorias de classes foram colocadas diretamente em ApplicationCore para evitar a necessidade de uso de interfaces e ganhar tempo.

## Quanto às camadas lógicas

O fluxo mais corriqueiro em aplicativos MVC é vir do client um request que bate direto num controller, este irá acessar a service que por sua vez acessa o repository. O fluxo depois disso vem ao contrário passando por cada camada até voltar ao client novamente. Neste ponto deve ser respeitado dentro do WebApp que uma camada não deve pular uma camada para chegar a outra camada, salvos casos necessários ou que isso não faça sentido.

1. **Controller** - recebe o request, envia o fluxo para  o service, quando fluxo volta para ele, faz as devidas alterações para acertar a chamada a View. Basicamente deve ter métodos IActionResult, outras necessidades devem ser colocadas na camada service. Não faz validações nem checa regras de negócio, por exemplo. 
2. **Service** - recebe o fluxo de controller, faz checagens de regras de negócios e validações e envia o fluxo para repository. 
3. **Repository** - recebe o fluxo de service e persiste os dados. Não faz validações de campos nem regras de negócio e devolve o fluxo para service que devolve para controller.
4. **View/ViewModel/Dto** - Quando o response do Controller for conteúdo HTML, é normal que o Controller passe o fluxo para a View para renderizar, essa usa a classe ViewModel que contém ou recebe dados que devem renderizados na view ou que são provinientes do client.

* **ViewModel** é uma classe de transferência da dados (DTO), também é a classe que usa o sistema de validação por DataAnnotation do asp.net. Os dados devem ser inseridos nas páginas por meio de ViewModels e também devem ser lidos do client por meio delas, nunca os Entities do WebApp. 

