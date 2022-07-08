




# <p align="center"> ROSSMANN STORE SALES PREDICTION </p> 


<p align="center"><img width=500 src="https://github.com/andersonftdj/DataScienceEmProducao/blob/main/img/RossmannImg.jpg"></p>



A Rossmann é uma rede de drogaria com mais de 3000 lojas que operam em 7 diferentes países da Europa.
Após uma reunião com vários gerentes regionais, o CFO(Diretor Financeiro) da Rossmann solicitou uma previsão de vendas para as próximas 6 semanas. Seu objetivo é saber a previsão de faturamento dessas lojas, individualmente, para direcionar uma verba específica para reformar de apenas algumas delas.

## 1. Entendimento do negócio


Para uma primeira etapa de entendimento do problema é necessário entender alguns fatorem que originaram a requisição da previsão de vendas. Os principais pontos identificados e respondidos foram os seguintes:
 - Motivação e contexto da requisição: O CFO requisitou uma previsão de vendas durante uma reunião mensal sobre o resultado de suas lojas.

 - Causa raiz do problema: Há uma dificuldade em determinar o valor de investimentos para a reforma de cada loja.

 - Quem é o stakeholder do problema: O stakeholder deste problema é o CFO da Rossmann.

 - Formato da solução: 
 
    - Granularidade: Será realizado uma previsão de vendas por dia e por loja para os próximos 42 dias (6 semanas).
    
    - Tipo de problema: Temos um problema de **previsão de vendas**.
    
    - Potênciais métodos: Séries temporais, Redes neurais ou **Regressão**.
    
    - Formato da Entrega: Será entregue um **bot no telegram** que ao receber o ID da loja será retornado o valor de faturamento previsto para as próximas 6 semanas.

    

Com os dados obtidos do [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales/overview), temos as seguintes features disponíveis:
Feature | Description
--|--
| Store |	Identificador de cada loja
| DayOfWeek | Dias da semana ( 1 =  Segunda, 7 = Domingo)
| Date |	Data de cada venda
| Sales |	Valor financeiro negociado no dia (Esta é a variável alvo)
| Customers |	O número de clientes em determinado dia
| Open |	Um indicador se a loja está aberta (1) ou fechada (0)
| StateHoliday |	Indica um feriado estadual. Normalmente todas as lojas, com algumas excessões, estão fechadas em  feriados estaduais. Obs: Todas as escolas são fechadas em feriados públicos e aos finais de semana. a = Feriado público, b = Páscoa, c = Natal, 0 = Nenhum
| SchoolHoliday |	Indica se a loja e data (Store, Date) foi afetada pelo  fechamento de escolas públicas
| StoreType |	Tipo de loja, diferenciado entre 4 modelos de lojas diferentes: a, b, c, d
| Assortment |	Descreve o nível de sortimento de uma loja: a = basic, b = extra, c = extended
| CompetitionDistance |	Distância em metros da loja concorrente mais próxima
| CompetitionOpenSinceMonth |	Fornece o mês aproximado em que o competidor mais próximo abriu
| CompetitionOpenSinceYear |	Fornece o ano aproximado em que o competidor mais próximo abriu
| Promo |	Indica se a loja está com alguma promoção ativa no dia
| Promo2 |	Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = Loja não está participando, 1 = Loja está participando
| Promo2Since[Year/Week] |	Descreve o ano e a semana do calendário quando a loja começou a participar da promo2
| PromoInterval |	Descreve os intervalos consecutivos em que a Promo2 é iniciada, nomeando os meses em que a promoção é iniciada novamente. Por exemplo. "Fevereiro, maio, agosto, novembro" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para essa loja


Foram adotadas as seguintes premissas:
 - Lojas com faturamento igual a 0 foram descartadas.
 - Dias em que as lojas estavam fechadas foram descartados.
 - Lojas sem competidores próximos foram alterados para 200.000 m de distância.



## 2. Planejamento da Solução

### Produto Final
Será entregue um bot no telegram que ao enviar o id da loja será retornado o valor de faturamento previsto para as próximas 6 semanas.

### Ferramentas utilizadas.

 - Jupyter notebook
 - Python 3.8.13
 - Heroku
 - Frontend API: Telegram Bot
 
### Processo
Foi utilizado a metodologia CRISP-DM, que trabalha através do desenvolvimento ciclico, com o objetivo de fazer *entregas de alto valor em um curto espaço de tempo*, com a possibilidade de poder incrementar a qualidade e o valor da solução em futuros ciclos. 
Até o presente momento esse projeto contempla apenas a primera passagem desse ciclo.

<p align="center"><img src="https://miro.medium.com/max/494/0*tA5OjppLK627FfFo"></p>

Os passos utilizados foram os seguintes:
( os códigos de cada passo a baixo estão disponíveis no jupyter notebook disponíveis acima do read.me)
 
 #### 1. Descrição dos Dados
 Nesta etapa a preocupação foi ter um entendimento geral dos dados, padronizar o nome das colunas, saber os tipos de dados disponíveis e realizar as adequações necessárias, além de verificar e preencher os valores ausentes. Por fim verificar algumas estatísticas básicas sobre os campos numéricos e categóricos com o objetivo de ter uma consciência situacional sobre o dataset.
 
#### 2. Feature Engineering - Engenharia de Features
Uma vez entendido os dados, passamos para o levantamento das hipóteses sobre os dados. Primeiro foi desenvolvido um mapa mental detalhado com base nos agentes envolvidos e seus atributos (imagem a baixo), em seguida foram desenvolvidas novas features, com o objetivo de encontrar novas características que melhor descrevam o fenômeno que estamos tentando modelar.

<p align="center"><img width=900 src="https://github.com/andersonftdj/DataScienceEmProducao/blob/main/img/mindMapHypothesis.png"></p>


#### 3. Filtragem das variáveis
Foram Filtrados as linhas e os atributos que não ajudam na modelagem do fenômeno ou que estão fora do escopo do problema.

#### 4. Análise Exploratória de Dados
Com o fim de buscar insights e entender o impacto de cada variável, Nesta etapa, trabalhamos em 3 tipos de análises:

 - Análise Univariada: Análise para entender o comportamento isolado das variáveis.
 
 - Análise Bivariada: Análise focada em entender o impacto das variáveis independentes com relação à variável dependente ('sales') e também validar as hipóteses levantadas na etapa de Feature Engineering.

- Análise Multivariada: Análise com o foco em entender a correlação entre todas as variáveis.

#### 5. Preparação dos Dados ou pré-processamento
Nesta fase o objetivo é transformar a escala dos dados sem causar perca de informações, para que eles estejam otimizados para a criação do modelo de Machine Learning.

Foram realizados Transformações nas variáveis numéricas e categóricas:

- Numérica
    
    - Transformação: Aplicação da transformação logarítmica na variável resposta, para trazer ela mais próxima a uma distribuição Gaussiana.
    
    - Rescaling: MinMax Scaler e Robust Scaler foram aplicadas em variáveis temporais não cíclicas (como ano) e na variável numérica CompetitionDistance
    
    - Natureza: Foram extraídos seno e cosseno das variáveis temporais cíclicas (dias da semana, mês), para que não se perdesse esse atributo valioso para o modelo.
   
- Categórica
    
    - One Hot Encoding: Cada nível de variável é transformado em uma coluna;
    
    - Label encoding: Troca aleatoriamente o nível da variável por números, sem ordem de importância;
    
    - Ordinal encoding: Troca o nível das variáveis por valores numéricos respeitando uma ordem hierárquica. 

#### 6. Seleção de atributos
Para selecionar as melhores features para o modelo foi utilizado o Boruta, um algoritmo que encontra todos os atributos que mais carregam informação relevantes para o modelo. Este algoritmo utiliza o método de seleção por subset (Wrapper Methods), que a cada iteração adiciona uma feature, treina o modelo e verifica se a precisão aumenta ou diminui, então só é selecionada a feature se há um aumento na precisão.


#### 7. Algoritmos de Macine Learning
Nesta etapa foram trabalhados 5 modelos de Machine Learning para comparar seus resultados. O primeiro foi um modelo de *Média*, escolhido como baseline por causa da sua simplicidade e eficiência. Também foi escolhido a *Regressão Linear* e a *Regressão Linear Regularizada* para podermos entender se era um fenômeno de baixa complexidade ou se necessitava de uma maior sofisticação. Por fim também foi treinado um modelo de *Random Forest* e um *XGBoster Regressor*, que são modelos mais robustos e costumam desempenhar bem em fenômenos mais complexos.

#### 7.1 Cross-Validation
A técnica de Validação Cruzada, através da reamostragem, utiliza diferentes partes do dataset como conjunto de treino e teste em várias iterações para *avaliar o desempenho real* do modelo. Neste caso, aplicado a um problema temporal, as amostras sempre seguem a ordem cronológica para não perder a característica fundamental do problema, o tempo.   

Esses foram os resultados usando 5 iterações no Cross Validation:
  
| Model Name | MAE CV | MAPE CV | RMSE CV |
|-----------|---------|-----------|---------|
| Linear Regression       |	2080.69 +/- 293.36 |	0.3 +/- 0.02  |	2950.49 +/- 464.71 |
| Lasso                   |	2116.38 +/- 341.5  |	0.29 +/- 0.01 |	3057.75 +/- 504.26 |
| Random Forest Regressor |	840.13 +/- 219.44  |	0.12 +/- 0.02 |	1259.12 +/- 320.91 |
| XGBoost Regressor       |	1022.52 +/- 148.47 |	0.14 +/- 0.01 |	1480.09 +/- 206.84 |

Apesar da superioridade da Random Forest regressor fornecendo um menor valor de erro, o modelo escolhido para as próximas etapas foi a XGBost Regressor, por dois motivos. Primeiro pelo fato da diferença de erros em ambas ser relativamente baixo, e segundo pelo fato de que modelos do tipo Random Forest tem um tamanho final muito grande, o que ocasionaria um alto custo para empresa mantê-lo em produção.

#### 8. Ajuste fino de Hiperparâmetros.
Nesta etapa o objetivo é encontrar o conjunto de parâmetros que maximizam o aprendizado do modelo, para isso usamos a técnica de _Random Search_. Nesta técnica escolhemos um conjunto de valores para cada parâmetro que queremos testar, selecionamos aleatoriamente os valores dos parâmetros, treinamos o modelo, e extraímos sua precisão. Isso é realizado diversas vezes e o conjunto de valores que trouxer a melhor precisão é usado para as próximas etapas. 

O modelo com o melhor desempenho retornou os seguintes resultados:

|Model Name|	MAE|	MAPE|	RMSE|
|--|--|--|--|
|	XGBoost Regressor|	651.048948|	0.094386|	946.094455|



#### 9. Interpretação e Traduação do erro

Com o modelo já treinado e com todas as métricas disponíveis, podemos sair um pouco do universo técnico e demonstrar quão bom é o nosso modelo com resultados financeiros de sua aplicação.


Como retorno da previsão temos o número da loja, o valor previsto de faturamento, o valor faturado no pior cenário, o valor faturado para o melhor cenário, os erros em valores absolutos e o percentual deles.

store|	predictions|	worst_scenario|	best_scenario|	MAE|	MAPE|
--|--|--|--|--|--|
782|	211271.76|	210603.91|	211939.61|	667.85|	0.19%

Com a soma de todas as lojas disponíveis temos o resultado financeiro total que este modelo retornará.

Scenario|	Values
--|--
predictions | R$285,130,112.00
worst_scenario	| R$284,399,916.22
best_scenario| 	R$285,860,331.71


Como podemos ver a seguir, as previsões do modelo ficaram muito perto dos dados reais, o que mostra o quão preciso nosso modelo foi.

<p align="center"><img width=800 src="https://github.com/andersonftdj/DataScienceEmProducao/blob/main/img/predicoes.png"></p>



Apesar dos ótimos resultados, sempre é possível melhorar. Como próximas sprints do CRISP-DM poderia ser buscado analisar caso a caso, ou talvez um grupo menor de lojas, para melhorar o desempenho. Ou até mesmo tentar outros tipos de modelos. 

#### 10. Deploy
Seguindo o projeto de fornecer essas previsões de uma maneirá fácil e rápida para os tomadores de decisão da Rossmann, foi feito um bot no telegram para essas consultas. Uma vez que o ID da loja seja fornecido via chat para o bot, a API do telegram entregará esses valores para o handler(arquivo principal que gerencia o funcionamento do backend do bot), e este direcionará para o modelo, que retornará os valores da predição.

<p align="center"><img width=300 src="https://github.com/andersonftdj/DataScienceEmProducao/blob/main/img/printBotTelegram.jpg"></p>



Para acessar o bot pesquise no seu Telegram @Rossmann_user_bot [(ou através nesse link)](https://t.me/rossmann_user_bot) e clique em "Start"
 após isso basta enviar o número da loja que deseja saber a previsão e o modelo retornará com o valor previsto.
 Tenha um pouco de paciência, pois a primeira requisição pode demorar alguns minutos para ser respondida.
 

 
Com isso se encerra um ciclo de projeto de ponta a ponta, passando desde o entendimento de negócio até o deploy. Em síntese, desde a concepção da ideia, passando pelo planejamento da solução até uma entrega que agregue valor!

## 3. Próximos passos:
 - Como o número de pessoas afeta diretamente o faturamento do dia, seria interessante realizar uma previsão da quantidade de clientes que terá no dia. Agregaria como uma nova feature para este projeto.
 - Aprimorar a quantidade de informações retornadas pelo bot.
 - Melhorar a performance do Modelo atual testando técnicas mais sofisticadas de fine tuning.
 - Testar mais tipos de modelos de Machine Learning.





> Este é um projeto end-to-end de previsão de vendas do curso Data Science Em Produção da Comunidade de DS, ministrado pelo mestre Meigarom Lopes
