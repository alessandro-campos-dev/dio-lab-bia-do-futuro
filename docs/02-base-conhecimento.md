# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Base de interações anteriores com os ivestidores |
| `perfil_investidor.json` | JSON | Personalizar recomendações de acordo com o perfil de investidor |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil de cada investidor|
| `transacoes.csv` | CSV | Analisar padrão de investimentos, e o valor das transações|

---

## Adaptações nos Dados

- Transações realistas do ecossistema crypto
- Valores condizentes com cada categoria
- Mistura de saídas (investimentos, custos) e entradas (rendimentos, vendas)
- Datas em ordem decrescente (mais recente primeiro)

---

## Estratégia de Integração

### Como os dados são carregados?
> Requisitos
```
import pandas as pd
import json
import numpy as np
from datetime import datetime
import matplotlib.pyplot as plt

```

> 1 Histórico de Atendimento (CSV)
```
def carregar_historico_atendimento():
    """
    Carrega o arquivo CSV com histórico de atendimento do assistente virtual
    """
    try:
        df_atendimento = pd.read_csv('historico_atendimento.csv')
        print("✅ Histórico de atendimento carregado com sucesso!")
        print(f"📊 Total de registros: {len(df_atendimento)}")
        print(f"📅 Período: {df_atendimento['data'].min()} até {df_atendimento['data'].max()}")
        print(f"🔧 Solicitações resolvidas: {df_atendimento['resolvido'].value_counts().get('Sim', 0)}")
        print(f"🔄 Solicitações pendentes: {df_atendimento['resolvido'].value_counts().get('Nao', 0)}")
        print("\n" + "="*60 + "\n")
        return df_atendimento
    except FileNotFoundError:
        print("❌ Arquivo 'historico_atendimento.csv' não encontrado!")
        return None
    except Exception as e:
        print(f"❌ Erro ao carregar histórico: {str(e)}")
        return None
```
---
> Perfis de Investidores (JSON)
```
def carregar_perfis_investidores():
    """
    Carrega o arquivo JSON com perfis de investidores em blockchain
    """
    try:
        with open('perfil_investidor.json', 'r', encoding='utf-8') as file:
            dados_perfis = json.load(file)
        
        print("✅ Perfis de investidores carregados com sucesso!")
        print(f"👥 Total de perfis: {len(dados_perfis['investidores'])}")
        
        # Criar DataFrame para análise
        df_perfis = pd.DataFrame(dados_perfis['investidores'])
        
        # Análise básica
        print(f"📈 Média de idade: {df_perfis['idade'].mean():.1f} anos")
        print(f"💰 Média de patrimônio: R$ {df_perfis['patrimonio_total'].mean():,.2f}")
        
        # Distribuição por perfil de investidor
        print("\n📊 Distribuição por perfil de investidor:")
        print(df_perfis['perfil_investidor'].value_counts())
        
        print("\n" + "="*60 + "\n")
        return dados_perfis, df_perfis
    except FileNotFoundError:
        print("❌ Arquivo 'perfil_investidor.json' não encontrado!")
        return None, None
    except Exception as e:
        print(f"❌ Erro ao carregar perfis: {str(e)}")
        return None, None

```
> 3 Produtos Financeiros (JSON)
```
def carregar_produtos_financeiros():
    """
    Carrega o arquivo JSON com produtos financeiros de blockchain
    """
    try:
        with open('produtos_financeiros_blockchain.json', 'r', encoding='utf-8') as file:
            dados_produtos = json.load(file)
        
        print("✅ Produtos financeiros carregados com sucesso!")
        print(f"📦 Total de produtos: {len(dados_produtos['produtos'])}")
        
        # Criar DataFrame para análise
        df_produtos = pd.DataFrame(dados_produtos['produtos'])
        
        # Análise básica
        print(f"📊 Categorias disponíveis: {df_produtos['categoria'].nunique()}")
        print(f"🎯 Produto com menor aporte: {df_produtos.loc[df_produtos['aporte_minimo'].idxmin()]['nome']} (R$ {df_produtos['aporte_minimo'].min():,.2f})")
        
        # Distribuição por nível de risco
        print("\n⚠️  Distribuição por nível de risco:")
        print(df_produtos['risco'].value_counts())
        
        print("\n" + "="*60 + "\n")
        return dados_produtos, df_produtos
    except FileNotFoundError:
        print("❌ Arquivo 'produtos_financeiros_blockchain.json' não encontrado!")
        return None, None
    except Exception as e:
        print(f"❌ Erro ao carregar produtos: {str(e)}")
        return None, None

```
> 4 Transações (CSV)
```
def carregar_transacoes():
    """
    Carrega o arquivo CSV com transações de investimentos
    """
    try:
        df_transacoes = pd.read_csv('transacoes_blockchain.csv')
        
        # Converter data para datetime
        df_transacoes['data'] = pd.to_datetime(df_transacoes['data'])
        
        print("✅ Transações carregadas com sucesso!")
        print(f"💰 Total de transações: {len(df_transacoes)}")
        print(f"📅 Período: {df_transacoes['data'].min().strftime('%Y-%m-%d')} até {df_transacoes['data'].max().strftime('%Y-%m-%d')}")
        
        # Análise financeira básica
        entradas = df_transacoes[df_transacoes['tipo'] == 'entrada']['valor'].sum()
        saidas = df_transacoes[df_transacoes['tipo'] == 'saida']['valor'].sum()
        saldo = entradas - saidas
        
        print(f"📈 Total de entradas: R$ {entradas:,.2f}")
        print(f"📉 Total de saídas: R$ {saidas:,.2f}")
        print(f"💰 Saldo líquido: R$ {saldo:,.2f}")
        
        # Top categorias
        print("\n🏆 Top 5 categorias por volume:")
        top_categorias = df_transacoes.groupby('categoria')['valor'].sum().sort_values(ascending=False).head(5)
        for categoria, valor in top_categorias.items():
            print(f"  {categoria}: R$ {valor:,.2f}")
        
        print("\n" + "="*60 + "\n")
        return df_transacoes
    except FileNotFoundError:
        print("❌ Arquivo 'transacoes_blockchain.csv' não encontrado!")
        return None
    except Exception as e:
        print(f"❌ Erro ao carregar transações: {str(e)}")
        return None

```

### Como os dados são usados no prompt?
```
Perfil do Investidor
 {
      "nome": "Carla Torres",
      "idade": 28,
      "profissao": "Desenvolvedora Web3",
      "renda_mensal": 8500.00,
      "perfil_investidor": "arrojado",
      "objetivo_principal": "Alocação em DeFi e governança de DAOs",
      "patrimonio_total": 125000.00,
      "exposicao_crypto": 45,
      "reserva_emergencia_atual": 30000.00,
      "aceita_risco": true,
      "conhecimento_blockchain": "avançado",
      "metas": [
        {
          "meta": "Alocação em yield farming (ETH)",
          "valor_necessario": 25000.00,
          "prazo": "2024-12",
          "tipo": "DeFi"
        },
        {
          "meta": "Participação em governança de DAO",
          "valor_necessario": 15000.00,
          "prazo": "2025-06",
          "tipo": "Governança"
        }
      ]
    },


Produtos Financeiros
 {
      "nome": "Kaito Guard - Auditoria Básica",
      "categoria": "Segurança",
      "risco": "baixo",
      "rentabilidade": "N/A (Produto de Proteção)",
      "aporte_minimo": 99.90,
      "indicado_para": "Investidores iniciantes que desejam verificar segurança de contratos antes de investir",
      "descricao": "Análise automatizada de contratos inteligentes para detectar vulnerabilidades comuns",
      "prazo_minimo": "Imediato",
      "tributacao": "Isento"
    },

Transações 
data,descricao,categoria,valor,tipo
2024-03-15,Compra de Bitcoin (BTC),compra,2500.00,saida
2024-03-15,Staking reward Ethereum,rendimento,85.30,entrada
2024-03-14,Yield Farming USDC Pool,defi,1200.00,saida
2024-03-14,Venda parcial Solana (SOL),venda,3200.50,entrada

Histórico de Atendimento

data,canal,tema,resumo,resolvido
2024-03-15,Telegram,Auditoria de Contrato,"Usuário enviou contrato de yield farming para análise de funções de mint não documentadas.",Sim
2024-03-15,Widget Web,Verificação de Rug Pull,"Cliente solicitou verificação de contrato de memecoin para padrões de rug pull clássicos.",Sim
2024-03-14,Discord,Análise de Herança,"Análise de herança de contrato upgradeable em protocolo de empréstimo.",Sim

```
## Exemplo de Contexto Montado

> Exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimos investimentos:
- 01/11: Cover de Seguros DeFi - R$ 5000
- 03/11: Node Operator Ethereum- R$ 10000
...
```
