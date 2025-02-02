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
            </tbody>
        </table>
    </div>
    <script>
        let banList = [];
        let caseList = [];
        let caseCounter = 1;

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
        
        function loadCaseList() {
            const caseTable = document.getElementById("case-list");
            caseTable.innerHTML = caseList.map((caseItem, index) => `
                <tr>
                    <td>${caseItem.caseNumber}</td>
                    <td>${caseItem.email}</td>
                    <td>
                        ${caseItem.claimed ? "Tatt" : `<button onclick="claimCase(${index})">Claim</button>`}
                        <button class="delete" onclick="removeCase(${index})">Slett</button>
                    </td>
                </tr>
            `).join('');
        }
        
        function claimCase(index) {
            caseList[index].claimed = true;
            loadCaseList();
        }

        function removeCase(index) {
            caseList.splice(index, 1);
            loadCaseList();
        }

        function removeBan(index) {
            banList.splice(index, 1);
            loadBanList();
        }

        function addBan() {
            let username = prompt("Brukernavn:");
            let reason = prompt("Årsak:");
            let date = new Date().toISOString().split('T')[0];
            if (username && reason) {
                banList.push({username, reason, date});
                loadBanList();
            }
        }

        function fetchEmails() {
            // Simulert e-posthenting (dette må kobles til en faktisk e-postserver i backend)
            let newEmails = [
                {email: "bruker1@fjellrim.eu"},
                {email: "bruker2@fjellrim.eu"}
            ];
            newEmails.forEach(email => {
                caseList.push({caseNumber: caseCounter++, email: email.email, claimed: false});
            });
            loadCaseList();
        }

        window.onload = function() {
            loadBanList();
            loadCaseList();
            fetchEmails();
        }
    </script>
</body>
</html>
