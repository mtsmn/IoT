---

copyright:
years: 2017, 2018
lastupdated: "2018-03-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informações de Referência de Gerenciamento de Dados

Use as informações de referência a seguir para saber sobre as restrições que se aplicam ao usar o componente de gerenciamento de dados do {{site.data.keyword.iot_full}}. 

## Restrições de Esqu

Os tipos de esquema a seguir são usados em gerenciamento de dados:
- Esquema de tipo de
- Esquema da interface lógica
- Esquema do tipo de encadeamento

A tabela a seguir lista as restrições que se aplicam a cada tipo de esquema:

Tipo de esquema     |        Restrição
------------------- | -------------
Todas        | Todos os esquemas devem se adequar ao JSON válido. 
Todas        | Todos os esquemas devem se adequar ao esquema JSON válido. 
Todas        | A raiz de todos os esquemas deve ser do tipo "object". 
Todas        | Todos os esquemas têm um máximo de sete níveis de aninhamento.
Interface lógica        |  Todas as propriedades que são especificadas conforme necessário devem ter um valor padrão definido. 
Interface lógica     | Se um objeto for definido no esquema, pelo menos uma propriedade deverá ser definida para esse objeto. 
Interface lógica | Se uma matriz for definida no esquema, os itens associados deverão ser de somente um tipo, por exemplo, somente do tipo "string". 
Tipo de item        | Somente propriedades de nível superior são permitidas. Nenhum aninhamento é permitido além do primeiro nível. 
Tipo de item        | A propriedade de nível superior deve ser do tipo "object".
Tipo de item        | A propriedade de nível superior deve referenciar um $logicalInterfaceRef e um tipo. O valor de $logicalInterfaceRef é identificador ou nome de alias de uma interface lógica. O tipo deve ser configurado como "object" ou "array". 

## Exemplos de Esquemas Válidos e Inválidos

### Exemplo 1 - uma definição de esquema de interface lógica válido
O exemplo a seguir define um esquema de interface lógica que se adequa às restrições que são descritas na seção "Restrições de esquema":

  - O esquema está em conformidade com o JSON válido e com o esquema JSON válido.
  - O esquema é do tipo "object".
  - O esquema tem um nível de aninhamento inferior a sete. 
  - Duas propriedades são definidas para o esquema. 
  - As propriedades necessárias **a** e **b** têm valores padrão especificados.

```
{
 "type": "object", "properties": {
    "a": {
       "type": "string"
    }, "b": {, "b": {
       "type": "number"
      }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### Exemplo 2 - uma definição de esquema de interface lógica inválido
O exemplo a seguir mostra um esquema de interface lógica inválido. O esquema é inválido porque a matriz está definida para conter itens do tipo "string" ou tipo "number". Se uma matriz for definida no esquema, os itens associados deverão ser de somente um tipo, por exemplo, "string".

```
{
 "type": "object", "properties": {
    "a": {
      "type":"array",
       "items":{
          "type": ["string", "number"]
       }
    }
  }
}
```
### Exemplo 3 - uma definição de esquema de tipo de coisa válido
O exemplo a seguir define um esquema de tipo de coisa que se adequa às restrições que são descritas na seção "Restrições de esquema":

  - O esquema está em conformidade com o JSON válido e com o esquema JSON válido.
  - O esquema é do tipo "object".
  - O esquema tem um nível de aninhamento inferior a sete. 
  - O esquema apenas define as propriedades de nível superior. 
  - As propriedades de nível superior referenciam um tipo $logicalInterfaceRef e um tipo que é configurado como "array" ou "object". O tipo "array" pode ser usado para agregar vários dispositivos ou Coisas, por exemplo, vários sensores de temperatura. O tipo "object" pode ser usado para referenciar um único dispositivo ou Coisa, por exemplo, um único sensor de umidade.   
  - As propriedades necessárias não precisam que um valor padrão seja especificado. Essa restrição se aplica somente aos esquemas de interface lógica. Um valor padrão não pode ser especificado nesse tipo de esquema. 

```
{
   "type": "object", "properties": {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
           "$logicalInterfaceRef": "5846cd7c6522050001db0e24"
            "type" : "object"
       }
   },
   "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
