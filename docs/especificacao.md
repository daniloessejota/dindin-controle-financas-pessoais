# Especificação — Sistema de Controle de Finanças Pessoais

**Versão:** 1.0 (rascunho) · **Metodologia:** Spec Driven Development (SDD)
**Status:** especificação em fechamento; desenvolvimento a seguir.

---

## 1. Objetivo

Sistema simples e objetivo para controle de finanças pessoais, baseado na dinâmica contábil de **entradas e saídas**. O usuário registra o que recebe e o que gasta, acompanha o saldo de cada conta ou fundo e analisa os totais por período.

**Princípio de design:** simplicidade. Qualquer recurso que torne o sistema complexo para um usuário comum fica fora da v1.

## 2. Glossário

| Termo | Definição |
|---|---|
| **Conta/Fundo** | Local onde o dinheiro está guardado: conta bancária, carteira, reserva ou outro fundo cadastrado pelo usuário. |
| **Saldo inicial** | Valor que a conta/fundo já possui no momento do cadastro. |
| **Entrada** | Ganho financeiro registrado. Aumenta o saldo da conta/fundo de destino. |
| **Saída** | Gasto ou despesa registrado. Diminui o saldo da conta/fundo de origem. |
| **Transferência** | Movimentação de dinheiro entre duas contas/fundos do próprio usuário. Não é receita nem despesa. |
| **Estorno** | Reversão de uma entrada ou saída já registrada. |
| **Origem da entrada** | Classificação da entrada: *salário* ou *outras receitas*. |
| **Meio de recebimento** | Forma como a entrada foi recebida (ex.: transferência eletrônica, depósito, dinheiro). Cadastrado pelo usuário. |
| **Tipo de despesa** | Categoria da saída (ex.: despesa fixa, despesa variável). Parte sugerida pelo sistema, editável pelo usuário. |
| **Referência** | Informação bancária de apoio ao lançamento (opcional). |
| **Saldo total** | Soma dos saldos de todas as contas/fundos. |

## 3. Escopo da v1

### 3.1 Dentro do escopo

- Configuração inicial de contas, fundos, carteira, meios de recebimento e tipos de despesa.
- Registro de entradas, saídas e transferências.
- Edição, exclusão e estorno de lançamentos.
- Dashboard com resumo do mês.
- Consulta aos últimos três meses.
- Análise simples por período.
- Ajuda de uso.
- Uso local, por uma única pessoa, com login.

### 3.2 Fora do escopo da v1

- Cartões de crédito e seus saldos (versão futura).
- Metas e orçamento por categoria.
- Indicador de "saúde financeira".
- Sugestão automática de despesas evitáveis (versão futura).
- Análises avançadas (versão futura).
- Multiusuário e acesso remoto.

## 4. Requisitos funcionais

### RF-01 — Configuração inicial
O usuário acessa a configuração no primeiro uso (e depois quando quiser) para definir:
- contas bancárias (quantas desejar) e seus saldos iniciais (podem ser zero);
- carteira e seu valor disponível;
- outros fundos ou reservas e seus valores;
- meios de recebimento;
- tipos de despesa (o sistema oferece um conjunto inicial sugerido).

O processo deve ser descomplicado para o usuário.

### RF-02 — Registro de entrada
Campos:

| Campo | Obrigatório |
|---|---|
| Conta/fundo de destino | Sim |
| Valor | Sim |
| Origem (salário ou outras receitas) | Sim |
| Meio de recebimento | Sim |
| Data | Sim |
| Descrição | Sim |
| Referência bancária | Não |
| Observações | Não |

Em "outras receitas", a natureza (juros, empréstimo, doação etc.) é informada pelo usuário em descrição, referência ou observações. O sistema não mantém lista dessas naturezas.

### RF-03 — Registro de saída
Campos:

| Campo | Obrigatório |
|---|---|
| Conta/fundo de origem do pagamento | Sim |
| Valor | Sim |
| Tipo de despesa | Sim |
| Data | Sim |
| Descrição | Sim |
| Referência do pagamento | Não |
| Observações | Não |

### RF-04 — Registro de transferência
Campos obrigatórios: conta/fundo de origem, conta/fundo de destino, valor e data. Descrição é opcional.

### RF-05 — Edição, exclusão e estorno
O usuário pode editar entradas e saídas, excluir qualquer lançamento e estornar entradas e saídas, conforme as regras RN-03 a RN-07.

### RF-06 — Dashboard
Tela inicial com:
- data do dia, mês e ano atuais;
- saldo atual de cada conta/fundo e saldo total;
- movimentações mais recentes;
- total de entradas e total de saídas do mês até o dia atual;
- link para o registro do mês atual e para adicionar lançamentos;
- links, embutidos no nome, para os **últimos três meses** registrados;
- acesso à ajuda, às configurações e à análise de dados.

### RF-07 — Análise de dados
Tela com filtro de período: **mês, bimestre, trimestre, semestre e ano**. Apresenta:
- mês com maior receita e mês com maior volume de saídas;
- total de receitas por origem;
- total de saídas por tipo de despesa;
- total de entradas e de saídas no ano até o momento.

Para cada período, o sistema mostra o total de entradas, o total de saídas e a diferença entre eles.

### RF-08 — Ajuda
Tela com orientações de uso dos recursos do sistema.

## 5. Regras de negócio

| Nº | Regra |
|---|---|
| **RN-01** | Todo lançamento exige valor, data, conta/fundo e tipo/origem. Referência e observações são opcionais. |
| **RN-02** | Entradas e saídas podem ser editadas a qualquer momento. |
| **RN-03** | O saldo de cada conta/fundo e todos os totais são **sempre recalculados a partir dos lançamentos**, sem valores guardados separadamente. |
| **RN-04** | Excluir um lançamento reverte seu impacto nos saldos e totais. |
| **RN-05** | Estornar uma **entrada** tem o mesmo efeito de excluí-la: o valor é subtraído do saldo e dos totais. |
| **RN-06** | Estornar uma **saída** devolve o valor à conta/fundo de origem e reduz o total de saídas. |
| **RN-07** | Excluir ou editar uma **transferência** reverte o movimento entre as duas contas/fundos. |
| **RN-08** | Transferências **não** são receita nem despesa: alteram apenas o saldo de cada conta envolvida e não alteram o saldo total nem os totais de entradas e saídas. |
| **RN-09** | Uma transferência exige contas/fundos de origem e destino diferentes. |
| **RN-10** | Uma conta/fundo pode ficar com saldo negativo. O sistema exibe aviso em destaque e não bloqueia a operação. |
| **RN-11** | O saldo de uma conta/fundo = saldo inicial + entradas − saídas + transferências recebidas − transferências enviadas. |
| **RN-12** | O sistema sugere um conjunto inicial de tipos de despesa (ex.: despesa fixa e despesa variável); o usuário pode alterá-lo na configuração. |
| **RN-13** | A moeda é o Real (R$) e a data é exibida no formato dia/mês/ano. |
| **RN-14** | Valores monetários não podem ser representados em ponto flutuante binário (usar centavos inteiros ou decimal exato). |

## 6. Critérios de aceite

Formato: *dado que… quando… então…*

**CA-01 — Entrada atualiza saldo**
Dado que a conta A tem saldo R$ 100,00, quando o usuário registra uma entrada de R$ 50,00 na conta A, então o saldo de A passa a R$ 150,00 e o total de entradas do mês aumenta em R$ 50,00.

**CA-02 — Saída atualiza saldo**
Dado que a conta A tem saldo R$ 100,00, quando o usuário registra uma saída de R$ 30,00 com origem na conta A, então o saldo de A passa a R$ 70,00 e o total de saídas do mês aumenta em R$ 30,00.

**CA-03 — Transferência não afeta totais**
Dado que a conta A tem R$ 100,00 e a carteira tem R$ 20,00, quando o usuário transfere R$ 40,00 de A para a carteira, então A fica com R$ 60,00, a carteira com R$ 60,00, o saldo total continua em R$ 120,00 e os totais de entradas e saídas não mudam.

**CA-04 — Transferência entre a mesma conta é rejeitada**
Dado que o usuário escolhe a mesma conta como origem e destino, quando tenta salvar a transferência, então o sistema impede o registro e informa o motivo.

**CA-05 — Exclusão de entrada**
Dado que existe uma entrada de R$ 50,00 na conta A, quando o usuário a exclui, então o saldo de A diminui R$ 50,00 e o total de entradas do período diminui R$ 50,00.

**CA-06 — Exclusão de saída**
Dado que existe uma saída de R$ 30,00 com origem na conta A, quando o usuário a exclui, então o saldo de A aumenta R$ 30,00 e o total de saídas do período diminui R$ 30,00.

**CA-07 — Estorno de entrada**
Dado que existe uma entrada de R$ 50,00 na conta A, quando o usuário a estorna, então o resultado é idêntico ao da exclusão (CA-05).

**CA-08 — Estorno de saída**
Dado que existe uma saída de R$ 30,00 com origem na conta A, quando o usuário a estorna, então R$ 30,00 voltam à conta A e o total de saídas diminui R$ 30,00.

**CA-09 — Exclusão ou edição de transferência**
Dado que existe uma transferência de R$ 40,00 de A para B, quando o usuário a exclui, então A recupera R$ 40,00 e B perde R$ 40,00.

**CA-10 — Edição de lançamento antigo**
Dado que existe uma saída de R$ 30,00 em mês anterior, quando o usuário altera o valor para R$ 45,00, então os saldos e os totais do mês e dos períodos que o contêm são recalculados e refletem R$ 45,00.

**CA-11 — Saldo negativo permitido com aviso**
Dado que a conta A tem R$ 20,00, quando o usuário registra uma saída de R$ 50,00 com origem em A, então o lançamento é salvo, o saldo de A passa a −R$ 30,00 e um aviso em destaque é exibido.

**CA-12 — Campos obrigatórios**
Dado que o usuário deixa valor, data, conta/fundo ou tipo/origem em branco, quando tenta salvar, então o sistema não registra o lançamento e indica os campos pendentes. Referência e observações em branco não impedem o registro.

**CA-13 — Dashboard**
Dado que há lançamentos no mês, quando o usuário abre o sistema, então vê a data de hoje, os saldos por conta, o saldo total, as movimentações recentes e os links para os últimos três meses registrados.

**CA-14 — Filtro de período na análise**
Dado que o usuário seleciona "trimestre" na análise, quando confirma, então os totais de entradas por origem e de saídas por tipo refletem apenas os três meses escolhidos.

**CA-15 — Categorias sugeridas**
Dado que o usuário acessa a configuração pela primeira vez, quando abre os tipos de despesa, então encontra o conjunto inicial sugerido e pode editá-lo.

## 7. Requisitos não funcionais

- **RNF-01 Simplicidade:** o fluxo de registro de um lançamento deve ser curto e sem termos técnicos.
- **RNF-02 Uso local:** na v1 os dados ficam apenas no dispositivo do usuário e não são enviados a terceiros.
- **RNF-03 Acesso:** login para uso exclusivo de uma única pessoa.
- **RNF-04 Privacidade:** por tratar de dados financeiros pessoais, o sistema segue os princípios da LGPD (minimização de dados, finalidade clara, controle do usuário).
- **RNF-05 Precisão:** cálculos monetários exatos (ver RN-14).

## 8. Questões em aberto

1. O que acontece ao excluir uma conta/fundo que já possui lançamentos (bloquear, arquivar ou excluir tudo)?
2. São permitidos lançamentos com data futura?
3. O que acontece com os lançamentos ao excluir um tipo de despesa ou meio de recebimento em uso?

## 9. Evoluções futuras

- Cartões de crédito e seus saldos.
- Destaque das despesas mais altas que poderiam ser evitadas.
- Novos tipos de análise de dados.
