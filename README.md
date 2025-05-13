<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Affine Cipher - Ergash</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #f0f0f0;
    }
    .container {
      max-width: 500px;
      background: #fff;
      padding: 20px;
      margin: auto;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
      border-radius: 8px;
    }
    input, textarea, button {
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ddd;
    }
    button {
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    h2 {
      text-align: center;
      color: #333;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Affine Cipher - Shifrlovchi va Deshifrlovchi</h2>
    <label>Matn:</label>
    <textarea id="text" rows="4"></textarea>

    <label>Kalit a:</label>
    <input type="number" id="a" value="5">

    <label>Kalit b:</label>
    <input type="number" id="b" value="8">

    <button onclick="encrypt()">Shifrlash</button>
    <button onclick="decrypt()">Deshifrlash</button>

    <label>Natija:</label>
    <textarea id="result" rows="4" readonly></textarea>
  </div>

  <script>
    function modInverse(a, m) {
      a = ((a % m) + m) % m;
      for (let x = 1; x < m; x++) {
        if ((a * x) % m === 1) return x;
      }
      return null;
    }

    function encrypt() {
      const input = document.getElementById("text").value.toLowerCase();
      const a = parseInt(document.getElementById("a").value);
      const b = parseInt(document.getElementById("b").value);
      const m = 26;

      if (modInverse(a, m) === null) {
        alert("a va 26 o‘zaro tub emas. a = " + a + " noto‘g‘ri.");
        return;
      }

      let result = "";
      for (let char of input) {
        if (char >= 'a' && char <= 'z') {
          let x = char.charCodeAt(0) - 97;
          let enc = (a * x + b) % m;
          result += String.fromCharCode(enc + 97);
        } else {
          result += char;
        }
      }

      document.getElementById("result").value = result;
    }

    function decrypt() {
      const input = document.getElementById("text").value.toLowerCase();
      const a = parseInt(document.getElementById("a").value);
      const b = parseInt(document.getElementById("b").value);
      const m = 26;
      const a_inv = modInverse(a, m);

      if (a_inv === null) {
        alert("a va 26 o‘zaro tub emas. a = " + a + " noto‘g‘ri.");
        return;
      }

      let result = "";
      for (let char of input) {
        if (char >= 'a' && char <= 'z') {
          let y = char.charCodeAt(0) - 97;
          let dec = (a_inv * (y - b + m)) % m;
          result += String.fromCharCode(dec + 97);
        } else {
          result += char;
        }
      }

      document.getElementById("result").value = result;
    }
  </script>
</body>
</html>
