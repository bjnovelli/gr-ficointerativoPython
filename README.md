## Gráfico em Pyhton - Ciência de Dados - 27-1-25
Gráfico criado em Python que desmotra a relação de cliente X incidents.

Resumo do que o código faz:

1-Carrega dados de um arquivo Excel com informações sobre chamados e empresas.

2-Agrupa os dados por empresa e conta quantos chamados cada uma tem.

3-Cria um gráfico de barras interativo que mostra a quantidade de chamados por empresa.

4-Ajusta o layout do gráfico para melhorar a visualização (ordem das empresas, títulos de eixos, estilo).

5-Exibe o gráfico na tela.

6-Salva o gráfico como um arquivo HTML (opcional) para visualização posterior.

## Detalhes do código

import pandas as pd
import plotly.express as px

### import pandas as pd:

Pandas -  é uma biblioteca do Python usada para manipulação e análise de dados. Ela fornece estruturas de dados como DataFrame e Series, que são eficientes para lidar com grandes volumes de dados, realizar operações e análise estatísticas, entre outras tarefas.
A palavra pd - é um apelido (alias) que estamos dando à biblioteca pandas. Esse alias torna o código mais compacto e fácil de escrever. Em vez de escrever pandas.DataFrame, podemos simplesmente escrever pd.DataFrame.

### import plotly.express as px:

Plotly.express -  é uma biblioteca para visualização de dados interativa, baseada no Plotly, que facilita a criação de gráficos bonitos e interativos com apenas algumas linhas de código.
Assim como com o pandas, a palavra px é um apelido (alias) que estamos utilizando para simplificar o código. Em vez de usar plotly.express.scatter, por exemplo, podemos usar px.scatter.

### Substitua pelo caminho correto da sua planilha
 file_path = '/content/incident (53) (1).xlsx':

Essa linha está atribuindo o caminho do arquivo Excel à variável file_path. O caminho do arquivo '/content/incident (53) (1).xlsx' parece indicar que o arquivo está armazenado em um ambiente onde o diretório /content/ é utilizado, como em uma plataforma de notebook na nuvem (por exemplo, Google Colab).
O arquivo é um documento Excel (com a extensão .xlsx) e seu nome é incident (53) (1).xlsx.


### data = pd.read_excel(file_path, sheet_name='Page 1'):

A função pd.read_excel()  - é usada para ler arquivos Excel e carregá-los como um DataFrame do Pandas.

File_path -  Esse é o caminho do arquivo Excel que estamos carregando.

sheet_name='Page 1' - Aqui, estamos especificando o nome da aba que queremos carregar dentro do arquivo Excel. Nesse caso, estamos buscando a aba chamada Page 1. Se não especificarmos essa parte, o Pandas carregaria a primeira aba por padrão.

data -  O resultado da leitura do arquivo é armazenado na variável data, que será um DataFrame.
Um DataFrame é uma estrutura de dados bidimensional (como uma tabela), onde as colunas podem ter tipos diferentes de dados, como números, texto ou datas.




### Agrupar dados por empresa e contar a quantidade de chamados
calls_per_company = data.groupby('Customer (Company)').size().reset_index(name='Call Count')

data.groupby('Customer (Company)') -  A função groupby() do Pandas permite agrupar os dados de um DataFrame com base em uma ou mais colunas.
No seu caso, você está agrupando os dados pela coluna 'Customer (Company)', que, conforme o nome, provavelmente contém o nome das empresas (ou clientes).
O que isso faz: Ela cria grupos de dados, um para cada empresa, de modo que as linhas pertencentes à mesma empresa sejam agrupadas juntas.

.size() - O método size() retorna o número de registros (ou linhas) em cada grupo formado - Neste contexto, ele vai contar quantos chamados (ou incidentes) estão associados a cada empresa no DataFrame. Ou seja, ele conta quantas ocorrências de cada empresa existem no conjunto de dados.

.reset_index(name='Call Count')- Após o agrupamento e a contagem, o índice do resultado será a coluna Customer (Company) (o nome da empresa), e o valor será o número de chamados para cada empresa.

O método reset_index()- é usado para transformar o índice de volta em uma coluna regular. Ele cria uma nova coluna de índice numérico, e a coluna Customer (Company) se torna uma coluna regular no DataFrame resultante.

O parâmetro name='Call Count' renomeia a nova coluna que contém a quantidade de chamados para Call Count. Assim, o resultado final terá duas colunas: uma com o nome das empresas e outra com a quantidade de chamados.



### Agrupar dados por empresa e contar a quantidade de chamados
fig = px.bar(calls_per_company,
x='Customer (Company)',
y='Call Count',
title='Quantidade de Chamados por Empresa',
labels={'Customer (Company)': 'Empresa', 'Call Count': 'Quantidade de Chamados'},
text='Call Count')

Cria um gráfico de barras interativo usando a biblioteca plotly.express. Vamos detalhar cada parte:

px.bar()-  A função bar() de plotly.express cria um gráfico de barras. Ela recebe um DataFrame como entrada (no caso, o calls_per_company) e plota as colunas especificadas nos eixos x e y.
Calls_per_company - Este é o DataFrame que você criou anteriormente, que contém o número de chamados por empresa.

x='Customer (Company)'- A coluna 'Customer (Company)' será usada no eixo x do gráfico, representando as empresas.

y='Call Count' - A coluna 'Call Count' será usada no eixo y, mostrando a quantidade de chamados para cada empresa.

title='Quantidade de Chamados por Empresa' - O título do gráfico será 'Quantidade de Chamados por Empresa', explicando claramente o que o gráfico está mostrando.

labels={'Customer (Company)': 'Empresa', 'Call Count': 'Quantidade de Chamados'}  - Esse parâmetro é um dicionário que mapeia os nomes das colunas para rótulos mais amigáveis.

'Customer (Company)' - será renomeado para 'Empresa' no gráfico.

'Call Count' - será renomeado para 'Quantidade de Chamados'.

text='Call Count ‘ - O parâmetro text especifica que o valor de Call Count será mostrado no topo de cada barra do gráfico. Ou seja, a quantidade de chamados será exibida diretamente sobre cada barra, tornando o gráfico mais informativo.


### Ajustar layout para melhor visualização
fig.update_layout(xaxis={'categoryorder': 'total descending'},
                  xaxis_title="Empresas",
                  yaxis_title="Quantidade de Chamados",
                  template="plotly_white")




fig.update_layout()- O método update_layout() -  é utilizado para modificar o layout do gráfico. O layout inclui características como título, rótulos de eixos, estilo e outros ajustes estéticos. Aqui, fig é o gráfico gerado com px.bar(), que foi atribuído anteriormente à variável ig. No entanto, se você estava usando ig, o nome correto seria ig.update_layout().

xaxis={'categoryorder': 'total descending'}- Esse parâmetro organiza as categorias (no caso, as empresas) no eixo x de forma decrescente, com base no valor total de cada barra (ou seja, na quantidade de chamados).

xaxis_title="Empresas"-  Isso define o título do eixo x, que será exibido abaixo do eixo. No caso, ele será alterado para "Empresas".

yaxis_title="Quantidade de Chamados"- Define o título do eixo y, que será exibido ao lado esquerdo do gráfico. Ele será alterado para "Quantidade de Chamados".
template="plotly_white":


                  











