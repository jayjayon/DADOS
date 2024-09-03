<!DOCTYPE html>
<!-- saved from url=(0051)file:///C:/Users/Julia/Documents/rpg_simulador.html -->
<html lang="pt-br"><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ficha RPG</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #333; /* Fundo cinza escuro */
            color: #fff; /* Texto branco para contraste */
        }
        .container {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            margin: 10px;
        }
        .row {
            display: flex;
            margin-bottom: 5px;
        }
        .button {
            width: 50px;
            height: 50px;
            margin: 2px;
            text-align: center;
            line-height: 50px;
            font-size: 18px;
            cursor: pointer;
            background-color: #ff4d4d; /* Botões vermelhos */
            color: #fff; /* Texto branco nos botões */
            border-radius: 12px; /* Cantos arredondados */
        }
        .result {
            margin-top: 10px;
            font-size: 20px;
            font-weight: bold;
        }
        #d20-result {
            margin-top: 10px;
            font-size: 20px;
            font-weight: bold;
        }
        #result {
            margin-top: 10px;
            font-size: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container" id="buttons-container">
        <div class="row">
            <!-- 1-3 -->
            <div class="button" onclick="roll(1)">1</div>
            <div class="button" onclick="roll(2)">2</div>
            <div class="button" onclick="roll(3)">3</div>
        </div>
        <div class="row">
            <!-- 4-6 -->
            <div class="button" onclick="roll(4)">4</div>
            <div class="button" onclick="roll(5)">5</div>
            <div class="button" onclick="roll(6)">6</div>
        </div>
        <div class="row">
            <!-- 7-9 -->
            <div class="button" onclick="roll(7)">7</div>
            <div class="button" onclick="roll(8)">8</div>
            <div class="button" onclick="roll(9)">9</div>
        </div>
        <div class="row">
            <!-- 10-12 -->
            <div class="button" onclick="roll(10)">10</div>
            <div class="button" onclick="roll(11)">11</div>
            <div class="button" onclick="roll(12)">12</div>
        </div>
        <div class="row">
            <!-- 13-15 -->
            <div class="button" onclick="roll(13)">13</div>
            <div class="button" onclick="roll(14)">14</div>
            <div class="button" onclick="roll(15)">15</div>
        </div>
        <div class="row">
            <!-- 16-18 -->
            <div class="button" onclick="roll(16)">16</div>
            <div class="button" onclick="roll(17)">17</div>
            <div class="button" onclick="roll(18)">18</div>
        </div>
        <div class="row">
            <!-- 19-20 -->
            <div class="button" onclick="roll(19)">19</div>
            <div class="button" onclick="roll(20)">20</div>
            <!-- D20 -->
            <div class="button" onclick="rollD20()">D20</div>
        </div>
    </div>

    <div id="result" class="result"></div>
    <div id="d20-result" class="result"></div>

    <script>
        const habilidades = {
            1: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19], normal: [20], bom: [], extremo: [] },
            2: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18], normal: [19], bom: [20], extremo: [] },
            3: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17], normal: [18, 19], bom: [20], extremo: [] },
            4: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16], normal: [17, 18], bom: [19, 20], extremo: [] },
            5: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15], normal: [16, 17, 18], bom: [19], extremo: [20] },
            6: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14], normal: [15, 16, 17], bom: [18, 19], extremo: [20] },
            7: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13], normal: [14, 15, 16, 17], bom: [18, 19], extremo: [20] },
            8: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12], normal: [13, 14, 15, 16], bom: [17, 18, 19], extremo: [20] },
            9: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11], normal: [12, 13, 14, 15, 16], bom: [17, 18, 19], extremo: [20] },
            10: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9, 10], normal: [11, 12, 13, 14, 15], bom: [16, 17, 18], extremo: [19, 20] },
            11: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8, 9], normal: [10, 11, 12, 13, 14, 15], bom: [16, 17, 18], extremo: [19, 20] },
            12: { desastre: [1], falha: [2, 3, 4, 5, 6, 7, 8], normal: [9, 10, 11, 12, 13, 14], bom: [15, 16, 17, 18], extremo: [19, 20] },
            13: { desastre: [1], falha: [2, 3, 4, 5, 6, 7], normal: [8, 9, 10, 11, 12, 13, 14], bom: [15, 16, 17, 18], extremo: [19, 20] },
            14: { desastre: [1], falha: [2, 3, 4, 5, 6], normal: [7, 8, 9, 10, 11, 12, 13], bom: [14, 15, 16, 17, 18], extremo: [19, 20] },
            15: { desastre: [1], falha: [2, 3, 4, 5], normal: [6, 7, 8, 9, 10, 11, 12, 13], bom: [14, 15, 16, 17], extremo: [18, 19, 20] },
            16: { desastre: [], falha: [1, 2, 3, 4], normal: [5, 6, 7, 8, 9, 10, 11, 12], bom: [13, 14, 15, 16, 17], extremo: [18, 19, 20] },
            17: { desastre: [], falha: [1, 2, 3], normal: [4, 5, 6, 7, 8, 9, 10, 11, 12], bom: [13, 14, 15, 16, 17], extremo: [18, 19, 20] },
            18: { desastre: [], falha: [1, 2], normal: [3, 4, 5, 6, 7, 8, 9, 10, 11], bom: [12, 13, 14, 15, 16, 17], extremo: [18, 19, 20] },
            19: { desastre: [], falha: [1], normal: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11], bom: [12, 13, 14, 15, 16, 17], extremo: [18, 19, 20] },
            20: { desastre: [], falha: [], normal: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], bom: [11, 12, 13, 14, 15, 16], extremo: [17, 18, 19,20] }
        };

        function roll(num) {
            const rollResult = Math.floor(Math.random() * 20) + 1;
            const habilidade = habilidades[num];
            let resultText = `Dado: ${rollResult}\nResultado: `;

            if (habilidade) {
                if (habilidade.desastre.includes(rollResult)) {
                    resultText += 'Desastre';
                } else if (habilidade.falha.includes(rollResult)) {
                    resultText += 'Falha';
                } else if (habilidade.normal.includes(rollResult)) {
                    resultText += 'Normal';
                } else if (habilidade.bom.includes(rollResult)) {
                    resultText += 'Bom';
                } else if (habilidade.extremo.includes(rollResult)) {
                    resultText += 'Extremo';
                } else {
                    resultText += 'Sem resultado definido';
                }
            } else {
                resultText += 'Habilidade não definida';
            }

            document.getElementById('result').innerText = resultText;
        }

        function rollD20() {
            const rollResult = Math.floor(Math.random() * 20) + 1;
            document.getElementById('d20-result').innerText = `Dado: ${rollResult}`;
        }
    </script>


</body></html>
