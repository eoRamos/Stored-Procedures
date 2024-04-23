Titulo:
Aprendendo Stored Procedures
Descrição:
Criar um banco no qual Stored Procedures seja necessário 

Identificação de Entidades, Atributos e Relacionamentos:
Entidades:

Aluno
Curso
Professor
Atributos:

Aluno: id_aluno (chave primária), nome, sobrenome, email, id_curso (chave estrangeira referenciando o curso que o aluno está matriculado)
Curso: id_curso (chave primária), nome_curso
Professor: id_professor (chave primária), nome_professor
Relacionamentos:

Um aluno está matriculado em um curso.
Um curso é ministrado por um professor.

Programa utilizado: 
MySqlWorkBench

Nome:Giovanni Ferreira Ramos - 222438


CREATE TABLE Professor (
    ID_professor INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(50),
    Sobrenome VARCHAR(50),
    Email VARCHAR(100)
);

CREATE TABLE Curso (
    ID_curso INT PRIMARY KEY AUTO_INCREMENT,
    Nome_do_Curso VARCHAR(100),
    Professor_ID INT,
    FOREIGN KEY (Professor_ID) REFERENCES Professor(ID_professor)
);

CREATE TABLE Aluno (
    ID_aluno INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(50),
    Sobrenome VARCHAR(50),
    Email VARCHAR(100),
    Curso_ID INT,
    FOREIGN KEY (Curso_ID) REFERENCES Curso(ID_curso)
);

-- Stored Procedure para inserção de cursos
DELIMITER //
CREATE PROCEDURE InserirCurso (
    IN curso_nome VARCHAR(100),
    IN professor_nome VARCHAR(50),
    IN professor_sobrenome VARCHAR(50)
)
BEGIN
    DECLARE professor_id INT;
    INSERT INTO Professor (Nome, Sobrenome, Email)
    VALUES (professor_nome, professor_sobrenome, CONCAT(LOWER(professor_nome), '.', LOWER(professor_sobrenome), '@dominio.com'));
    SET professor_id = LAST_INSERT_ID();
    INSERT INTO Curso (Nome_do_Curso, Professor_ID)
    VALUES (curso_nome, professor_id);
END //
DELIMITER ;

-- Stored Procedure para seleção de cursos
DELIMITER //
CREATE PROCEDURE SelecionarCursos ()
BEGIN
    SELECT * FROM Curso;
END //
DELIMITER ;

-- Stored Procedure para inserção de alunos com email único
DELIMITER //
CREATE PROCEDURE InserirAluno (
    IN aluno_nome VARCHAR(50),
    IN aluno_sobrenome VARCHAR(50),
    IN curso_id INT
)
BEGIN
    DECLARE aluno_count INT;
    DECLARE aluno_email VARCHAR(100);
    SET aluno_count = (SELECT COUNT(*) FROM Aluno WHERE Nome = aluno_nome AND Sobrenome = aluno_sobrenome);
    IF aluno_count > 0 THEN
        SET aluno_email = CONCAT(LOWER(aluno_nome), '.', LOWER(aluno_sobrenome), aluno_count + 1, '@dominio.com');
    ELSE
        SET aluno_email = CONCAT(LOWER(aluno_nome), '.', LOWER(aluno_sobrenome), '@dominio.com');
    END IF;
    INSERT INTO Aluno (Nome, Sobrenome, Email, Curso_ID)
    VALUES (aluno_nome, aluno_sobrenome, aluno_email, curso_id);
END //
DELIMITER ;
