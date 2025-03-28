<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Penjumlahan</title>
    <style>
        body {
            font-family: 'San Francisco', 'Helvetica Neue', Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: linear-gradient(100deg, rgb(255, 255, 255), #b2b2b2);
            animation: gradientAnimation 10s infinite alternate;
        }

        @keyframes gradientAnimation {
            0% { background: linear-gradient(100deg, rgb(255, 255, 255), #b2b2b2); }
            100% { background: linear-gradient(100deg, #b2b2b2, rgb(255, 255, 255)); }
        }

        .container {
            text-align: center;
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.08);
            max-width: 450px;
            width: 100%;
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .container:hover {
            transform: translateY(-10px);
            box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
        }

        h1 {
            color: #333;
            margin-bottom: 30px;
            font-weight: 600;
        }

        .input-container {
            margin-bottom: 30px;
        }

        .input-container label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
            color: #666;
        }

        .input-container input {
            width: 65%;
            padding: 14px;
            margin-bottom: 10px;
            border: 1px solid #000000;
            border-radius: 10px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        .input-container input:focus {
            border-color: #000000;
            outline: none;
        }

        .input-container button {
            width: 70%;
            padding: 14px;
            background-color: #1e1e1e;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s, transform 0.3s;
        }

        .input-container button:hover {
            background-color: #000000;
            transform: scale(1.05);
        }

        .result-container {
            margin-top: 30px;
        }

        .result-container h2 {
            font-size: 22px;
            color: #555;
        }

        .result-container span {
            font-weight: 600;
            color: #000000;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>AI Penjumlahan</h1>
        <div class="input-container">
            <label for="input1">Input 1:</label>
            <input type="number" id="input1" value="1">
            <label for="input2">Input 2:</label>
            <input type="number" id="input2" value="1">
            <button id="predictButton">Prediksi</button>
        </div>
        <div class="result-container">
            <h2>Hasil Prediksi: <span id="predictionResult">-</span></h2>
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
    <script>
        const xs = tf.tensor2d([[1, 1], [1, 0], [1, 2], [1, 3], [2, 3], [3, 4], [4, 5], [4, 2], [5, 4], [12, 12]]);
        const ys = tf.tensor2d([[2], [1], [3], [4], [5], [7], [9], [4], [9], [24]]);

        const model = tf.sequential();
        model.add(tf.layers.dense({ inputShape: [2], units: 10, activation: 'relu' }));
        model.add(tf.layers.dense({ units: 10, activation: 'relu' }));
        model.add(tf.layers.dense({ units: 1 }));

        model.compile({
          loss: 'meanSquaredError',
          optimizer: 'adam'
        });

        async function trainAndFindBest() {
            let bestPrediction = 0;

            for (let i = 1; i <= 100; i++) {
                console.log(`\n Training ke-${i}...`);
                await model.fit(xs, ys, { epochs: 200 });

                const result = model.predict(tf.tensor2d([[1, 1]]));
                const predictedValue = (await result.data())[0];
                console.log(`Hasil Prediksi ke-${i}: ${predictedValue}`);

                if (predictedValue > bestPrediction) {
                    bestPrediction = predictedValue;
                }
            }

            console.log(`\n✅ Hasil terbaik: ${bestPrediction}`);
        }

        document.getElementById('predictButton').addEventListener('click', async () => {
            const input1 = parseFloat(document.getElementById('input1').value);
            const input2 = parseFloat(document.getElementById('input2').value);

            const result = model.predict(tf.tensor2d([[input1, input2]]));
            const predictedValue = (await result.data())[0];

            // Membulatkan hasil prediksi
            const roundedPrediction = Math.round(predictedValue);

            document.getElementById('predictionResult').innerText = roundedPrediction;
        });

        trainAndFindBest();
    </script>
</body>
</html>
