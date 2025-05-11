# Análise de Resultados de um Evento de Vendas Online

## Descrição

Este projeto realiza uma **análise de vendas de uma empresa de vendas online** com foco na identificação dos clientes que mais gastaram durante um evento promocional de 5 dias. O objetivo principal é **identificar o cliente com a maior compra** durante o período do evento, que será premiado pela loja. Além disso, a análise visa ajudar a empresa a **criar estratégias para atrair mais clientes** e melhorar a eficácia de futuras campanhas promocionais.

## Objetivos

- **Analisar os dados de vendas** de um evento de 5 dias.
- **Identificar o cliente com maior gasto** durante o evento e premiá-lo.
- **Visualizar o total gasto por cliente** para compreender os padrões de compra.
- **Fornecer insights estratégicos** para melhorar as campanhas e aumentar o engajamento de clientes.

## Estrutura do Projeto

1. **Leitura dos Dados**: O conjunto de dados é carregado a partir de um arquivo JSON contendo informações detalhadas sobre as compras dos clientes.
2. **Normalização dos Dados**: Os dados, que estão aninhados em colunas específicas, são expandidos e normalizados para facilitar a análise.
3. **Análise Exploratória**: São realizadas verificações gerais sobre o DataFrame, como informações, estatísticas descritivas e verificação de valores nulos.
4. **Filtragem de Dados**: Os dados são filtrados para o período de 5 dias do evento.
5. **Cálculo do Total Gasto por Cliente**: O total gasto por cada cliente é calculado e o cliente com maior gasto é identificado.
6. **Visualização dos Resultados**: São gerados gráficos para visualizar os 10 clientes que mais gastaram durante o evento.

## Pré-requisitos

- Python 3.x
- Bibliotecas:
  - Pandas
  - Matplotlib
  - Seaborn

Para instalar as bibliotecas necessárias, execute:

```bash
pip install pandas matplotlib seaborn
```
## Base de dados:

```bash
https://cdn3.gnarususercontent.com.br/2928-transformacao-manipulacao-dados/dados_vendas_clientes.json
```
 
## Como Executar
1. Clone este repositório:

git clone https://github.com/seu-usuario/analise-evento-vendas.git

cd analise-evento-vendas

2. Certifique-se de ter o arquivo dados_vendas_clientes.json disponível no diretório /content ou forneça o caminho correto.

3. Execute o código em seu ambiente local ou no Google Colab:

```bash
# Importe as bibliotecas necessárias
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Caminho para o arquivo JSON
file_path = '/content/dados_vendas_clientes.json'

# Leitura do arquivo JSON
dados = pd.read_json(file_path)

# Expandindo os dados dentro da coluna 'dados_vendas'
dados_normalizados = pd.json_normalize(dados['dados_vendas'])

# Convertendo a coluna 'Data de venda' para o tipo datetime
dados_normalizados['Data de venda'] = pd.to_datetime(dados_normalizados['Data de venda'])

# Expandindo as colunas de 'Cliente' e 'Valor da compra' para ter uma linha para cada cliente
dados_normalizados = dados_normalizados.explode('Cliente')
dados_normalizados = dados_normalizados.explode('Valor da compra')

# Removendo a formatação da moeda e convertendo para número
dados_normalizados['Valor da compra'] = dados_normalizados['Valor da compra'].replace(
    {'R\$': '', ',': ''}, regex=True).astype(float)

# Filtragem dos dados para o período de 5 dias
inicio_periodo = dados_normalizados['Data de venda'].min()
fim_periodo = inicio_periodo + pd.Timedelta(days=5)
dados_filtrados = dados_normalizados[(dados_normalizados['Data de venda'] >= inicio_periodo) & 
                                     (dados_normalizados['Data de venda'] <= fim_periodo)]

# Cálculo do total gasto por cliente
total_gasto_por_cliente = dados_filtrados.groupby('Cliente')['Valor da compra'].sum().reset_index()

# Identificação do cliente com maior gasto
cliente_maior_gasto = total_gasto_por_cliente.loc[total_gasto_por_cliente['Valor da compra'].idxmax()]

# Exibição do cliente com maior gasto
print("Cliente com o Maior Gasto Durante o Evento:")
print(cliente_maior_gasto)

# Gerar o gráfico de barras
plt.figure(figsize=(8, 6))
sns.barplot(x='Cliente', y='Valor da compra', data=total_gasto_por_cliente.sort_values(by='Valor da compra', ascending=False).head(10))
plt.title('Top 10 Clientes com Maior Gasto Durante o Evento')
plt.xlabel('Cliente')
plt.ylabel('Valor Total da Compra (R$)')
plt.xticks(rotation=45, ha='right')  # Rotacionar os nomes dos clientes para facilitar a leitura
plt.tight_layout()  # Ajustar o layout para não cortar nada
plt.show()
```

## Contribuição
Se você deseja contribuir para este projeto, siga as etapas abaixo:

Faça um fork deste repositório.

1. Crie uma nova branch (git checkout -b feature/nova-análise).

2. Realize as mudanças necessárias e faça commit (git commit -am 'Adiciona nova análise').

3. Envie a branch para o repositório (git push origin feature/nova-análise).

4. Abra um Pull Request para revisar e mesclar suas alterações.

## Licença
Este projeto está licenciado sob a Licença MIT - veja o arquivo LICENSE para mais detalhes.


Este README está estruturado de maneira clara e organizada para o GitHub. Se precisar de mais detalhes ou ajustes, é só me avisar!
