[Página Pai](./indexTabManager.md)

## SPA

O comportamento no TabManager SPA é obtido abrindo uma nova aba (tab) na mesma página e ocultando a anteior, depois fechando da última aba no sentido da primeira. Cada aba é **Modal** - não permite a volta para a anteior de fechar a atual.

### Eventos das abas SPA

1. **onShow**: Ocorre depois a nova aba foi aberta. O código deste evento deve estar colocado em _tabManager.GetLastTab().bag.onShow(lastTab)_;
2. **onBack**: Ocorre quando a aba posterior a essa foi fechada e a aba atual passa a ser essa novamente. O código deste evento deve estar colocado em _tabManager.GetLastTab().bag.onShow(lastTab)_;
3.  **onClose**: Ocorre depois que a aba foi fechada. O codigo desde evento eve estar colado em _tabManager.GetLastTab().bag.onClose(lastTab)_;

Ex:
```javascript   
<script type="text/javascript">
    tabManager.GetLastTab().bag.onShow = function(lastTab) {
        alert("Oi " + lastTab.titulo);
    }

    tabManager.GetLastTab().bag.onBack = function(lastTab) {
        alert("Oi " + lastTab.titulo);
    }

    tabManager.GetLastTab().bag.onClose = function(lastTab) {
        alert("Tchau " + lastTab.titulo);
    }
</script>
```

## Atributos data-

Os atributos _data-template, data-alias e data-event_ fornecem facilidades ao desenvolvedor por ligar o HTML a javascript. Qualquer elemento HTML pode ser marcado com esses 3 atributos. 

> O TabManager também  estabelece um convenção onde funções, variáveis e ojetos devem ser sempre colocados em tabManager.bag ou tabManager.GetLastTab().bag e nunca na raiz "window" da página. 

## Templates

Template é uma função javascript que o binder do TabManager executa ao encontrar um elemento marcado com o atributo _data-template_. 

Pode ser passado parâmetros adicionais para a função usando o seguinte atributo: data-template-params="Red,Black,Yellow,10". Passar os parâmtros separados por vírgula. Todos os parâmetros são injetados dentro do template como strings. Mais de um template pode ser colocado fazendo a separação por pipe "t1|t2" e também seus respectivos parâmetros quando houverem "|param1,param2".

Ex:

```html
<elemento data-template="AjaxRazorGrid2Template" ...> ... </elemento>
```

```typescript
tabManager.bag["AjaxRazorGridTemplate"] = function (element: JQuery<HTMLElement>, tm: TabManager) {
    ...
}
```

## Eventos

Associa uma função aos eventos desejados.

Ex:

```html
<elemento data-event="change blur etc:nomeFuncao">...</elemento>
```

```typescript
tabManager.GetLastTab().bag.onTipoPessoa = function(lastTab, el) {
    ...
}
```

## Alias

Cria em GetLastTab().bag um JQuery com o alias informado no elemento, com o prefixo $, exemplo: $combobox. disponivel logo após o termino do bind, como no evento onShow.

```html
<elemento data-alias="nomeDoAlias">...</elemento>
```

```javascript
$nomeDoAlias.hide();
```

## AJAX

Templates que fazem requisições ajax podem ter atributos de eventos ajax associados a eles, os eventos vão acionar a função implementada no momento apropriado. 

Esse elementos podem ser o seguintes data-attr de eventos adicionados:
  
## data-before-ajax='funcao(callerElement, [lastTab/null])' [ambos] 
   
Executa a função exatamente antes de chamar por ajax.
   
## data-before-ajax-apply-response='funcao(callerElement, data, jqXHR, lastTab)' [ambos]

Executa a função exatamente antes de trabalhar o response do ajax. IMPORANTE: retornar true da função para deixar o TabManager executar o comportamento padrão (tratar o response) e false para o tratamento ser apenas o que foi escrito na função.
   
## data-after-ajax='funcao(callerElement, data, jqXHR, lastTab)' [ambos] 

Executa a função depois que o response já foi tratado pelo TabManager.
Cuidado com o parâmetro lastTab, pois se a função callback do ajax tiver fechado alguma aba, pode ser que você fique sem entender o que pode estar dando errado em sua lógica.

```html
    <a data-before-ajax="beforeAjaxEx" 
        data-before-ajax-apply-response="beforeAjaxApplyResponseEx"
        data-after-ajax="afterAjaxEx">exemplo
    </a>

    <form data-before-ajax="beforeAjaxEx" 
        data-before-ajax-apply-response="beforeAjaxApplyResponseEx"
        data-after-ajax="afterAjaxEx">
        exemplo
    </form>
}
```

```javascript
tabManager.GetLastTab().bag.beforeAjaxEx = function(callerElement, lastTab) {
    ...
}

tabManager.GetLastTab().bag.beforeAjaxApplyResponseEx = function(callerElement, data, jqXHR, lastTab) {
    ...
    return true/false;
}

tabManager.GetLastTab().bag.afterAjaxEx = function(callerElement, data, jqXHR, lastTab) {
    ...
}
```

## MPA

Não quer usar SPA, use MPA, o TabManger também está pronto para isso.

## Fazer o botão voltar funcionar

https://css-tricks.com/using-the-html5-history-api/
