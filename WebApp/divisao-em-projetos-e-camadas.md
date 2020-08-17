[Página Pai](./indexWebApp.md)

## Divisão em camadas

Cada camada do WebApp é um projeto dentro da solução, os projetos são:

1. ApplicationCore - com as entidades, DTOs (views models), interfaces, specifications e outros.
2. Infrastructure - com os repositórios, serviços e outros.
3. Web - Com os Controllers, Views, TS, JS e outros.

O uso de Intefaces não foi amplamente adotado, por isso algumas categorias de classes foram colocadas diretamente em ApplicationCore.

## Quanto a passagem dos fluxo de execução pelas classes e camadas.

O fluxo mais corriqueiro em aplicativos MVC é vir do client um request ao controller, este irá acessar a service que por sua vez acessa o repository. O fluxo depois disso vem ao contrário passando por cada camada até voltar ao client novamente. 

## Principais tipos de Classes

1. **Controller** - Trata request/response e usa outros tipos de classe como **Service** para implementar funcionalidades e se manter o mais "magra" possível. Basicamente deve ter métodos IActionResult. 
2. **Service** - Normalmente chamada pelo Controller, implamenta todas as regras de negócio e validações de regras de negócio mais complexas incompatíveis com as ViewModels. Normalmente chama Repository para tratar de assuntos de banco de dados. 
3. **Repository** - Normalmente chamada pela Service, implementa persistencia, métodos que precisam ler o banco de dados para obtenção de entidades ou coleção de entidades e metodos que acessem o banco de dados para validação ou implementação de regras de negócio.
4. **View/ViewModel/Dto** - Uma view (.cshtml) com model tipado é o padrão a ser usado, isso quer dizer que ela usa uma classe do tipo ViewModel que também pode ser conhecida como uma classe DTO. ViewModel/DTO é uma classe que basicamente possue propriedades e metodos de cast. Também pode implementar o método de validação nativa do asp.net core quando herdando IValidatableObject, mas deve ser o "magra" possível. Servem para transitar dados entre a Views a outros tipos de classe e para fazer a validação nativa do asp.net core de dados digitados nos campos.
5. **Entity** - São as entidades do banco de dados, devem possuir apenas propriedades e propriedades de navegação.
6. **FilterSpecfication** - É um tipo de classe que é passado como parametro para Repository, ela implementa expressões de busca de registros ao banco de dados. É interessante porque provê reuso de código.


