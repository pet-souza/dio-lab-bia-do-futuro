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

Para facilitar, foi criada uma classe BaseConhecimento que carrega todas as informações na inicialização. Como está sendo usada uma base pequena de informações, esse comando será eficiente pois carregará todas as informações uma única vez, deixa a busca mais inteligente e executa consultas de forma instantânea.

```text
# Exemplo de uso
import pandas as pd
import json

class BaseConhecimento:
    def __init__(self):
        self.historico_atendimento = pd.read_csv("historico_atendimento.csv")
        self.transacoes = pd.read_csv("transacoes.csv")
        
        with open("perfil_investidor.json", "r", encoding="utf-8") as f:
            self.perfil_investidor = json.load(f)

        with open("produtos_financeiros.json", "r", encoding="utf-8") as f:
            self.produtos_financeiros = json.load(f)

        with open("fontes_de_pesquisa.json", "r", encoding="utf-8") as f:
            self.fontes_de_pesquisa = json.load(f)

    # Exemplos de métodos úteis
    def buscar_produto(self, nome):
        return next((p for p in self.produtos_financeiros if p["nome"] == nome), None)

    def buscar_fontes(self):
        return self.fontes_de_pesquisa

    def historico_cliente(self):
        return self.historico_atendimento

    def transacoes_cliente(self):
        return self.transacoes

# Execução no Agente
base = BaseConhecimento()

produto = base.buscar_produto("Tesouro Selic")
fontes = base.buscar_fontes()
transacoes = base.transacoes_cliente()
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
