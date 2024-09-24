# jay_fit
jayfit.github.io

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>마라톤 페이스 계산기</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Arial', sans-serif;
        }
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: #f0f0f0;
            padding: 0;
        }
        .container {
            background-color: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            width: 60vw; /* 너비를 60%로 설정하여 세로 직사각형 */
            height: 80vh; /* 높이를 80%로 설정 */
            max-width: 400px; /* 최대 너비 제한 */
            max-height: 700px; /* 최대 높이 제한 */
        }
        h1 {
            text-align: center;
            margin-bottom: 20px;
            color: #333;
            font-size: 2rem;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #555;
            font-size: 1rem;
        }
        input, select, button {
            width: 100%;
            padding: 12px;
            margin-bottom: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
        }
        button {
            background-color: #4facfe;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #00f2fe;
        }
        .result {
            text-align: center;
            margin-top: 20px;
            color: red;
            font-size: 1.5rem;
            font-weight: bold;
        }
        .time-inputs {
            display: flex;
            justify-content: space-between;
            gap: 10px;
        }
        .time-inputs div {
            flex: 1;
        }
        .source {
            text-align: center;
            margin-top: 30px;
            font-size: 0.9rem;
            color: #777;
        }

        @media (max-width: 600px) {
            .container {
                width: 80vw;
                height: 85vh;
                padding: 30px;
            }
            h1 {
                font-size: 1.8rem;
            }
            input, select, button {
                padding: 10px;
                font-size: 0.9rem;
            }
            .result {
                font-size: 1.3rem;
            }
        }

        @media (max-width: 400px) {
            .container {
                width: 90vw;
                height: 90vh;
                padding: 20px;
            }
            h1 {
                font-size: 1.5rem;
            }
            input, select, button {
                padding: 8px;
                font-size: 0.8rem;
            }
            .result {
                font-size: 1.2rem;
            }
        }

    </style>
</head>
<body>
    <div class="container">
        <h1>마라톤 페이스 계산기</h1>

        <div class="time-inputs">
            <div>
                <label for="hours">시간 (hh)</label>
                <input type="number" id="hours" min="0" max="24" placeholder="시간">
            </div>
            <div>
                <label for="minutes">분 (mm)</label>
                <input type="number" id="minutes" min="0" max="59" placeholder="분">
            </div>
        </div>

        <label for="distance">거리 선택</label>
        <select id="distance">
            <option value="5">5km</option>
            <option value="10">10km</option>
            <option value="21.0975">하프 마라톤 (21km)</option>
            <option value="42.195">풀 마라톤 (42.195km)</option>
            <option value="custom">직접 입력</option>
        </select>

        <div id="customDistanceContainer" style="display:none;">
            <label for="customDistance">직접 입력 거리 (km)</label>
            <input type="number" id="customDistance" step="0.01" min="0" placeholder="직접 입력할 거리">
        </div>

        <button type="button" onclick="calculatePace()">평균 페이스 계산</button>

        <div class="result" id="result"></div>

        <div class="source">
            출처: <a href="https://www.instagram.com/_jay.fit__/" target="_blank">https://www.instagram.com/_jay.fit__/</a>
        </div>
    </div>

    <script>
        const distanceSelect = document.getElementById('distance');
        const customDistanceContainer = document.getElementById('customDistanceContainer');

        distanceSelect.addEventListener('change', function() {
            if (this.value === 'custom') {
                customDistanceContainer.style.display = 'block';
            } else {
                customDistanceContainer.style.display = 'none';
            }
        });

        function calculatePace() {
            const hours = parseInt(document.getElementById('hours').value) || 0;
            const minutes = parseInt(document.getElementById('minutes').value) || 0;
            const selectedDistance = distanceSelect.value;
            let distance = selectedDistance === 'custom' ? parseFloat(document.getElementById('customDistance').value) : parseFloat(selectedDistance);

            if (isNaN(hours) || isNaN(minutes) || isNaN(distance) || distance <= 0) {
                document.getElementById('result').innerText = "올바른 시간을 입력하고 거리를 선택하세요.";
                return;
            }

            const totalSeconds = (hours * 3600) + (minutes * 60);
            const paceSeconds = totalSeconds / distance;
            const paceMinutes = Math.floor(paceSeconds / 60);
            const paceRemainingSeconds = Math.round(paceSeconds % 60);

            const formattedPace = `${paceMinutes}:${paceRemainingSeconds < 10 ? '0' + paceRemainingSeconds : paceRemainingSeconds}`;
            document.getElementById('result').innerText = `평균 페이스: ${formattedPace} / km`;
        }
    </script>
</body>
</html>
