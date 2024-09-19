<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora Elétrica</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f0f0f0;
        }
        h1, h2 {
            color: #333;
            text-align: center;
        }
        .calculator {
            background-color: #fff;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-top: 10px;
        }
        input, select {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .button-group {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
        }
        button {
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
        }
        .calculate {
            background-color: #4CAF50;
            color: white;
        }
        .calculate:hover {
            background-color: #45a049;
        }
        .clear {
            background-color: #f44336;
            color: white;
        }
        .clear:hover {
            background-color: #d32f2f;
        }
        .result {
            margin-top: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Calculadora Elétrica</h1>
    
    <div class="calculator">
        <h2>Calculadora Básica</h2>
        <label for="voltage">Tensão (V):</label>
        <input type="number" id="voltage" placeholder="Digite a tensão">
        
        <label for="current">Corrente (A):</label>
        <input type="number" id="current" placeholder="Digite a corrente">
        
        <label for="resistance">Resistência (Ω):</label>
        <input type="number" id="resistance" placeholder="Digite a resistência">
        
        <label for="power">Potência (W):</label>
        <input type="number" id="power" placeholder="Digite a potência">
        
        <div class="button-group">
            <button class="calculate" onclick="calculate()">Calcular</button>
            <button class="clear" onclick="clearFields('basic')">Limpar</button>
        </div>
        
        <div id="basic-result" class="result"></div>
    </div>

    <div class="calculator">
        <h2>Cálculo de Bitola de Cabos</h2>
        <label for="cable-current">Corrente (A):</label>
        <input type="number" id="cable-current" placeholder="Digite a corrente">

        <label for="cable-length">Comprimento do cabo (m):</label>
        <input type="number" id="cable-length" placeholder="Digite o comprimento">

        <label for="cable-voltage">Tensão (V):</label>
        <input type="number" id="cable-voltage" placeholder="Digite a tensão">

        <label for="voltage-drop">Queda de tensão máxima (%):</label>
        <input type="number" id="voltage-drop" placeholder="Digite a queda de tensão máxima">

        <label for="conductor-material">Material do condutor:</label>
        <select id="conductor-material">
            <option value="copper">Cobre</option>
            <option value="aluminum">Alumínio</option>
        </select>

        <div class="button-group">
            <button class="calculate" onclick="calculateCableSize()">Calcular Bitola</button>
            <button class="clear" onclick="clearFields('cable')">Limpar</button>
        </div>

        <div id="cable-result" class="result"></div>
    </div>

    <script>
        function calculate() {
            const voltage = parseFloat(document.getElementById('voltage').value);
            const current = parseFloat(document.getElementById('current').value);
            const resistance = parseFloat(document.getElementById('resistance').value);
            const power = parseFloat(document.getElementById('power').value);
            
            let result = '';

            if (!isNaN(voltage) && !isNaN(current)) {
                const calcPower = voltage * current;
                const calcResistance = voltage / current;
                result += `Potência: ${calcPower.toFixed(2)} W<br>`;
                result += `Resistência: ${calcResistance.toFixed(2)} Ω<br>`;
            }

            if (!isNaN(voltage) && !isNaN(resistance)) {
                const calcCurrent = voltage / resistance;
                const calcPower = Math.pow(voltage, 2) / resistance;
                result += `Corrente: ${calcCurrent.toFixed(2)} A<br>`;
                result += `Potência: ${calcPower.toFixed(2)} W<br>`;
            }

            if (!isNaN(current) && !isNaN(resistance)) {
                const calcVoltage = current * resistance;
                const calcPower = Math.pow(current, 2) * resistance;
                result += `Tensão: ${calcVoltage.toFixed(2)} V<br>`;
                result += `Potência: ${calcPower.toFixed(2)} W<br>`;
            }

            if (!isNaN(power) && !isNaN(voltage)) {
                const calcCurrent = power / voltage;
                const calcResistance = Math.pow(voltage, 2) / power;
                result += `Corrente: ${calcCurrent.toFixed(2)} A<br>`;
                result += `Resistência: ${calcResistance.toFixed(2)} Ω<br>`;
            }

            if (!isNaN(power) && !isNaN(current)) {
                const calcVoltage = power / current;
                const calcResistance = power / Math.pow(current, 2);
                result += `Tensão: ${calcVoltage.toFixed(2)} V<br>`;
                result += `Resistência: ${calcResistance.toFixed(2)} Ω<br>`;
            }

            if (!isNaN(power) && !isNaN(resistance)) {
                const calcVoltage = Math.sqrt(power * resistance);
                const calcCurrent = Math.sqrt(power / resistance);
                result += `Tensão: ${calcVoltage.toFixed(2)} V<br>`;
                result += `Corrente: ${calcCurrent.toFixed(2)} A<br>`;
            }

            if (result === '') {
                result = 'Por favor, insira pelo menos dois valores para calcular.';
            }

            document.getElementById('basic-result').innerHTML = result;
        }

        function calculateCableSize() {
            const current = parseFloat(document.getElementById('cable-current').value);
            const length = parseFloat(document.getElementById('cable-length').value);
            const voltage = parseFloat(document.getElementById('cable-voltage').value);
            const voltageDrop = parseFloat(document.getElementById('voltage-drop').value);
            const material = document.getElementById('conductor-material').value;

            if (isNaN(current) || isNaN(length) || isNaN(voltage) || isNaN(voltageDrop)) {
                document.getElementById('cable-result').innerHTML = 'Por favor, preencha todos os campos.';
                return;
            }

            const resistivity = material === 'copper' ? 0.0172 : 0.0282; // Ω.mm²/m
            const maxVoltageDrop = voltage * (voltageDrop / 100);

            // Cálculo da área da seção transversal em mm²
            const area = (2 * resistivity * length * current) / maxVoltageDrop;

            // Bitolas padrão em mm²
            const standardSizes = [1.5, 2.5, 4, 6, 10, 16, 25, 35, 50, 70, 95, 120, 150, 185, 240, 300];

            // Encontrar a bitola adequada
            let selectedSize = standardSizes[standardSizes.length - 1];
            for (let size of standardSizes) {
                if (size >= area) {
                    selectedSize = size;
                    break;
                }
            }

            const result = `
                Área calculada: ${area.toFixed(2)} mm²<br>
                Bitola recomendada: ${selectedSize} mm²<br>
                Material: ${material === 'copper' ? 'Cobre' : 'Alumínio'}
            `;

            document.getElementById('cable-result').innerHTML = result;
        }

        function clearFields(section) {
            if (section === 'basic') {
                document.getElementById('voltage').value = '';
                document.getElementById('current').value = '';
                document.getElementById('resistance').value = '';
                document.getElementById('power').value = '';
                document.getElementById('basic-result').innerHTML = '';
            } else if (section === 'cable') {
                document.getElementById('cable-current').value = '';
                document.getElementById('cable-length').value = '';
                document.getElementById('cable-voltage').value = '';
                document.getElementById('voltage-drop').value = '';
                document.getElementById('conductor-material').selectedIndex = 0;
                document.getElementById('cable-result').innerHTML = '';
            }
        }
    </script>
</body>
</html>
