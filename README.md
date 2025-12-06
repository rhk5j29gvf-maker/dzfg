<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>挑码助手</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4efe9 100%);
            color: #333;
            line-height: 1.6;
            min-height: 100vh;
            padding: 10px;
            touch-action: manipulation;
        }
        
        .container {
            width: 100%;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #ff6b6b 0%, #ffa726 100%);
            color: white;
            text-align: center;
            padding: 15px 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        h1 {
            font-size: 1.5rem;
            margin-bottom: 8px;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
        }
        
        .subtitle {
            font-size: 0.9rem;
            opacity: 0.9;
        }
        
        .main-content {
            display: flex;
            flex-direction: column;
            gap: 10px;
            padding: 10px;
        }
        
        .panel {
            background: #f9f9f9;
            border-radius: 8px;
            padding: 10px;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.05);
        }
        
        .panel-title {
            font-size: 1.1rem;
            color: #2c3e50;
            margin-bottom: 10px;
            padding-bottom: 8px;
            border-bottom: 2px solid #ffa726;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .panel-title i {
            margin-right: 8px;
            color: #ff6b6b;
        }
        
        .numbers-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
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
            font-size: 0.9rem;
            color: white;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: relative;
            user-select: none;
            touch-action: manipulation;
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
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        }
        
        .number-ball.killed {
            opacity: 0.4;
            transform: scale(0.85);
            background: #b2bec3 !important;
        }
        
        .number-ball.selected::after {
            content: "✓";
            position: absolute;
            font-size: 16px;
            color: #00b894;
            font-weight: bold;
        }
        
        .number-ball.killed::after {
            content: "✕";
            position: absolute;
            font-size: 16px;
            color: #ff7675;
            font-weight: bold;
        }
        
        .zodiac-label {
            font-size: 8px;
            margin-top: 2px;
            opacity: 0.9;
        }
        
        .control-buttons {
            display: flex;
            gap: 8px;
            margin: 10px 0;
        }
        
        .control-btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            font-size: 0.9rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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
        
        .control-btn:active {
            transform: scale(0.98);
        }
        
        .lists-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 15px;
        }
        
        .list-box {
            background: white;
            border-radius: 8px;
            padding: 10px;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
        }
        
        .list-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 8px;
            text-align: center;
            padding-bottom: 6px;
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
            gap: 6px;
            min-height: 80px;
            padding: 8px 0;
        }
        
        .list-number {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
            color: white;
        }
        
        .empty-message {
            color: #aaa;
            font-style: italic;
            text-align: center;
            width: 100%;
            margin-top: 15px;
            font-size: 0.9rem;
        }
        
        .category-section {
            margin-bottom: 15px;
        }
        
        .section-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 8px;
            color: #2c3e50;
            display: flex;
            align-items: center;
        }
        
        .section-title i {
            margin-right: 6px;
            color: #0984e3;
        }
        
        .category-buttons {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
        }
        
        .category-buttons.tail-buttons {
            grid-template-columns: repeat(5, 1fr);
        }
        
        .category-buttons.head-buttons {
            grid-template-columns: repeat(5, 1fr);
        }
        
        .category-buttons.property-buttons {
            grid-template-columns: repeat(3, 1fr);
        }
        
        .category-btn {
            padding: 8px 4px;
            border: none;
            border-radius: 5px;
            font-size: 0.8rem;
            background: #dfe6e9;
            color: #2d3436;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
        }
        
        .category-btn:active {
            transform: scale(0.98);
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
        
        .category-btn.killed {
            background: linear-gradient(135deg, #b2bec3, #636e72);
            color: white;
        }
        
        .zodiac-chart {
            margin-top: 15px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 8px;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.05);
        }
        
        .zodiac-chart-title {
            font-size: 1.1rem;
            color: #2c3e50;
            margin-bottom: 10px;
            text-align: center;
            padding-bottom: 8px;
            border-bottom: 2px solid #ffa726;
        }
        
        .zodiac-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        
        .zodiac-item {
            background: white;
            border-radius: 6px;
            padding: 10px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.08);
        }
        
        .zodiac-name {
            font-size: 1rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 6px;
        }
        
        .zodiac-conflict {
            font-size: 0.8rem;
            color: #ff7675;
            margin-bottom: 8px;
        }
        
        .zodiac-numbers {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 4px;
        }
        
        .zodiac-number {
            width: 25px;
            height: 25px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            color: white;
        }
        
        footer {
            text-align: center;
            padding: 15px;
            background: #2c3e50;
            color: #ecf0f1;
            margin-top: 15px;
            font-size: 0.8rem;
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
                <div class="panel-title">
                    <span><i>●</i> 数字选号区</span>
                </div>
                
                <div class="numbers-grid" id="numbersGrid">
                    <!-- 数字1-49将通过JavaScript动态生成 -->
                </div>
                
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
                <div class="panel-title">
                    <i>●</i> 分类筛选
                </div>
                
                <div class="category-section">
                    <div class="section-title">
                        <i>🐭</i> 十二生肖
                    </div>
                    <div class="category-buttons" id="zodiacButtons">
                        <!-- 生肖按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">
                        <i>🔢</i> 尾号
                    </div>
                    <div class="category-buttons tail-buttons" id="tailButtons">
                        <!-- 尾号按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">
                        <i>🔢</i> 头号
                    </div>
                    <div class="category-buttons head-buttons" id="headButtons">
                        <!-- 头号按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">
                        <i>🎨</i> 波色与属性
                    </div>
                    <div class="category-buttons property-buttons" id="propertyButtons">
                        <!-- 属性按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
            </div>
        </div>
        
        <div class="zodiac-chart">
            <div class="zodiac-chart-title">生肖号码对照表</div>
            <div class="zodiac-grid" id="zodiacChart">
                <!-- 生肖对照表将通过JavaScript动态生成 -->
            </div>
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
            loadData();
            renderNumberGrid();
            renderCategoryButtons();
            renderZodiacChart();
            setupEventListeners();
            console.log('挑码助手初始化完成');
        }
        
        // 加载本地存储的数据
        function loadData() {
            const savedSelected = localStorage.getItem('selectedNumbers');
            const savedKilled = localStorage.getItem('killedNumbers');
            
            if (savedSelected) {
                selectedNumbers = JSON.parse(savedSelected);
            }
            
            if (savedKilled) {
                killedNumbers = JSON.parse(savedKilled);
            }
        }
        
        // 保存数据到本地存储
        function saveData() {
            localStorage.setItem('selectedNumbers', JSON.stringify(selectedNumbers));
            localStorage.setItem('killedNumbers', JSON.stringify(killedNumbers));
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
            
            updateNumberGrid();
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
            saveData();
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
            saveData();
        }
        
        // 处理分类按钮单击
        function handleCategorySingleClick(button) {
            const category = button.dataset.category;
            const value = button.dataset.value;
            
            // 根据分类和值获取对应的数字
            let numbers = getNumbersByCategory(category, value);
            
            // 移除已杀状态
            button.classList.remove('killed');
            
            // 切换选中状态
            if (button.classList.contains('active')) {
                // 如果已经是激活状态，则移除对应数字
                numbers.forEach(num => {
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                    killedNumbers = killedNumbers.filter(n => n !== num);
                });
                button.classList.remove('active');
            } else {
                // 否则添加对应数字到已选列表
                numbers.forEach(num => {
                    if (!selectedNumbers.includes(num)) selectedNumbers.push(num);
                    // 从已杀列表中移除
                    killedNumbers = killedNumbers.filter(n => n !== num);
                });
                button.classList.add('active');
            }
            
            updateNumberGrid();
            updateLists();
            saveData();
        }
        
        // 处理分类按钮双击
        function handleCategoryDoubleClick(button) {
            const category = button.dataset.category;
            const value = button.dataset.value;
            
            // 根据分类和值获取对应的数字
            let numbers = getNumbersByCategory(category, value);
            
            // 移除选中状态
            button.classList.remove('active');
            
            // 切换已杀状态
            if (button.classList.contains('killed')) {
                // 如果已经是已杀状态，则移除对应数字
                numbers.forEach(num => {
                    killedNumbers = killedNumbers.filter(n => n !== num);
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                });
                button.classList.remove('killed');
            } else {
                // 否则添加对应数字到已杀列表
                numbers.forEach(num => {
                    if (!killedNumbers.includes(num)) killedNumbers.push(num);
                    // 从已选列表中移除
                    selectedNumbers = selectedNumbers.filter(n => n !== num);
                });
                button.classList.add('killed');
            }
            
            updateNumberGrid();
            updateLists();
            saveData();
        }
        
        // 根据分类获取数字
        function getNumbersByCategory(category, value) {
            switch(category) {
                case 'zodiac':
                    // 根据生肖返回对应数字
                    return numbersData
                        .filter(data => data.zodiac === value)
                        .map(data => data.num);
                case 'tail':
                    // 根据尾号返回对应数字
                    const tailNum = parseInt(value);
                    return numbersData
                        .filter(data => data.num % 10 === tailNum)
                        .map(data => data.num);
                case 'head':
                    // 根据头号返回对应数字
                    const headNum = parseInt(value);
                    return numbersData
                        .filter(data => Math.floor(data.num / 10) === headNum)
                        .map(data => data.num);
                case 'property':
                    // 根据属性返回对应数字
                    switch(value) {
                        case '红波':
                            return numbersData
                                .filter(data => data.color === 'red')
                                .map(data => data.num);
                        case '绿波':
                            return numbersData
                                .filter(data => data.color === 'green')
                                .map(data => data.num);
                        case '蓝波':
                            return numbersData
                                .filter(data => data.color === 'blue')
                                .map(data => data.num);
                        case '大':
                            return numbersData
                                .filter(data => data.num >= 25)
                                .map(data => data.num);
                        case '小':
                            return numbersData
                                .filter(data => data.num < 25)
                                .map(data => data.num);
                        case '单':
                            return numbersData
                                .filter(data => data.num % 2 === 1)
                                .map(data => data.num);
                        case '双':
                            return numbersData
                                .filter(data => data.num % 2 === 0)
                                .map(data => data.num);
                        default:
                            return [];
                    }
                default:
                    return [];
            }
        }
        
        // 更新数字网格显示
        function updateNumberGrid() {
            document.querySelectorAll('.number-ball').forEach(ball => {
                const number = parseInt(ball.dataset.number);
                ball.classList.remove('selected', 'killed');
                
                if (selectedNumbers.includes(number)) {
                    ball.classList.add('selected');
                } else if (killedNumbers.includes(number)) {
                    ball.classList.add('killed');
                }
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
                    
                    // 获取数字的颜色
                    const numberData = numbersData.find(data => data.num === num);
                    if (numberData) {
                        if (numberData.color === 'red') {
                            listNumber.style.background = 'linear-gradient(135deg, #ff5252, #ff7675)';
                        } else if (numberData.color === 'green') {
                            listNumber.style.background = 'linear-gradient(135deg, #00b894, #55efc4)';
                        } else {
                            listNumber.style.background = 'linear-gradient(135deg, #74b9ff, #0984e3)';
                        }
                    }
                    
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    selectedList.appendChild(listNumber);
                });
            }
            
            // 更新已杀列表（强制灰色）
            killedList.innerHTML = '';
            if (killedNumbers.length === 0) {
                killedList.innerHTML = '<div class="empty-message">暂无已杀号码</div>';
            } else {
                killedNumbers.sort((a, b) => a - b).forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.style.background = '#b2bec3';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    killedList.appendChild(listNumber);
                });
            }
        }
        
        // 从列表中移除数字
        function removeFromList(listNumber, listType) {
            const number = parseInt(listNumber.dataset.number);
            
            if (listType === 'selected') {
                selectedNumbers = selectedNumbers.filter(n => n !== number);
            } else if (listType === 'killed') {
                killedNumbers = killedNumbers.filter(n => n !== number);
            }
            
            updateNumberGrid();
            updateLists();
            saveData();
        }
        
        // 清空所有
        function clearAll() {
            selectedNumbers = [];
            killedNumbers = [];
            updateNumberGrid();
            updateLists();
            saveData();
            
            // 重置分类按钮状态
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.remove('active', 'killed');
            });
        }
        
        // 复制结果
        function copyResults() {
            if (selectedNumbers.length === 0) {
                alert('没有已选号码可复制');
                return;
            }
            
            // 只复制已选号码，用逗号分隔
            const numbersToCopy = selectedNumbers.sort((a, b) => a - b).join(', ');
            
            navigator.clipboard.writeText(numbersToCopy)
                .then(() => {
                    alert('已选号码已复制到剪贴板: ' + numbersToCopy);
                })
                .catch(err => {
                    console.error('复制失败: ', err);
                    alert('复制失败，请手动复制');
                });
        }
        
        // 分享结果
        function shareResults() {
            const selectedText = selectedNumbers.length > 0 ? `已选: ${selectedNumbers.sort((a, b) => a - b).join(', ')}` : '已选: 无';
            const killedText = killedNumbers.length > 0 ? `已杀: ${killedNumbers.sort((a, b) => a - b).join(', ')}` : '已杀: 无';
            const resultText = `${selectedText} | ${killedText}`;
            
            if (navigator.share) {
                navigator.share({
                    title: '挑码助手结果',
                    text: resultText
                })
                .catch(err => {
                    console.error('分享失败: ', err);
                    alert('分享失败，请手动复制结果');
                });
            } else {
                // 如果不支持Web Share API，则复制到剪贴板
                copyResults();
            }
        }
        
        // 初始化应用
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
