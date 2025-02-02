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
            background-color: #e9ecef;
            visibility: hidden;
            margin: 0;
            padding: 0;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.2);
            width: 80%;
            max-width: 800px;
            margin-top: 30px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: left;
        }
        th {
            background-color: #17a2b8;
            color: white;
        }
        button {
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background-color: #007bff;
            color: white;
            margin-top: 20px;
        }
        .delete {
            background-color: #dc3545;
        }
        .announcement-container {
            margin-top: 40px;
            width: 100%;
        }
        .announcement-box {
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 5px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        }
        .announcement-input {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            border: 1px solid #ced4da;
        }
        .announcement-button {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
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
        
        <div class="announcement-container">
            <h3>Staff Announcements</h3>
            <div class="announcement-box" id="announcement-box"></div>
            <textarea class="announcement-input" id="announcement-input" placeholder="Skriv en melding..."></textarea>
            <button class="announcement-button" onclick="addAnnouncement()">Legg til Announsement</button>
        </div>
    </div>
    <script>
        const validCredentials = {
            "admin": "pass123",
            "moderator1": "modpass1",
            "moderator2": "modpass2",
            "moderator3": "modpass3",
            "moderator4": "modpass4"
        };

        function checkLogin() {
            const username = prompt("Brukernavn:");
            const password = prompt("Passord:");
            if (validCredentials[username] && validCredentials[username] === password) {
                localStorage.setItem("loggedIn", "true");
                localStorage.setItem("loggedUser", username);
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
                loadBanList();
                loadAnnouncements();
                checkAdminPrivileges();
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

        function checkAdminPrivileges() {
            const loggedUser = localStorage.getItem("loggedUser");
            if (loggedUser !== "admin") {
                document.getElementById("announcement-input").style.display = "none";
                document.getElementById("announcement-button").style.display = "none";
            }
        }

        let announcements = JSON.parse(localStorage.getItem("announcements")) || [];

        function loadAnnouncements() {
            const announcementBox = document.getElementById("announcement-box");
            announcementBox.innerHTML = announcements.map(announcement => `
                <p>${announcement}</p>
            `).join('');
        }

        function addAnnouncement() {
            const announcementInput = document.getElementById("announcement-input").value;
            if (announcementInput) {
                announcements.push(announcementInput);
                localStorage.setItem("announcements", JSON.stringify(announcements));
                loadAnnouncements();
                document.getElementById("announcement-input").value = "";
            }
        }

        window.onload = function() {
            if (localStorage.getItem("loggedIn") !== "true") {
                checkLogin();
            } else {
                document.body.style.visibility = "visible";
                loadBanList();
                loadAnnouncements();
                checkAdminPrivileges();
            }
        };
    </script>
</body>
</html>
