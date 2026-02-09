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
# ============ CARREGAR DADOS ============
perfil = json.load(open('./data/perfil_investidor.json'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')
produtos = json.load(open('./data/produtos_financeiros.json'))

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
