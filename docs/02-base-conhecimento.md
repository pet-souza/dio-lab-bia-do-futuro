# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores de forma a dar continuidade ou iniciar um novo atendimento de forma eficiente |
| `perfil_investidor.json` | JSON | Personalizar recomendações e conhecer os possíveis perfis para ser mais assertivo na explicação dada ao cliente |
| `produtos_financeiros.json` | JSON | Conhecer os produtos financeiros disponibilizados para proporcionar uma explicação mais completa ao cliente |
| `transacoes.csv` | CSV | Analisar diversos padrões de gastos que um cliente pode ter |
| `fontes_de_pesquisa.json` | JSON | Pesquisar informações de investimentos confiáveis |


## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Sim! Foram inseridas mais informações nos arquivos, complementando o histórico de atendimento, perfil do investidor, produtos financeiros e transações. Ainda, foi inserido o arquivo fontes_de_pesquisa.json com informações e links de sites que o agente poderá consultar para o fornecimento de informações precisas e confiáveis para o usuário.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Existem duas possibilidades de carregamento dos arquivos. A primeira, seria injetando os dados diretamente no prompt (ctrl + C, ctrl + V) e, a segunda, carregando os arquivos via código conforme exemplos abaixo:

```pyton
import pandas as pd
import json

# Carregar arquivos CSV
historico_atendimento = pd.read_csv("historico_atendimento.csv")
transacoes = pd.read_csv("transacoes.csv")

# Carregar arquivos JSON
with open("perfil_investidor.json", "r", encoding="utf-8") as f:
    perfil_investidor = json.load(f)

with open("produtos_financeiros.json", "r", encoding="utf-8") as f:
    produtos_financeiros = json.load(f)

with open("fontes_de_pesquisa.json", "r", encoding="utf-8") as f:
    fontes_de_pesquisa = json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para facilitar, podemos simplesmente injetar os dados com o prompt, garantindo que o agente tenha o melhor contexto possível. Lembrando que, em soluções mais robustas, o ideal é que essas informações sejam carregadas dinamicamente possibilitando o ganho que flexibilidade.

```text
# Exemplo de uso

```

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
