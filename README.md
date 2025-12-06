<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>挑码助手</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4efe9 100%);
            color: #333;
            line-height: 1.6;
            min-height: 100vh;
            padding: 10px;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #ff6b6b 0%, #ffa726 100%);
            color: white;
            text-align: center;
            padding: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        h1 {
            font-size: 2rem;
            margin-bottom: 5px;
        }
        
        .subtitle {
            font-size: 1rem;
            opacity: 0.9;
        }
        
        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            padding: 20px;
        }
        
        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }
        
        .panel {
            background: #f9f9f9;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
        }
        
        .panel-title {
            font-size: 1.2rem;
            color: #2c3e50;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #ffa726;
        }
        
        .numbers-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .number-ball {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.2);
            position: relative;
            user-select: none;
            font-size: 1.2rem; /* 增大字体 */
        }
        
        .number-ball.red {
            background: linear-gradient(135deg, #ff5252, #ff7675);
        }
        
        .number-ball.green {
            background: linear-gradient(135deg, #00b894, #55efc4);
        }
        
        .number-ball.blue {
            background: linear-gradient(135deg, #0984e3, #74b9ff);
        }
        
        .number-ball.selected {
            opacity: 0.6;
            transform: scale(0.9);
        }
        
        .number-ball.killed {
            opacity: 0.4;
            transform: scale(0.85);
            background: #b2bec3 !important;
        }
        
        .number-ball.selected::after {
            content: "✓";
            position: absolute;
            font-size: 20px;
            color: #00b894;
            font-weight: bold;
        }
        
        .number-ball.killed::after {
            content: "✕";
            position: absolute;
            font-size: 20px;
            color: #ff7675;
            font-weight: bold;
        }
        
        .zodiac-label {
            font-size: 0.7rem;
            margin-top: 2px;
            opacity: 0.9;
        }
        
        .control-buttons {
            display: flex;
            gap: 10px;
            margin: 15px 0;
        }
        
        .control-btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
        }
        
        .clear-btn {
            background: linear-gradient(135deg, #ff7675, #ff5252);
            color: white;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #74b9ff, #0984e3);
            color: white;
        }
        
        .share-btn {
            background: linear-gradient(135deg, #00b894, #00a085);
            color: white;
        }
        
        .control-btn:hover {
            transform: translateY(-2px);
        }
        
        .lists-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 15px;
        }
        
        .list-box {
            background: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }
        
        .list-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 10px;
            text-align: center;
            padding-bottom: 8px;
            border-bottom: 2px solid #ddd;
        }
        
        .selected-list .list-title {
            color: #00b894;
            border-bottom-color: #00b894;
        }
        
        .killed-list .list-title {
            color: #ff7675;
            border-bottom-color: #ff7675;
        }
        
        .list-items {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            min-height: 120px;
            padding: 10px 0;
        }
        
        .list-number {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem; /* 增大字体 */
            font-weight: bold;
            color: white;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .selected-list .list-number {
            background: #00b894;
        }
        
        .killed-list .list-number {
            background: #b2bec3;
        }
        
        .empty-message {
            color: #aaa;
            font-style: italic;
            text-align: center;
            width: 100%;
            margin-top: 20px;
        }
        
        .category-section {
            margin-bottom: 15px;
        }
        
        .section-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: #2c3e50;
        }
        
        .category-buttons {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 8px;
        }
        
        .category-buttons.tail-buttons {
            grid-template-columns: repeat(5, 1fr);
        }
        
        .category-buttons.head-buttons {
            grid-template-columns: repeat(5, 1fr);
        }
        
        .category-buttons.property-buttons {
            grid-template-columns: repeat(7, 1fr);
        }
        
        .category-btn {
            padding: 10px 5px;
            border: none;
            border-radius: 6px;
            font-size: 0.9rem;
            background: #dfe6e9;
            color: #2d3436;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
        }
        
        .category-btn.red {
            background: linear-gradient(135deg, #ff7675, #ff5252);
            color: white;
        }
        
        .category-btn.green {
            background: linear-gradient(135deg, #00b894, #00a085);
            color: white;
        }
        
        .category-btn.blue {
            background: linear-gradient(135deg, #74b9ff, #0984e3);
            color: white;
        }
        
        .category-btn.active {
            background: linear-gradient(135deg, #fdcb6e, #e17055);
            color: white;
        }
        
        .zodiac-chart {
            margin-top: 20px;
            padding: 20px;
            background: #f9f9f9;
            border-radius: 10px;
        }
        
        .zodiac-chart-title {
            font-size: 1.3rem;
            color: #2c3e50;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .zodiac-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
        }
        
        @media (max-width: 768px) {
            .zodiac-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
        .zodiac-item {
            background: white;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.08);
        }
        
        .zodiac-name {
            font-size: 1.2rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 5px;
        }
        
        .zodiac-conflict {
            font-size: 0.9rem;
            color: #ff7675;
            margin-bottom: 10px;
        }
        
        .zodiac-numbers {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        .zodiac-number {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.9rem;
            font-weight: bold;
            color: white;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            background: #2c3e50;
            color: #ecf0f1;
        }
        
        /* 响应式调整 */
        @media (max-width: 480px) {
            .number-ball {
                font-size: 1rem;
            }
            
            .list-number {
                width: 35px;
                height: 35px;
                font-size: 0.9rem;
            }
            
            .zodiac-number {
                width: 35px;
                height: 35px;
                font-size: 0.8rem;
            }
            
            .category-buttons {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .category-buttons.tail-buttons {
                grid-template-columns: repeat(5, 1fr);
            }
            
            .category-buttons.head-buttons {
                grid-template-columns: repeat(5, 1fr);
            }
            
            .category-buttons.property-buttons {
                grid-template-columns: repeat(4, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>挑码助手</h1>
            <p class="subtitle">智能选号，轻松挑码</p>
        </header>
        
        <div class="main-content">
            <div class="panel">
                <div class="panel-title">数字选号区</div>
                
                <div class="numbers-grid" id="numbersGrid"></div>
                
                <div class="control-buttons">
                    <button class="control-btn clear-btn" id="clearBtn">清空选择</button>
                    <button class="control-btn copy-btn" id="copyBtn">复制结果</button>
                    <button class="control-btn share-btn" id="shareBtn">分享结果</button>
                </div>
                
                <div class="lists-container">
                    <div class="list-box selected-list">
                        <div class="list-title">已选号码</div>
                        <div class="list-items" id="selectedList">
                            <div class="empty-message">暂无已选号码</div>
                        </div>
                    </div>
                    
                    <div class="list-box killed-list">
                        <div class="list-title">已杀号码</div>
                        <div class="list-items" id="killedList">
                            <div class="empty-message">暂无已杀号码</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="panel">
                <div class="panel-title">分类筛选</div>
                
                <div class="category-section">
                    <div class="section-title">十二生肖</div>
                    <div class="category-buttons" id="zodiacButtons"></div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">尾号</div>
                    <div class="category-buttons tail-buttons" id="tailButtons"></div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">头号</div>
                    <div class="category-buttons head-buttons" id="headButtons"></div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">波色与属性</div>
                    <div class="category-buttons property-buttons" id="propertyButtons"></div>
                </div>
            </div>
        </div>
        
        <div class="zodiac-chart">
            <div class="zodiac-chart-title">生肖号码对照表</div>
            <div class="zodiac-grid" id="zodiacChart"></div>
        </div>
        
        <footer>
            <p>挑码助手 &copy; 2023 - 专业选号工具</p>
        </footer>
    </div>

    <script>
        // 数字数据
        const numbersData = Array.from({length: 49}, (_, i) => {
            const num = i + 1;
            let color = 'blue';
            if ([1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46].includes(num)) color = 'red';
            if ([5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49].includes(num)) color = 'green';
            
            const zodiacs = {
                '蛇': [1, 13, 25, 37, 49],
                '龙': [2, 14, 26, 38],
                '兔': [3, 15, 27, 39],
                '虎': [4, 16, 28, 40],
                '牛': [5, 17, 29, 41],
                '鼠': [6, 18, 30, 42],
                '猪': [7, 19, 31, 43],
                '狗': [8, 20, 32, 44],
                '鸡': [9, 21, 33, 45],
                '猴': [10, 22, 34, 46],
                '羊': [11, 23, 35, 47],
                '马': [12, 24, 36, 48]
            };
            
            let zodiac = '';
            for (const [zodiacName, numbers] of Object.entries(zodiacs)) {
                if (numbers.includes(num)) {
                    zodiac = zodiacName;
                    break;
                }
            }
            
            return {num, color, zodiac};
        });

        // 分类数据
        const categories = {
            zodiac: ['鼠', '牛', '虎', '兔', '龙', '蛇', '马', '羊', '猴', '鸡', '狗', '猪'],
            tail: ['0尾', '1尾', '2尾', '3尾', '4尾', '5尾', '6尾', '7尾', '8尾', '9尾'],
            head: ['0头', '1头', '2头', '3头', '4头'],
            property: ['红波', '绿波', '蓝波', '大', '小', '单', '双']
        };

        // 生肖冲突数据
        const zodiacConflicts = {
            '鼠': '冲马',
            '牛': '冲羊',
            '虎': '冲猴',
            '兔': '冲鸡',
            '龙': '冲狗',
            '蛇': '冲猪',
            '马': '冲鼠',
            '羊': '冲牛',
            '猴': '冲虎',
            '鸡': '冲兔',
            '狗': '冲龙',
            '猪': '冲蛇'
        };

        // 状态管理
        let selectedNumbers = [];
        let killedNumbers = [];
        let clickTimers = {};
        
        // 初始化函数
        function init() {
            renderNumberGrid();
            renderCategoryButtons();
            renderZodiacChart();
            setupEventListeners();
        }
        
        // 渲染数字网格
        function renderNumberGrid() {
            const numbersGrid = document.getElementById('numbersGrid');
            numbersGrid.innerHTML = '';
            
            numbersData.forEach(data => {
                const numberBall = document.createElement('div');
                numberBall.className = `number-ball ${data.color}`;
                numberBall.textContent = data.num;
                numberBall.dataset.number = data.num;
                
                const zodiacLabel = document.createElement('div');
                zodiacLabel.className = 'zodiac-label';
                zodiacLabel.textContent = data.zodiac;
                numberBall.appendChild(zodiacLabel);
                
                numbersGrid.appendChild(numberBall);
            });
        }
        
        // 渲染分类按钮
        function renderCategoryButtons() {
            // 生肖按钮
            const zodiacButtons = document.getElementById('zodiacButtons');
            categories.zodiac.forEach(zodiac => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                button.textContent = zodiac;
                button.dataset.category = 'zodiac';
                button.dataset.value = zodiac;
                zodiacButtons.appendChild(button);
            });
            
            // 尾号按钮
            const tailButtons = document.getElementById('tailButtons');
            categories.tail.forEach(tail => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                button.textContent = tail;
                button.dataset.category = 'tail';
                button.dataset.value = tail;
                tailButtons.appendChild(button);
            });
            
            // 头号按钮
            const headButtons = document.getElementById('headButtons');
            categories.head.forEach(head => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                button.textContent = head;
                button.dataset.category = 'head';
                button.dataset.value = head;
                headButtons.appendChild(button);
            });
            
            // 属性按钮
            const propertyButtons = document.getElementById('propertyButtons');
            categories.property.forEach(property => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                
                if (property === '红波') button.classList.add('red');
                else if (property === '绿波') button.classList.add('green');
                else if (property === '蓝波') button.classList.add('blue');
                
                button.textContent = property;
                button.dataset.category = 'property';
                button.dataset.value = property;
                propertyButtons.appendChild(button);
            });
        }
        
        // 渲染生肖对照表
        function renderZodiacChart() {
            const zodiacChart = document.getElementById('zodiacChart');
            zodiacChart.innerHTML = '';
            
            categories.zodiac.forEach(zodiac => {
                const zodiacItem = document.createElement('div');
                zodiacItem.className = 'zodiac-item';
                
                const zodiacName = document.createElement('div');
                zodiacName.className = 'zodiac-name';
                zodiacName.textContent = zodiac;
                zodiacItem.appendChild(zodiacName);
                
                const zodiacConflict = document.createElement('div');
                zodiacConflict.className = 'zodiac-conflict';
                zodiacConflict.textContent = zodiacConflicts[zodiac];
                zodiacItem.appendChild(zodiacConflict);
                
                const zodiacNumbers = document.createElement('div');
                zodiacNumbers.className = 'zodiac-numbers';
                
                // 获取该生肖对应的数字
                const numbers = numbersData
                    .filter(data => data.zodiac === zodiac)
                    .map(data => data.num);
                
                numbers.forEach(num => {
                    const numberElement = document.createElement('div');
                    numberElement.className = 'zodiac-number';
                    
                    // 根据数字设置颜色
                    const numberData = numbersData.find(data => data.num === num);
                    if (numberData) {
                        if (numberData.color === 'red') {
                            numberElement.style.background = 'linear-gradient(135deg, #ff7675, #ff5252)';
                        } else if (numberData.color === 'green') {
                            numberElement.style.background = 'linear-gradient(135deg, #00b894, #00a085)';
                        } else {
                            numberElement.style.background = 'linear-gradient(135deg, #74b9ff, #0984e3)';
                        }
                    }
                    
                    numberElement.textContent = num;
                    zodiacNumbers.appendChild(numberElement);
                });
                
                zodiacItem.appendChild(zodiacNumbers);
                zodiacChart.appendChild(zodiacItem);
            });
        }
        
        // 设置事件监听器
        function setupEventListeners() {
            // 数字按钮点击事件
            document.getElementById('numbersGrid').addEventListener('click', function(e) {
                const numberBall = e.target.closest('.number-ball');
                if (!numberBall) return;
                
                const number = parseInt(numberBall.dataset.number);
                const timerId = `number-${number}`;
                
                if (clickTimers[timerId]) {
                    clearTimeout(clickTimers[timerId]);
                    handleDoubleClick(number, numberBall);
                    delete clickTimers[timerId];
                } else {
                    clickTimers[timerId] = setTimeout(() => {
                        handleSingleClick(number, numberBall);
                        delete clickTimers[timerId];
                    }, 300);
                }
            });
            
            // 分类按钮点击事件
            document.querySelectorAll('.category-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const timerId = `category-${button.dataset.category}-${button.dataset.value}`;
                    
                    if (clickTimers[timerId]) {
                        clearTimeout(clickTimers[timerId]);
                        handleCategoryDoubleClick(button);
                        delete clickTimers[timerId];
                    } else {
                        clickTimers[timerId] = setTimeout(() => {
                            handleCategorySingleClick(button);
                            delete clickTimers[timerId];
                        }, 300);
                    }
                });
            });
            
            // 控制按钮事件
            document.getElementById('clearBtn').addEventListener('click', clearAll);
            document.getElementById('copyBtn').addEventListener('click', copyResults);
            document.getElementById('shareBtn').addEventListener('click', shareResults);
            
            // 列表项点击事件（从列表中移除）
            document.getElementById('selectedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (listNumber) removeFromList(listNumber, 'selected');
            });
            
            document.getElementById('killedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (listNumber) removeFromList(listNumber, 'killed');
            });
        }
        
        // 处理数字单击
        function handleSingleClick(number, numberBall) {
            if (killedNumbers.includes(number)) {
                killedNumbers = killedNumbers.filter(n => n !== number);
                numberBall.classList.remove('killed');
            }
            
            const index = selectedNumbers.indexOf(number);
            if (index === -1) {
                selectedNumbers.push(number);
                numberBall.classList.add('selected');
            } else {
                selectedNumbers.splice(index, 1);
                numberBall.classList.remove('selected');
            }
            
            updateLists();
        }
        
        // 处理数字双击
        function handleDoubleClick(number, numberBall) {
            if (selectedNumbers.includes(number)) {
                selectedNumbers = selectedNumbers.filter(n => n !== number);
                numberBall.classList.remove('selected');
            }
            
            const index = killedNumbers.indexOf(number);
            if (index === -1) {
                killedNumbers.push(number);
                numberBall.classList.add('killed');
            } else {
                killedNumbers.splice(index, 1);
                numberBall.classList.remove('killed');
            }
            
            updateLists();
        }
        
        // 处理分类按钮单击
        function handleCategorySingleClick(button) {
            const numbers = getNumbersByCategory(button.dataset.category, button.dataset.value);
            
            button.classList.remove('killed');
            
            if (button.classList.contains('active')) {
                numbers.forEach(num => {
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                    killedNumbers = killedNumbers.filter(n => n !== num);
                });
                button.classList.remove('active');
            } else {
                numbers.forEach(num => {
                    if (!selectedNumbers.includes(num)) selectedNumbers.push(num);
                    killedNumbers = killedNumbers.filter(n => n !== num);
                });
                button.classList.add('active');
            }
            
            updateNumberGrid();
            updateLists();
        }
        
        // 处理分类按钮双击
        function handleCategoryDoubleClick(button) {
            const numbers = getNumbersByCategory(button.dataset.category, button.dataset.value);
            
            button.classList.remove('active');
            
            if (button.classList.contains('killed')) {
                numbers.forEach(num => {
                    killedNumbers = killedNumbers.filter(n => n !== num);
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                });
                button.classList.remove('killed');
            } else {
                numbers.forEach(num => {
                    if (!killedNumbers.includes(num)) killedNumbers.push(num);
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                });
                button.classList.add('killed');
            }
            
            updateNumberGrid();
            updateLists();
        }
        
        // 根据分类获取数字
        function getNumbersByCategory(category, value) {
            switch(category) {
                case 'zodiac':
                    return numbersData
                        .filter(data => data.zodiac === value)
                        .map(data => data.num);
                case 'tail':
                    const tailNum = parseInt(value);
                    return numbersData
                        .filter(data => data.num % 10 === tailNum)
                        .map(data => data.num);
                case 'head':
                    const headNum = parseInt(value);
                    return numbersData
                        .filter(data => Math.floor(data.num / 10) === headNum)
                        .map(data => data.num);
                case 'property':
                    switch(value) {
                        case '红波': return numbersData.filter(d => d.color === 'red').map(d => d.num);
                        case '绿波': return numbersData.filter(d => d.color === 'green').map(d => d.num);
                        case '蓝波': return numbersData.filter(d => d.color === 'blue').map(d => d.num);
                        case '大': return numbersData.filter(d => d.num >= 25).map(d => d.num);
                        case '小': return numbersData.filter(d => d.num < 25).map(d => d.num);
                        case '单': return numbersData.filter(d => d.num % 2 === 1).map(d => d.num);
                        case '双': return numbersData.filter(d => d.num % 2 === 0).map(d => d.num);
                        default: return [];
                    }
                default: return [];
            }
        }
        
        // 更新数字网格显示
        function updateNumberGrid() {
            document.querySelectorAll('.number-ball').forEach(ball => {
                const num = parseInt(ball.dataset.number);
                ball.classList.remove('selected', 'killed');
                
                if (selectedNumbers.includes(num)) ball.classList.add('selected');
                if (killedNumbers.includes(num)) ball.classList.add('killed');
            });
        }
        
        // 更新列表显示
        function updateLists() {
            const selectedList = document.getElementById('selectedList');
            const killedList = document.getElementById('killedList');
            
            // 更新已选列表
            selectedList.innerHTML = '';
            if (selectedNumbers.length === 0) {
                selectedList.innerHTML = '<div class="empty-message">暂无已选号码</div>';
            } else {
                selectedNumbers.sort((a, b) => a - b).forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    
                    // 保持数字原来的颜色
                    const originalData = numbersData.find(d => d.num === num);
                    if (originalData) {
                        listNumber.style.backgroundColor = 
                            originalData.color === 'red' ? '#ff5252' : 
                            originalData.color === 'green' ? '#00b894' : '#0984e3';
                    }
                    
                    selectedList.appendChild(listNumber);
                });
            }
            
            // 更新已杀列表
            killedList.innerHTML = '';
            if (killedNumbers.length === 0) {
                killedList.innerHTML = '<div class="empty-message">暂无已杀号码</div>';
            } else {
                killedNumbers.sort((a, b) => a - b).forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    killedList.appendChild(listNumber);
                });
            }
        }
        
        // 从列表中移除数字
        function removeFromList(listNumber, listType) {
            const num = parseInt(listNumber.dataset.number);
            
            if (listType === 'selected') {
                selectedNumbers = selectedNumbers.filter(n => n !== num);
            } else if (listType === 'killed') {
                killedNumbers = killedNumbers.filter(n => n !== num);
            }
            
            updateNumberGrid();
            updateLists();
        }
        
        // 清空所有
        function clearAll() {
            selectedNumbers = [];
            killedNumbers = [];
            updateNumberGrid();
            updateLists();
            
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.remove('active', 'killed');
            });
        }
        
        // 复制结果
        function copyResults() {
            const text = `已选: ${selectedNumbers.sort((a,b)=>a-b).join(', ') || '无'}\n已杀: ${killedNumbers.sort((a,b)=>a-b).join(', ') || '无'}`;
            
            navigator.clipboard.writeText(text).then(() => {
                alert('结果已复制到剪贴板');
            }).catch(err => {
                console.error('复制失败:', err);
                alert('复制失败，请手动复制');
            });
        }
        
        // 分享结果
        function shareResults() {
            const text = `已选: ${selectedNumbers.sort((a,b)=>a-b).join(', ') || '无'}\n已杀: ${killedNumbers.sort((a,b)=>a-b).join(', ') || '无'}`;
            
            if (navigator.share) {
                navigator.share({
                    title: '挑码助手结果',
                    text: text
                }).catch(err => {
                    console.error('分享失败:', err);
                    copyResults();
                });
            } else {
                copyResults();
            }
        }
        
        // 初始化应用
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
