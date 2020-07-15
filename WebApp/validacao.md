[Página Pai](./indexWebApp.md)

## Validações Aplicaveis na classe ViewModel

Muitas validações podem ser feitas usando a implamentação nativa do asp.net core.
Basicamente adiciona-se o namespace "using System.ComponentModel.DataAnnotations;" e depois aplica-se os atributos de validação.

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


