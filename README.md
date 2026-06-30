# 📋 Roteiro de Testes — Formulário de Cadastro de Cliente

> **Disciplina:** Engenharia de Qualidade e Casos de Teste  
> **Sistema:** CLIENT-MANAGER v2.0  
> **Tela:** Gestão de Clientes | Novo Cadastro  
> **Modelo:** UNIQ-MOD-2024-01  
> **Responsável QA:** Q.A  

---

## 1. Contextualização Técnica

A confiabilidade de um sistema depende diretamente da cobertura e da precisão de seus casos de teste. Este documento estabelece o plano de verificação e validação (V&V) da tela de **Cadastro de Cliente**, mapeando cenários positivos (fluxos de sucesso) e negativos (fluxos de exceção), com o objetivo de garantir que o comportamento do sistema seja idêntico ao especificado na fase de levantamento de requisitos.

---

## 2. Escopo da Tela

### 2.1 Campos de Entrada

| Campo | Tipo | Obrigatório | Observações |
|---|---|---|---|
| Identificação (CPF/CNPJ) | Alfanumérico com máscara | ✅ Sim | Formato: `000.000.000-00` (CPF) ou `00.000.000/0000-00` (CNPJ) |
| Data de Cadastro | Data | ✅ Sim | Formato: `DD/MM/AAAA` |
| Nome do Cliente | Texto | ✅ Sim | Campo alfanumérico |
| Número | Alfanumérico | ✅ Sim | Número do endereço |
| Endereço (Logradouro) | Texto | ✅ Sim | Campo alfanumérico |
| Complemento | Texto | ❌ Não | Campo opcional |
| Bairro | Texto | ✅ Sim | Campo alfanumérico |
| Telefone Fixo | Numérico com máscara | ❌ Não | Formato: `(00) 0000-0000` |
| Cidade | Texto | ✅ Sim | Campo alfanumérico |
| Estado | Lista (Select) | ✅ Sim | Sigla UF — ex.: `PR - Paraná` |
| Celular | Numérico com máscara | ❌ Não | Formato: `(00) 00000-0000` |

### 2.2 Botões de Ação

| Botão | Ícone | Função Esperada |
|---|---|---|
| Incluir | ➕ | Habilita o formulário em modo de criação |
| Editar | ✏️ | Habilita o formulário em modo de edição |
| Excluir | 🗑️ | Remove o registro selecionado |
| Confirmar | ✅ | Salva/persiste os dados inseridos ou editados |
| Cancelar | ✖️ | Descarta as alterações e reseta o formulário |
| Fechar | 🚪 | Fecha o formulário e retorna à tela anterior |

---

## 3. Roteiro de Testes

### 3.1 Cenários de Validação de Campos (Inputs)

#### CT-001 — Incluir com todos os campos obrigatórios preenchidos corretamente

| Atributo | Valor |
|---|---|
| **Categoria** | Positivo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Inputs |
| **Criticidade** | Alta |

**Pré-condição:** Formulário em modo de inclusão (botão Incluir acionado).  
**Passos:**
1. Preencher CPF com `876.543.210-98`
2. Preencher Nome do Cliente com `Ana Júlia Pereira de Souza`
3. Preencher Endereço com `Rua das Tulipas`
4. Preencher Número com `401`
5. Preencher Complemento com `Ap. 401, Bloco C`
6. Preencher Bairro com `Jardim Botânico`
7. Selecionar Estado `PR - Paraná`
8. Preencher Cidade com `Curitiba`
9. Preencher Telefone Fixo com `(41) 3000-4000`
10. Preencher Celular com `(41) 98000-9000`
11. Clicar em **Confirmar**

**Resultado Esperado:** Registro salvo com sucesso; campos limpos; botões de ação padrão habilitados.

---

#### CT-002 — Incluir com campo "Nome do Cliente" em branco

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Inputs / Obrigatoriedade |
| **Criticidade** | Alta |

**Pré-condição:** Formulário em modo de inclusão.  
**Passos:**
1. Deixar o campo "Nome do Cliente" em branco
2. Preencher os demais campos obrigatórios normalmente
3. Clicar em **Confirmar**

**Resultado Esperado:** Sistema não salva o registro; exibe mensagem de alerta informando a obrigatoriedade do campo "Nome do Cliente"; foco retorna ao campo em branco.

---

#### CT-003 — Inserir CPF com formato inválido

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Campo / Máscara / Formato |
| **Criticidade** | Alta |

**Pré-condição:** Formulário em modo de inclusão.  
**Passos:**
1. Inserir no campo CPF/CNPJ um valor com formato incorreto (ex.: `1234`, letras, ou sequência com menos de 11 dígitos como `111.111.111-11`)
2. Clicar fora do campo ou tentar avançar

**Resultado Esperado:** Sistema exibe mensagem informando que o CPF/CNPJ é inválido e impede o envio do formulário.

---

#### CT-004 — Inserir CPF/CNPJ já cadastrado no sistema

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Unicidade / Regra de Negócio |
| **Criticidade** | Média |

**Pré-condição:** Já existe um cliente cadastrado com o CPF `876.543.210-98`.  
**Passos:**
1. Preencher o campo CPF/CNPJ com `876.543.210-98`
2. Preencher os demais campos
3. Clicar em **Confirmar**

**Resultado Esperado:** Sistema exibe mensagem de erro informando que o cliente com esse CPF/CNPJ já está cadastrado (duplicidade); registro não é salvo.

---

#### CT-005 — Inserir telefone com caracteres não numéricos

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Tipo de Dado / Máscara |
| **Criticidade** | Média |

**Pré-condição:** Formulário em modo de inclusão.  
**Passos:**
1. Tentar digitar letras ou caracteres especiais no campo "Telefone Fixo" ou "Celular"

**Resultado Esperado:** O campo rejeita a entrada; apenas dígitos numéricos são aceitos, com a máscara aplicada automaticamente.

---

#### CT-006 — Selecionar Estado sem escolher nenhuma opção

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Unidade |
| **Funcionalidade** | Validação de Obrigatoriedade / Select |
| **Criticidade** | Alta |

**Pré-condição:** Formulário em modo de inclusão.  
**Passos:**
1. Preencher todos os campos, exceto o campo "Estado"
2. Clicar em **Confirmar**

**Resultado Esperado:** Sistema não salva o registro; exibe mensagem de alerta indicando que o campo "Estado" é obrigatório.

---

### 3.2 Cenários de Validação da Máquina de Estados (Botões de Ação)

#### CT-007 — Acionar botão Cancelar em modo de inclusão

| Atributo | Valor |
|---|---|
| **Categoria** | Positivo |
| **Tipo** | Unidade |
| **Funcionalidade** | Cancelar |
| **Criticidade** | Média |

**Pré-condição:** Formulário em modo de inclusão com alguns campos preenchidos.  
**Passos:**
1. Preencher parcialmente os campos do formulário
2. Clicar em **Cancelar**

**Resultado Esperado:** Todas as alterações são descartadas; campos limpos/resetados; botões de ação padrão habilitados novamente.

---

#### CT-008 — Acionar botão Fechar com formulário aberto

| Atributo | Valor |
|---|---|
| **Categoria** | Positivo |
| **Tipo** | Sistema |
| **Funcionalidade** | Fechar |
| **Criticidade** | Média |

**Pré-condição:** Formulário de cadastro aberto (qualquer estado).  
**Passos:**
1. Clicar no botão **Fechar**

**Resultado Esperado:** A janela do formulário é fechada; sistema retorna à tela principal ou à tela anterior.

---

#### CT-009 — Acionar botão Editar com registro selecionado

| Atributo | Valor |
|---|---|
| **Categoria** | Positivo |
| **Tipo** | Unidade |
| **Funcionalidade** | Editar |
| **Criticidade** | Alta |

**Pré-condição:** Existe ao menos um registro cadastrado; registro selecionado na lista.  
**Passos:**
1. Selecionar um cliente na lista
2. Clicar em **Editar**
3. Alterar um ou mais campos
4. Clicar em **Confirmar**

**Resultado Esperado:** Os dados alterados são persistidos corretamente; o sistema exibe mensagem de sucesso; o formulário retorna ao estado de leitura.

---

#### CT-010 — Acionar botão Excluir com registro selecionado

| Atributo | Valor |
|---|---|
| **Categoria** | Positivo |
| **Tipo** | Unidade |
| **Funcionalidade** | Excluir |
| **Criticidade** | Alta |

**Pré-condição:** Existe ao menos um registro cadastrado; registro selecionado.  
**Passos:**
1. Selecionar um cliente
2. Clicar em **Excluir**
3. Confirmar a exclusão (se houver diálogo de confirmação)

**Resultado Esperado:** O registro é removido do sistema; a lista é atualizada sem o cliente excluído.

---

#### CT-011 — Acionar botão Confirmar sem estar no modo Incluir ou Editar

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Sistema |
| **Funcionalidade** | Confirmar / Máquina de Estados |
| **Criticidade** | Média |

**Pré-condição:** Formulário em modo de visualização (sem ação ativa).  
**Passos:**
1. Sem acionar Incluir ou Editar, tentar clicar em **Confirmar**

**Resultado Esperado:** O botão **Confirmar** está desabilitado ou não produz efeito; nenhuma ação é executada.

---

#### CT-012 — Acionar botão Editar sem selecionar um registro

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Sistema |
| **Funcionalidade** | Editar / Máquina de Estados |
| **Criticidade** | Média |

**Pré-condição:** Nenhum registro selecionado na lista.  
**Passos:**
1. Clicar em **Editar** sem selecionar nenhum cliente

**Resultado Esperado:** Sistema exibe mensagem solicitando que o usuário selecione um registro antes de editar; formulário não é aberto em modo de edição.

---

#### CT-013 — Acionar botão Excluir sem selecionar um registro

| Atributo | Valor |
|---|---|
| **Categoria** | Negativo |
| **Tipo** | Sistema |
| **Funcionalidade** | Excluir / Máquina de Estados |
| **Criticidade** | Média |

**Pré-condição:** Nenhum registro selecionado.  
**Passos:**
1. Clicar em **Excluir** sem selecionar nenhum cliente

**Resultado Esperado:** Sistema exibe mensagem informando que nenhum registro foi selecionado; nenhuma exclusão é realizada.

---

## 4. Resumo Consolidado dos Casos de Teste

| ID | Funcionalidade | Categoria | Criticidade | Status |
|---|---|---|---|---|
| CT-001 | Incluir — campos obrigatórios preenchidos | Positivo | Alta | 🟢 A Executar |
| CT-002 | Incluir — Nome do Cliente em branco | Negativo | Alta | 🟢 A Executar |
| CT-003 | Validar Campo — CPF/CNPJ inválido | Negativo | Alta | 🟢 A Executar |
| CT-004 | Incluir — CPF/CNPJ duplicado | Negativo | Média | 🟢 A Executar |
| CT-005 | Validar Campo — Telefone com caracteres inválidos | Negativo | Média | 🟢 A Executar |
| CT-006 | Validar Campo — Estado não selecionado | Negativo | Alta | 🟢 A Executar |
| CT-007 | Cancelar — descartar alterações | Positivo | Média | 🟢 A Executar |
| CT-008 | Fechar — retorno à tela anterior | Positivo | Média | 🟢 A Executar |
| CT-009 | Editar — alterar e confirmar registro | Positivo | Alta | 🟢 A Executar |
| CT-010 | Excluir — remoção de registro | Positivo | Alta | 🟢 A Executar |
| CT-011 | Confirmar — sem modo ativo | Negativo | Média | 🟢 A Executar |
| CT-012 | Editar — sem registro selecionado | Negativo | Média | 🟢 A Executar |
| CT-013 | Excluir — sem registro selecionado | Negativo | Média | 🟢 A Executar |

---

## 5. Critérios de Aceitação

- Todos os campos marcados com `*` devem ser obrigatórios e validados antes do envio.
- Máscaras de CPF, CNPJ, telefone e celular devem ser aplicadas automaticamente durante a digitação.
- O sistema deve impedir o cadastro de CPF/CNPJ duplicados.
- Os botões de ação devem respeitar a máquina de estados (habilitados/desabilitados conforme o contexto).
- Mensagens de erro devem ser claras, objetivas e apontar o campo ou a ação responsável pelo problema.
- Ao cancelar ou fechar, nenhuma alteração não confirmada deve ser persistida.

---

## 6. Ambiente e Ferramentas

| Item | Descrição |
|---|---|
| **Sistema** | CLIENT-MANAGER v2.0 |
| **Ambiente** | QA / Homologação |
| **Tipo de Teste** | Funcional — Caixa Preta |
| **Responsável** | Q.A |
| **Técnica** | Partição de Equivalência + Análise de Valor Limite |

---
