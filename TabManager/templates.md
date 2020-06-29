[Página Pai](./indexTabManager.md)
  
## Templates

São funções implementadas em typescript ou javascript que são associadas ao elemento HTML. Elas podem alterar o DOM da forma que for preciso, como aplicar estilos, adicionar eventos, implementar funcionalidades, etc. 

### *AjaxLinkTemplate*

Para serem ligados ao elemento **a**.

> Esse template reage a parâmetros no Header.

#### Parâmetros no Header podem ser:

Param    | Valores
-------- | ----------------------------------------------------------------------------------------------
view     | none - não altera a view atual em nada. retornar new EmptyResult();
postback | [ success / info / warning / error ]. Exibe um toaster para o usuário. Requer o parâmetro "msg" também informado.
msg	     | Texto com a mensagem para o usuário 

#### Atributos

1. **data-title='Título da tab'** [tabs] - Título da nova aba. 
2. **data-target='#elemento'** [ambos] - Insere o response do ajax dentro do container substituindo o conteúdo atual. Quando especificado não abre nenhuma aba.
3. **data-find-in-page='true'** [ambos] - Ignora a tab atual (GetLastTab()) e busca o data-tagert na 
4. **data-close-all-tabs='true'** [tabs] - Fecha todas as tabs abertas antes de carregar o response do ajax.
página.
5. **data-in-popup** - abre o response dentro de um popup.
6. **data-content-in-container** - não realiza um request ajax, os dados a serem colocados em uma aba estão dentro de um div container. Passar o ID do div #idDivContainer como parâmetro.
7. **data-refresh-tab='true'** - atualiza o conteúdo da tab atual. Passar "#" em href para usar a url do último GET.

### *AjaxFormTemplate*

Para serem ligados ao elemento **form**.

> Esse template reage a parâmetros no Header.

#### Parâmetros no Header podem ser:

Param         | Valores
------------- | --------------------------------------------------------------------------------
view          | [ search / grid / novatab / popup / custom]
titulo        | Quando "view" for passada o valor "novatab" será título da nova tab, ou este poderá ser colocado via javascript.
postback      | [ success / info / warning / error ] **NOTA**. Requer o parâmetro msg também informado.
msg           | Texto com a mensagem para o usuário 

#### Atributos 

1. **data-no-close-last-tab='true'** [tabs] - Não fecha a tab atual apos enviar o form.
2. **data-razor-grid='#id'** [ambos] - Especifica o id da razor-grid que receberá o response do envio do form. normalmente usado na combinação form de pesquisa de grid.
3. **data-previous-tab-refresh='true'** [tabs] - Informa que a tab anterior deve ser atualizada após o envio de form. Chama novamente a url do carregamento da tab.
4. **data-click-grid-active-pager='#id'** [ambos] - clica no pager da razor grid após o envio do form para efeitos de atualização da grid.

### *CascadeDropDownTemplate*

Para serem ligados ao elemento **select**.

#### Atributos 

1. **data-controller='...Controller'** [ambos] - Nome do Controller que receberá o request.
2. **data-action='Json...'** [ambos] - Nome da Action que receberáo request.
3. **data-controle-filho='#...'** [ambos] - Id do select filho que será alimentado com o Json do response.
4. **data-method-param-name='id'** [ambos] - Nome do parâmetro da Action que recebe o valor do controle com esse template, o controle Pai.
5. **data-manter-value-filho='#id'** [ambos] - Passar o valor do Select > Option que não será removido quando o template atualizar a lista do Select.

Ex:
```html
<select asp-for="IdUF" class="form-control" asp-items="Model.SelectUFs" 
    data-template="CascadeDropDownTemplate" data-controller="/Cidade" data-action="JsonCidades", 
    data-controle-filho="#IdCidade" data-method-param-name="id" data-manter-value-filho="">
    <option value="">Opcional</option>
</select>
```

### *ElWithGuidTemplate*

Para serem ligados a qualquer elemento.

#### Atributos 

1. **data-template-params='a,b,c'** [ambos] - onde "a" é o nome do atributo onde a Guid vai ser inserido, "b" é opcional, representa um alias para o Guid, o alias é usado para poder recuperar o valor do Guid em referências futuras e "c" é o prefixo opcional que pode ser concatenado com o Guid.

Cada alias trabalha dentro do contexto de sua tab e por isso pode ser repetido dentro de outras tabs.

Ex:
```html

```