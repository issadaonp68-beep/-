<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>สายรหัสการดูแลและการจัดการสุขภาพผู้สูงอายุ👵🏻♥️</title>
    <style>
        /* CSS สำหรับตกแต่งหน้าเว็บ */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #74b9ff, #a29bfe);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
        }

        .container {
            background-color: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
            text-align: center;
            max-width: 400px;
            width: 90%;
        }

        h1 {
            color: #2d3436;
            margin-bottom: 20px;
            font-size: 24px;
        }

        input[type="text"] {
            width: 80%;
            padding: 12px 20px;
            margin: 8px 0;
            box-sizing: border-radius;
            border: 2px solid #ccc;
            border-radius: 25px;
            font-size: 16px;
            text-align: center;
            outline: none;
            transition: 0.3s;
        }

        input[type="text"]:focus {
            border-color: #6c5ce7;
        }

        button {
            background-color: #6c5ce7;
            color: white;
            padding: 12px 28px;
            margin: 10px 0;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: 0.3s;
        }

        button:hover {
            background-color: #5b4bc4;
            transform: scale(1.05);
        }

        .result-box {
            margin-top: 25px;
            padding: 15px;
            border-radius: 10px;
            background-color: #f1f2f6;
            display: none; /* ซ่อนไว้ก่อนจนกว่าจะกดค้นหา */
            animation: fadeIn 0.5s;
        }

        .hint-text {
            font-size: 18px;
            color: #ff7675;
            font-weight: bold;
            word-wrap: break-word;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🔍 ตามหาคำใบ้พี่รหัส</h1>
        <p>กรอกรหัสนิสิตของน้องๆเพื่อดูคำใบ้</p>
        
        <input type="text" id="studentId" placeholder="ตัวอย่าง: 68473783" autocomplete="off">
        <br>
        <button onclick="findHint()">ดูคำใบ้</button>

        <div id="resultBox" class="result-box">
            <h3 style="margin-top: 0; color: #2d3436;">คำใบ้ของน้องคือ:</h3>
            <p id="hintDisplay" class="hint-text"></p>
        </div>
    </div>

    <script>
        // 1. Database จำลอง: 
        const hintData = {
            "69471856": "โซนB",
            "69471900": "789",
            "69471924": "กลมๆสีส้ม",
            "69471948": "ภัยพิบัติระดับมหาลัย",
            "69472341": "ความสุข",
            "69472013": "ตุ้ยนุ้ยจ้ำม่ำ",
            "69472303": "อยากเจอพี่ให้หาสีชมพู",
            "69472310": "สีขาว",
            "69471870": "ท้ายมอนอ",
            "69472082": "มะพร้าว",
            "69472266": "อโวคาโด้ปั่น หวานปกติ",
            "69472440": "ชอบกิบแซ่บ",
            "69472181": "สีเหลืองเยลโล่ เยลลี่ ปีโป้ แล้วดีโด้เป็นสสารชนิดใด",
            "69472259": "รักเธอมากเกิน",
            "69472273": "ฟันเหล็กเด็กแนว",
            "69472150": "น้ำ",
            "69472402": "สีฟ้าน่ารัก",
            "69472198": "สีฟ้าน่ารัก",
            "69472020": "ไอแดงมันเป็นนักสู้ ไอเขียวมันเป็นนักซิ่ง",
            "69472235": "ไอแดงมันเป็นนักสู้ ไอเขียวมันเป็นนักซิ่ง",
            "69472419": "น้ำปิง นภัสกร",
            "69472167": "แฟนเก่ง หฤษฎ์",
            "69472365": "ก่อนเกิดชอบเตะฟุตบอลแต่หลังเกิดไม่ชอบ",
            "69472037": "อยู่ในบัรนซ์",
            "69472242": "มีสี มีกลิ่น",
            "69472099": "ศิลปะ",
            "69472211": "อิ่มจัง⬆️",
            "69471917": "ลมแรง🍃",
            "69472136": "แม่พี่น่า",
            "69472105": "แม่พี่น่า",
            "69472228": "วิว กุลวุฒิ เมย์ รัชนก",
            "69472334": "วิว กุลวุฒิ เมย์ รัชนก",
            "69471979": "#ไม่ใช่ผู้หญิง",
            "69472112": "ชาเขียว",
            "69472129": "ชาเขียว",
            "69472174": "กลม ๆ +🖐🏻",
            "69471955": "โปเกม่อนลำดับที่ 151",
            "69472112": "ชาเขียว",
            "69472327": "ตัวพี่มีหลายสี",
            "69471986": "ธรรมชาติสร้างได้ แต่มนุษย์เริ่ม",
        };

        function findHint() {
            // ดึงค่าจากช่องกรอกข้อมูล และตัดช่องว่างออก
            const inputId = document.getElementById("studentId").value.trim();
            const resultBox = document.getElementById("resultBox");
            const hintDisplay = document.getElementById("hintDisplay");

            // ตรวจสอบว่าได้กรอกข้อมูลไหม
            if (inputId === "") {
                alert("กรุณากรอกรหัสนิสิตก่อนน😭");
                return;
            }

            // ค้นหาคำใบ้จาก Database
            if (hintData[inputId]) {
                // ถ้าเจอข้อมูล
                hintDisplay.innerText = hintData[inputId];
                resultBox.style.display = "block"; // แสดงกล่องข้อความ
            } else {
                // ถ้าไม่เจอข้อมูล
                hintDisplay.innerText = "❌ ไม่พบข้อมูลรหัสนี้ในระบบ (ลองเช็คเลขดูอีกทีนะ หรือน้องอาจจะยังไม่มีพี่รหัส 🤔)";
                resultBox.style.display = "block";
            }
        }
    </script>

</body>
</html>
