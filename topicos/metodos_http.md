# Métodos HTTP

>  entendendo melhor os métodos HTTP

## O que são métodos HTTP?

Métodos **HTTP (HyperText Transfer Protocol)** são verbos utilizados para indicar a ação que deve ser executada em um determinado recurso da web. Cada método tem um propósito específico e é utilizado em situações diferentes.

## 🚩 Tipos de Metodos HTTP

### Métodos para obter os dados:

- 🔵 GET

usamos o método GET para obter dados atraves do protocolo HTTP.

### Metodos para criação:

- 🔵 POST

usamos o metodo POST para criar novos dados em nossa API.

### Métodos para edição:

- 🔵 PUT

Esse método é usado para quando você quer editar todos os topicos do seu dado.

Vamos ver um exemplo do uso do método **PUT**, dado o seguinte JSON:

```txt
{
    {
        'id': 1,
        'nome': 'carlos',
        'idade': 19,
    }
}
```

- 🔵 PATH

Esse metodo é usado para editar um subcojunto presente no seu dados.

Vamos ver um exemplo do uso do método **PATH**, dado o seguinte JSON:

```txt
{
    {
        'id': 1,
        'nome': 'carlos',
        'idade': 19,
    }
}
```

### Métodos para deletar uma instância

- 🔵 DELETE

O método DELETE é usado para deletar uma instância em nossa API.
