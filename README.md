# SRCM - Sistema de Redes de Clínicas Médicas

## 📋 Descrição

Sistema de banco de dados relacional completo para gestão de clínicas médicas, desenvolvido em PostgreSQL. O projeto contempla desde o cadastro de pacientes e profissionais de saúde até o controle de agendamentos, atendimentos, prontuários e convênios.

## 🎯 Objetivo

Fornecer uma estrutura de dados robusta e normalizada para clínicas que necessitam gerenciar:
- Cadastro de pacientes, médicos, enfermeiros e técnicos em radiologia
- Agendamento e controle de consultas, triagens e exames
- Prontuário eletrônico com histórico completo
- Gestão de convênios e coberturas
- Emissão de documentos médicos (atestados, prescrições, laudos)
- Controle financeiro de serviços e pagamentos

## 🏗️ Arquitetura

### Módulos Principais

#### 👥 Gestão de Pessoas
- **PESSOA**: Cadastro base com dados pessoais
- **PACIENTE**: Pacientes atendidos pela clínica
- **FUNCIONARIO**: Colaboradores da clínica
- **MEDICO, ENFERMEIRO, TEC_RADIOLOGIA**: Especializações de funcionários
- **ADMINISTRADOR**: Gestores do sistema

#### 🏥 Gestão Clínica
- **CLINICA**: Dados das unidades de atendimento
- **CLINICA_PESSOA**: Vínculo entre pessoas e clínicas
- **SERVICO**: Catálogo de serviços oferecidos
- **AGENDAMENTO**: Controle de horários e status

#### 🩺 Atendimento Médico
- **ATENDIMENTO**: Registro de atendimentos realizados
- **TRIAGEM**: Avaliação inicial por enfermeiros
- **CONSULTA**: Atendimento médico com anamnese e diagnóstico
- **EXAME**: Exames radiológicos e laboratoriais
- **DOCUMENTO**: Atestados, prescrições e laudos

#### 💼 Gestão Financeira
- **CONVENIO**: Cadastro de planos de saúde
- **COBERTURA**: Serviços cobertos por cada convênio
- **VINCULO**: Cartões e vínculos de pacientes com convênios
- **ITEM**: Valores de serviços por atendimento

#### 📄 Prontuário
- **PRONTUARIO**: Histórico clínico do paciente
- **ATUALIZACAO_PRONTUARIO**: Auditoria de modificações

## ✨ Características Técnicas

### Normalização
- ✅ **3FN (Terceira Forma Normal)** - Eliminação de dependências transitivas
- ✅ Atributos derivados calculados dinamicamente (valor_total, dia_semana)
- ✅ Separação adequada de entidades e relacionamentos

### Integridade de Dados
- 🔒 **Chaves primárias** e **estrangeiras** em todas as relações
- ✅ **Constraints CHECK** para validação de domínios
- ✅ Validação de valores enumerados (status, tipos)
- ✅ Validação de valores monetários não-negativos
- ✅ Validação de períodos de datas
- ✅ Lógica XOR para médicos internos vs externos

### Rastreabilidade
- 📅 Campos de auditoria (data_cadastro, data_atualizacao)
- 👤 Registro de responsáveis por alterações
- 📊 Histórico completo de atualizações no prontuário

### Flexibilidade
- 🔄 Suporte a múltiplas clínicas
- 💳 Gestão de múltiplos convênios por paciente
- 📞 Múltiplos contatos (telefones e e-mails)
- 🏥 Médicos internos e externos
- 🌈 Campos inclusivos (nome_social, sexo com opção 'Outro')

## 🗄️ Tecnologias

- **PostgreSQL 12+**
- **SQL/DDL** com constraints avançadas
- Tipos de dados otimizados (timestamp, decimal, boolean)
- Identity columns para auto-incremento

## 📊 Diagrama ER

O schema contém **29 tabelas** organizadas em 5 módulos funcionais, com relacionamentos 1:N e N:N adequadamente modelados através de tabelas associativas.

## 🚀 Como Usar

### Instalação

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/srcm-database.git

# Acesse o diretório
cd srcm-database

# Execute o script no PostgreSQL
psql -U seu_usuario -d sua_database -f SRCM.sql
```

### Pré-requisitos

- PostgreSQL 12 ou superior
- Cliente psql ou ferramenta de gestão de banco de dados (DBeaver, pgAdmin)

## 📝 Exemplos de Uso

### Consultar agendamentos com dia da semana
```sql
SELECT 
  id_agendamento,
  data_hora_prevista,
  TO_CHAR(data_hora_prevista, 'TMDay') AS dia_semana,
  tipo_agendamento,
  status_agendamento
FROM AGENDAMENTO
WHERE data_hora_prevista >= CURRENT_DATE;
```

### Calcular valor total de um agendamento
```sql
SELECT 
  a.id_agendamento,
  p.nome_pessoa,
  SUM(i.valor_paciente + i.valor_convenio) AS valor_total
FROM AGENDAMENTO a
JOIN PACIENTE pac ON a.id_paciente = pac.id_paciente
JOIN PESSOA p ON pac.cpf = p.cpf
JOIN ITEM i ON a.id_agendamento = i.id_agendamento
GROUP BY a.id_agendamento, p.nome_pessoa;
```

### Histórico completo de um paciente
```sql
SELECT 
  at.data_hora_atendimento,
  at.tipo_atendimento,
  c.diagnostico,
  c.cid,
  m.nome_pessoa AS medico
FROM ATENDIMENTO at
JOIN PACIENTE p ON at.id_paciente = p.id_paciente
LEFT JOIN CONSULTA c ON at.id_atendimento = c.id_atendimento
LEFT JOIN PESSOA m ON c.cpf_medico = m.cpf
WHERE p.cpf = '12345678901'
ORDER BY at.data_hora_atendimento DESC;
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para:
- Reportar bugs
- Sugerir melhorias
- Submeter pull requests
- Adicionar queries úteis de exemplo

## 📄 Licença

Este projeto está sob a licença MIT.

## 👨‍💻 Autor

Desenvolvido como projeto acadêmico de modelagem de banco de dados para sistemas de saúde.  
•	Bruno Gonçalves Bezerra  
•	Eduardo Mendes Souza  
•	Emerson Moron Rosa  
•	Letícia Meneses Marques  
INSTITUTO FEDERAL DE EDUCAÇÃO, CIÊNCIA E TECNOLOGIA DE BRASÍLIA, CAMPUS BRASÍLIA  
CURSO TÉCNICO EM DESENVOLVIMENTO DE SISTEMAS  

## 📧 Contato

Para dúvidas ou sugestões, abra uma issue no repositório.

---

⭐ Se este projeto foi útil para você, considere dar uma estrela no repositório!
