<h1>:boom:Hospital-Fundamental:boom:</h1>
<h3>Parte 1 - desenvolvimento do DER</h3>
Um pequeno hospital local busca desenvolver um novo sistema que atenda melhor às suas necessidades. Atualmente, parte da operação ainda se apoia em planilhas e arquivos antigos, mas espera-se que esses dados sejam transferidos para o novo sistema assim que ele estiver funcional. Neste momento, é necessário analisar com cuidado as necessidades desse cliente e sugerir uma estrutura de banco de dados adequada por meio de um Diagrama Entidade-Relacionamento.<br></br>

<a href="https://ibb.co/6FjH05M"><img src="https://i.ibb.co/NKc92zv/Captura-de-tela-2024-11-11-181717.png" alt="Captura-de-tela-2024-11-11-181717" border="0"></a><br/>

# Os Segredos do Hospital
<h3>Parte 2 - expansão do DER e Script</h3>
Com base no diagrama desenvolvido anterior, conectamos com o novo diagrama. Por último, criamos um script SQL para a geração do banco de dados e para instruções de montagem de cada uma das entidades/tabelas presentes no diagrama completo.<br></br>
<a href="https://ibb.co/mH05V4V"><img src="https://i.ibb.co/xMYqt6t/hospital2.png" alt="hospital2" border="0"></a><br /><a target='_blank' href='https://imgbb.com/'></a><br />

<h2>:panda_face:Script do hospital:panda_face:</h2>
  
```sql
CREATE DATABASE Hospital;
USE Hospital;

-- Tabela de Médicos
CREATE TABLE Medico (
    medico_id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    datanasc DATE,
    endereco VARCHAR(255),
    telefone VARCHAR(20),
    email VARCHAR(100),
    tipo VARCHAR(50)
);

-- Tabela de Especialidades
CREATE TABLE Especialidade (
    especialidade_id INT PRIMARY KEY,
    descricao VARCHAR(100) NOT NULL
);

-- Tabela de Consultas
CREATE TABLE Consulta (
    consulta_id INT PRIMARY KEY,
    convenio_id INT,
    especialidade_id INT,
    paciente_id INT,
    medico_id INT,
    num_carteira VARCHAR(50),
    dt_hr DATETIME,
    valor_consult DECIMAL(10, 2),
    FOREIGN KEY (convenio_id) REFERENCES Convenio(convenio_id),
    FOREIGN KEY (especialidade_id) REFERENCES Especialidade(especialidade_id),
    FOREIGN KEY (paciente_id) REFERENCES Paciente(paciente_id),
    FOREIGN KEY (medico_id) REFERENCES Medico(medico_id)
);

-- Tabela de Receitas
CREATE TABLE Receita (
    receita_id INT PRIMARY KEY,
    consulta_id INT,
    data_emissao DATE,
    FOREIGN KEY (consulta_id) REFERENCES Consulta(consulta_id)
);

-- Tabela de Medicamentos
CREATE TABLE Medicamento (
    med_id INT PRIMARY KEY,
    nome VARCHAR(100),
    descricao TEXT
);

-- Tabela de Itens de Receita
CREATE TABLE Item_Receita (
    item_id INT PRIMARY KEY,
    quantidade INT,
    instrucao TEXT,
    med_id INT,
    receita_id INT,
    FOREIGN KEY (med_id) REFERENCES Medicamento(med_id),
    FOREIGN KEY (receita_id) REFERENCES Receita(receita_id)
);

-- Tabela de Convênios
CREATE TABLE Convenio (
    convenio_id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cnpj VARCHAR(18),
    tempo_carencia INT
);

-- Tabela de Pacientes
CREATE TABLE Paciente (
    paciente_id INT PRIMARY KEY,
    convenio_id INT,
    telefone VARCHAR(20),
    endereco VARCHAR(255),
    datanasc DATE,
    email VARCHAR(100),
    nome VARCHAR(100),
    rg VARCHAR(20),
    cpf VARCHAR(20),
    FOREIGN KEY (convenio_id) REFERENCES Convenio(convenio_id)
);

-- Tabela de Tipos de Quarto
CREATE TABLE Tipo_Quarto (
    id_tipo INT PRIMARY KEY,
    valor_diaria DECIMAL(10, 2),
    descricao VARCHAR(100)
);

-- Tabela de Quartos
CREATE TABLE Quarto (
    id_quarto INT PRIMARY KEY,
    numero INT,
    tipo INT,
    FOREIGN KEY (tipo) REFERENCES Tipo_Quarto(id_tipo)
);

-- Tabela de Enfermeiros
CREATE TABLE Enfermeiro (
    id_enfe INT PRIMARY KEY,
    nome VARCHAR(100),
    cpf VARCHAR(20),
    cne VARCHAR(20)
);

-- Tabela de Internações
CREATE TABLE Internacao (
    id_inter INT PRIMARY KEY,
    data_entrada DATE,
    data_prev_alta DATE,
    data_alta DATE,
    procedimento TEXT
);

```
