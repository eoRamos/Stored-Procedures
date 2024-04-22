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

CREATE TABLE Curso (
    id_curso INT PRIMARY KEY AUTO_INCREMENT,
    nome_curso VARCHAR(100)
);

CREATE TABLE Professor (
    id_professor INT PRIMARY KEY AUTO_INCREMENT,
    nome_professor VARCHAR(100)
);

CREATE TABLE Aluno (
    id_aluno INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(50),
    sobrenome VARCHAR(50),
    email VARCHAR(100),
    id_curso INT,
    FOREIGN KEY (id_curso) REFERENCES Curso(id_curso)
);
-- Stored Procedure para inserir um novo curso
DELIMITER //
CREATE PROCEDURE InserirCurso (
    IN nome_curso_param VARCHAR(100)
)
BEGIN
    INSERT INTO Curso (nome_curso) VALUES (nome_curso_param);
END //
DELIMITER ;

-- Stored Procedure para selecionar todos os cursos
DELIMITER //
CREATE PROCEDURE SelecionarCursos ()
BEGIN
    SELECT * FROM Curso;
END //
DELIMITER ;

CREATE FUNCTION GerarEmail (
    nome VARCHAR(50),
    sobrenome VARCHAR(50)
)
RETURNS VARCHAR(100)
BEGIN
    DECLARE count_same_name INT;
    DECLARE email VARCHAR(100);
    
    SET count_same_name = (SELECT COUNT(*) FROM Aluno WHERE nome = nome AND sobrenome = sobrenome);
    
    IF count_same_name > 1 THEN
        SET email = CONCAT(nome, '.', sobrenome, count_same_name, '@dominio.com');
    ELSE
        SET email = CONCAT(nome, '.', sobrenome, '@dominio.com');
    END IF;
    
    RETURN email;
END;
