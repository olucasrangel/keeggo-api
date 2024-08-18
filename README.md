# Testes de APIs

## Objetivo
Esse projeto atende os requisitos do desafio da Keeggo que são:

Documentação de todas APIs:
- https://www.advantageonlineshopping.com/api/docs/
#### Procure por um produto (GET):
 API:
https://www.advantageonlineshopping.com/catalog/api/v1/products/search
- Verifique se a lista exibe somente produtos conforme sua busca.
- Validar o status code da resposta do serviço.
#### Atualize a imagem de um produto (POST):
 API:
https://www.advantageonlineshopping.com/catalog/api/v1/product/image/{userId}/{source}/{color}
- Verifique que o produto foi atualizado corretamente.
- Verifique o id da image nova inserida
- Validar o status code da resposta do serviço.


## Pré-requisitos

 - Postman

## Instalação

Clone este repositório e faça a importação no Postman:

```bash
git clone https://github.com/olucasrangel/keeggo-api.git
cd keeggo-api
```
## Postman

1. Abrir o Postman

Inicie o Postman no seu computador.

2. Realizar importação

	•	No canto superior esquerdo, clique no botão Importar.

	•	A tela de importação será exibida.

    •	Selecione o arquivo baixado no formato ".json" e realize a importação.

## Primeiro desafio: Procure por um produto

```javascript
// Verifica o Status Code como 200
pm.test("Status Code is 200", function () {
    pm.response.to.have.status(200);
});

// Teste para verificar se todos os produtos retornados na resposta correspondem ao termo de busca esperado
pm.test("Todos os produtos correspondem à busca", function () {
    
    // Extrai os dados JSON da resposta da API
    let jsonData = pm.response.json();
    
    // Define o termo de busca esperado (nome do produto que todos os resultados devem corresponder)
    let searchTerm = "Beats Studio 2 Over-Ear Matte Black Headphones";
    
    // Itera sobre cada categoria de produto na resposta
    jsonData.forEach(category => {
        category.products.forEach(product => {
            
            // Verifica se o nome do produto corresponde ao termo de busca esperado
            pm.expect(product.productName).to.eql(searchTerm);
        });
    });
});
```


## Segundo Desafio: Atualizar imagem

1. Selecionar: Atualizar imagem com o método POST.

ATENÇÃO:
Para esse teste é necessário ter uma conta cadastrada e possuir o user id.
Consultar o [Swagger]([https://www.advantageonlineshopping.com/api/docs/)

```javascript
// Verificar se o status code é 200
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Verificar se a resposta contém sucesso
pm.test("Product was updated successfully", function () {
    let jsonData = pm.response.json();
    pm.expect(jsonData.success).to.be.true;
    pm.expect(jsonData.reason).to.eql("Product was updated successful");
});

// Extrair imageId da resposta e armazenar em uma variável
pm.test("Extract imageId", function () {
    let jsonData = pm.response.json();
    pm.environment.set("imageId", jsonData.imageId);
});
```

2. Selecionar: Verificar imagem com o método GET
```javascript
// Obter o imageId armazenado
let expectedImageId = pm.environment.get("imageId");

// Verificar se o status code é 200
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Verificar se o produto contém o imageId esperado
pm.test("Product contains updated imageId", function () {
    let jsonData = pm.response.json();
    let productImages = jsonData.images || [];

    let imageFound = productImages.some(function (image) {
        return image.id === expectedImageId;
    });

    pm.expect(imageFound).to.be.true;
});
```

## License

[MIT](https://choosealicense.com/licenses/mit/)