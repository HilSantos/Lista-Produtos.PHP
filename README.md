# Lista-Produtos.PHP
Criação de uma lista de produtos em PHP.

<?php
include('segurancadez.php');
include('cabecalho.php');
include('conn.php');

if (isset($_GET["buscar"])) {
    $termo = $_GET["buscar"];
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto
            FROM tb_produtos 
            WHERE nome_produto LIKE '%$termo%'" AND status_produto = 1";} 
else {
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto 
            FROM tb_produtos" WHERE status_produto = 1";
}

$result = mysqli_query($link, $sql);
mysqli_close($link);
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista Produtos</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200" />
    <link rel="stylesheet" href="lista.css">
</head>
<body>
    <h1>Lista Produtos</h1>
    <form action="listaprodutos.php" method="get">
        <input type="text" name="buscar" placeholder="Buscar por nome">
        <input type="submit" value="Pesquisar">
        <a href="listaprodutos.php"><input type="button" value="Voltar"></a>
    </form>
    </div>
    <table border="1">
        <tr>
            <th>****</th>
            <th>Nome</th>
            <th>Estoque</th>
            <th>Imagem</th>
            <th>Valor</th>
            <th>****</th>
            <th>****</th>
        </tr>
        <?php 
        while($tbl = mysqli_fetch_array($result)){
        ?>
        <tr>
            <td><a href="detalhaproduto.php?id=<?=$tbl[0] ?>"><span class="material-symbols-outlined">search</span>
</a>
</td>
            <td></td>    
            <td><?= $tbl[1] ?></td>
        <td><?= $tbl[2] ?></td>
        <td><img src="imagens/<?= $tbl[3] ?>" width="40"></td>
        <td>R$ <?=number_format($tbl[4],2,",",".") ?></td>
        <td> <a href="editeproduto.php?id=<?=$tbl[0]?>">
                        <span class="material-symbols-outlined">
                            build_circle
                        </span></td>
        <td>
            <a href="deleteproduto.php?id=<?=$tbl[0]?>">
            <span class="material-symbols-outlined">
delete
</span>
            </a>
        </td>
        </tr>
        <?php
        }
        ?>
    </table>
</body>
</html>
