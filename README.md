Projeto de Análise de Vendas de Varejo com SQL

Visão Geral do Projeto
Título do Projeto: Análise de Vendas de Varejo
Nível: Iniciante
Banco de Dados: p1_retail_db (ou o nome que você utilizou)
Este projeto foi desenhado para demonstrar habilidades e técnicas de SQL tipicamente usadas por analistas de dados para explorar, limpar e analisar dados de vendas de varejo. O projeto envolve a configuração de um banco de dados de vendas, a realização de análise exploratória de dados (EDA) e a resposta a perguntas de negócio específicas através de consultas SQL. Este projeto é ideal para quem está começando sua jornada em análise de dados e quer construir uma base sólida em SQL.

🎯 Objetivos

1.	Configurar um banco de dados de vendas: Criar e popular um banco de dados de varejo com os dados de vendas fornecidos.
2.	Limpeza de Dados: Identificar e remover quaisquer registros com valores nulos ou ausentes.
3.	Análise Exploratória de Dados (EDA): Realizar uma análise exploratória básica para entender o dataset.
4.	Análise de Negócio: Usar SQL para responder a perguntas de negócio específicas e extrair insights dos dados.

________________________________________


🏗️ Estrutura do Projeto

1. Configuração do Banco de Dados
•	Criação do Banco de Dados: O projeto começa criando um banco de dados.
•	Criação da Tabela: Uma tabela chamada retail_sales é criada para armazenar os dados de vendas.

SQL
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE, 
    sale_time TIME,
    customer_id INT,    
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,   
    cogs FLOAT,
    total_sale FLOAT
);

2. Exploração e Limpeza de Dados
•	Contagem de Registros: Determinar o número total de registros.
•	Contagem de Clientes: Descobrir quantos clientes únicos estão no dataset.
•	Contagem de Categorias: Identificar todas as categorias de produtos únicas.
•	Verificação de Valores Nulos: Checar por valores nulos e deletar registros com dados ausentes.
SQL
-- Contagem total
SELECT COUNT(*) FROM retail_sales;

-- Contagem de clientes únicos
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Categorias únicas
SELECT DISTINCT category FROM retail_sales;

-- Encontra valores nulos
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Deleta valores nulos
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;


3. Análise de Dados e Descobertas

As seguintes consultas SQL foram desenvolvidas para responder a perguntas de negócio específicas:


1. Recuperar todas as colunas para vendas feitas em '2022-11-05':
SQL
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

2. Recuperar transações da categoria 'Clothing' com quantidade > 4 em Nov-2022:
SQL
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;

3. Calcular o total de vendas (total_sale) para cada categoria:
SQL
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;

4. Encontrar a idade média dos clientes da categoria 'Beauty':
SQL
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';

5. Encontrar transações onde o total_sale é maior que 1000:
SQL
SELECT * FROM retail_sales
WHERE total_sale > 1000;

6. Encontrar o número total de transações por gênero e categoria:
SQL
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY 
    category,
    gender
ORDER BY 1;

7. Descobrir o mês com a melhor média de venda de cada ano:
SQL
SELECT 
       year,
       month,
    avg_sale
FROM 
(   
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1;

8. Encontrar os 5 principais clientes com base no maior total de vendas:
SQL
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

9. Encontrar o número de clientes únicos por categoria:
SQL
SELECT 
    category,  
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;

10. Criar turnos (Manhã, Tarde, Noite) e contar o número de pedidos em cada:
SQL
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;

________________________________________

📈 Descobertas
•	Demografia dos Clientes: O dataset inclui clientes de várias faixas etárias, com vendas distribuídas por diferentes categorias, como Vestuário (Clothing) e Beleza (Beauty).
•	Transações de Alto Valor: Diversas transações tiveram um valor total de venda superior a 1000, indicando compras premium.
•	Tendências de Vendas: A análise mensal mostra variações nas vendas, ajudando a identificar picos de temporada.
•	Insights de Clientes: A análise identifica os clientes que mais gastam e as categorias de produtos mais populares.

________________________________________

🏁 Conclusão
Este projeto serve como uma introdução abrangente ao SQL para analistas de dados, cobrindo configuração de banco de dados, limpeza de dados, análise exploratória e consultas SQL orientadas a negócios. As descobertas deste projeto podem ajudar a impulsionar decisões de negócios ao entender padrões de vendas, comportamento do cliente e desempenho do produto.

________________________________________

🚀 Como Usar
1.	Clone o Repositório: Clone este repositório do GitHub.
2.	Configure o Banco de Dados: Execute os scripts SQL fornecidos para criar e popular o banco de dados.
3.	Execute as Consultas: Use as consultas SQL deste README para realizar sua análise.
4.	Explore e Modifique: Sinta-se à vontade para modificar as consultas para explorar diferentes aspectos do dataset.

________________________________________

👨‍💻 Autor - Vinicius Stoc
Este projeto faz parte do meu portfólio, demonstrando as habilidades de SQL essenciais para funções de analista de dados. Se você tiver quaisquer perguntas, feedback, ou quiser colaborar, sinta-se à vontade para entrar em contato!
