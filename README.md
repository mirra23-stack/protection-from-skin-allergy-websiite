<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RadM | Your Skin's Bestie</title>
    <link href="https://fonts.googleapis.com/css2?family=Pacifico&family=Quicksand:wght@400;600&link=swap" rel="stylesheet">
    <style>
        :root {
            --green-main: #4CAF50; /* Fresh Leaf Green */
            --green-light: #f1f8e9; /* Soft Mint background for cards */
            --green-border: #c8e6c9;
            --text-dark: #2e4d30; /* Dark Forest Green for text */
            --accent-yellow: #ffa000; /* For UV warnings */
        }

        body {
            margin: 0;
            font-family: 'Quicksand', sans-serif;
            background-color: #f9fbf9;
            background-image: radial-gradient(var(--green-border) 0.5px, transparent 0.5px);
            background-size: 20px 20px; 
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: var(--text-dark);
        }

        .container {
            background: white;
            width: 90%;
            max-width: 450px;
            padding: 40px;
            border-radius: 40px;
            box-shadow: 0 15px 35px rgba(76, 175, 80, 0.15);
            text-align: center;
            transition: all 0.5s ease;
        }

        .hidden { display: none; }

        h1 { font-family: 'Pacifico', cursive; color: var(--green-main); font-size: 2.5rem; margin-bottom: 10px; }
        h2 { color: var(--green-main); margin-bottom: 20px; }
        
        input, select {
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            border: 1px solid var(--green-border);
            border-radius: 15px;
            box-sizing: border-box;
            outline: none;
            font-family: inherit;
            background-color: #fff;
        }

        input:focus {
            border-color: var(--green-main);
        }

        .btn {
            background: var(--green-main);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 15px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            margin-top: 20px;
            transition: transform 0.2s, background 0.3s;
        }

        .btn:hover { transform: scale(1.02); background: #388E3C; }

        .allergy-options {
            text-align: left;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 15px 0;
        }

        .checkbox-item { font-size: 0.85rem; display: flex; align-items: center; gap: 5px; }

        .stat-card {
            background: var(--green-light);
            padding: 20px;
            border-radius: 20px;
            margin: 15px 0;
            border: 1px solid var(--green-border);
        }
        .aqi-val { font-size: 3rem; font-weight: bold; color: var(--green-main); }
        .warning-text { font-style: italic; color: #1b5e20; margin-top: 15px; font-size: 0.9rem; }
    </style>
</head>
<body>

    <div id="page-login" class="container">
        <h1>RadM 🌿</h1>
        <p>Log in to check your environment (and your glow!).</p>
        <input type="text" placeholder="Username" id="user">
        <input type="password" placeholder="Password">
        <button class="btn" onclick="showPage('page-details')">Login</button>
        <p style="font-size: 0.8rem; margin-top: 20px;">Don't have an account? <span style="color:var(--green-main); font-weight:bold; cursor:pointer;">Sign Up</span></p>
    </div>

    <div id="page-details" class="container hidden">
        <h2>About You ✨</h2>
        <input type="text" id="nameInput" placeholder="Full Name">
        <div style="display: flex; gap: 10px;">
            <input type="date" placeholder="DOB">
            <input type="number" placeholder="Age">
        </div>
        <input type="text" placeholder="Place of Birth">
        
        <p style="text-align: left; font-weight: 600; margin-bottom: 5px; color: var(--green-main);">Skin Allergies:</p>
        <div class="allergy-options">
            <label class="checkbox-item"><input type="checkbox"> Eczema</label>
            <label class="checkbox-item"><input type="checkbox"> Hives</label>
            <label class="checkbox-item"><input type="checkbox"> Heat Rash</label>
            <label class="checkbox-item"><input type="checkbox"> Rosacea</label>
            <label class="checkbox-item"><input type="checkbox"> UV Sensitivity</label>
            <label class="checkbox-item"><input type="checkbox"> Dust Mites</label>
        </div>
        <input type="text" placeholder="Other (please specify)...">
        
        <button class="btn" onclick="generateReport()">Analyze My Environment 🌿</button>
    </div>

    <div id="page-result" class="container hidden">
        <h1>RadM Report</h1>
        <p id="greeting">Hey Gorgeous!</p>
        
        <div class="stat-card">
            <p>Air Quality Index (AQI)</p>
            <div class="aqi-val" id="aqi-display">168</div>
            <p id="aqi-desc">Unhealthy for Sensitive Groups</p>
        </div>

        <div class="stat-card" style="background: #fff9e6; border: 1px solid #ffe082;">
            <p>Sun & UV Radiation</p>
            <div class="aqi-val" style="color: var(--accent-yellow);">8.2</div>
            <p style="color: #795548;">High UV Exposure</p>
        </div>

        <div class="warning-text">
            ⚠️ <strong>Alert:</strong> There is a 30% chance of a sun allergy flare-up today in your location.
        </div>

        <p id="final-advice" style="margin-top: 20px; font-weight: 600; font-size: 0.95rem; line-height: 1.4;"></p>
        
        <button class="btn" onclick="showPage('page-login')">Back to Home 🏠</button>
    </div>

    <script>
        function showPage(pageId) {
            document.querySelectorAll('.container').forEach(div => div.classList.add('hidden'));
            document.getElementById(pageId).classList.remove('hidden');
        }

        function generateReport() {
            const name = document.getElementById('nameInput').value || "Gorgeous";
            document.getElementById('greeting').innerText = `Hey ${name}! ✨`;
            
            const advice = "The air in Malayambakkam is a bit dusty today. Since you have sensitive skin, please wear your SPF 50 and a fresh green mask! It's safe to go out, but stay in the shade! 🌿";
            document.getElementById('final-advice').innerText = advice;

            showPage('page-result');
        }
    </script>
</body>
</html>
