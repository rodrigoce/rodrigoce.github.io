[Página Pai](../index.md)

# WebApp

É um Aplicativo Web monolítico construído com Asp.Net Core, EF Core, Razor, TypeScript, JavaScript, JQuery, etc.

## Server Side

### Diretrizes de Desenvolvimento

1. [Divisão em projetos e camadas](divisao-em-projetos-e-camadas)
2. [Autenticação e autorização](autenticacao-e-autorizacao)

## Client Side

Como foi escolhido escrever a UI em Razor, descartando Angular, Vue, React, etc., a curva de amprendizado para
Client Side ficou menor, mas abriu uma lacuna: uma library client side reutilizável, que melhore a proditividade de código,
e que melhore a UX. Para isso foi criada a library [TabManager](../../TabManager/indexTabManager.md).