# Rossmann Store Sales Prediction

![]( /img/banner_rossmann_project.png )

# 1. Problema de Negócio

A Rossmann é uma das maiores redes de farmácia da Europa, com mais de 56 mil funcionários e 4 mil lojas espalhadas por países como Alemanha (sede da empresa), Polônia, República Tcheca e Hungria. Com esta grande quantidade e variedade de lojas e diante da necessidade de definir orçamento para reformas das lojas, em uma das reuniões mensais com todos os Gerentes, o CFO da empresa pediu para que eles trouxessem uma previsão de vendas das próximas 6 semanas de cada uma de suas lojas. Com esta previsão, o CFO poderá saber o quanto investir em reformas. Porém a previsão de vendas atual apresenta muitas divergências e então foi solicitado ao time de dados que criassem uma solução para este problema do time de negócios.

# 2. Premissas assumidas

As variáveis originais do conjunto de dados:

- **Id** - um Id que representa (Loja, Data) dentro do conjunto de teste
- **Store** - Id único para cada loja
- **Sales** - o volume de negócios em um determinado dia
- **Customers** - o número de clientes em um determinado dia
- **Open** - um indicador para saber se a loja estava aberta: 0 = fechada, 1 = aberta
- **StateHoliday** - indica um feriado estadual. Normalmente todas as lojas, com poucas exceções, fecham nos feriados estaduais. Observe que todas as escolas fecham nos feriados e finais de semana. a = feriado, b = feriado da Páscoa, c = Natal, 0 = Nenhum
- **SchoolHoliday** - indica se (Loja, Data) foi afetado pelo fechamento de escolas públicas
- **StoreType** - diferença entre 4 modelos de loja diferentes: a, b, c, d
- **Assortment** - descreve um nível de sortimento: a = básico, b = extra, c = estendido
- **CompetitionDistance** - distância em metros até a loja concorrente mais próxima
- **CompetitionOpenSince[Month/Year]** - o ano e mês aproximados em que o concorrente mais próximo foi aberto
- **Promo** - indica se uma loja está fazendo uma promoção naquele dia
- **Promo2** - Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = a loja não está participando, 1 = a loja está participando
- **Promo2Since[Year/Week]** - descreve o ano e a semana em que a loja começou a participar da Promo2
- **PromoInterval** - descreve os intervalos consecutivos de início da Promo2, nomeando os meses em que a promoção é iniciada novamente. Por exemplo. "Fev, maio, agosto, novembro" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para aquela loja

# 3. Planejamento da Solução

A estratégia e a execução do planejamento deste projeto seguiram a metodologia CRISP-DM (Cross Industry Standard Process for Data Mining), que é uma metodologia cíclica e flexível de desenvolvimento voltada para a resolução de problemas de empresa o mais rápido possível entregando valor para o time de negócio. O modelo foi dividido em 10 partes:

1. Entendimento do negócio e problemas e serem resolvidos 
2. Coleta dos dados 
3. Descrição dos Dados, Elaboração de Variáveis derivadas e Filtragem de Dados
4. Análise Exploratória dos Dados (EDA)
5. Preparação dos dados para os algoritmos de Machine Learning
6. Seleção de Variáveis
7. Treinamento de algoritmos de Machine Learning
8. Avaliação do algoritmo e hiperparâmetros 
9. Tradução do erro em métricas de negócio
10. Deploy do modelo em produção

# 4. Os principais insights do negócio

Durante a Análise Exploratória de Dados (EDA), foram levantadas 11 hipóteses:

1. Lojas com maior sortimentos deveriam vender mais.
2. Lojas com competidores mais próximos deveriam vender menos.
3. Lojas com competidores há mais tempo deveriam vender mais.
4. Lojas com promoções ativas por mais tempo deveriam vender mais.
5. Lojas com mais promoções consecutivas deveriam vender mais.
6. Lojas abertas durante o feriado de Natal deveriam vender mais.
7. Lojas deveriam vender mais ao longo dos anos.
8. Lojas deveriam vender mais no segundo semestre do ano.
9. Lojas deveriam vender mais depois do dia 10 de cada mês.
10. Lojas deveriam vender menos aos finais de semana.
11. Lojas deveriam vender menos durante os feriados escolares.

Destas, as que foram consideradas mais importantes são:

## 1. Lojas com mais sortimentos deveriam vender mais.

![]( /img/H1_lojas_com_mais_sortimentos_vendem_mais.png )

**VERDADEIRA**: Pelo gráfico podemos ver que, em média, quanto maior a variedade de produtos na loja (um maior sortimento), mais elas vendem.

## 4. Lojas com promoções ativas por mais tempo deveriam vender mais. 

![]( /img/H4_lojas_com_promos_ativas.png )
**FALSA**: Lojas com promoções ativas por mais tempo vendem menos depois de um certo período de promoção.

## 6. Lojas abertas durante o feriado de Natal deveriam vender mais. 

![]( /img/H6_lojas_nos_feriados_vendem_mais.png )

**VERDADEIRA**: Em média, lojas abertas durante o feriado de Natal vendem mais do que nos demais feriados, a exceção nestes dados é a páscoa de 2013.

## 11. Lojas deveriam vender menos durante os feriados escolares. 

![]( /img/H11_lojas_nos_feriados_escolares_vendem_menos.png )

**FALSA**: Em média, as lojas vendem mais durantes os feriados escolares, exceto no mês de Dezembro.

# 5. Modelos de Machine Learning

Para analisar a previsão de vendas foram usados os seguintes modelos de Machine Learning:

- *Average Model*
- *Linear Regression*
- *Linear Regression Regularized - Lasso*
- *Random Forest Regressor*
- *XGBoost Regressor*

# 6. Performance do Modelo de Machine Learning

Com a análise dos erros (MAE, MAPE, RMSE), optei por prosseguir com o *XGBoost Regressor*, pois embora o *Random Forest* tenha desempenhado melhor inicialmente, ele tem um tamanho muito maior do que o *XGBoost Regressor*. 

## 6.1. *Performance Singular*
| Model Name	| MAE	| MAPE |	RMSE |
| --- | --- | --- | --- |
| Random Forest Regressor |	679.598831 | 0.099913 |	1011.119437 |
| Average Model | 1354.800353 |	0.455051 | 1835.135542 |
| Linear Regression | 1867.089774 |	0.292694 | 2671.049215 |
| Linear Regression Regularized - Lasso | 1891.704881 |	0.289106 | 2744.451737 |
| XGBoost Regressor | 6683.544086 |	0.949457 | 7330.812159 |

## 6.2. *Performance Real - Cross Validation*

Após a aplicação do Cross-Validation, podemos observar na tabela abaixo que os erros ficaram próximos aos erros do dataset de treino acima, mostrando que o modelo está com uma boa capacidade de generalização.

| Model Name	| MAE CV	| MAPE CV |	RMSE CV |
| --- | --- | --- | --- |
| Linear Regression | 2081.73 +/- 295.63 | 0.3 +/- 0.02 | 2952.52 +/- 468.37 |
| Linear Regression Regularized - Lasso | 2116.38 +/- 341.5 | 0.29 +/- 0.01 | 3057.75 +/- 504.26 |
| Random Forest Regressor | 836.61 +/- 217.1 | 0.12 +/- 0.02 | 1254.3 +/- 316.17 |
| XGBoost Regressor | 7049.17 +/- 588.63 | 0.95 +/- 0.0 | 7715.17 +/- 689.51 |


## 6.3. *Modelo Final*

A partir dos ajustes dos hiperparâmetros, o modelo final performou desta forma:

| Model Name | MAE | MAPE | RMSE |
| --- | --- | --- | --- |
| XGBoost Regressor | 770.209978 | 0.115623 | 1108.062869 |


# 7. Resultados de Negócio

Com as previsões feitas pelo modelo, podemos traduzir e interpretar o erro encontrado em valores estimados para o negócio.
Na tabela abaixo trazemos as previsões feitas pelo modelo considerando o melhor e o pior cenário em valores financeiros.

| Scenario | Values |
| --- | --- |
| predictions | R$ 285,817,920.00 |
| worst_scenario | R$ 284,955,855.73 |
| best_scenario | R$ 286,679,971.81 |

# 8. Conclusões

Após o envio do modelo para produção, o produto final foi um Bot no aplicativo de mensagens Telegram que pode ser acessado pelo CFO de qualquer dispositivo com o Telegram instalado e com acesso à internet. Desta forma, o CFO tem de forma rápida e prática a possibilidade de definição do orçamento para a reforma de cada uma das lojas através do aplicativo, para além dos insights gerados pelas hipóteses que foram levantadas que podem ser utilizados para aumentar as vendas.

O Bot no Telegram busca informações em tempo real da previsão de vendas das próximas 6 semanas para a loja selecionada, bastando apenas informar o ID da Loja:

![]( /img/bot_telegram.jpg )

# 9. Próximos passos

Após a disponibilização do Bot através do aplicativo, alguns passos devem ser dados pensando na melhora contínua do modelo:
- Workshop do Modelo para os usuários
- Coletar Feedbacks sobre a usabilidade

Após a colocação do modelo em produção, a apresentação do modelo e a coleta de feedbacks, poderiam ser realizados mais ciclos do CRISP focados em aspectos como:
- Testar outros modelos de Machine Learning
- Melhorar a performance do algoritmo
- Mais opções de consulta no Bot do Telegram.
