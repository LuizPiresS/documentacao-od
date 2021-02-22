> # Dialog flow  

> ### O que é?  
> É no dialog flow que se define o fluxo do dialogo que o bot terá com o usuário final  
> ![Dialog Flow](../images/dialog-flow.png)  
> Para criar o **dialog flow** utilizamos o **OBotML** que é uma implementação do YAML do ODA.   

> ### Estruturando um Dialog Flow  
> 
> A estrutura de um Dialog Flow é dividida em três partes **context**, **defaultTransitions** e **states**  
> As veriaveis globais devem ser defidas no **context**  
> O fluxo em si deve ser definido na sessão de **states**  
> 
>  Abaixo exemplo de código em OBotML:
```YAML
main: true
name: "HelloKids"
context:
  variables:
    variable1: "entity1"
    variable2: "error"
...
States      
    state1:
      component: "a custom or built-in component" 
      properties: 
        property1: "component-specific property value"
        property2: "component-specific property value"
      transitions:
        actions:
          action1: "value1"
          action2: "value2"
    state2:
      component: "a custom or built-in component" 
      properties: 
        property1: "component-specific property value"
        property2: "component-specific property value"
      transitions:
        actions:
          action1: "value1"
          action2: "value2"
...
```
> ### Context
> As variavés declaradas dentro de **context** devem ser de tipos primitivos como int, string, boolean ou float  
> Também podemos declarar variavies para as entidades integradas ou personalizadas.  
> Outro tipo de variavel que podemos declarar são do tipo _nresult_ que contem o intent resolvido pela entrada do usuário

```YAML
main: true
name: "PizzaBot"
context:
  variables:
    size: "PizzaSize"
    type: "PizzaType"
    crust: "PizzaCrust"
    iResult: "nlpresult"
```

>### defaultTransitions
> É possivel defeinir as transições em doi lugares:
> Como parte das definições de um component ou no defaultTransitions
```YAML
defaultTransitions
  next: "..."
  error: "..."
  actions:
    action_name1: "..."
    action_name2: "..."
 ````
>A defaultTransition atua como um fallback pois é acionada quando não há transições definidas ou as condições não são atendidas para acionar uma transiction.  
>É nesta parte em que vamos definir como o bot irá reagir a ações inesperadas do usuário.  
> Como mostrado no trecho de código abaixo com a action NONE conseguimos definir uma action para que ela lide com todas as ações inesperadas  
```YAML
defaultTransitions:
  error: "globalErrorHandler"
 ...
globalErrorHandler:
  component: System.Switch
  properties:
   source: "${system.errorState}"
   values:
   - "getOrderStatus"
   - "displayOrderStatus"
   - "createOrder"
  transitions:
    actions:
      NONE: "unhandledErrorToHumanAgent"
      getOrderStatus: "handleOrderStatusError"
      displayOrderStatus: "handleOrderStatusError"
      createOrder: "handleOrderStatusError"
```


>### States
> Aqui são definidos cada parte do dialogo assim como suas operações relacionadas como uma sequencia de estados transitórios, que gerenciam a lógica dentro daquele dialogo.  
> Para indicar a ação, cada state em sua definição nomeia um componente que fornece uma funcionalidade que é necessaria para aquele ponto do dialogo.  
> Um state é algo proximo a uma função, quando chamamos um state estamos executando um component que é um conjunto de ações.
> Eles contem propriedades do escopo do component e definem as transições para outros states que são acionados após a execução do component
```YAML
  state_name:
    component: "component_name"
    properties:
      component_property: "value"
      component_proprety: "value"
    transitions:
      actions:
        action_string1: "go_to_state1"
        action_string2: "go_to_state2"
```

> A definição de um state pode incluir as transições que são especificas para o componet ou para o fluxo em si.  
> Elas podem ser de quatro tipos next, error, actions ou ainda return  
```YAML
state_name:
  component: "component name"
  properties:
    component_property: "value"
    component_proprety: "value"
  transitions:
    next: "go_to_state"
    error: "go_to_error_handler"
    actions:
      action_string1: "go_to_state1"
      action_string2: "go_to_state2"
```

> # Como estrever fluxos com OBotML