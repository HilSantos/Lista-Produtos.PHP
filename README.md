# Lista-Produtos.PHP
Criação de uma lista de produtos em PHP.

<?php
include('conn.php');

if (isset($_GET["buscar"])) {
    $termo = $_GET["buscar"];
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto
            FROM tb_produtos 
            WHERE nome_produto LIKE '%$termo%'";
} else {
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto 
            FROM tb_produtos";
}

$result = mysqli_query($link, $sql);
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista Produtos</title>
    <link rel="stylesheet" href="lista.css">
</head>
<body>
    <h1>Lista Produtos</h1>
    <form action="listaprodutos.php" method="get">
        <input type="text" name="buscar" placeholder="Buscar por nome">
        <input type="submit" value="Pesquisar">
        <a href="listaprodutos.php"><input type="button" value="Voltar"></a>
    </form>
    <hr>
    <table border="1">
        <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Estoque</th>
            <th>Imagem</th>
            <th>Valor</th>
            <th>Alterar</th>
            <th>Excluir</th>
        </tr>
        <?php while($tbl = mysqli_fetch_array($result)): ?>
        <tr>
            <td><?= $tbl['id_produto'] ?></td>
            <td><?= $tbl['nome_produto'] ?></td>
            <td><?= $tbl['estoque_produto'] ?></td>
            <td><img src="imagens/<?= $tbl['imagem_produto'] ?>" width="40"></td>
            <td>R$ <?= number_format($tbl['valor_produto'], 2, ",", ".") ?></td>
            <td><a href="alteraproduto.php?id=<?= $tbl['id_produto'] ?>">Editar</a></td>
            <td><a href="excluiproduto.php?id=<?= $tbl['id_produto'] ?>" onclick="return confirm('Deseja realmente excluir este produto?')">Excluir</a></td>
        </tr>
        <?php endwhile; ?>
    </table>
</body>
</html>
