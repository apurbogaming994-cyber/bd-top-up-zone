    }

    .header h1 {
      margin: 0;
      font-size: 25px;
    }

    .header p {
      margin: 7px 0 0;
      opacity: .9;
    }

    .container {
      padding: 18px;
      max-width: 600px;
      margin: auto;
    }

    .title {
      font-size: 20px;
      font-weight: bold;
      margin: 8px 0 14px;
    }

    .packages {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .package {
      background: white;
      border: 2px solid transparent;
      border-radius: 17px;
      padding: 18px 12px;
      text-align: center;
      box-shadow: 0 3px 12px rgba(0,0,0,.06);
      cursor: pointer;
    }

    .package.selected {
      border-color: #1687f5;
      background: #eaf5ff;
    }

    .diamond {
      font-size: 31px;
    }

    .package h3 {
      margin: 8px 0 5px;
    }

    .price {
      font-size: 18px;
      font-weight: bold;
      color: #1687f5;
    }

    .order-box {
      background: white;
      margin-top: 20px;
      padding: 18px;
      border-radius: 18px;
      box-shadow: 0 3px 12px rgba(0,0,0,.06);
    }

    label {
      display: block;
      margin-top: 12px;
      font-weight: bold;
    }

    input, select {
      width: 100%;
      padding: 14px;
      margin-top: 7px;
      border: 1px solid #d5dbe1;
      border-radius: 12px;
      font-size: 16px;
      outline: none;
    }

    .btn {
      width: 100%;
      margin-top: 18px;
      padding: 15px;
      border: 0;
      border-radius: 13px;
      background: #1687f5;
      color: white;
      font-size: 17px;
      font-weight: bold;
      cursor: pointer;
    }

    .btn:active {
      transform: scale(.98);
    }

    .status {
      margin-top: 12px;
      text-align: center;
      font-weight: bold;
    }

    .footer {
      text-align: center;
      color: #777;
      padding: 25px;
      font-size: 13px;
    }
  </style>
</head>

<body>

  <div class="header">
    <h1>🎮 BD Top UP Zone</h1>
    <p>Free Fire Diamond Top-Up</p>
  </div>

  <div class="container">

    <div class="title">💎 Diamond Package</div>

    <div class="packages">

      <div class="package" data-package="100 Diamonds" data-price="90">
        <div class="diamond">💎</div>
        <h3>100 Diamonds</h3>
        <div class="price">৳90</div>
      </div>

      <div class="package" data-package="310 Diamonds" data-price="250">
        <div class="diamond">💎</div>
        <h3>310 Diamonds</h3>
        <div class="price">৳250</div>
      </div>

      <div class="package" data-package="520 Diamonds" data-price="420">
        <div class="diamond">💎</div>
        <h3>520 Diamonds</h3>
        <div class="price">৳420</div>
      </div>

      <div class="package" data-package="1060 Diamonds" data-price="800">
        <div class="diamond">💎</div>
        <h3>1060 Diamonds</h3>
        <div class="price">৳800</div>
      </div>

    </div>

    <div class="order-box">

      <div class="title">🛒 Order করুন</div>

      <label>Free Fire UID</label>
      <input
        type="number"
        id="uid"
        placeholder="আপনার Free Fire UID"
      >

      <label>Selected Package</label>
      <input
        type="text"
        id="selectedPackage"
        placeholder="উপরের একটি Package নির্বাচন করুন"
        readonly
      >

      <label>Payment Method</label>
      <select id="payment">
        <option value="bKash">📱 bKash</option>
        <option value="Nagad">📱 Nagad</option>
      </select>

      <label>Transaction ID</label>
      <input
        type="text"
        id="txid"
        placeholder="Payment TXID লিখুন"
      >

      <button class="btn" onclick="submitOrder()">
        ✅ Order Confirm
      </button>

      <div class="status" id="status"></div>

    </div>

  </div>

  <div class="footer">
    © 2026 BD Top UP Zone
  </div>

  <script>

    // Telegram Mini App
    const tg = window.Telegram.WebApp;

    tg.ready();
    tg.expand();

    let selectedPackage = "";
    let selectedPrice = "";

    // Package নির্বাচন
    document.querySelectorAll(".package").forEach(packageCard => {

      packageCard.addEventListener("click", function () {

        document.querySelectorAll(".package")
          .forEach(card => card.classList.remove("selected"));

        this.classList.add("selected");

        selectedPackage = this.dataset.package;
        selectedPrice = this.dataset.price;

        document.getElementById("selectedPackage").value =
          selectedPackage + " - ৳" + selectedPrice;
      });

    });

    // Order Submit
    function submitOrder() {

      const uid = document.getElementById("uid").value.trim();
      const payment = document.getElementById("payment").value;
      const txid = document.getElementById("txid").value.trim();
      const status = document.getElementById("status");

      if (!uid) {
        status.style.color = "red";
        status.innerText = "⚠️ Free Fire UID দিন";
        return;
      }

      if (!selectedPackage) {
        status.style.color = "red";
        status.innerText = "⚠️ একটি Diamond Package নির্বাচন করুন";
        return;
      }

      if (!txid) {
        status.style.color = "red";
        status.innerText = "⚠️ Transaction ID দিন";
        return;
      }

      const order = {
        uid: uid,
        package: selectedPackage,
        price: selectedPrice,
        payment: payment,
        txid: txid
      };

      console.log("ORDER:", order);

      status.style.color = "green";
      status.innerText =
        "✅ Order গ্রহণ করা হয়েছে!";

      /*
        পরে এখানে Backend API যুক্ত করা হবে।
        তখন Order Database-এ যাবে এবং
        Admin Telegram-এ Notification যাবে।
      */

      // Telegram Bot-এ data পাঠানোর জন্য প্রস্তুত
      if (tg.sendData) {
        tg.sendData(JSON.stringify(order));
      }
    }

  </script>

</body>
</html>
