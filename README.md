# :boom: Hospital-Fundamental :boom:
<h3>Parte 1 - desenvolvimento do DER</h3>
Um pequeno hospital local busca desenvolver um novo sistema que atenda melhor às suas necessidades. Atualmente, parte da operação ainda se apoia em planilhas e arquivos antigos, mas espera-se que esses dados sejam transferidos para o novo sistema assim que ele estiver funcional. Neste momento, é necessário analisar com cuidado as necessidades desse cliente e sugerir uma estrutura de banco de dados adequada por meio de um Diagrama Entidade-Relacionamento.<br></br>

<a href="https://ibb.co/6FjH05M"><img src="https://i.ibb.co/NKc92zv/Captura-de-tela-2024-11-11-181717.png" alt="Captura-de-tela-2024-11-11-181717" border="0"></a><br/>

# Os Segredos do Hospital - Parte 2
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
</br>

# PARTE 3 - Alimentando o banco de dados :pushpin:

Alimentar o banco é a proxima etapa.</br>

```sql
-- Inserindo Especialidades
INSERT INTO Especialidade (especialidade_id, descricao) VALUES 
(1, 'Pediatria'),
(2, 'Clínica Geral'),
(3, 'Gastroenterologia'),
(4, 'Dermatologia'),
(5, 'Cardiologia'),
(6, 'Ortopedia'),
(7, 'Neurologia');

-- Inserindo Médicos
INSERT INTO Medico (medico_id, nome, datanasc, endereco, telefone, email, tipo) VALUES 
(1, 'Dr. Carlos Silva', '1975-02-10', 'Rua A, 123', '11987654321', 'carlos.silva@hospital.com', 'Médico'),
(2, 'Dra. Ana Souza', '1980-05-15', 'Rua B, 234', '11987654322', 'ana.souza@hospital.com', 'Médico'),
(3, 'Dr. João Pedro', '1972-08-20', 'Rua C, 345', '11987654323', 'joao.pedro@hospital.com', 'Médico'),
(4, 'Dra. Beatriz Gomes', '1982-11-25', 'Rua D, 456', '11987654324', 'beatriz.gomes@hospital.com', 'Médico'),
(5, 'Dr. Paulo Mendes', '1978-03-10', 'Rua E, 567', '11987654325', 'paulo.mendes@hospital.com', 'Médico'),
(6, 'Dra. Carla Moraes', '1985-06-30', 'Rua F, 678', '11987654326', 'carla.moraes@hospital.com', 'Médico'),
(7, 'Dr. Eduardo Costa', '1977-09-15', 'Rua G, 789', '11987654327', 'eduardo.costa@hospital.com', 'Médico'),
(8, 'Dra. Luciana Lima', '1981-12-05', 'Rua H, 890', '11987654328', 'luciana.lima@hospital.com', 'Médico'),
(9, 'Dr. Ricardo Alves', '1983-04-18', 'Rua I, 901', '11987654329', 'ricardo.alves@hospital.com', 'Médico'),
(10, 'Dra. Fernanda Martins', '1979-07-22', 'Rua J, 101', '11987654330', 'fernanda.martins@hospital.com', 'Médico');

-- Inserindo Relação entre Médico e Especialidade
INSERT INTO Medico_Especialidade (medico_id, especialidade_id) VALUES 
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 1),
(9, 2),
(10, 3);

-- Inserindo Convênios
INSERT INTO Convenio (convenio_id, nome, cnpj, tempo_carencia) VALUES 
(1, 'Unimed', '12345678000101', 30),
(2, 'Amil', '98765432000199', 45),
(3, 'Bradesco Saúde', '11223344000177', 60),
(4, 'SulAmérica', '22334455000155', 90);

-- Inserindo Pacientes
INSERT INTO Paciente (paciente_id, convenio_id, telefone, endereco, datanasc, email, nome, rg, cpf) VALUES 
(1, 1, '11912345678', 'Rua Z, 123', '1990-01-01', 'paciente1@gmail.com', 'Alice Santos', '1234567', '12345678901'),
(2, 2, '11912345679', 'Rua Y, 234', '1992-02-02', 'paciente2@gmail.com', 'Bruno Lima', '2345678', '23456789012'),
(3, 3, '11912345680', 'Rua X, 345', '1985-03-03', 'paciente3@gmail.com', 'Carla Mendes', '3456789', '34567890123'),
(4, 4, '11912345681', 'Rua W, 456', '1978-04-04', 'paciente4@gmail.com', 'Daniel Souza', '4567890', '45678901234'),
(5, 1, '11912345682', 'Rua V, 567', '1982-05-05', 'paciente5@gmail.com', 'Eduarda Silva', '5678901', '56789012345'),
;

-- Inserindo Consultas
INSERT INTO Consulta (consulta_id, convenio_id, especialidade_id, paciente_id, medico_id, num_carteira, dt_hr, valor_consult) VALUES 
(1, 1, 1, 1, 1, 'ABC123', '2015-01-10 14:00:00', 100.00),
(2, 2, 2, 2, 2, 'DEF456', '2015-02-15 10:30:00', 150.00),
(3, 3, 3, 3, 3, 'GHI789', '2015-03-20 09:00:00', 200.00),
(4, 4, 4, 4, 4, 'JKL012', '2016-04-25 15:00:00', 250.00),
(5, 1, 5, 5, 5, 'MNO345', '2016-05-30 08:30:00', 300.00),
;

-- Inserindo Receitas com medicamentos
INSERT INTO Receita (receita_id, consulta_id, data_emissao) VALUES 
(1, 1, '2015-01-10'),
(2, 2, '2015-02-15'),
(3, 3, '2015-03-20'),
;

INSERT INTO Medicamento (med_id, nome, descricao) VALUES 
(1, 'Ibuprofeno', 'Analgésico e anti-inflamatório'),
(2, 'Paracetamol', 'Analgésico e antitérmico'),
(3, 'Amoxicilina', 'Antibiótico de amplo espectro'),

;

INSERT INTO Item_Receita (item_id, quantidade, instrucao, med_id, receita_id) VALUES 
(1, 1, 'Tomar 1 comprimido a cada 8 horas', 1, 1),
(2, 1, 'Tomar 1 comprimido a cada 12 horas', 2, 1),

;

-- Inserindo Tipos de Quarto
INSERT INTO Tipo_Quarto (id_tipo, valor_diaria, descricao) VALUES 
(1, 200.00, 'Apartamento'),
(2, 150.00, 'Quarto Duplo'),
(3, 100.00, 'Enfermaria');

-- Inserindo Quartos
INSERT INTO Quarto (id_quarto, numero, tipo) VALUES 
(1, 101, 1),
(2, 102, 2),
(3, 103, 3);

-- Inserindo Enfermeiros
INSERT INTO Enfermeiro (id_enfe, nome, cpf, cne) VALUES 
(1, 'Enf. Lucas', '12345678901', 'CNE001'),
(2, 'Enf. Maria', '23456789012', 'CNE002');

INSERT INTO Internacao (id_inter, data_entrada, data_prev_alta, data_alta, procedimento, medico_id, paciente_id) VALUES 
(1, '2016-01-15', '2016-01-20', '2016-01-19', 'Cirurgia de Apêndice', 1, 1);

INSERT INTO Internacao_Enfermeiro (id_inter, id_enfe) VALUES 
(1, 1),
(1, 2);

```

# PARTE 4 - Alterando o banco de dados

</br>

```sql
-- Adicionar a coluna "em_atividade" à tabela Medico
ALTER TABLE Medico
ADD COLUMN em_atividade BOOLEAN DEFAULT TRUE;

-- Atualizar o status de atividade dos médicos
-- Definir dois médicos como inativos e os demais como ativos
UPDATE Medico SET em_atividade = FALSE WHERE medico_id IN (1, 2);
UPDATE Medico SET em_atividade = TRUE WHERE medico_id NOT IN (1, 2);
```
