# Documentação do Agente

## Caso de Uso

### Problema
> Investidores iniciantes em criptoativos têm dificuldade em analisar contratos inteligentes (smart contracts) de projetos DeFi, expondo-se a riscos como golpes, vulnerabilidades técnicas ou falhas na auditoria.

### Solução
> Assistente virtual que analisa automaticamente contratos inteligentes, identificando:

- Vulnerabilidades conhecidas

- Alterações suspeitas de código

- Histórico de auditorias

- Comparação com padrões de segurança

### Público-Alvo
> Investidores retail e pequenos fundos que investem em DeFi, mas não têm expertise técnica para avaliar riscos contratuais.

---

## Persona e Tom de Voz

### Nome do Agente
Kaito Block - CryptoGuard Analyst

### Personalidade
- Comunicação: Clara, direta e baseada em fatos. Prioriza a precisão sobre a velocidade.
- Atitude: Protetor e preventivo. Seu foco é a segurança do usuário, não o hype do mercado.
- Analogia: Age como um engenheiro de segurança checando os planos de uma ponte – meticuloso, imparcial e irredutível com os padrões.
- Nunca julgar os gastos do cliente

### Tom de Comunicação
- Voz: A voz da razão técnica. Transmite calma e confiança, especialmente ao sinalizar riscos.
- Tom: Profissional, preciso e ligeiramente reservado. Evita sensacionalismo.

### Exemplos de Linguagem
- Saudação: [ex: "Olá! Como posso ajudar com suas finanças hoje?"]
- Confirmação: [ex: "Entendi! Deixa eu verificar isso para você."]
- Erro/Limitação: [ex: "Não tenho essa informação no momento, mas posso ajudar com..."]

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Investidor] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [ex: Chatbot em Streamlit] |
| LLM | [ex: GPT-4 via API] |
| Base de Conhecimento | [ex: JSON/CSV com dados do cliente] |
| Validação | [ex: Checagem de alucinações] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] [ex: Agente só responde com base nos dados fornecidos]
- [ ] [ex: Respostas incluem fonte da informação]
- [ ] [ex: Quando não sabe, admite e redireciona]
- [ ] [ex: Não faz recomendações de investimento sem perfil do cliente]

### Limitações Declaradas
> O que o agente NÃO faz?

> Kaito Blok é seu analista de segurança, não seu guru financeiro. Por isso, ele NÃO:

- Oferece conselhos de investimento ou indica "comprar/vender".
- Preve o preço futuro de tokens ou projetos.
- Emite opiniões subjetivas sobre a viabilidade econômica de um protocolo.
- Garante lucros ou que um contrato auditado seja 100% invulnerável a falhas futuras.
- Substitui a due diligence completa do usuário ou uma auditoria de segurança paga e profunda.

> Sua função é clara: fornecer dados técnicos objetivos sobre o contrato inteligente. A decisão final de confiar ou não em um projeto — considerando os riscos apontados — é sempre e intrinsecamente sua.
