# Fatores de risco de AVC

## Introdução

Neste projeto, foram usados classificadores para identificar quais são os fatores de risco para o acidente vascular cerebral (AVC). Um AVC (stroke) é um dano cerebral causado pela interrupção de seu fluxo sanguíneo.

## Implementação

1. Certifique-se de ter o python instalado em seu computador;<br>
2. Clone esse repositório para algum lugar de sua máquina, utilizando o comando `https://github.com/insper-classroom/241-alglin-aps6-ribollarafa.git`;<br>
3. Instale as bibliotecas necessárias contidas no arquivo `requirements.txt`, através do comando: `pip install -r requirements.txt`.

## Organização

1. Criação do arquivo `demo.py` para demonstração;<br>
2. Adição de comentários de explicação ao decorrer do código;<br>
3. Criação do `requirements.txt` para instalação de bibliotecas; <br>
4. Arquivo `.gitignore` com pasta "archive" incluída.

## Arquivo Demo

1. Rode o arquivo `demo.py`;<br>
2. O arquivo gerará o resultado da acurácia.<br>

## Tratamento de Dados

Os dados para treinamento do nosso sistema foram baixados do site [Kaggle - The Movies Dataset](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset) e armazenados no arquivo `archive/healthcare-dataset-stroke-data.csv`, arquivo que foi incluído no `.gitignore` e consequentemente não está nesse repositório dado seu tamanho. 

Os dados recebidos eram descritos em linguagem natural, por exemplo, o dataframe possuia a coluna "gender" que podia ter como resposta "male", "female" e "other". Esse tipo de descrição facilita o entendimento dos dados por pessoas, mas para o computador isso não é facilmente interpretado. Asssim utilizamos a função `get_dummies` da biblioteca pandas para reorganizar os dados de forma que facilitaria a interpretação pelo computador. Além disso, na coluna onde o "bmi" era representando para alguns o dado não existia assim prenchemos estas linhas com a média do bmi do dataframe.

Após esse tratamento incial separamos o dataframe original em dois, o primeiro o `df_features` que não contém as colunas de "id" e "stroke", ou seja, possuiem os atributos que afetam a predição de saber se a pessoa tem chance de ter um AVC ou não, o segundo dataframe `df_target` possuí somente a coluna de stroke, sendo que 1 representa uma pessoa que teve um AVC e -1 uma que não teve.

## Regressão Linear

O primeiro método utilizado foi o da regressão linear. Para isso primeiramente transformamos nossos dois dataframes `df_features` e 
`df_target` em numpy arrays, e então usamos a função `train_test_split` da biblioteca sklearn para divir nossos arrays em um conjuto de teste e um de treino.

Então criamos a função `loss` que contém a função que será usado pelo gradiente. Ela recebe um array com o pesos das features, o vies, as features de treino e o alvo de treino. Apartir desta função calculamos o seu gradiente, multiplicamos por alpha e atualizamos a o vies e os pesoss e calculamos novamente. Fazemos esse processo 10000, com um alpha de $10^{-5}$. Ou seja, vamos nos aproximando aos poucos do nosso valor mínimo do EQM, que é quando nosso modelo erra menos.

Por último depois de chegarmos no valor ideal para nossos pesos e viés, podemos fazer nossa predição de se uma pessoa teria um AVC ou não usando o conjunto teste.

## Árvore de Decisão

Para fazer a predição utilizando a Árvore de Decisão, primeiro separamos nossos dados em dois conjuntos um de treino e um de teste. Então utilizando a função `DecisionTreeClassifier` da biblioteca sklearn uma árvore de decisão é gerado. Esta árvore é baseada em que features do nosso conjunto de treino possuem um valor de entropia maior, ou seja, quais features são mais decisivas na predição de se uma pessoa terá AVC ou não.

## Acurária e Hipótese Nula

Para calcular a acurácia de ambas as técnicas de predição foi utilizado a função `accuracy(y_test, y_est)` nela comparamos o sinal do resultado da predição e da informação correta se estes forem iguais então o modelo acertou. E então retornamos a média dos acertos. 
Nossa acurácia para o modelo que utilizava a regressão linear obteve entorno de 0.75 na média, já para a árvore de decisão a média da nossa acurácia foi de entorno de 0.88. 

Apesar das acurácias aparentarem serem boas em uma primeira análise, elas não são melhores que a hipótese nula, ou seja, se nosso modelo simplesmente dissesse que todos não teram AVC, teriamos uma acurácia mais alta dado que somente aproximadamente 0.04 das pessoas do banco de dados tiveram AVC. Isso demonstra que nosso modelo poderia ser melhorado.

## Fatores ligados ao AVC

Para analisarmos os fatores ligados ao Acidente Vascular Cerebral (AVC) escolhemos três formas para encontrarmos: Correlação, Regressão Linear e Árvore de Decisão. Para a correlação calculamos através do `.corr()`, para a regressão linear utilizamos a transposta da nossa matriz de coeficientes de peso e para a árvore de decisão, usamos o método `feature_importances_`, abaixo estão representados os 10 mais relevantes de cada um:

Ordem | Correlação | Regressão Linear | Árvore de Decisão
--- | --- | --- | --- |
 1 | Idade | Gênero Outro | Criança |
 2 | Doença do Coração | Doença do Coração | Nível de Glicose |
 3 | Nível de Glicose | Nunca Fumou | Idade |
 4 | Hipertensão | Já Casado | Índice de Massa Corporal |
 5 | Já Casado | Ex Fumante | Fumante |
 6 | Status Fumante | Residente Urbano | Trabalhador Privado |
 7 | Índice de Massa Corporal | Fumante | Doença do Coração |
 8 | Residente Urbano | Residente Rural | Não Fumante |
 9 | Trabalhador Privado | Nível de Glicose | Status de Fumante Desconhecido |
 10 | Gênero Masculino | Gênero Masculino | Trabalhador do Governo |

Dentre os resultados gerados, percebemos que: idade, doença do coração, hipertensão, status de fumante, nível de glicose e tipo de residência foram os mais relevantes e após encontramos estudos que citam essas características:

1- Idade: <br>
- [Influence of Age and Health Behaviors on Stroke Risk: Lessons from Longitudinal Studies](https://agsjournals.onlinelibrary.wiley.com/doi/abs/10.1111/j.1532-5415.2010.02915.x) <br>
- [Age and Sex Are Critical Factors in Ischemic Stroke Pathology](https://academic.oup.com/endo/article/159/8/3120/5051605?login=false) <br>

2- Doença do Coração: <br>
- [Heart Disease and Stroke Statistics—2022 Update: A Report From the American Heart Association](https://www.ahajournals.org/doi/full/10.1161/CIR.0000000000001052) <br>

3- Hipertensão: <br>
- [Hypertension: a harbinger of stroke and dementia](https://www.ahajournals.org/doi/full/10.1161/HYPERTENSIONAHA.113.01063) <br>
- [Haematocrit, hypertension and risk of stroke](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1365-2796.1994.tb01050.x) <br>

4- Status de Fumante: <br>
- [Smoking and Risk of Stroke](https://journals.sagepub.com/doi/abs/10.1177/204748739900600403) <br>
- [Cigarette Smoking and Risk of Stroke in Middle-Aged Women](https://www.nejm.org/doi/full/10.1056/NEJM198804143181501) <br>
- [Meta-analysis of relation between cigarette smoking and stroke](https://www.bmj.com/content/298/6676/789.short) <br>

5- Nível de Glicose: <br>
- [Acute blood glucose level and outcome from ischemic stroke](https://www.neurology.org/doi/abs/10.1212/WNL.52.2.280) <br>
- [Glucose and Acute Stroke](https://www.ahajournals.org/doi/full/10.1161/STROKEAHA.111.631218) <br>

6- Tipo de Residência: <br>
- [Rural-urban differences in stroke risk](https://www.sciencedirect.com/science/article/pii/S0091743521002309?) <br>

## Referências

1. Código desenvolvido com o auxílio do [Notebook](https://github.com/mfstabile/AlgLin24-1) realizado pelo professor [Marcio Fernando Stabile Junior](https://github.com/mfstabile);
2. Auxílio do [ChatGPT](https://chat.openai.com/) para saciar dúvidas relacionadas ao projeto.

## Desenvolvedores

Projeto desenvolvido por Gustavo Colombi Ribolla e Rafaela Afférri de Oliveira.