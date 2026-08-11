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

O exemplo de contexto montado abaixo, tem como base os dados originais apresentados na base de conhecimento, mas apresenta apenas algumas informações sintetizadas deixando no conteúdo apenas as informações mais relevantes.

```
# Fontes de Pesquisa (JSON)

Nome: Banco Central do Brasil
Sigla: BCB
Tipo: regulador
URL: https://www.bcb.gov.br
Descrição: Informações sobre sistema financeiro, taxas, regulamentação e estatísticas econômicas

Nome: Comissão de Valores Mobiliários
Sigla: CVM
Tipo: regulador
URL: https://www.gov.br/cvm
Descrição: Fiscalização e regulamentação do mercado de capitais, fundos, ações e ofertas públicas

Nome: Superintendência de Seguros Privados
Sigla: SUSEP
Tipo: regulador
URL: https://www.gov.br/susep
Descrição: Regulação e supervisão de seguros, previdência privada aberta e capitalização
 

# Histórico de Atendimento (CSV)

15-09-2025,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
22-09-2025,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
04-10-2026,telefone,COE,Cliente quis entender cenários de retorno,sim
11-10-2026,e-mail,Tesouro Selic,Cliente pediu ajuda para entender marcação a mercado,sim

# Perfil do Investidor (JSON)

Nome: Maria Oliveira
Idade: 41
Profissão: Enfermeira
Renda Mensal: 7200.00
Perfil do Investidor: conservador
Objetivo Principal: Aumentar segurança financeira da família
Patrimônio Total: 35000.00
Reserva de Emergência Atual: 20000.00
Aceita Risco: false
Meta1: Completar reserva de emergência
Valor Necessário: 30000.00
Prazo: 2026-12
Meta2: Educação dos filhos
Valor Necessário: 80000.00
Prazo: 2030-06 

# Produtos Financeiros (JSON)

Nome: Tesouro Selic
Categoria: renda_fixa
Risco: baixo
Rentabilidade: 100% da Selic
Aporte Mínimo: 30.00
Indicado Para: Reserva de emergência e iniciantes

# Transações (CSV)

2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
```
