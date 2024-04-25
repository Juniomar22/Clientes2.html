<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Organizador de Clientes</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        header, footer {
            text-align: center;
            padding: 10px;
            background-color: #f2f2f2;
        }
        main {
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid #dddddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        .textarea {
            width: 100%;
            height: 100px;
        }
        .button {
            padding: 5px 10px;
            background-color: #007bff;
            color: #fff;
            border: none;
            cursor: pointer;
            margin-right: 5px;
        }
        .button:hover {
            background-color: #0056b3;
        }
        .cliente-form {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        .cliente-form input,
        .cliente-form textarea {
            flex: 1;
            min-width: 200px;
            padding: 5px;
        }
    </style>
</head>
<body>

<header>
    <h1>Organizador de Clientes</h1>
</header>

<main>
    <form id="clienteForm" class="cliente-form">
        <input type="text" placeholder="Nome" class="nome-cliente">
        <input type="text" placeholder="Contato" class="contato-cliente">
        <textarea class="textarea comentarios-cliente" placeholder="Comentários"></textarea>
        <button type="button" onclick="adicionarCliente()" class="button">Adicionar Cliente</button>
    </form>

    <table id="clientesTable">
        <thead>
            <tr>
                <th>Nome do Cliente</th>
                <th>Contato</th>
                <th>Comentários</th>
                <th>Ações</th>
            </tr>
        </thead>
        <tbody>
            <!-- Dados dos clientes serão adicionados aqui -->
        </tbody>
    </table>
</main>

<footer>
    <p>&copy; 2024 Organizador de Clientes</p>
</footer>

<script>
    const dictionary = new Typo("pt_BR", false, false, { dictionaryPath: "https://cdn.jsdelivr.net/npm/typo-js/dictionaries" });

    function corrigirOrtografia(texto) {
        return dictionary.check(texto) ? texto : (dictionary.suggest(texto)[0] || texto);
    }

    function adicionarCliente() {
        const nome = document.querySelector('.nome-cliente').value;
        const contato = document.querySelector('.contato-cliente').value;
        const comentarios = document.querySelector('.comentarios-cliente').value;

        const newRow = document.getElementById("clientesTable").getElementsByTagName('tbody')[0].insertRow();
        newRow.innerHTML = `
            <td>${nome}</td>
            <td>${contato}</td>
            <td>${comentarios}</td>
            <td>
                <button onclick="excluirCliente(this)" class="button">Excluir</button>
                <button onclick="editarCliente(this)" class="button">Editar</button>
            </td>
        `;

        document.querySelector('.nome-cliente').value = '';
        document.querySelector('.contato-cliente').value = '';
        document.querySelector('.comentarios-cliente').value = '';
    }

    function excluirCliente(button) {
        if (confirm("Tem certeza que deseja excluir este cliente?")) {
            const row = button.parentNode.parentNode;
            row.parentNode.removeChild(row);
        }
    }

    function editarCliente(button) {
        const row = button.parentNode.parentNode;
        const cells = row.querySelectorAll('td');
        const nome = cells[0].textContent;
        const contato = cells[1].textContent;
        const comentarios = cells[2].textContent;

        document.querySelector('.nome-cliente').value = nome;
        document.querySelector('.contato-cliente').value = contato;
        document.querySelector('.comentarios-cliente').value = comentarios;

        row.parentNode.removeChild(row);
    }

    // Corrigir ortografia quando o usuário sai do campo de texto
    document.addEventListener("blur", function(event) {
        const element = event.target;
        if (element.classList.contains('nome-cliente') || element.classList.contains('contato-cliente') || element.classList.contains('comentarios-cliente')) {
            element.value = corrigirOrtografia(element.value);
        }
    }, true);
</script>

</body>
</html>
