
# Cardiovascular Disease Detection

- O caso em questão foi proposto pela Comunidade DS como aprendizado.
- Os dados foram obtidos da plataforma do Kaggle 

## Business Problem

A Cardio Catch Disease é uma empresa especializada em detecção de doenças cardíacas em estágio inicial, tendo seu modelo de negócio como um serviço onde oferece o diagnóstico precoce de uma doença por um preço, baseando em sua precisão atual.

Atualmente, o diagnóstico de uma doença cardiovascular é feita manualmente por uma equipe de especialistas. A precisão atual do diagnóstico varia entre 55% e 65%, devido a complexidade do diagnóstico e também da fadiga da equipe que se revezam em turnos para minimizar os riscos. O custo fixo de toda essa operação (incluindo aparelhos e mão de obra) é de R$ 1000.

O preço do diagnóstico a ser pago pelo paciente varia de acordo com a precisão obtida pelo time de especialista, onde o paciente paga R$ 500 a cada 5% de acurácia acima de 50%.

Por exemplo :

    - Para uma precisão de 55%, o diagnóstico custa R$ 500
    - Para uma precisão de 60%, o diagnóstico custa R$ 1000
    - Se a precisão for de 50% ou menor, o cliente não paga pelo diagnóstico.

Nota-se que esse modelo de negócio gera uma incerteza de faturamento, onde não é possível assegurar um lucro para a empresa, tendo um fluxo de caixa totalmente imprevisível.

Como um cientista de dados, o objetivo era de criar um modelo que permitisse obter uma melhor precisão e com menor variação para garantir uma operação previsível e rentável para a empresa.

Dito isso, estabeleci duas métricas a serem obtidas pelo modelo para estabelecer a solução do caso.

        1. Qual a acurácia e precisão desse modelo
        2. Qual será o lucro da empresa com o uso dessa nova ferramenta.

## Solution Strategy

Buscar entender mais sobre o negócio e sobre as informações disponíveis sobre os pacientes.

Usar o método CRISP-DM para um melhor entendimento do negócio e criação de um modelo de Machine Learning que melhor se adeque para a solução do problema. No caso, optei por um modelo de classificação que otimiza a identificação de doença cardíaca nos pacientes através dos dados coletados.

O método em questão consiste em um sistema cíclico de solução de problemas envolvendo a Ciência de Dados. Nesse sistema, em alguns passos, podemos agregar valor à solução de uma forma ágil, permitindo ajustes e melhorias posteriores.

O sistema passa pelos passos de:  Data Description, Feature Engineering, Exploratory Data Analysis (EDA), Data Preparation, Feature Selection, Machine Learning Modelling, Hyperparameter Fine Tunning, Convert Model Performance to Business Values.

Ao finalizar, mensurar o ganho da operação com a implementação da solução proposta e identificar possíveis melhorias para o negócio com o auxílio dos insights obtidos.

## Business Assumptions 

Os dados fornecidos nesse case são referentes à base de clientes da empresa. Consistem em sua maioria em informações sobre medidas e hábitos de vida.

Age (idade)| Objective Feature | age | int (days)

Height (altura)| Objective Feature | height | int (cm) 

Weight (peso)| Objective Feature | weight | float (kg) 

Gender (gênero)| Objective Feature | gender | categorical code | 1: female, 2: male

Systolic blood pressure (pressão sanguínea sistólica)| Examination Feature | ap_hi | int 

Diastolic blood pressure | Examination Feature | ap_lo | int 

Cholesterol | Examination Feature | cholesterol | 1: normal, 2: above normal, 3: well above normal

Glucose | Examination Feature | gluc | 1: normal, 2: above normal, 3: well above normal 

Smoking | Subjective Feature | smoke | binary | 0: No, 1: Yes

Alcohol intake | Subjective Feature | alco | binary | 0: No, 1: Yes

Physical activity | Subjective Feature | active | binary | 0: No, 1: Yes

Presence or absence of cardiovascular disease | Target Variable | cardio | binary | 0: No, 1: Yes

### Tratativa de outliers e dados não condizentes com a literatura:

Após vários testes, notou-se um padrão de erro de preenchimento em alguns dados da base referentes às pressões sistólicas e diastólicas que precisaram ser corrigidos para a solução do caso. Caso optasse por excluir esses dados, parte das demais informações seriam perdidas.

## Insights

Uso de mapa mental para auxiliar na Análise Exploratória de Dados.

<div align="center">
<img src="https://imgur.com/a/cyiCYgS" />
</div><br>


Com a Análise Exploratória de Dados, foi possível validar algumas hipóteses, que embora de conhecimento / senso comum, auxiliam no entendimento do negócio e na validação das informações coletadas. Os principais insights obtidos foram:

### Hipótese 1 - Pacientes com maior IMC (Índice de Massa Corporal) apresentam mais doença cardíaca : 

<strong><span style="color: red;">Verdadeiro</span></strong>


Após o cálculo de IMC baseado nas informações coletadas, foi possível observar que quanto mais acima do IMC ideal, maiores as chances do paciente apresentar doença cardíaca.

<div align="center">
<img src="https://imgur.com/a/1bN7kXH" />
</div><br>

### Hipótese 2 - Pacientes com pressão elevada apresentam mais doença cardíaca : 

<strong><span style="color: red;">Verdadeiro</span></strong>

<div align="center">
<img src="https://imgur.com/6ztkewq" />
</div><br>

### Hipótese 3 - Pacientes que não praticam exercícios físicos apresentam mais doença cardíaca : 

<strong><span style="color: red;">Verdadeiro</span></strong>

<div align="center">
<img src="https://imgur.com/UwUWY46" />
</div><br>

## Machine Learning Modelling & Performance

<div align="center">
<img src="https://imgur.com/0QXMccY" />
</div><br>

<div align="center">
<img src="https://imgur.com/a/C6YjeaH" />
</div><br>


- Nota-se que tanto o XGBoost quanto o LightGBM apresentaram bons resultados, tanto nas métricas quanto na curva ROC que demonstra quão bem o modelo performa na distinção entre as classes positiva e negativa, ou seja, o quão bem ele consegue classificar de forma correta.


- Apesar do caso revelar que apenas a métrica de precisão é considerada para a precificação do serviço, deve-se levar em conta outras métricas, como a recall que mede a capacidade do teste em identificar corretamente os casos positivos da doença. Um modelo com recall baixo, acaba por não identificar um diagnóstico com mais frequência. Tal fato poderia comprometer a credibilidade da empresa e por isso deve ser considerado. Dessa maneira, optei por usar o F1 Score como referencial para a escolha do modelo, uma vez que representa o equilíbrio entre as duas métricas (precisão e recall).


- Com essa abordagem, foi possível filtrar quais modelos estariam aptos a serem utilizados, ficando entre o XGBoost e o LightGBM. É possível perceber também que o modelo de LightGBM performa quase igual o XGBoost, dado que ambos são implementações de gradient boosting, técnica de aprendizado que se utiliza da combinação de árvores de decisão fracas para melhoria do modelo. Optei por usar o LightGBM dado à sua velocidade de execução, que traz agilidade ao processo, gerando valor em um menor período de tempo.





|                   | Accuracy   | Precision    | Recall     | F1 Score         |
|:------------------|----------------:|-------------:|-----------:|-----------------:|
|LightGBM  | 73.68% +- 0.28% |75.6% +- 0.35%| 68.81% +- 0.76%| 72.59% +- 0.39%

## Business Results

Com a precisão obtida pelo modelo e considerando a precificação do negócio atual, temos que:

   | Precisão     | Valor Cobrado     | Custo Fixo     | Lucro/diagnóstico|
   |:-------------|:------------------|:---------------|:-----------------|
   |     75%      |    R\$2500\.00    |   R\$1000\.00  |   R\$1500\.00    |


**Como não há uma estimativa de diagnósticos mensais, toma-se como base o número de pacientes da base de dados e a divisão pelo número de anos que, por lei, as instituições de saúde devem armazenar cópias de exames, que seriam 20 anos. Dessa forma assumi que houveram 3500 diagnósticos ao ano, ou seja, cerca de 292 diagnósticos/mês.**
       


| Precisão     | Valor Cobrado     | Custo Fixo     | Lucro/diagnóstico|  Número de diagnósticos/mês   |   Lucro mensal   |
|:-------------|:------------------|:---------------|:-----------------|:------------------------------|:-----------------|
|     75%      |    R\$2\.500\,00    |   R\$1\.000\,00  |   R\$1\.500\,00    |           292 (premissa)      |    R\$438\.000\,00

## Conclusion

Utilizando os dados que foram fornecidos e as informações obtidas através de pesquisas, partimos de um modelo incerto e que colocava em risco a saúde da operação e criamos um modelo estável e lucrativo para a empresa, estimando um ganho mensal de **R\$ 438.000**

## Recomendations

1. Dado que parte da dificuldade do caso foi interpretar alguns valores e entender que houveram casos de typos, se faz necessário a checagem da cadeia de dados, garantindo que os valores sejam corretamente preenchidos

2. Adicionar coletas de outras variáveis relacionadas à saúde, como por exemplo se há histórico na família, para melhorar ainda mais a precisão do modelo futuramente
 
## Next Steps

- Colocar o modelo em produção em nuvem utilizando serviços da AWS ou Google Cloud
