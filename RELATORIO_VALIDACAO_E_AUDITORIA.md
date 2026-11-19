# Relatório de validação estática e auditoria — iFood Grupo 2

## Resultado do processamento do DDL

| Verificação | Resultado |
|---|---:|
| Tabelas `CREATE TABLE` documentadas | 22 |
| Colunas documentadas | 185 |
| Views identificadas | 5 |
| Triggers identificadas | 65 |
| Procedures identificadas | 6 |
| Functions identificadas | 3 |
| Events identificados | 1 |
| Linhas do script SQL original | 2.423 |

Os tipos físicos são extraídos das definições das colunas no DDL (por exemplo, `INT UNSIGNED`, `VARCHAR(100)`, `DECIMAL(10,2)`, `DATETIME`, `JSON`). A extração contempla nulabilidade, PK, FK, UNIQUE, CHECK, DEFAULT, AUTO_INCREMENT e colunas geradas.

## Verificações estáticas concluídas

- Sintaxe JavaScript verificada com `node --check`.
- Presença dos arquivos locais referenciados pelo HTML: `style.css`, `script.js`, PDF de requisitos e `assets/der.png`.
- SQL original embutido no modal do DDL comparado com o `.sql` de origem após decodificação HTML: correspondência integral.
- O JavaScript não usa `fetch()` para buscar o SQL externo.
- A navegação e as seções das tabelas são montadas a partir do inventário extraído do DDL.
- Foram encontrados 22 nomes de tabela e 185 atributos no conjunto gerado.

## Auditoria de coerência documental

### 1. Coluna gerada não registrada no DER e na seção 6.1

O DDL define `endereco_cliente.id_cliente_principal_ativo`, coluna gerada a partir dos indicadores `principal` e `ativo` e utilizada em índice UNIQUE. Ela não consta no DER PNG escolhido nem na lista de atributos da entidade `endereco_cliente` na página 31 da especificação.

**Ajuste necessário:** acrescentar a coluna técnica gerada no DER e na seção 6.1 da especificação, ou modificar o DDL para implementar a unicidade sem essa coluna. O dicionário mantém a coluna porque sua fonte de verdade nesta versão é o DDL.

### 2. Regra de endereço principal exatamente um

A regra RF01 (página 9) exige exatamente um endereço principal entre os endereços ativos. O índice UNIQUE baseado na coluna gerada impede mais de um, mas não garante, sozinho, que exista ao menos um.

**Ajuste necessário:** acrescentar uma operação transacional que garanta ao menos um endereço principal ativo após cadastro/alteração, ou explicitar na especificação que essa condição é garantida na camada de aplicação. Não atribuir essa garantia apenas ao UNIQUE.

### 3. Cobertura de auditoria em entidades com dados pessoais

O DDL tem rotinas de auditoria para cliente, entregador, restaurante, produto, pedido, pagamento, estorno, usuário e cupom; não há cobertura equivalente identificada para `endereco_cliente` e `solicitacao_titular`.

**Ajuste recomendado:** incluir eventos de auditoria para inclusão, alteração e inativação de endereços e para o ciclo de atendimento das solicitações de titular, com mascaramento dos dados pessoais nos snapshots; ou documentar uma justificativa de governança.

### 4. RBAC e privilégios SQL

A matriz por perfil consta nas páginas 20–21 da especificação. O DDL descreve que a aplicação deve aplicar permissões por titular/estabelecimento, mas não provisiona contas/roles e GRANTs para cada perfil.

**Ajuste recomendado:** manter o RBAC de negócio na camada de aplicação e criar um artefato de implantação com roles e privilégios mínimos, incluindo proteção append-only de `log_auditoria` contra UPDATE/DELETE/TRUNCATE.

### 5. Retenção e arquivamento

PR01 estabelece retenção por cinco anos e arquivamento após o prazo. O DDL bloqueia exclusões físicas de registros históricos e programa anonimização de titulares, mas a anonimização não é a mesma operação que arquivamento dos dados transacionais.

**Ajuste recomendado:** documentar destino, controles, evidência de transferência e critérios de descarte/arquivamento dos dados transacionais.

### 6. Requisitos dependentes de infraestrutura e aplicação

Backup/binlog cifrado, TLS 1.3, proteção AES-256 com chaves externas, retries de deadlock, prepared statements e segregação de contas de serviço dependem de infraestrutura e/ou código da aplicação; não podem ser considerados entregues apenas pela criação do esquema.

**Ajuste recomendado:** manter esses controles em uma matriz operacional e obter evidências quando o ambiente estiver disponível.

### 7. Revisão do documento de requisitos

O histórico de revisões da página 2 possui cinco registros (0.1, 0.2, 0.3, 1.0 e 1.1), acima do mínimo indicado no prompt. Na extração textual das 40 páginas não foram identificados placeholders entre colchetes, instruções “COMO UTILIZAR ESTE MODELO” nem textos “Orientação:”. A página 40 usa a expressão “Representante Simulado do iFood”, identificando uma representação acadêmica e não uma assinatura oficial.

**Ajuste recomendado:** depois de resolver as divergências, registrar uma nova revisão sem apagar o histórico existente. Confirmar se a identificação de representante simulado deve permanecer na versão a entregar.

## Limite de validação

O DDL não foi executado em um servidor MySQL nesta entrega. Portanto, não se afirma que tabelas, constraints, views, procedures, functions, triggers ou events foram criados com sucesso, nem que os fluxos transacionais e concorrentes foram testados. A validação realizada é estática e documental. A publicação pública final deve aguardar a resolução das divergências e a validação do DDL num ambiente MySQL compatível.

## Classificação LGPD

As etiquetas `[DP]` e `[N/A]` são classificações contextuais preliminares. Não foram identificadas colunas que representem categorias especiais de dados pessoais nos termos da LGPD; por isso, não há campos marcados `[DPS]`. CPF, telefone, endereço, credenciais e dados transacionais vinculáveis a pessoas continuam sujeitos a controles de proteção. A classificação requer aprovação do responsável de dados do grupo.
