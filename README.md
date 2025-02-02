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
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            width: 80%;
            max-width: 600px;
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
                </tr>
            </thead>
            <tbody id="ban-list">
                <!-- Ban-data blir lagt inn her -->
            </tbody>
        </table>
        <h3>Kontaktforespørsler</h3>
        <table>
            <thead>
                <tr>
                    <th>Saksnummer</th>
                    <th>E-post</th>
                    <th>Handling</th>
                </tr>
            </thead>
            <tbody id="case-list">
                <!-- Saksnummer blir lagt inn her -->
            </tbody>
        </table>
    </div>
    <script>
        const banList = [
            {username: "Spiller1", reason: "Juksing", date: "2024-02-02"},
            {username: "Spiller2", reason: "Trakassering", date: "2024-02-01"}
        ];
        const caseList = [
            {caseNumber: 1, email: "bruker1@example.com", claimed: false},
            {caseNumber: 2, email: "bruker2@example.com", claimed: false}
        ];
        function loadBanList() {
            const banTable = document.getElementById("ban-list");
            banTable.innerHTML = banList.map(ban => `<tr><td>${ban.username}</td><td>${ban.reason}</td><td>${ban.date}</td></tr>`).join('');
        }
        function loadCaseList() {
            const caseTable = document.getElementById("case-list");
            caseTable.innerHTML = caseList.map(caseItem => `
                <tr>
                    <td>${caseItem.caseNumber}</td>
                    <td>${caseItem.email}</td>
                    <td>
                        ${caseItem.claimed ? "Tatt" : `<button onclick="claimCase(${caseItem.caseNumber})">Claim</button>`}
                    </td>
                </tr>
            `).join('');
        }
        function claimCase(caseNumber) {
            const caseItem = caseList.find(c => c.caseNumber === caseNumber);
            if (caseItem) {
                caseItem.claimed = true;
                loadCaseList();
            }
        }
        window.onload = function() {
            loadBanList();
            loadCaseList();
        }
    </script>
</body>
</html>
