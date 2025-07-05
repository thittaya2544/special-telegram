<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ล็อกอินผ่าน LINE</title>
  <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@500&display=swap');
    
    body {
      font-family: 'Kanit', sans-serif;
      background-color: #fef6f9;
      color: #5a2a4a;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      text-align: center;
    }

    h1 {
      font-size: 2rem;
      margin-bottom: 1rem;
    }

    #status {
      margin-bottom: 2rem;
      font-size: 1.1rem;
    }

    button.pastel-btn {
      background-color: #ffd6e8;
      border: 2px solid #ffa6c9;
      border-radius: 15px;
      color: #5a2a4a;
      font-size: 18px;
      font-weight: 600;
      padding: 14px 40px;
      cursor: pointer;
      box-shadow: 0 4px 8px rgba(255, 182, 193, 0.3);
      transition: 0.3s;
    }

    button.pastel-btn:hover {
      background-color: #ffb6cc;
      border-color: #ff7fa3;
    }
  </style>
</head>
<body>
  <h1>เข้าสู่ระบบด้วย LINE</h1>
  <div id="status">กำลังโหลด...</div>
  <button id="btnLogin" class="pastel-btn" style="display: none;">เข้าสู่ระบบ 🦋</button>

  <script>
    const liffId = "2007693716-B7D4wZow"; // ← ใส่ LIFF ID ที่ได้จาก LINE Developer
    const scriptURL = "https://script.google.com/macros/s/AKfycbyZ3IAnQopiFCwWNqQcHeExB6WP9x9zmTjPlPlm0YkT6Bjg81k2-0wzCbdiH5ZwjUJM/exec"; // ← ใส่ URL ที่ได้จากการ Deploy Google Apps Script
    const redirectURL = "https://mchlink.my.canva.site/notepreg"; // ← เปลี่ยนเป็น URL ที่ต้องการ redirect

    async function main() {
      await liff.init({ liffId });

      if (!liff.isLoggedIn()) {
        document.getElementById("status").innerText = "กรุณาล็อกอินผ่าน LINE";
        document.getElementById("btnLogin").style.display = "inline-block";
        document.getElementById("btnLogin").onclick = () => liff.login();
      } else {
        const profile = await liff.getProfile();
        const userId = profile.userId;

        document.getElementById("status").innerText = `ยินดีต้อนรับ!`;

        // ส่ง userId ไปยัง Apps Script
        fetch(scriptURL, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ userId: userId }),
        })
        .then((res) => res.json())
        .then((data) => {
          if (data.status === "success") {
            window.location.href = redirectURL;
          } else {
            alert("เกิดข้อผิดพลาด: " + data.message);
          }
        })
        .catch((err) => {
          alert("ไม่สามารถส่งข้อมูลได้");
          console.error(err);
        });
      }
    }

    main();
  </script>
</body>
</html>
