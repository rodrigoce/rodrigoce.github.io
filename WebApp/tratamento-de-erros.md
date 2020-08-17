[Página Pai](./indexWebApp.md)

### Don’t repeat yourself (DRY)

Tramento de erros é um requisito, naturalmente, mas não pode-se usar infinitos try's por todo o código. 
Com isso, a artuitetura deve ser pensada achando um lugar onde um try possa ser retutilizado, quando possível.

Alem disso na ocorrencia de um erro, basicamente sempre o usuário tem que ser notificado.

Além disso, os datelhes dos erros devem ser persitidos para futura consulta do Usuário ou Técnica.

## Classes auxiliars para tratamento de erros.

``` C#
    .
    .
    .

    catch (Exception e)
    {
        var t = new PlannedException(e);
        HttpContext.GetExceptionsHelper().AddException(t);
    }
```

PlannedException: é uma classe para fazer a captura e organização dos dados do erro.

GetExceptionsHelper(): é um metodo de extensão de HttpContext de retorna uma class ExceptionsHelper,
essa é utilizada para ir acumulando os erros em HttpContext.Items, para ao final os erros poderem 
ser gravados em log e exibidos ao usuário.

## Tratando erros de gravação de banco de dados

DbContext.SaveChanges() foi sobreescrito, e faz toda função de tratamento de erros de gravação de
banco de dados. Isso quer dizer basicamente ele possui o trecho de código exibido acima e que não 
deve-se tratar esse tipo de erro pois já foram tratador. Para saber se algum erro ocorreu em 
SaveChanges() chame por HttpContext.GetExceptionsHelper().[alguma coisa].

## Exibindo o erro para o usuário

Ao final da Actions que o Request solitou, é o ponto para devolver a UI do usuário e é ai que mais 
uma vez vai-se usar HttpContext.GetExceptionsHelper().[alguma coisa].

Veja no exemplo 1:

``` C#

    [HttpPost]
    public IActionResult DeleteModulo(string id)
    {
        moduloSer.DeleteModulo(id);
        
        if (HttpContext.GetExceptionsHelper().NoError)
            return ResponseHelper.ReturnPostBackSuccess(this.HttpContext,
                string.Format(Mensagens.X_EXCLUIDO_COM_SUCESSO, ViewBag.NomeEntidade));
        else
            return ResponseHelper.ReturnPostBackError(this.HttpContext, 
                string.Format(Mensagens.ERRO_AO_EXCLUIR_X, ViewBag.NomeEntidade));
    }
```

ResponseHelper: é um classe que favorece para que DRY aconteça.

Exemplo 2.

Está previsto para o Service não ter exceptions não tratadas e apenas retornar boolean informando se a operação foi realizada ou não. Contudo, as exceptions do Service podem ser colocadas em HttpContext.GetExceptionsHelper().AddException, ficando assim código do Controller invocando uma Service que retorna true ou false.


