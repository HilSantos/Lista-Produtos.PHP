# Lista-Produtos.PHP
Criação de uma lista de produtos em PHP.

<?php
// Inclui arquivos de segurança, cabeçalho da página e conexão com o banco de dados //
include('segurancadez.php');
include('cabecalho.php');
include('conn.php');

// Verifica se há um termo de busca enviado via GET //
if(isset($_GET["buscar"])){
    $termo = $_GET["buscar"]; // Captura o termo digitado pelo usuário //

    // Consulta produtos cujo nome contenha o termo buscado e que estejam ativos (status = 1) //
    // Utiliza LIKE para permitir buscas parciais //
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto
            FROM tb_produtos 
            WHERE nome_produto LIKE '%$termo%' AND status_produto = 1";
} else {
    // Se não houver busca, lista todos os produtos ativos //
    $sql = "SELECT id_produto, nome_produto, estoque_produto, imagem_produto, valor_produto 
            FROM tb_produtos 
            WHERE status_produto = 1";
}

// Executa a consulta no banco de dados //
$result = mysqli_query($link, $sql);

// Fecha a conexão com o banco //
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
    <br>
    <div>
<form action="listaprodutos.php" method="get">
        <input type="text" name="buscar" placeholder="Buscar por nome">
        <input type="submit" value="pesquisar">
        <a href="listaprodutos.php"><input type="button" value="voltar"></a>

<hr>

</form>
    </div>
    <?php
    // Exibe mensagem se não houver produtos encontrados //
    if(mysqli_num_rows($result) == 0){
        echo "<h3>Nenhum produto encontrado!</h3>";
        exit();
    }
    ?>
    <!-- Tabela para exibir os produtos encontrados -->
    <table border = "1">
        <tr>
            <th>*****</th> <!-- Ações (detalhes) -->
            <th>Nome</th>
            <th>Estoque</th>
            <th>Imagem</th>
            <th>Valor</th>
            <th>*****</th> <!-- Ações (editar) -->
            <th>*****</th> <!-- Ações (excluir) -->
        </tr>
        <?php
        while($tbl = mysqli_fetch_array($result)){
        ?>
        <tr>
        <!-- Link para detalhes do produto -->
        <td><a href="detalhaproduto.php?id=<?=$tbl[0] ?>"><span class="material-symbols-outlined">search</span>
</a>
</td>
        <td></td>
        <!-- Nome do produto -->
        <td><?= $tbl[1] ?></td>
        <!-- Quantidade em estoque -->
        <td><?= $tbl[2] ?></td>
        <!-- Imagem do produto -->
        <td><img src="imagens/<?= $tbl[3] ?>" width="40"></td>
        <!-- Valor formatado -->
        <td>R$ <?=number_format($tbl[4],2,",",".") ?></td>
        <!-- Link para editar o produto -->
        <td> <a href="editeproduto.php?id=<?=$tbl[0]?>">
                        <span class="material-symbols-outlined">
                            build_circle
                        </span></td>
                        <!-- Link para excluir o produto -->
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
