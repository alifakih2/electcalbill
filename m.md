<html lang="ar" dir="ltr">
<head>
    <meta charset="UTF-8">
    <title>ELECTRICITY BILL CALCULATION - ALI FAKIH</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/0.4.1/html2canvas.min.js"></script>
    <style>
        * {
            font-family: 'Cairo', sans-serif;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            padding: 30px;
            width: 100%;
            max-width: 800px;
            border: 1px solid rgba(0,0,0,0.1);
        }

        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 30px;
            font-size: 2em;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .header-radio {
            margin-bottom: 30px;
            padding: 15px;
            background: #f1f3f5;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            gap: 20px;
            border: 1px solid rgba(0,0,0,0.1);
        }

        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .input-group {
            position: relative;
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #495057;
            font-weight: 600;
            font-size: 0.9em;
        }

        input, select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #dee2e6;
            border-radius: 8px;
            font-size: 1em;
            transition: all 0.3s ease;
            background: #fff;
        }

        input:focus {
            border-color: #4dabf7;
            box-shadow: 0 0 0 3px rgba(77, 171, 247, 0.1);
            outline: none;
        }

        .radio-group {
            display: flex;
            gap: 15px;
            margin-top: 10px;
        }

        .button-group {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 25px 0;
        }

        button {
            padding: 14px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .calculate-btn {
            background: #40c057;
            color: white;
            grid-column: span 3;
        }

        .print-btn { background: #228be6; color: white; }
        .pdf-btn { background: #fa5252; color: white; }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        #result {
            margin-top: 25px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 10px;
            display: none;
            border: 1px solid rgba(0,0,0,0.05);
        }

        .bill-detail {
            padding: 12px 0;
            border-bottom: 1px dashed #dee2e6;
            display: flex;
            justify-content: space-between;
        }

        .total-box {
            background: #4dabf7;
            color: white;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
            text-align: center;
        }

        .signature {
            text-align: center;
            margin-top: 30px;
            color: #868e96;
            font-style: italic;
            font-size: 0.9em;
        }

        @media (max-width: 768px) {
            .grid-container {
                grid-template-columns: 1fr;
            }
            .button-group {
                grid-template-columns: 1fr;
            }
            .calculate-btn {
                grid-column: span 1;
            }
        }

        @media print {
            body { 
                background: white !important; 
                -webkit-print-color-adjust: exact; 
            }
            .container {
                box-shadow: none !important;
                border: none !important;
                padding: 10px !important;
            }
            button { display: none !important; }
            #result { display: block !important; }
        }

        .print-only { display: none; }
        @media print {
            .print-only { display: block; }
            .no-print { display: none; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🖩 ELECTRICITY BILL CALCULATION</h1>
  <h1>🖩 إحتساب فاتورة كهرباء</h1>
        
        <div class="header-radio">
            <div class="radio-group">
                <label>
                    <input type="radio" name="monthType" value="auto" checked onclick="toggleMonthInput(true)">
                    AUTO CHECK
                </label>
                <label>
                    <input type="radio" name="monthType" value="manual" onclick="toggleMonthInput(false)">
                    MANUAL CHECK
                </label>
            </div>
        </div>

        <div class="grid-container">
            <div class="input-group">
                <label>NAME
 الإسم</label>
                <input type="text" id="name" placeholder="Input name">
            </div>

            <div class="input-group">
                <label>Phone number
رقم الهاتف</label>
                <input type="tel" id="phone" placeholder="Input phone number">
            </div>
            
            <div class="input-group">
                <label>الاشتراك BRANCH/
INSTALLATION الشعبة</label>
                <input type="text" id="section" placeholder="Input Branch number">
            </div>

            <div class="input-group">
                <label>LAST READING
 العداد السابق (KWH)</label>
                <input type="number" id="previous" step="any">
            </div>

            <div class="input-group">
                <label>NEW READING
 العداد الحالي(KWH)</label>
                <input type="number" id="current" step="any">
            </div>

            <div class="input-group">
                <label>NET CONSOMMATION
 (KWH) - المقطوعية الصافية</label>
                <input type="number" id="totalReading" step="any" placeholder="NET CONSOMMATION" disabled>
            </div>

            <div class="input-group">
                <label>FROM DATE
 (منذ)</label>
                <input type="date" id="startDate">
            </div>

            <div class="input-group">
                <label>TO DATE
 (لغاية)</label>
                <input type="date" id="endDate">
            </div>

            <!-- حقل MONTHS الجديد تحت TO DATE -->
            <div class="input-group">
               <label>MONTHS
 (عدد الاشهر)</label>
                <input type="number" id="manualMonths" placeholder="MONTHS" disabled>
            </div>

            <div class="input-group">
                <label> indicative exchange (LBP/USD)
 سعر الصرف</label>
                <input type="number" id="exchange" value="89500">
            </div>

            <div class="input-group">
                <label>AMPERE
 (الامبير)</label>
                <input type="number" id="ampere" value="15">
            </div>

            <div class="input-group">
                <label>T.V.A (الضريبة الضافة)</label>
                <input type="number" id="vatInput" value="11" step="any">
            </div>

            <div class="input-group">
                <label>Additional Fees (الطابع المالي)</label>
                <input type="number" id="additional" value="5000" step="any">
            </div>
        </div>

        <div class="button-group">
            <button class="calculate-btn" onclick="calculateBill()">⚡ calculateBill</button>
            <button class="print-btn" onclick="window.print()">🖨️ PRINT</button>
            <button class="pdf-btn" onclick="generatePDF()">📥 PDF</button>
        </div>

        <div id="result">
            <div class="print-only">
                <h2>BILL - IMFORMATION                                  معلومات الفاتورة</h2>

                <div class="bill-detail"><span>NAME الاسم:</span> <span id="nameDisplay"></span></div>
                <div class="bill-detail"><span>PHONE الهاتف:</span> <span id="phoneDisplay"></span></div>
                <div class="bill-detail"><span>BRANCH الشعبة:</span> <span id="sectionDisplay"></span></div>
            </div>
            
            <div class="bill-detail"><span>CONSOMMATION (المقطوعية):</span> <span id="consumption"></span></div>
            <div class="bill-detail"><span>Duration (المدة):</span> <span id="duration"></span></div>
            <div class="bill-detail"><span>ElectricityCost
(قيمة الاستهلاك):</span> <span id="electricityCost"></span></div>
            <div class="bill-detail"><span>AmpereCost (رسم العداد):</span> <span id="ampereCost"></span></div>
            <div class="bill-detail"><span>vat (الضريبة المضافة):</span> <span id="vat"></span></div>
            <div class="bill-detail"><span>Additional Fees (الطابع المالي):</span> <span id="additionalDisplay"></span></div>
            
            <div class="total-box">
                <div id="totalLBP"></div>
                <div id="totalUSD" style="margin-top:10px;"></div>
            </div>
        </div>

        <div class="signature">
           CREATED BY MR. ALI AHMAD FAKIH<br>
            <span id="todayDate"></span> signature
        </div>
    </div>

    <script>
        function toggleMonthInput(auto) {
            const manualMonthsField = document.getElementById('manualMonths');
            const startDateField = document.getElementById('startDate');
            const endDateField = document.getElementById('endDate');
            const totalReadingField = document.getElementById('totalReading');
            const previousField = document.getElementById('previous');
            const currentField = document.getElementById('current');
            
            if(auto) {
                manualMonthsField.disabled = true;
                manualMonthsField.value = '';
                startDateField.disabled = false;
                endDateField.disabled = false;
                totalReadingField.disabled = true;
                totalReadingField.value = '';
                previousField.disabled = false;
                currentField.disabled = false;
            } else {
                manualMonthsField.disabled = false;
                startDateField.disabled = true;
                endDateField.disabled = true;
                startDateField.value = '';
                endDateField.value = '';
                totalReadingField.disabled = false;
                previousField.disabled = true;
                currentField.disabled = true;
                previousField.value = '';
                currentField.value = '';
            }
        }

        function calculateBill() {
            document.getElementById('nameDisplay').textContent = document.getElementById('name').value;
            document.getElementById('phoneDisplay').textContent = document.getElementById('phone').value;
            document.getElementById('sectionDisplay').textContent = document.getElementById('section').value;

            const inputs = {
                previous: parseFloat(document.getElementById('previous').value),
                current: parseFloat(document.getElementById('current').value),
                totalReading: parseFloat(document.getElementById('totalReading').value),
                ampere: parseFloat(document.getElementById('ampere').value),
                exchange: parseFloat(document.getElementById('exchange').value),
                vatInput: parseFloat(document.getElementById('vatInput').value),
                startDate: new Date(document.getElementById('startDate').value),
                endDate: new Date(document.getElementById('endDate').value),
                manualMonths: parseFloat(document.getElementById('manualMonths').value),
                additional: parseFloat(document.getElementById('additional').value) || 0
            };

            let months = inputs.manualMonths;
            if(!months) {
                const timeDiff = Math.abs(inputs.endDate - inputs.startDate);
                months = Math.ceil(timeDiff / (1000 * 3600 * 24 * 30));
            }

            const consumption = inputs.totalReading || (inputs.current - inputs.previous);
            const monthlyConsumption = consumption / months;

            let electricityUSD = 0;
            if(monthlyConsumption > 100) {
                electricityUSD = (100 * 0.10) + ((monthlyConsumption - 100) * 0.26);
            } else {
                electricityUSD = monthlyConsumption * 0.10;
            }
            electricityUSD *= months;

            const ampereUSD = (inputs.ampere * 0.25) * months;

            let totalLBP = (electricityUSD + ampereUSD) * inputs.exchange;
            
            const vat = totalLBP * (inputs.vatInput / 100);
            totalLBP += vat;

            totalLBP += inputs.additional;

            totalLBP = Math.ceil(totalLBP / 1000) * 1000;

            const totalUSD = totalLBP / inputs.exchange;

            document.getElementById('consumption').textContent = `${consumption.toFixed(2)} KWH`;
            document.getElementById('duration').textContent = `${months} MONTHS`;
            document.getElementById('electricityCost').textContent = `${electricityUSD.toFixed(2)} USD`;
            document.getElementById('ampereCost').textContent = `${ampereUSD.toFixed(2)} USD`;
            document.getElementById('vat').textContent = `${vat.toFixed(2)} LBP`;
            document.getElementById('additionalDisplay').textContent = `${inputs.additional.toFixed(2)} LBP`;
            document.getElementById('totalLBP').textContent = `total: ${totalLBP.toFixed(2)} LBP`;
            document.getElementById('totalUSD').textContent = `TOTAL: ${totalUSD.toFixed(2)} USD`;

            document.getElementById('result').style.display = 'block';
        }

        document.getElementById('todayDate').textContent = new Date().toLocaleDateString('');

        function generatePDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            doc.setFontSize(16);
            doc.text('ELECTRICITY BILLCALCULATION إحتساب فاتورة الكهرباء', 20, 20);
            doc.setFontSize(12);
            doc.text(`NAME الإسم: ${document.getElementById('nameDisplay').textContent}`, 20, 30);
            doc.text(`Phone number رقم الهاتف: ${document.getElementById('phoneDisplay').textContent}`, 20, 40);
            doc.text(`branch الشعبة: ${document.getElementById('sectionDisplay').textContent}`, 20, 50);
            doc.text(`CONSOMMATION المقطوعية: ${document.getElementById('consumption').textContent}`, 20, 60);
            doc.text(`CONSOMMATION COST ثمن المقطوعية: ${document.getElementById('electricityCost').textContent}`, 20, 70);
            doc.text(`AMPERE COST رسم العداد: ${document.getElementById('ampereCost').textContent}`, 20, 80);
            doc.text(`TAV الضريبة المضافة: ${document.getElementById('vat').textContent}`, 20, 90);
            doc.text(`Additional Fees الطابع المالي: ${document.getElementById('additionalDisplay').textContent}`, 20, 100);

            doc.setFontSize(14);
            doc.text('TOTAL:', 20, 120);
            doc.text(`LBP COST: ${document.getElementById('totalLBP').textContent}`, 20, 130);
            doc.text(`DOLLAR COST: ${document.getElementById('totalUSD').textContent}`, 20, 140);

            doc.save('electricity_bill.pdf');
        }

        // Initialize on page load
        toggleMonthInput(true);
    </script>
</body>
</html>
