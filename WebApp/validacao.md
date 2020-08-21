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

