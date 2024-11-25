# DBA Challenge 20240802


## Introdução

Nesse desafio trabalharemos utilizando a base de dados da empresa Bike Stores Inc com o objetivo de obter métricas relevantes para equipe de Marketing e Comercial.

Com isso, teremos que trabalhar com várioas consultas utilizando conceitos como `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `GROUP BY` e `COUNT`.

### Antes de começar
 
- O projeto deve utilizar a Linguagem específica na avaliação. Por exempo: SQL, T-SQL, PL/SQL e PSQL;
- Considere como deadline da avaliação a partir do início do teste. Caso tenha sido convidado a realizar o teste e não seja possível concluir dentro deste período, avise a pessoa que o convidou para receber instruções sobre o que fazer.
- Documentar todo o processo de investigação para o desenvolvimento da atividade (README.md no seu repositório); os resultados destas tarefas são tão importantes do que o seu processo de pensamento e decisões à medida que as completa, por isso tente documentar e apresentar os seus hipóteses e decisões na medida do possível.
 
 

## O projeto

- Criar as consultas utilizando a linguagem escolhida;
- Entregar o código gerado do Teste.

### Modelo de Dados:

Para entender o modelo, revisar o diagrama a seguir:

![<img src="samples/model.png" height="500" alt="Modelo" title="Modelo"/>](samples/model.png)


## Selects

Construir as seguintes consultas:

- Listar todos Clientes que não tenham realizado uma compra;
- Listar os Produtos que não tenham sido comprados
- Listar os Produtos sem Estoque;
- Agrupar a quantidade de vendas que uma determinada Marca por Loja. 
- Listar os Funcionarios que não estejam relacionados a um Pedido.


## Readme do Repositório

- Deve conter o título do projeto
- Uma descrição sobre o projeto em frase
- Deve conter uma lista com linguagem, framework e/ou tecnologias usadas
- Como instalar e usar o projeto (instruções)
- Não esqueça o [.gitignore](https://www.toptal.com/developers/gitignore)
- Se está usando github pessoal, referencie que é um challenge by coodesh:  

>  This is a challenge by [Coodesh](https://coodesh.com/)

## Finalização e Instruções para a Apresentação

1. Adicione o link do repositório com a sua solução no teste
2. Verifique se o Readme está bom e faça o commit final em seu repositório;
3. Envie e aguarde as instruções para seguir. Sucesso e boa sorte. =)


RESPONSE
Name: Alessandro Silvestre
Phone:(11) 99904-0202
eMail: devops.asilvestre@gmail.com



-- 01. Listar todos Clientes que não tenham realizado uma compra;
SELECT cus.customer_id, cus.first_name, cus.email, cus.phone
FROM dbo.customers cus WITH (NOLOCK) 
JOIN dbo.orders    ord WITH (NOLOCK) on cus.customer_id != ord.customer_id
order by cus.first_name

-- 02. Listar os Produtos que não tenham sido comprados
SELECT prd.product_id, prd.product_name, cat.category_name
  FROM dbo.products    prd WITH (NOLOCK) 
  JOIN dbo.categories  cat WITH (NOLOCK) ON prd.category_id = cat.category_id
  JOIN dbo.order_items ite WITH (NOLOCK) on prd.product_id != ite.product_id
  order by prd.product_name

-- 03. Listar os Produtos sem Estoque;
SELECT prd.product_id, prd.product_name, stk.quantity
  FROM dbo.products    prd WITH (NOLOCK) 
  JOIN dbo.stocks      stk WITH (NOLOCK) on prd.product_id = stk.product_id 
  WHERE stk.quantity <= 0
  ORDER BY stk.quantity

-- 04. Agrupar a quantidade de vendas que uma determinada Marca por Loja.
SELECT ord.store_id, sto.store_name, ite.product_id, prd.product_name, cat.category_name,  sum(ite.quantity) as total_quant
FROM dbo.orders      ord WITH (NOLOCK) 
JOIN dbo.order_items ite WITH (NOLOCK) on ite.order_id		= ord.order_id
JOIN dbo.products    prd WITH (NOLOCK) on prd.product_id	= ite.product_id 
JOIN dbo.categories  cat WITH (NOLOCK) on cat.category_id   = prd.category_id
join dbo.stores      sto WITH (NOLOCK) on sto.store_id      = ord.store_id
WHERE sto.store_name LIKE '%Customer Name%'
GROUP BY 
ord.store_id, sto.store_name, ite.product_id, prd.product_name, cat.category_name
order by sto.store_name

-- 05. Listar os Funcionarios que não estejam relacionados a um Pedido.
Select 
sta.staff_id
,sta.first_name
,sta.email
,sta.phone
FROM dbo.staffs sta WITH (NOLOCK) where sta.staff_id not in (select staff_id FROM dbo.Orders)
order by sta.first_name
