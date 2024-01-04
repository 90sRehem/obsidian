> Selecionar produto
 - ao selecionar o produto, fazer a chamada para priceInfo obter os dados
 - se o `productType` for `tabela`, mostrar um input do tipo select como os dados da propriedade `productPriceTable`. Para obter as options do select, é necessário selecionar o tipo de recorrencia
 - caso o produto tenha a propriedade `recurrenceRequired` como true, mostrar o select de recorrencia e input para inserir valor recorrente
 - caso o produto tenha a propriedade `relatedProducts`, gerar os switchs com os dados de cada objeto
- caso o produto tenha a propriedade `quantityRequired` como true, exibir um input do tipo texto com o label 'Quantidade'


**Selecionar Produto**

Ao escolher um produto, é necessário efetuar a chamada para `priceInfo` a fim de obter os dados relacionados. Dependendo do tipo de produto (`productType`), a interface deve ser adaptada da seguinte maneira:

- Se o tipo for uma `tabela`, mostrar um menu suspenso (select) com os dados provenientes da propriedade `productPriceTable`. Para obter as opções desse menu, é necessário selecionar o tipo de recorrência.

- Caso o produto tenha a propriedade `recurrenceRequired` definida como verdadeira, apresentar o menu suspenso de recorrência e um campo para inserir o valor recorrente.

- Se o produto possuir a propriedade `relatedProducts`, gerar interruptores (switches) correspondentes aos dados de cada objeto.

- Se a propriedade `quantityRequired` do produto estiver definida como verdadeira, exibir um campo de texto com o rótulo 'Quantidade'.
