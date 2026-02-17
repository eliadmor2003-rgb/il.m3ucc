<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>אימות אנושי</title>
    <style>
        body { font-family: sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #f0f0f0; margin: 0; }
        .captcha-box { background: white; padding: 20px; border: 1px solid #ccc; box-shadow: 0 4px 10px rgba(0,0,0,0.1); width: 350px; }
        h2 { font-size: 18px; margin-bottom: 10px; color: #444; }
        .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 5px; margin-bottom: 15px; }
        .grid img { width: 100%; height: 100px; object-fit: cover; cursor: pointer; border: 2px solid transparent; transition: 0.2s; }
        .grid img.selected { border-color: #4285f4; opacity: 0.7; }
        .btn { background: #4285f4; color: white; border: none; padding: 10px 20px; cursor: pointer; float: left; font-weight: bold; }
        .error-msg { color: red; font-size: 14px; display: none; margin-top: 10px; }
        
        /* דף הצלחה */
        #success-page { display: none; text-align: center; }
        #success-page h1 { color: #d63384; font-size: 2.5rem; }
    </style>
</head>
<body>

    <div id="captcha-container" class="captcha-box">
        <h2>תלחצי על תמונות שאת אוהבת</h2>
        <div class="grid" id="imageGrid">
            </div>
        <button class="btn" onclick="checkCaptcha()">אישור</button>
        <div id="error" class="error-msg">תלחצי על כל התמונות שאת אוהבת...</div>
    </div>

    <div id="success-page">
        <h1>בוקר טוב, אני אוהב אותך ❤️</h1>
        <p style="font-size: 1.5rem;">שיהיה לך יום מדהים!</p>
    </div>

    <script>
        // רשימת התמונות - 4 נכונות ו-5 רגילות
        const images = [
            { src: 'love1.jpg', correct: true },
            { src: 'other1.jpg', correct: false },
            { src: 'love2.jpg', correct: true },
            { src: 'other2.jpg', correct: false },
            { src: 'other3.jpg', correct: false },
            { src: 'love3.jpg', correct: true },
            { src: 'other4.jpg', correct: false },
            { src: 'love4.jpg', correct: true },
            { src: 'other5.jpg', correct: false }
        ];

        const grid = document.getElementById('imageGrid');
        let selectedCount = 0;

        // רינדור התמונות
        images.forEach((imgData, index) => {
            const img = document.createElement('img');
            img.src = imgData.src;
            img.dataset.correct = imgData.correct;
            img.onclick = () => {
                img.classList.toggle('selected');
            };
            grid.appendChild(img);
        });

        function checkCaptcha() {
            const allImgs = document.querySelectorAll('.grid img');
            let isCorrect = true;

            allImgs.forEach(img => {
                const isSelected = img.classList.contains('selected');
                const shouldBeSelected = img.dataset.correct === "true";
                
                // אם משהו לא תואם (נבחר כשלא היה צריך, או לא נבחר כשכן היה צריך)
                if (isSelected !== shouldBeSelected) {
                    isCorrect = false;
                }
            });

            if (isCorrect) {
                document.getElementById('captcha-container').style.display = 'none';
                document.getElementById('success-page').style.display = 'block';
                document.body.style.backgroundColor = "#fff0f5"; // צבע רקע רומנטי
            } else {
                document.getElementById('error').style.display = 'block';
            }
        }
    </script>
</body>
</html>
