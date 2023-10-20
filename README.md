# Heatmap-falhas-FTVIEW
Arquivo python para avaliação de concentração de falhas 

# Explicação das funcionalidade e como o script pode ser adaptado
Esse código foi desenvolvido para consumir os dados de trend das tags COMM_ERR dos PLC's Rockwell historiadas no Factorytalk view e gerar um mapa de calor. A ideia é possibilitar encontrar padrões de falhas em determinados hórarios que podem ser causadas por rotinas em ambientes de rede e que podem afetar aplicações distribuidas do sistema supervisório.
Os dados são importados a partir do export dos dados de trend manualmente para o excel. A etapa de consumo pode ser substituida por exemplo para uma consulta direta ao SQL que permite nesse caso automação da geração do arquivo.

## Passo 1: Importar o arquivo

O arquivo db.xlsx é um arquivo genérico de exemplo com um padrão de falhas todos os dias no intervalo entre 18 e 19 horas, para testar o código e entender o padrão do dataframe importamos o arquivo com o código abaixo

```df = pd.read_excel('db.xlsx', sheet_name='Planilha1')```

## Passo 2: Tratamento do dataframe

Essa etapa realiza o tratamento do dataframe com as seguintes operações:

- Remove colunas não utilizadas
- Soma as falhas por linha
- Cria a variável DateTime (originalmente divididas em 2 colunas)
- Separa o somatório de falhas em outro dataframe
- Agrupa por hora os somatórios
- Converte o horário para escala de segundos

## Passo 3: Geração dos gráficos Scatterplot
Utilizando o matplotlib realizamos a geração de 2 modelos de sccaterplot, um com todos os pontos de horários do intervalo total de dados e outro aplicando um filtro para eliminar os pontos que não possuem grande concentração de falhas, o que auxilia a analise dos dados.
O código abaixo é onde podemos ajustar o filtro minimo:

```
df_filt1 =  df_final[df_final['falhas'] >= 20]
```
## Passo 4: Geração do heatmap
Utilizando o seaborn realizamos a geração de um heatmap dos dados. É possivel alterar o esquema de cores personalizados utilizado através do código

```
# Defina as cores inicial (branco) e final (vermelho)
cor_inicio = "white"
cor_inicio2 = "yellow"
cor_meio = "orange"
cor_fim = "red"
```
Também é editavel o range de minimo e máximo manualmente através do código

```
custom_norm = mcolors.Normalize(vmin=0, vmax=80)
sns.heatmap(pivot_df, annot=False, fmt="d", cmap= cmaping,norm=custom_norm)
```

