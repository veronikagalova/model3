<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Экономико-математическая модель для оценки целесообразности выхода на ОРЭМ</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700;800&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background: #f5f5f5;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 10px;
        }

        .poster-container {
            width: 100%;
            max-width: 1280px;
            min-height: 720px;
            background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
            border-radius: 8px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .header {
            background: linear-gradient(90deg, #2e7d32 0%, #388e3c 100%);
            color: white;
            padding: 15px 20px;
            position: relative;
            z-index: 10;
        }

        .header h1 {
            font-family: 'Montserrat', sans-serif;
            font-size: 20px;
            font-weight: 700;
            margin-bottom: 5px;
            letter-spacing: -0.5px;
            line-height: 1.2;
        }

        .header p {
            font-size: 12px;
            opacity: 0.9;
            font-weight: 400;
        }

        .content {
            padding: 15px;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            position: relative;
            z-index: 10;
            color: white;
            overflow-x: auto;
        }

        .controls {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            gap: 15px;
        }

        .dropdown-container {
            position: relative;
            width: 48%;
        }

        .dropdown-button {
            width: 100%;
            padding: 10px 12px;
            background-color: #2e2e2e;
            color: white;
            border: 1px solid #3a3a3a;
            border-radius: 4px;
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.3s ease;
        }

        .dropdown-button:hover {
            background-color: #333;
            border-color: #2e7d32;
        }

        .dropdown-content {
            display: none;
            position: absolute;
            background-color: #2e2e2e;
            min-width: 100%;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            z-index: 1;
            border-radius: 4px;
            margin-top: 5px;
            overflow: hidden;
            border: 1px solid #3a3a3a;
        }

        .dropdown-content a {
            color: #e0e0e0;
            padding: 8px 10px;
            text-decoration: none;
            display: block;
            transition: all 0.2s ease;
            border-bottom: 1px solid #3a3a3a;
            font-size: 12px;
        }

        .dropdown-content a:hover {
            background-color: #2e7d32;
            color: white;
        }

        .dropdown-content a:last-child {
            border-bottom: none;
        }

        .show {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .months-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }

        .month-cell {
            background-color: #2e2e2e;
            border-radius: 4px;
            padding: 6px;
            display: flex;
            flex-direction: column;
            align-items: center;
            transition: all 0.3s ease;
            cursor: pointer;
            border: 1px solid #3a3a3a;
            position: relative;
            overflow: hidden;
        }

        .month-cell::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #2e7d32, #388e3c);
            transform: scaleX(0);
            transform-origin: left;
            transition: transform 0.3s ease;
        }

        .month-cell:hover::before {
            transform: scaleX(1);
        }

        .month-cell:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            border-color: #2e7d32;
        }

        .quarter-label {
            position: absolute;
            top: -8px;
            right: -8px;
            background-color: #2e7d32;
            color: white;
            font-size: 9px;
            font-weight: 600;
            padding: 2px 4px;
            border-radius: 2px;
        }

        .month-icon {
            font-size: 14px;
            color: #2e7d32;
            margin-bottom: 3px;
        }

        .month-name {
            font-size: 10px;
            font-weight: 600;
            color: #e0e0e0;
            margin-bottom: 3px;
            text-align: center;
        }

        .month-input {
            width: 100%;
            padding: 4px;
            border: 1px solid #3a3a3a;
            border-radius: 4px;
            text-align: center;
            font-size: 12px;
            font-weight: 700;
            color: #2e7d32;
            background-color: #1a1a1a;
            transition: all 0.3s ease;
        }

        .month-input:focus {
            outline: none;
            border-color: #2e7d32;
            box-shadow: 0 0 0 2px rgba(46, 125, 50, 0.2);
        }

        .total-container {
            margin-bottom: 15px;
            background-color: #2e2e2e;
            border-radius: 4px;
            padding: 10px;
            border-left: 4px solid #2e7d32;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .total-label {
            font-size: 14px;
            font-weight: 600;
            color: #e0e0e0;
        }

        .total-value {
            font-size: 20px;
            font-weight: 700;
            color: #2e7d32;
        }

        .costs-section {
            margin-top: 5px;
        }

        .section-title {
            font-size: 14px;
            font-weight: 600;
            color: #e0e0e0;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
        }

        .section-title i {
            margin-right: 8px;
            color: #2e7d32;
        }

        .costs-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            margin-bottom: 10px;
        }

        .cost-cell {
            background-color: #2e2e2e;
            border-radius: 4px;
            padding: 8px;
            display: flex;
            flex-direction: column;
            align-items: center;
            transition: all 0.3s ease;
            border: 1px solid #3a3a3a;
            position: relative;
            overflow: hidden;
        }

        .cost-cell::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #2e7d32, #388e3c);
            transform: scaleX(0);
            transform-origin: left;
            transition: transform 0.3s ease;
        }

        .cost-cell:hover::before {
            transform: scaleX(1);
        }

        .cost-cell:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            border-color: #2e7d32;
        }

        .cost-icon {
            font-size: 16px;
            color: #2e7d32;
            margin-bottom: 3px;
        }

        .cost-title {
            font-size: 10px;
            font-weight: 600;
            color: #e0e0e0;
            margin-bottom: 3px;
            text-align: center;
        }

        .cost-input {
            width: 100%;
            padding: 4px;
            border: 1px solid #3a3a3a;
            border-radius: 4px;
            text-align: center;
            font-size: 12px;
            font-weight: 700;
            color: #2e7d32;
            background-color: #1a1a1a;
            transition: all 0.3s ease;
        }

        .cost-input:focus {
            outline: none;
            border-color: #2e7d32;
            box-shadow: 0 0 0 2px rgba(46, 125, 50, 0.2);
        }

        .cost-result {
            margin-top: 3px;
            font-size: 10px;
            font-weight: 600;
            color: #a5d6a7;
        }

        .salary-table {
            width: 100%;
            border-collapse: collapse;
            background-color: #2e2e2e;
            border-radius: 4px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            margin-bottom: 10px;
        }

        .salary-table th, .salary-table td {
            padding: 6px 8px;
            text-align: left;
            border-bottom: 1px solid #3a3a3a;
            font-size: 12px;
        }

        .salary-table th {
            background-color: rgba(46, 125, 50, 0.2);
            color: #e0e0e0;
            font-weight: 600;
        }

        .salary-table td {
            color: #bdbdbd;
        }

        .salary-table tr:last-child td {
            border-bottom: none;
        }

        .salary-value {
            font-weight: 700;
            color: #2e7d32;
        }

        .footer {
            padding: 10px 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            background: rgba(0, 0, 0, 0.2);
        }

        .social-icons {
            display: flex;
            gap: 15px;
        }

        .social-icons a {
            color: #bdbdbd;
            transition: color 0.3s ease;
        }

        .social-icons a:hover {
            color: #2e7d32;
        }

        .grid-pattern {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image:
                linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 20px 20px;
            z-index: 0;
        }

        .accent-shape {
            position: absolute;
            background-color: rgba(46, 125, 50, 0.1);
            z-index: 1;
        }

        .accent-shape-1 {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            top: -150px;
            right: -100px;
        }

        .accent-shape-2 {
            width: 200px;
            height: 200px;
            border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
            bottom: -80px;
            left: -80px;
        }

        .calculation-info {
            margin-top: 8px;
            padding: 6px 10px;
            background-color: rgba(46, 125, 50, 0.1);
            border-radius: 4px;
            font-size: 12px;
            color: #e0e0e0;
            display: none;
        }

        .calculation-info.active {
            display: block;
        }

        .formula-display {
            margin-top: 6px;
            padding: 6px 8px;
            background-color: rgba(46, 125, 50, 0.05);
            border-radius: 4px;
            font-family: monospace;
            font-size: 11px;
            color: #a5d6a7;
            display: none;
        }

        .formula-display.active {
            display: block;
        }

        .coefficient-table {
            margin-top: 8px;
            border-collapse: collapse;
            width: 100%;
            background-color: rgba(46, 125, 50, 0.05);
            border-radius: 4px;
            overflow: hidden;
            display: none;
            font-size: 11px;
        }

        .coefficient-table.active {
            display: table;
        }

        .coefficient-table th, .coefficient-table td {
            padding: 5px 8px;
            text-align: left;
            border-bottom: 1px solid rgba(46, 125, 50, 0.2);
        }

        .coefficient-table th {
            background-color: rgba(46, 125, 50, 0.2);
            color: #e0e0e0;
            font-weight: 600;
            font-size: 11px;
        }

        .coefficient-table td {
            color: #bdbdbd;
            font-size: 11px;
        }

        .coefficient-table tr:last-child td {
            border-bottom: none;
        }

        .coefficient-value {
            font-weight: 700;
            color: #2e7d32;
        }

        .total-costs-container {
            margin-top: 10px;
            background-color: #2e2e2e;
            border-radius: 4px;
            padding: 10px;
            border-left: 4px solid #2e7d32;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .total-costs-label {
            font-size: 14px;
            font-weight: 600;
            color: #e0e0e0;
        }

        .total-costs-value {
            font-size: 20px;
            font-weight: 700;
            color: #2e7d32;
        }

        .summary-container {
            margin-top: 15px;
            background-color: #2e2e2e;
            border-radius: 4px;
            padding: 12px;
            border-left: 4px solid #2e7d32;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .summary-title {
            font-size: 16px;
            font-weight: 700;
            color: #e0e0e0;
            margin-bottom: 10px;
            text-align: center;
        }

        .metrics-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .metric-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .metric-label {
            font-size: 14px;
            font-weight: 600;
            color: #e0e0e0;
            display: flex;
            align-items: center;
        }

        .metric-label i {
            margin-right: 8px;
            color: #2e7d32;
        }

        .metric-value {
            font-size: 18px;
            font-weight: 700;
            color: #2e7d32;
        }

        .metric-value.negative {
            color: #e53935;
        }

        .formula-info {
            margin-top: 8px;
            padding: 8px;
            background-color: rgba(46, 125, 50, 0.05);
            border-radius: 4px;
            font-size: 12px;
            color: #a5d6a7;
            text-align: center;
        }

        /* Медиа-запросы для адаптивности */
        @media (max-width: 1024px) {
            .months-grid {
                grid-template-columns: repeat(4, 1fr);
            }

            .costs-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .header h1 {
                font-size: 18px;
            }
        }

        @media (max-width: 768px) {
            .controls {
                flex-direction: column;
                gap: 10px;
            }

            .dropdown-container {
                width: 100%;
            }

            .months-grid {
                grid-template-columns: repeat(3, 1fr);
                gap: 6px;
            }

            .month-cell {
                padding: 5px;
            }

            .month-icon {
                font-size: 12px;
            }

            .month-name {
                font-size: 9px;
            }

            .month-input {
                font-size: 11px;
                padding: 3px;
            }

            .costs-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 6px;
            }

            .cost-cell {
                padding: 6px;
            }

            .cost-icon {
                font-size: 14px;
            }

            .cost-title {
                font-size: 9px;
            }

            .cost-input {
                font-size: 11px;
                padding: 3px;
            }

            .cost-result {
                font-size: 9px;
            }

            .header h1 {
                font-size: 16px;
            }

            .header p {
                font-size: 11px;
            }

            .content {
                padding: 10px;
            }

            .total-container, .total-costs-container {
                padding: 8px;
            }

            .total-label, .total-costs-label {
                font-size: 13px;
            }

            .total-value, .total-costs-value {
                font-size: 18px;
            }

            .metric-label {
                font-size: 13px;
            }

            .metric-value {
                font-size: 16px;
            }

            .section-title {
                font-size: 13px;
            }

            .salary-table th, .salary-table td {
                padding: 5px 6px;
                font-size: 11px;
            }

            .coefficient-table th, .coefficient-table td {
                padding: 4px 6px;
                font-size: 10px;
            }
        }

        @media (max-width: 480px) {
            .months-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .costs-grid {
                grid-template-columns: 1fr;
            }

            .header h1 {
                font-size: 14px;
            }

            .header p {
                font-size: 10px;
            }

            .content {
                padding: 8px;
            }

            .month-cell {
                padding: 4px;
            }

            .month-icon {
                font-size: 11px;
            }

            .month-name {
                font-size: 8px;
            }

            .month-input {
                font-size: 10px;
                padding: 2px;
            }

            .cost-cell {
                padding: 5px;
            }

            .cost-icon {
                font-size: 13px;
            }

            .cost-title {
                font-size: 8px;
            }

            .cost-input {
                font-size: 10px;
                padding: 2px;
            }

            .cost-result {
                font-size: 8px;
            }

            .total-container, .total-costs-container {
                flex-direction: column;
                align-items: flex-start;
                gap: 5px;
            }

            .metric-row {
                flex-direction: column;
                align-items: flex-start;
                gap: 5px;
            }

            .salary-table th, .salary-table td {
                padding: 4px 5px;
                font-size: 10px;
            }

            .coefficient-table th, .coefficient-table td {
                padding: 3px 5px;
                font-size: 9px;
            }
        }
    </style>
</head>
<body>
<div class="poster-container">
    <!-- Background Elements -->
    <div class="grid-pattern"></div>
    <div class="accent-shape accent-shape-1"></div>
    <div class="accent-shape accent-shape-2"></div>

    <div class="header">
        <h1>Экономико-математическая модель для оценки целесообразности выхода промышленных потребителей на ОРЭМ</h1>
        <p>Выберите компанию и значения для каждого месяца</p>
    </div>

    <div class="content">
        <div class="controls">
            <div class="dropdown-container">
                <button class="dropdown-button" onclick="toggleDropdown('company-dropdown')">
                    <span>Выберите компанию</span>
                    <i class="fas fa-chevron-down"></i>
                </button>
                <div id="company-dropdown" class="dropdown-content">
                    <a href="#">Русэнергосбыт</a>
                    <a href="#">ПСК</a>
                </div>
            </div>

            <div class="dropdown-container">
                <button class="dropdown-button" onclick="toggleDropdown('values-dropdown')">
                    <span>Выберите значение</span>
                    <i class="fas fa-chevron-down"></i>
                </button>
                <div id="values-dropdown" class="dropdown-content">
                    <a href="#">менее 670 кВт</a>
                    <a href="#">от 670 кВт до 10 МВт</a>
                    <a href="#">не менее 10 МВт</a>
                </div>
            </div>
        </div>

        <div class="months-grid">
            <div class="month-cell">
                <div class="quarter-label">Q1</div>
                <i class="month-icon fas fa-snowflake"></i>
                <div class="month-name">Январь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-heart"></i>
                <div class="month-name">Февраль</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-seedling"></i>
                <div class="month-name">Март</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <div class="quarter-label">Q2</div>
                <i class="month-icon fas fa-cloud-rain"></i>
                <div class="month-name">Апрель</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-sun"></i>
                <div class="month-name">Май</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-umbrella-beach"></i>
                <div class="month-name">Июнь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <div class="quarter-label">Q3</div>
                <i class="month-icon fas fa-fire"></i>
                <div class="month-name">Июль</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-leaf"></i>
                <div class="month-name">Август</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-apple-alt"></i>
                <div class="month-name">Сентябрь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <div class="quarter-label">Q4</div>
                <i class="month-icon fas fa-wind"></i>
                <div class="month-name">Октябрь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-cloud"></i>
                <div class="month-name">Ноябрь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
            <div class="month-cell">
                <i class="month-icon fas fa-gift"></i>
                <div class="month-name">Декабрь</div>
                <input type="number" class="month-input" placeholder="0" onchange="updateTotal()">
            </div>
        </div>

        <div class="total-container">
            <div class="total-label">
                <i class="fas fa-calculator mr-2"></i>Экономия
            </div>
            <div class="total-value" id="total-value">0</div>
        </div>

        <div class="calculation-info" id="calculation-info">
            <i class="fas fa-info-circle mr-2"></i>
            <span id="calculation-text">Расчет с коэффициентом</span>
        </div>

        <div class="formula-display" id="formula-display">
            <span id="formula-text">Формула расчета:</span>
        </div>

        <table class="coefficient-table" id="coefficient-table">
            <thead>
            <tr>
                <th>Компания</th>
                <th>Диапазон мощности</th>
                <th>Коэффициент</th>
            </tr>
            </thead>
            <tbody>
            <tr>
                <td>Русэнергосбыт</td>
                <td>менее 670 кВт</td>
                <td class="coefficient-value">0.46882</td>
            </tr>
            <tr>
                <td>Русэнергосбыт</td>
                <td>от 670 кВт до 10 МВт</td>
                <td class="coefficient-value">0.24950</td>
            </tr>
            <tr>
                <td>Русэнергосбыт</td>
                <td>не менее 10 МВт</td>
                <td class="coefficient-value">0.17991</td>
            </tr>
            <tr>
                <td>ПСК</td>
                <td>менее 670 кВт</td>
                <td class="coefficient-value">0.50474</td>
            </tr>
            <tr>
                <td>ПСК</td>
                <td>от 670 кВт до 10 МВт</td>
                <td class="coefficient-value">0.27032</td>
            </tr>
            <tr>
                <td>ПСК</td>
                <td>не менее 10 МВт</td>
                <td class="coefficient-value">0.17891</td>
            </tr>
            </tbody>
        </table>

        <div class="costs-section">
            <div class="section-title">
                <i class="fas fa-coins"></i>Затраты
            </div>
            <div class="costs-grid">
                <div class="cost-cell">
                    <i class="cost-icon fas fa-tools"></i>
                    <div class="cost-title">Технический специалист</div>
                    <input type="number" class="cost-input" id="tech-count" placeholder="Кол-во" onchange="updateCosts()">
                    <div class="cost-result" id="tech-cost">Затраты: 0 руб.</div>
                </div>
                <div class="cost-cell">
                    <i class="cost-icon fas fa-balance-scale"></i>
                    <div class="cost-title">Юрист</div>
                    <input type="number" class="cost-input" id="lawyer-count" placeholder="Кол-во" onchange="updateCosts()">
                    <div class="cost-result" id="lawyer-cost">Затраты: 0 руб.</div>
                </div>
                <div class="cost-cell">
                    <i class="cost-icon fas fa-chart-line"></i>
                    <div class="cost-title">Аналитик рынка</div>
                    <input type="number" class="cost-input" id="analyst-count" placeholder="Кол-во" onchange="updateCosts()">
                    <div class="cost-result" id="analyst-cost">Затраты: 0 руб.</div>
                </div>
                <div class="cost-cell">
                    <i class="cost-icon fas fa-shopping-cart"></i>
                    <div class="cost-title">Менеджер по закупкам</div>
                    <input type="number" class="cost-input" id="manager-count" placeholder="Кол-во" onchange="updateCosts()">
                    <div class="cost-result" id="manager-cost">Затраты: 0 руб.</div>
                </div>
            </div>

            <table class="salary-table">
                <thead>
                <tr>
                    <th>Должность</th>
                    <th>Зарплата (руб.)</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    <td>Технический специалист</td>
                    <td class="salary-value">75 000</td>
                </tr>
                <tr>
                    <td>Юрист</td>
                    <td class="salary-value">85 000</td>
                </tr>
                <tr>
                    <td>Аналитик рынка</td>
                    <td class="salary-value">105 000</td>
                </tr>
                <tr>
                    <td>Менеджер по закупкам</td>
                    <td class="salary-value">75 000</td>
                </tr>
                </tbody>
            </table>

            <div class="total-costs-container">
                <div class="total-costs-label">
                    <i class="fas fa-calculator mr-2"></i>Общие затраты
                </div>
                <div class="total-costs-value" id="total-costs-value">0</div>
            </div>
        </div>

        <div class="summary-container">
            <div class="summary-title">Итого</div>
            <div class="metrics-container">
                <div class="metric-row">
                    <div class="metric-label">
                        <i class="fas fa-chart-line"></i>Эффективность
                    </div>
                    <div class="metric-value" id="efficiency-value">0%</div>
                </div>
                <div class="metric-row">
                    <div class="metric-label">
                        <i class="fas fa-coins"></i>Эффектность
                    </div>
                    <div class="metric-value" id="effectiveness-value">0 руб.</div>
                </div>
            </div>
            <div class="formula-info">

            </div>
        </div>
    </div>

    <div class="footer">
    </div>
</div>

<script>
    function toggleDropdown(id) {
        // Close all dropdowns first
        const allDropdowns = document.querySelectorAll('.dropdown-content');
        allDropdowns.forEach(dropdown => {
            if (dropdown.id !== id) {
                dropdown.classList.remove('show');
            }
        });

        // Toggle the selected dropdown
        document.getElementById(id).classList.toggle("show");
    }

    // Close dropdowns when clicking outside
    window.onclick = function(event) {
        if (!event.target.matches('.dropdown-button') && !event.target.matches('i') && !event.target.matches('span')) {
            const dropdowns = document.querySelectorAll('.dropdown-content');
            dropdowns.forEach(dropdown => {
                if (dropdown.classList.contains('show')) {
                    dropdown.classList.remove('show');
                }
            });
        }
    }

    // Add event listeners to dropdown options
    document.querySelectorAll('.dropdown-content a').forEach(option => {
        option.addEventListener('click', function(e) {
            e.preventDefault();
            const selectedText = this.textContent;
            const button = this.closest('.dropdown-container').querySelector('.dropdown-button span');
            button.textContent = selectedText;

            // Close the dropdown
            this.closest('.dropdown-content').classList.remove('show');

            // Update total when dropdown changes
            updateTotal();
        });
    });

    // Function to update total
    function updateTotal() {
        const inputs = document.querySelectorAll('.month-input');
        let total = 0;
        let coefficient = 1; // Default coefficient
        let calculationText = '';
        let formulaText = 'Формула расчета: ';

        // Check which calculation is needed
        const companyButton = document.querySelector('#company-dropdown').previousElementSibling.querySelector('span');
        const valueButton = document.querySelector('#values-dropdown').previousElementSibling.querySelector('span');

        const company = companyButton.textContent;
        const value = valueButton.textContent;

        // Show calculation info
        const calcInfo = document.getElementById('calculation-info');
        const calcText = document.getElementById('calculation-text');
        const formulaDisplay = document.getElementById('formula-display');
        const formulaTextElement = document.getElementById('formula-text');
        const coefficientTable = document.getElementById('coefficient-table');

        if (company === 'Русэнергосбыт' && value === 'менее 670 кВт') {
            coefficient = 0.46882;
            calculationText = 'Расчет с коэффициентом 0.46882 для Русэнергосбыт (менее 670 кВт)';
            formulaText += 'Итого = (Январь × 0.46882) + (Февраль × 0.46882) + ... + (Декабрь × 0.46882)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else if (company === 'Русэнергосбыт' && value === 'от 670 кВт до 10 МВт') {
            coefficient = 0.24950;
            calculationText = 'Расчет с коэффициентом 0.24950 для Русэнергосбыт (от 670 кВт до 10 МВт)';
            formulaText += 'Итого = (Январь × 0.24950) + (Февраль × 0.24950) + ... + (Декабрь × 0.24950)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else if (company === 'Русэнергосбыт' && value === 'не менее 10 МВт') {
            coefficient = 0.17991;
            calculationText = 'Расчет с коэффициентом 0.17991 для Русэнергосбыт (не менее 10 МВт)';
            formulaText += 'Итого = (Январь × 0.17991) + (Февраль × 0.17991) + ... + (Декабрь × 0.17991)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else if (company === 'ПСК' && value === 'менее 670 кВт') {
            coefficient = 0.50474;
            calculationText = 'Расчет с коэффициентом 0.50474 для ПСК (менее 670 кВт)';
            formulaText += 'Итого = (Январь × 0.50474) + (Февраль × 0.50474) + ... + (Декабрь × 0.50474)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else if (company === 'ПСК' && value === 'от 670 кВт до 10 МВт') {
            coefficient = 0.27032;
            calculationText = 'Расчет с коэффициентом 0.27032 для ПСК (от 670 кВт до 10 МВт)';
            formulaText += 'Итого = (Январь × 0.27032) + (Февраль × 0.27032) + ... + (Декабрь × 0.27032)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else if (company === 'ПСК' && value === 'не менее 10 МВт') {
            coefficient = 0.17891;
            calculationText = 'Расчет с коэффициентом 0.17891 для ПСК (не менее 10 МВт)';
            formulaText += 'Итого = (Январь × 0.17891) + (Февраль × 0.17891) + ... + (Декабрь × 0.17891)';
            calcInfo.classList.add('active');
            formulaDisplay.classList.add('active');
            coefficientTable.classList.add('active');
        }
        else {
            calculationText = 'Простое суммирование значений';
            formulaText += 'Итого = Январь + Февраль + ... + Декабрь';
            calcInfo.classList.remove('active');
            formulaDisplay.classList.remove('active');
            coefficientTable.classList.remove('active');
        }

        calcText.textContent = calculationText;
        formulaTextElement.textContent = formulaText;

        // Calculate total
        inputs.forEach(input => {
            const value = parseFloat(input.value) || 0;
            total += value * coefficient;
        });

        document.getElementById('total-value').textContent = total.toFixed(2);

        // Update efficiency and effectiveness
        updateMetrics();
    }

    // Function to update costs
    function updateCosts() {
        // Salaries from table
        const salaries = {
            tech: 75000,
            lawyer: 85000,
            analyst: 105000,
            manager: 75000
        };

        // Get counts from inputs
        const techCount = parseFloat(document.getElementById('tech-count').value) || 0;
        const lawyerCount = parseFloat(document.getElementById('lawyer-count').value) || 0;
        const analystCount = parseFloat(document.getElementById('analyst-count').value) || 0;
        const managerCount = parseFloat(document.getElementById('manager-count').value) || 0;

        // Calculate costs for each position
        const techCost = techCount * salaries.tech;
        const lawyerCost = lawyerCount * salaries.lawyer;
        const analystCost = analystCount * salaries.analyst;
        const managerCost = managerCount * salaries.manager;

        // Update individual cost displays
        document.getElementById('tech-cost').textContent = `Затраты: ${techCost.toLocaleString('ru-RU')} руб.`;
        document.getElementById('lawyer-cost').textContent = `Затраты: ${lawyerCost.toLocaleString('ru-RU')} руб.`;
        document.getElementById('analyst-cost').textContent = `Затраты: ${analystCost.toLocaleString('ru-RU')} руб.`;
        document.getElementById('manager-cost').textContent = `Затраты: ${managerCost.toLocaleString('ru-RU')} руб.`;

        // Calculate and update total costs
        const totalCosts = (techCost + lawyerCost + analystCost + managerCost)*12+825000*4;
        document.getElementById('total-costs-value').textContent = totalCosts.toLocaleString('ru-RU');

        // Update efficiency and effectiveness
        updateMetrics();
    }

    // Function to update efficiency and effectiveness
    function updateMetrics() {
        // Get economy value
        const economyValue = parseFloat(document.getElementById('total-value').textContent) || 0;

        // Get total costs value
        const totalCostsText = document.getElementById('total-costs-value').textContent;
        // Remove spaces and convert to number
        const totalCosts = parseFloat(totalCostsText.replace(/\s/g, '')) || 0;

        // Calculate efficiency
        let efficiency = 0;
        if (totalCosts > 0) {
            efficiency = (economyValue / totalCosts) * 100;
        }

        // Calculate effectiveness
        const effectiveness = economyValue - totalCosts;

        // Update efficiency display
        const efficiencyElement = document.getElementById('efficiency-value');
        efficiencyElement.textContent = efficiency.toFixed(2) + '%';

        // Update effectiveness display
        const effectivenessElement = document.getElementById('effectiveness-value');
        effectivenessElement.textContent = effectiveness.toLocaleString('ru-RU') + ' руб.';

        // Add negative class if values are negative
        if (efficiency < 0) {
            efficiencyElement.classList.add('negative');
        } else {
            efficiencyElement.classList.remove('negative');
        }

        if (effectiveness < 0) {
            effectivenessElement.classList.add('negative');
        } else {
            effectivenessElement.classList.remove('negative');
        }
    }
</script>
</body>
</html>
