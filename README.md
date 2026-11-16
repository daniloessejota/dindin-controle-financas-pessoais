# Controle de Finanças Pessoais

Sistema simples para controlar as finanças pessoais com base em **entradas e saídas**: o usuário registra o que recebe e o que gasta, acompanha o saldo de cada conta e analisa os totais por período.

> **Status:** em construção. Especificação e regras de negócio em fechamento; desenvolvimento a seguir.

## O problema

Muitas pessoas não sabem para onde vai o dinheiro porque as ferramentas existentes são complexas demais. Este projeto parte do oposto: **fazer apenas o essencial e fazer bem**.

## Solução (v1)

- Contas, carteira e fundos com saldo inicial.
- Registro de **entradas**, **saídas** e **transferências** entre contas (estas não contam como receita nem despesa).
- Edição, exclusão e estorno, com saldos sempre recalculados a partir dos lançamentos.
- **Dashboard** com saldos, total geral e movimentações recentes.
- **Análise** por mês, bimestre, trimestre, semestre e ano.
- Uso local, por uma única pessoa, com login.

## Decisões de produto

| Decisão | Motivo |
|---|---|
| Sem metas, orçamento ou "saúde financeira" na v1 | Manter a simplicidade para o usuário comum |
| Conta pode ficar negativa, com aviso em destaque | Refletir a realidade sem bloquear o usuário |
| Transferência é um tipo próprio de lançamento | Evitar distorção nos totais de receitas e despesas |
| Saldos calculados a partir dos lançamentos | Evitar inconsistência após edições e exclusões |
| Cartões de crédito ficam para versão futura | Reduzir o escopo do MVP |

## Como o projeto é construído

O desenvolvimento segue **Spec Driven Development**: a especificação vem primeiro e guia o código gerado por IA.

1. Definição de requisitos e regras de negócio (feito por mim).
2. Especificação com regras numeradas e critérios de aceite.
3. Geração do código com **Gemini** e **Antigravity**, orientada pela especificação.
4. Validação do resultado contra os critérios de aceite.

## Documentação

- [Especificação completa](docs/especificacao.md): glossário, requisitos, regras de negócio, critérios de aceite e itens fora do escopo.

## Próximos passos

- [x] Levantamento de requisitos e regras de negócio
- [x] Especificação com critérios de aceite
- [ ] Design das telas
- [ ] Implementação do MVP
- [ ] Testes contra os critérios de aceite

## Autor

**Danilo** · Estudante de Gestão da Tecnologia da Informação
LinkedIn: www.linkedin.com/in/danilosj
