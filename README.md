<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nhận Lì Xì</title>
  <style>
    /* Giữ nguyên CSS như trước */
    body {
      font-family: 'Arial', sans-serif;
      background-color: #fff5e6;
      color: #333;
    }
    .login-container, .lixi-container {
      text-align: center;
      color: #d62828;
    }
    .input-field {
      padding: 10px;
      margin: 5px;
      width: 250px;
      font-size: 16px;
      border: 2px solid #ff6600;
      border-radius: 5px;
    }
    .submit-btn {
      padding: 10px 20px;
      font-size: 16px;
      background-color: #ffcc00;
      color: white;
      border: none;
      cursor: pointer;
      font-weight: bold;
      border-radius: 5px;
    }
    .submit-btn:hover {
      background-color: #e6b800;
    }
    .message {
      font-size: 20px;
      color: #d62828;
      margin: 20px 0;
      font-weight: bold;
    }
    .ranking {
      margin-top: 20px;
      font-weight: bold;
    }
    .ranking-table {
      width: 100%;
      border-collapse: collapse;
    }
    .ranking-table th, .ranking-table td {
      padding: 10px;
      border: 1px solid #ddd;
    }
    .ranking-table th {
      background-color: #ffcc00;
      color: #fff;
    }
    .ranking-table td {
      background-color: #fff5e6;
    }
    .ranking-table tr:nth-child(even) {
      background-color: #ffeb99;
    }
    .logout-btn {
      position: fixed;
      top: 10px;
      right: 10px;
      background-color: #ff0000;
      color: white;
      padding: 10px;
      font-size: 14px;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    .logout-btn:hover {
      background-color: #e60000;
    }
  </style>
</head>
<body>

  <div class="login-container" id="loginContainer">
    <h1>Đăng Nhập</h1>
    <input type="email" id="emailInput" class="input-field" placeholder="Nhập Họ Tên Của Bạn" />
    <button class="submit-btn" onclick="login()">Login</button>
  </div>

  <div class="lixi-container" id="lixiContainer" style="display: none;">
    <button class="logout-btn" onclick="logout()">Ra Ngoài 🔥</button>
    <h1>Nhận Lì Xì</h1>
    <input type="text" class="input-field" id="keyInput" placeholder="Nhập Mã Đi Pé 👾" />
    <button class="submit-btn" onclick="validateKey()">Mở Lì Xì ❄</button>
    <div class="message" id="message" style="display: none;"></div>
    <button class="submit-btn" id="refresh" onclick="refreshPage()" style="display: none;">Lấy Lì Xì Mới</button>
    <div class="ranking">
      <h2>🎁 𝙏𝙊𝙋 𝙉𝙃𝘼𝙉 𝙇𝙄𝙓𝙄 🎁</h2>
      <table class="ranking-table" id="rankingTable">
        <thead>
          <tr>
            <th>🏆 𝙏𝙊𝙋</th>
            <th>🌌 𝙉𝘼𝙈𝙀</th>
            <th>💰 𝙈𝙊𝙉𝙀𝙔</th>
          </tr>
        </thead>
        <tbody id="rankingBody">
          <!-- Bảng xếp hạng sẽ được hiển thị tại đây -->
        </tbody>
      </table>
    </div>
  </div>

  <script>
    let hasOpened = false;

    // Danh sách mã lì xì và giá trị mặc định
    const keyAmounts = {
      "Yeurua": 50000,
      "AnhChoNe": 50000,
      "Cuc": 50000,
      "Huan": 50000,
      "Huanlixi": Math.floor(Math.random() * 1000) + 5000, // Giá trị ngẫu nhiên
      "Tetvuive": Math.floor(Math.random() * 2000) + 10000 // Giá trị ngẫu nhiên
    };

    // Hàm cập nhật bảng xếp hạng
    function updateRanking() {
      const rankingData = JSON.parse(localStorage.getItem("rankingData")) || [];
      const rankingBody = document.getElementById("rankingBody");
      rankingBody.innerHTML = ""; // Xóa bảng cũ

      rankingData.sort((a, b) => b.amount - a.amount); // Sắp xếp theo số tiền nhận

      rankingData.forEach((entry, index) => {
        const row = document.createElement("tr");
        row.innerHTML = `
          <td>${index + 1}</td>
          <td>${entry.email}</td>
          <td>${entry.amount.toLocaleString()}đ</td>
        `;
        rankingBody.appendChild(row);
      });
    }

    function login() {
      const email = document.getElementById("emailInput").value;
      if (email) {
        localStorage.setItem("userEmail", email);
        document.getElementById("loginContainer").style.display = "none";
        document.getElementById("lixiContainer").style.display = "block";
        updateRanking(); // Cập nhật bảng xếp hạng khi người dùng đăng nhập
      } else {
        alert("Vui lòng nhập tên hợp lệ.");
      }
    }

    function logout() {
      localStorage.removeItem("userEmail");
      document.getElementById("loginContainer").style.display = "block";
      document.getElementById("lixiContainer").style.display = "none";
    }

    function validateKey() {
      const key = document.getElementById("keyInput").value;
      const email = localStorage.getItem("userEmail");

      if (hasOpened) {
        alert("Bạn đã mở lì xì rồi, chỉ một lần duy nhất!");
        return;
      }

      // Kiểm tra xem tài khoản đã nhận lì xì chưa
      const userData = JSON.parse(localStorage.getItem("userData")) || {};
      if (userData[email] && userData[email].opened) {
        alert("𝘽𝙖𝙣 𝘿𝙖 𝙈𝙤 𝙇𝙞𝙭𝙞 𝙍𝙤𝙞");
        return;
      }

      if (keyAmounts[key] !== undefined) {
        const amount = keyAmounts[key];
        document.getElementById("message").style.display = "block";
        document.getElementById("message").innerHTML = `🎉 Chúc mừng! Bạn nhận được ${amount.toLocaleString()}đ lì xì!`;
        document.getElementById("keyInput").style.display = "none";
        document.querySelector(".submit-btn").style.display = "none";
        document.getElementById("refresh").style.display = "inline-block";
        hasOpened = true;

        // Lưu thông tin vào localStorage
        if (!userData[email]) {
          userData[email] = { opened: true };
        }

        // Lưu vào bảng xếp hạng
        const rankingData = JSON.parse(localStorage.getItem("rankingData")) || [];
        rankingData.push({ email: email, amount: amount });
        localStorage.setItem("rankingData", JSON.stringify(rankingData));

        // Lưu trạng thái đã mở lì xì
        localStorage.setItem("userData", JSON.stringify(userData));

        updateRanking(); // Cập nhật bảng xếp hạng sau khi nhận lì xì
      } else {
        alert("Mã không đúng hoặc đã hết. Vui lòng thử lại!");
      }
    }

    function refreshPage() {
      window.location.reload();
    }

    window.onload = function() {
      if (localStorage.getItem("userEmail")) {
        document.getElementById("loginContainer").style.display = "none";
        document.getElementById("lixiContainer").style.display = "block";
        updateRanking(); // Cập nhật bảng xếp hạng khi trang được tải lại
      }
    };
  </script>
</body>
</html>
