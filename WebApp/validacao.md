[Página Pai](./indexWebApp.md)

## Validando Regras de Negócio

Validação é verificar se alguma regra de negócio foi violada. Essa regra pode ser simples, como digitar pelo menos 3 caracteres em algum campo ou uma regra de negócio mais elaborada como se um cliente for pessoa física, não poder finalizar uma compra com o opção de parcelamento se ele possuir débitos pendentes.

### Validações Aplicaveis na classe ViewModel

Muitas validações podem ser feitas usando a implamentação nativa do asp.net core.
Adiciona-se o namespace "using System.ComponentModel.DataAnnotations;" e depois aplica-se os atributos de validação.

Adicionamente pode-se implementar a interface IValidatableObject que irá solicitar para ser implementado o método "public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)"

Ex.

``` C#

    public class EditModuloVM : IValidatableObject
    {
        [Required(ErrorMessage = Mensagens.O_CAMPO_X_E_OBRIGATORIO)]
        [MaxLength(30, ErrorMessage = Mensagens.O_CAMPO_X_DEVE_TER_NO_MAXIMO_Y_CARACTERES)]
        [Display(Name = "Nome")]
        public string Nome { get; set; }

        public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
        {
            var context = validationContext.GetRequiredService<IHttpContextAccessor>();
            
            var headerParams = context.HttpContext.GetHeaderParamsHelper();
            string idAnterior = headerParams.ReadServerState(nameof(Id));
            if (!string.IsNullOrEmpty(idAnterior))
                if (Id != idAnterior) 
                    yield return new ValidationResult(
                        "O ID não pode ser alterado.", new[] { nameof(Id) });
                    
        }
 
    }

```
### Validações Aplicáveis na classe Service 

A classe servive pode apoiar-se na classe repository para fazer uma validação mas é service quem faz. Para que a classe service possa inserir erros de validação dentro ModelState o metodo de service deve receber o parâmetro ModelState do Controller.

### Exemplo de Validação em View e Service

No exemplo abaixo o Controller verifica se ModelState é válido primeiramente testando ViewModel e depois testando em Service. Note que o método service recebe como parâmetro o ModelState para que ele possa inserir suas mensagens de validação. Perceba também que o método de Service retorna void, pois ele esta escrevendo suas considerações em ModelState.

``` C#

    [HttpPost]
    [ValidateAntiForgeryToken]
    public IActionResult EditBairroPost(EditBairroVM vm)
    {
        if (ModelState.IsValid)
        {
            bairroSer.AddOrUpdate(vm, ModelState);
            if (ModelState.IsValid)
            {
                return ResponseHelper.ReturnPostBackSuccess(this.HttpContext,
                        string.Format(Mensagens.X_SALVO_COM_SUCESSO, ViewBag.NomeEntidade));
            }
        }
        bairroSer.PopulateEdit(vm);
        return PartialView("EditBairro", vm);
    }
```