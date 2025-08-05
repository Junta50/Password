<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pro Password Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #1e1e2f;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    .container {
      background: #2b2b3d;
      padding: 20px;
      border-radius: 10px;
      width: 350px;
      text-align: center;
      max-height: 90vh;
      overflow-y: auto;
    }

    input, button {
      width: 100%;
      padding: 10px;
      margin: 5px 0;
      border: none;
      border-radius: 5px;
      font-size: 14px;
    }

    button {
      background: #4CAF50;
      color: white;
      cursor: pointer;
      font-size: 15px;
    }

    button:hover {
      background: #45a049;
    }

    .password-list {
      margin-top: 10px;
    }

    .password-item {
      background: #3a3a50;
      padding: 10px;
      border-radius: 5px;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 14px;
    }

    .password-text {
      word-break: break-all;
      max-width: 150px;
    }

    .buttons {
      display: flex;
      gap: 5px;
    }

    .strength {
      font-weight: bold;
      font-size: 12px;
      margin-top: 3px;
      color: #FFD700;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🔐 Pro Password Generator</h1>

    <label>Password Length:</label>
    <input type="number" id="length" min="4" max="50" value="12">

    <label>How many passwords?</label>
    <input type="number" id="count" min="1" max="20" value="3">

    <label><input type="checkbox" id="uppercase" checked> Include Uppercase</label>
    <label><input type="checkbox" id="lowercase" checked> Include Lowercase</label>
    <label><input type="checkbox" id="numbers" checked> Include Numbers</label>
    <label><input type="checkbox" id="symbols" checked> Include Symbols</label>

    <label>Custom Symbols (optional):</label>
    <input type="text" id="customSymbols" placeholder="e.g. @$%&*">

    <button onclick="generatePasswords()">Generate Passwords</button>

    <div id="passwordList" class="password-list"></div>
  </div>

  <script>
    function generatePasswords() {
        const length = parseInt(document.getElementById("length").value);
        const count = parseInt(document.getElementById("count").value);
        const characters = getCharacters();

        const list = document.getElementById("passwordList");
        list.innerHTML = "";

        if (characters === "") {
            list.innerHTML = "<p>Select options!</p>";
            return;
        }

        for (let j = 0; j < count; j++) {
            const password = createPassword(length, characters);
            addPasswordItem(password, list, length, characters);
        }
    }

    function createPassword(length, characters) {
        let password = "";
        for (let i = 0; i < length; i++) {
            password += characters.charAt(Math.floor(Math.random() * characters.length));
        }
        return password;
    }

    function addPasswordItem(password, list, length, characters) {
        const item = document.createElement("div");
        item.className = "password-item";

        const text = document.createElement("div");
        text.className = "password-text";
        text.textContent = password;

        const strength = document.createElement("div");
        strength.className = "strength";
        strength.textContent = "Strength: " + checkStrength(password);

        const buttons = document.createElement("div");
        buttons.className = "buttons";

        const copyBtn = document.createElement("button");
        copyBtn.textContent = "Copy";
        copyBtn.onclick = () => {
            navigator.clipboard.writeText(password);
            alert("Password copied!");
        };

        const regenBtn = document.createElement("button");
        regenBtn.textContent = "Regenerate";
        regenBtn.onclick = () => {
            const newPass = createPassword(length, characters);
            text.textContent = newPass;
            strength.textContent = "Strength: " + checkStrength(newPass);
        };

        buttons.appendChild(copyBtn);
        buttons.appendChild(regenBtn);

        item.appendChild(text);
        item.appendChild(buttons);
        item.appendChild(strength);
        list.appendChild(item);
    }

    function getCharacters() {
        const includeUppercase = document.getElementById("uppercase").checked;
        const includeLowercase = document.getElementById("lowercase").checked;
        const includeNumbers = document.getElementById("numbers").checked;
        const includeSymbols = document.getElementById("symbols").checked;
        const customSymbols = document.getElementById("customSymbols").value;

        const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        const lowercase = "abcdefghijklmnopqrstuvwxyz";
        const numbers = "0123456789";
        const symbols = "!@#$%^&*()_+[]{}|;:,.<>?";

        let characters = "";
        if (includeUppercase) characters += uppercase;
        if (includeLowercase) characters += lowercase;
        if (includeNumbers) characters += numbers;
        if (includeSymbols) characters += symbols;
        if (customSymbols) characters += customSymbols;

        return characters;
    }

    function checkStrength(password) {
        let strengthScore = 0;

        if (password.length >= 8) strengthScore++;
        if (/[A-Z]/.test(password)) strengthScore++;
        if (/[0-9]/.test(password)) strengthScore++;
        if (/[^A-Za-z0-9]/.test(password)) strengthScore++;

        if (strengthScore <= 1) return "Weak";
        if (strengthScore === 2) return "Medium";
        return "Strong";
    }
  </script>
</body>
</html>
