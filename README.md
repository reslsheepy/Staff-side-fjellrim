<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Moderator Dashboard</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #f4f4f4;
            visibility: hidden;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            width: 80%;
            max-width: 800px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #28a745;
            color: white;
        }
        button {
            padding: 5px 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background-color: #007bff;
            color: white;
        }
        .delete {
            background-color: #dc3545;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Moderator Dashboard</h2>
        <h3>Ban-Logg</h3>
        <table>
            <thead>
                <tr>
                    <th>Brukernavn</th>
                    <th>Årsak</th>
                    <th>Dato</th>
                    <th>Handling</th>
                </tr>
            </thead>
            <tbody id="ban-list">
            </tbody>
        </table>
        <button onclick="addBan()">Legg til Ban</button>
    </div>
    <script>
        const validCredentials = {
            "Sheepy": "Jomis456",
            "Jonas": "Jonas2505",
            "moderator2": "modpass2",
            "moderator3": "modpass3",
            "moderator4": "modpass4"
        };

        function checkLogin() {
            const username = prompt("Brukernavn:");
            const password = prompt("Passord:");
            if (validCredentials[username] && validCredentials[username] === password) {
                localStorage.setItem("loggedIn", "true");
                document.body.style.visibility = "visible";
            } else {
                alert("Feil brukernavn eller passord!");
                checkLogin();
            }
        }

        window.onload = function() {
            if (localStorage.getItem("loggedIn") !== "true") {
                checkLogin();
            } else {
                document.body.style.visibility = "visible";
            }
        };

        let banList = JSON.parse(localStorage.getItem("banList")) || [];

        function loadBanList() {
            const banTable = document.getElementById("ban-list");
            banTable.innerHTML = banList.map((ban, index) => `
                <tr>
                    <td>${ban.username}</td>
                    <td>${ban.reason}</td>
                    <td>${ban.date}</td>
                    <td><button class="delete" onclick="removeBan(${index})">Slett</button></td>
                </tr>
            `).join('');
        }

        function removeBan(index) {
            const password = prompt("Skriv inn passordet for å slette denne loggen:");
            if (password === "tørketbanan") {
                banList.splice(index, 1);
                localStorage.setItem("banList", JSON.stringify(banList));
                loadBanList();
            } else {
                alert("Feil passord!");
            }
        }

        function addBan() {
            let username = prompt("Brukernavn:");
            let reason = prompt("Årsak:");
            let date = new Date().toISOString().split('T')[0];
            if (username && reason) {
                banList.push({username, reason, date});
                localStorage.setItem("banList", JSON.stringify(banList));
                loadBanList();
            }
        }

        window.onload = function() {
            if (localStorage.getItem("loggedIn") !== "true") {
                checkLogin();
            } else {
                document.body.style.visibility = "visible";
                loadBanList();
            }
        };
    </script>
</body>
</html>
