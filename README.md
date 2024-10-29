Registrar_avaliacao.php
  <?php include 'conexao.php'; ?>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $aluno_id = $_POST['aluno_id'];
    $disciplina_id = $_POST['disciplina_id'];
    $nota = $_POST['nota'];

    $sql = "INSERT INTO avaliacoes (aluno_id, disciplina_id, nota) VALUES (?, ?, ?)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$aluno_id, $disciplina_id, $nota]);
    echo "Avaliação registrada com sucesso!";
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Registrar Avaliação</title>
    <link rel="stylesheet" href="css/estilo.css">
</head>
<body>
    <h1>Registrar Avaliação</h1>
    <form method="post">
        Selecione o Aluno:
        <select name="aluno_id">
            <?php
            $alunos = $pdo->query("SELECT * FROM alunos")->fetchAll();
            foreach ($alunos as $aluno) {
                echo "<option value='{$aluno['id']}'>{$aluno['nome']}</option>";
            }
            ?>
        </select><br>

        Selecione a Disciplina:
        <select name="disciplina_id">
            <?php
            $disciplinas = $pdo->query("SELECT * FROM disciplinas")->fetchAll();
            foreach ($disciplinas as $disciplina) {
                echo "<option value='{$disciplina['id']}'>{$disciplina['nome']}</option>";
            }
            ?>
        </select><br>

        Nota: <input type="number" step="0.01" name="nota" required><br>
        <input type="submit" value="Registrar">
    </form>
    <a href="index.php">Voltar</a>
</body>
</html>
  Adicionar_disciplina.php
  <?php include 'conexao.php'; ?>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nome = $_POST['nome'];
    $sql = "INSERT INTO disciplinas (nome) VALUES (?)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$nome]);
    echo "Disciplina adicionada com sucesso!";
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Adicionar Disciplina</title>
    <link rel="stylesheet" href="css/estilo.css">
</head>
<body>
    <h1>Adicionar Nova Disciplina</h1>
    <form method="post">
        Nome: <input type="text" name="nome" required><br>
        <input type="submit" value="Adicionar">
    </form>
    <a href="index.php">Voltar</a>
</body>
</html>
  Adicionar_aluno.php
 <?php include 'conexao.php'; ?>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nome = $_POST['nome'];
    $sql = "INSERT INTO alunos (nome) VALUES (?)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$nome]);
    echo "Aluno adicionado com sucesso!";
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Adicionar Aluno</title>
    <link rel="stylesheet" href="css/estilo.css">
</head>
<body>
    <h1>Adicionar Novo Aluno</h1>
    <form method="post">
        Nome: <input type="text" name="nome" required><br>
        <input type="submit" value="Adicionar">
    </form>
    <a href="index.php">Voltar</a>
</body>
</html>
  Índex.php
 <!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Gerenciamento de Alunos</title>
    <link rel="stylesheet" href="css/estilo.css">
</head>
<body>
    <h1>Bem-vindo ao Sistema de Gerenciamento de Alunos</h1>
    <ul>
        <li><a href="adicionar_aluno.php">Adicionar Aluno</a></li>
        <li><a href="adicionar_disciplina.php">Adicionar Disciplina</a></li>
        <li><a href="registrar_avaliacao.php">Registrar Avaliação</a></li>
    </ul>
</body>
</html>
 Conexão.php
 <?php
$pdo = new PDO("mysql:host=localhost;dbname=escola", "root", "");  // Ajuste o 'root' e a senha conforme sua configuração
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
?>
 Banco de dados
CREATE DATABASE escola;

USE escola;

Banco de dados
CREATE TABLE alunos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE disciplinas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE avaliacoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    aluno_id INT,
    disciplina_id INT,
    nota DECIMAL(4, 2),
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (disciplina_id) REFERENCES disciplinas(id)
);

CREATE TABLE alunos (
  id_alunos INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100)
);

CREATE TABLE disciplinas (
  id_disciplinas INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100)
);

CREATE TABLE avaliacoes (
  id_avaliacoes INT PRIMARY KEY AUTO_INCREMENT,
  nota DECIMAL(5,2),
  id_alunos INT,
  id_disciplinas INT,
  FOREIGN KEY (id_alunos) REFERENCES alunos(id_alunos),
  FOREIGN KEY (id_disciplinas) REFERENCES disciplinas(id_disciplinas)
);

INSERT INTO alunos (nome) VALUES ('Maria Silva');
INSERT INTO alunos (nome) VALUES ('João Souza');
INSERT INTO alunos (nome) VALUES ('Ana Oliveira');

INSERT INTO disciplinas (nome) VALUES ('Matemática');
INSERT INTO disciplinas (nome) VALUES ('Português');
INSERT INTO disciplinas (nome) VALUES ('História');

INSERT INTO avaliacoes (id_alunos, id_disciplinas, nota) VALUES (1, 1, 8.50);
INSERT INTO avaliacoes (id_alunos, id_disciplinas, nota) VALUES (2, 2, 7.00);
INSERT INTO avaliacoes (id_alunos, id_disciplinas, nota) VALUES (3, 3, 9.20);
