<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>挑码助手专业版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Microsoft YaHei", -apple-system, BlinkMacSystemFont, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4efe9 100%);
            color: #333;
            line-height: 1.5;
            min-height: 100vh;
            padding: 8px;
            touch-action: manipulation;
            -webkit-overflow-scrolling: touch;
        }
        
        .container {
            width: 100%;
            max-width: 100%;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #ff6b6b 0%, #ffa726 100%);
            color: white;
            text-align: center;
            padding: 12px 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        h1 {
            font-size: 1.5rem;
            margin-bottom: 6px;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
            font-weight: 700;
        }
        
        .subtitle {
            font-size: 0.85rem;
            opacity: 0.9;
            font-weight: 500;
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
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
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
            font-weight: 600;
        }
        
        .panel-title i {
            margin-right: 8px;
            color: #ff6b6b;
        }
        
        .numbers-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 6px;
            margin-bottom: 12px;
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
            font-size: 1.2rem;
            color: white;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: relative;
            user-select: none;
            touch-action: manipulation;
        }
        
        .number-ball.red {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
        }
        
        .number-ball.green {
            background: linear-gradient(135deg, #27ae60, #229954);
        }
        
        .number-ball.blue {
            background: linear-gradient(135deg, #2980b9, #1a5276);
        }
        
        .number-ball.selected {
            opacity: 0.7;
            transform: scale(0.9);
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        }
        
        .number-ball.killed {
            opacity: 0.4;
            transform: scale(0.85);
            background: #7f8c8d !important;
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
            font-size: 11px;
            margin-top: 3px;
            opacity: 0.9;
            font-weight: bold;
        }
        
        .control-section {
            margin: 10px 0;
        }
        
        .control-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        
        .counter {
            font-size: 0.9rem;
            color: #2c3e50;
            font-weight: 500;
        }
        
        .selected-count {
            color: #27ae60;
        }
        
        .killed-count {
            color: #e74c3c;
        }
        
        .control-buttons {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
        }
        
        .control-btn {
            padding: 10px 6px;
            border: none;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            min-width: 0;
            touch-action: manipulation;
        }
        
        .clear-btn {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
        }
        
        .group-btn {
            background: linear-gradient(135deg, #9b59b6, #8e44ad);
            color: white;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
        }
        
        .copy-group-btn {
            background: linear-gradient(135deg, #e67e22, #d35400);
            color: white;
        }
        
        .select-all-btn {
            background: linear-gradient(135deg, #27ae60, #229954);
            color: white;
        }
        
        .random-btn {
            background: linear-gradient(135deg, #8e44ad, #9b59b6);
            color: white;
        }
        
        .control-btn:active {
            transform: scale(0.97);
        }
        
        .control-btn.disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .animation-section {
            background: white;
            border-radius: 8px;
            padding: 10px;
            margin: 10px 0;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
            border: 2px solid #9b59b6;
        }
        
        .animation-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 8px;
            text-align: center;
            padding-bottom: 6px;
            border-bottom: 2px solid #9b59b6;
            color: #9b59b6;
        }
        
        .animation-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
        }
        
        .timer {
            font-size: 0.9rem;
            font-weight: bold;
            color: #2c3e50;
            text-align: center;
        }
        
        .timer-value {
            font-size: 1.1rem;
            color: #e74c3c;
        }
        
        .progress-bar {
            width: 100%;
            height: 8px;
            background: #f0f0f0;
            border-radius: 4px;
            overflow: hidden;
            margin: 10px 0;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #9b59b6, #8e44ad);
            width: 0%;
            transition: width 0.1s linear;
        }
        
        .animation-groups {
            display: flex;
            flex-direction: column;
            gap: 12px;
            width: 100%;
        }
        
        .animation-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .group-label-small {
            font-size: 0.9rem;
            font-weight: bold;
            color: #2c3e50;
            text-align: center;
        }
        
        .animation-numbers {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 8px;
        }
        
        .animation-number {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            font-weight: bold;
            color: white;
            background: linear-gradient(135deg, #9b59b6, #8e44ad);
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
            transition: all 0.1s ease;
        }
        
        .animation-number.rolling {
            animation: pulse 0.3s infinite alternate;
        }
        
        @keyframes pulse {
            from { transform: scale(1); }
            to { transform: scale(1.1); }
        }
        
        .comma {
            font-size: 1.2rem;
            font-weight: bold;
            color: #9b59b6;
        }
        
        .group-result-section {
            background: white;
            border-radius: 8px;
            padding: 10px;
            margin: 10px 0;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
            border: 2px solid #9b59b6;
        }
        
        .group-result-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 8px;
            text-align: center;
            padding-bottom: 6px;
            border-bottom: 2px solid #9b59b6;
            color: #9b59b6;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .clear-groups-btn {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
            border: none;
            border-radius: 4px;
            padding: 4px 8px;
            font-size: 0.8rem;
            cursor: pointer;
        }
        
        .clear-groups-btn:active {
            transform: scale(0.95);
        }
        
        .group-result {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .group-row {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px 0;
            border-bottom: 1px solid #eee;
        }
        
        .group-row:last-child {
            border-bottom: none;
        }
        
        .group-label {
            font-weight: bold;
            color: #2c3e50;
            min-width: 50px;
            font-size: 0.9rem;
        }
        
        .group-numbers {
            flex: 1;
            padding: 8px;
            background: #f8f9fa;
            border-radius: 6px;
            font-family: monospace;
            font-size: 0.9rem;
            color: #2c3e50;
        }
        
        .lists-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 12px;
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
            color: #27ae60;
            border-bottom-color: #27ae60;
        }
        
        .killed-list .list-title {
            color: #e74c3c;
            border-bottom-color: #e74c3c;
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
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            user-select: none;
            touch-action: manipulation;
        }
        
        .list-number:active {
            transform: scale(0.95);
        }
        
        .empty-message {
            color: #aaa;
            font-style: italic;
            text-align: center;
            width: 100%;
            margin-top: 15px;
            font-size: 0.85rem;
        }
        
        .category-section {
            margin-bottom: 12px;
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
            margin-right: 8px;
            color: #2980b9;
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
            border-radius: 6px;
            font-size: 0.85rem;
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
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
        }
        
        .category-btn.green {
            background: linear-gradient(135deg, #27ae60, #229954);
            color: white;
        }
        
        .category-btn.blue {
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
        }
        
        .category-btn.active {
            background: linear-gradient(135deg, #f39c12, #e67e22);
            color: white;
        }
        
        .category-btn.killed {
            background: linear-gradient(135deg, #7f8c8d, #616a6b);
            color: white;
        }
        
        .zodiac-chart {
            margin-top: 12px;
            padding: 12px;
            background: #f9f9f9;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
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
            border-radius: 8px;
            padding: 10px;
            text-align: center;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
        }
        
        .zodiac-name {
            font-size: 1rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 6px;
        }
        
        .zodiac-conflict {
            font-size: 0.85rem;
            color: #e74c3c;
            margin-bottom: 8px;
        }
        
        .zodiac-numbers {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 4px;
        }
        
        .zodiac-number {
            width: 28px;
            height: 28px;
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
            padding: 12px;
            background: #2c3e50;
            color: #ecf0f1;
            margin-top: 12px;
            font-size: 0.8rem;
        }
        
        /* iPhone 13 Pro Max 特殊优化 */
        @media screen and (max-width: 430px) and (min-height: 800px) {
            body {
                padding: 6px;
            }
            
            h1 {
                font-size: 1.4rem;
            }
            
            .subtitle {
                font-size: 0.8rem;
            }
            
            .number-ball {
                font-size: 1.1rem;
            }
            
            .zodiac-label {
                font-size: 10px;
            }
            
            .control-buttons {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .control-btn {
                padding: 8px 4px;
                font-size: 0.8rem;
            }
        }
        
        /* 针对截图中的布局进行调整 */
        @media screen and (min-width: 768px) {
            .container {
                max-width: 768px;
                margin: 0 auto;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>挑码助手专业版</h1>
            <p class="subtitle">智能选号，组号生成，批量操作</p>
        </header>
        
        <div class="main-content">
            <div class="panel">
                <div class="panel-title">
                    <span><i>●</i> 数字选号区 (1-49)</span>
                </div>
                
                <div class="numbers-grid" id="numbersGrid">
                    <!-- 数字1-49将通过JavaScript动态生成 -->
                </div>
                
                <div class="control-section">
                    <div class="control-header">
                        <div class="counter">已选: <span id="selectedCount" class="selected-count">0</span> | 已杀: <span id="killedCount" class="killed-count">0</span></div>
                        <div class="counter">剩余: <span id="remainingCount" class="">49</span></div>
                    </div>
                    <div class="control-buttons">
                        <button class="control-btn clear-btn" id="clearBtn">清空选择</button>
                        <button class="control-btn group-btn" id="groupBtn">生成五组</button>
                        <button class="control-btn copy-btn" id="copyBtn">复制结果</button>
                        <button class="control-btn copy-group-btn" id="copyGroupBtn">复制组号</button>
                        <button class="control-btn select-all-btn" id="selectAllBtn">全部选中</button>
                        <button class="control-btn random-btn" id="randomBtn">随机分组</button>
                    </div>
                </div>
                
                <div class="animation-section" id="animationSection" style="display: none;">
                    <div class="animation-title">生成五组号码动画</div>
                    <div class="animation-container">
                        <div class="timer">剩余时间: <span id="timerValue" class="timer-value">8.88</span>秒</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="progressFill"></div>
                        </div>
                        <div class="animation-groups" id="animationGroups">
                            <!-- 五组动画将通过JavaScript动态生成 -->
                        </div>
                    </div>
                </div>
                
                <div class="group-result-section" id="groupResultSection" style="display: none;">
                    <div class="group-result-title">
                        <span>生成的组号 (<span id="groupCount">0</span>)</span>
                        <button class="clear-groups-btn" id="clearGroupsBtn">清空组号</button>
                    </div>
                    <div class="group-result" id="groupResult">
                        <!-- 组号行将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="lists-container">
                    <div class="list-box selected-list">
                        <div class="list-title">已选号码 (<span id="selectedListCount">0</span>)</div>
                        <div class="list-items" id="selectedList">
                            <div class="empty-message">暂无已选号码</div>
                        </div>
                    </div>
                    
                    <div class="list-box killed-list">
                        <div class="list-title">已杀号码 (<span id="killedListCount">0</span>)</div>
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
            <p>挑码助手专业版 &copy; 2023 - 双击数字标记为"已杀"，单击切换"已选"状态</p>
        </footer>
    </div>

    <script>
        // 数字数据 - 根据2026年丙午马年调整，所有生肖号码+1
        const numbersData = Array.from({length: 49}, (_, i) => {
            const num = i + 1;
            let color = 'blue';
            if ([1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46].includes(num)) color = 'red';
            if ([5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49].includes(num)) color = 'green';
            
            // 2026年丙午马年：所有生肖号码+1
            const zodiacs = {
                '马': [1, 13, 25, 37, 49],  // 原本蛇(1,13,25,37,49) -> 马
                '蛇': [2, 14, 26, 38],       // 原本龙(2,14,26,38) -> 蛇
                '龙': [3, 15, 27, 39],       // 原本兔(3,15,27,39) -> 龙
                '兔': [4, 16, 28, 40],       // 原本虎(4,16,28,40) -> 兔
                '虎': [5, 17, 29, 41],       // 原本牛(5,17,29,41) -> 虎
                '牛': [6, 18, 30, 42],       // 原本鼠(6,18,30,42) -> 牛
                '鼠': [7, 19, 31, 43],       // 原本猪(7,19,31,43) -> 鼠
                '猪': [8, 20, 32, 44],       // 原本狗(8,20,32,44) -> 猪
                '狗': [9, 21, 33, 45],       // 原本鸡(9,21,33,45) -> 狗
                '鸡': [10, 22, 34, 46],      // 原本猴(10,22,34,46) -> 鸡
                '猴': [11, 23, 35, 47],      // 原本羊(11,23,35,47) -> 猴
                '羊': [12, 24, 36, 48]       // 原本马(12,24,36,48) -> 羊
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

        // 生肖冲突数据 - 根据2026年调整
        const zodiacConflicts = {
            '马': '冲鼠',  // 马年冲鼠
            '蛇': '冲猪',
            '龙': '冲狗',
            '兔': '冲鸡',
            '虎': '冲猴',
            '牛': '冲羊',
            '鼠': '冲马',
            '猪': '冲蛇',
            '狗': '冲龙',
            '鸡': '冲兔',
            '猴': '冲虎',
            '羊': '冲牛'
        };

        // 状态管理
        let selectedNumbers = [];
        let killedNumbers = [];
        let clickTimers = {};
        let allGroups = []; // 存储所有生成的组号
        let isAnimating = false; // 是否正在动画中
        let animationInterval = null; // 动画定时器
        let animationTimeout = null; // 动画结束定时器
        let animationStartTime = null; // 动画开始时间
        let animationDuration = 8880; // 动画持续时间8.88秒（8880毫秒）
        let animationGroupsData = []; // 存储动画中的五组数据
        
        // 初始化函数
        function init() {
            loadData();
            renderNumberGrid();
            renderCategoryButtons();
            renderZodiacChart();
            setupEventListeners();
            updateCounters();
            console.log('挑码助手专业版（2026年马年版本）初始化完成');
        }
        
        // 加载本地存储的数据
        function loadData() {
            const savedSelected = localStorage.getItem('selectedNumbers');
            const savedKilled = localStorage.getItem('killedNumbers');
            const savedGroups = localStorage.getItem('generatedGroups');
            
            if (savedSelected) {
                selectedNumbers = JSON.parse(savedSelected);
            }
            
            if (savedKilled) {
                killedNumbers = JSON.parse(savedKilled);
            }
            
            if (savedGroups) {
                allGroups = JSON.parse(savedGroups);
                updateGroupDisplay();
            }
        }
        
        // 保存数据到本地存储
        function saveData() {
            localStorage.setItem('selectedNumbers', JSON.stringify(selectedNumbers));
            localStorage.setItem('killedNumbers', JSON.stringify(killedNumbers));
            localStorage.setItem('generatedGroups', JSON.stringify(allGroups));
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
                            numberElement.style.background = 'linear-gradient(135deg, #e74c3c, #c0392b)';
                        } else if (numberData.color === 'green') {
                            numberElement.style.background = 'linear-gradient(135deg, #27ae60, #229954)';
                        } else {
                            numberElement.style.background = 'linear-gradient(135deg, #3498db, #2980b9)';
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
            document.getElementById('groupBtn').addEventListener('click', generateFiveGroups);
            document.getElementById('copyBtn').addEventListener('click', copyResults);
            document.getElementById('copyGroupBtn').addEventListener('click', copyGroups);
            document.getElementById('selectAllBtn').addEventListener('click', selectAll);
            document.getElementById('clearGroupsBtn').addEventListener('click', clearGroups);
            document.getElementById('randomBtn').addEventListener('click', randomizeGroupSelected);
            
            // 已选号码列表点击事件
            document.getElementById('selectedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (!listNumber) return;
                
                const number = parseInt(listNumber.dataset.number);
                const timerId = `list-selected-${number}`;
                
                if (clickTimers[timerId]) {
                    clearTimeout(clickTimers[timerId]);
                    selectedNumbers = selectedNumbers.filter(n => n !== number);
                    updateNumberGrid();
                    updateLists();
                    updateCounters();
                    saveData();
                    delete clickTimers[timerId];
                } else {
                    clickTimers[timerId] = setTimeout(() => {
                        delete clickTimers[timerId];
                    }, 300);
                }
            });
            
            // 已杀号码列表点击事件
            document.getElementById('killedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (!listNumber) return;
                
                const number = parseInt(listNumber.dataset.number);
                const timerId = `list-killed-${number}`;
                
                if (clickTimers[timerId]) {
                    clearTimeout(clickTimers[timerId]);
                    killedNumbers = killedNumbers.filter(n => n !== number);
                    updateNumberGrid();
                    updateLists();
                    updateCounters();
                    saveData();
                    delete clickTimers[timerId];
                } else {
                    clickTimers[timerId] = setTimeout(() => {
                        delete clickTimers[timerId];
                    }, 300);
                }
            });
        }
        
        // 随机分组功能
        function randomizeGroupSelected() {
            if (selectedNumbers.length === 0) {
                alert('请先选择号码再进行随机分组');
                return;
            }
            
            if (selectedNumbers.length < 25) {
                alert('至少需要25个已选号码才能分成五组（每组5个号码）');
                return;
            }
            
            const shuffled = [...selectedNumbers];
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            
            const fiveGroups = [];
            for (let i = 0; i < 5; i++) {
                const group = shuffled.slice(i * 5, (i + 1) * 5).sort((a, b) => a - b);
                fiveGroups.push(group);
            }
            
            fiveGroups.forEach(group => {
                allGroups.push(group);
            });
            
            updateGroupDisplay();
            saveData();
            
            let resultText = '随机分组完成（五组，每组五个号码）:\n';
            fiveGroups.forEach((group, index) => {
                resultText += `第${index+1}组: ${group.join(', ')}\n`;
            });
            alert(resultText);
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
            updateCounters();
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
            updateCounters();
            saveData();
        }
        
        // 处理分类按钮单击
        function handleCategorySingleClick(button) {
            const category = button.dataset.category;
            const value = button.dataset.value;
            
            let numbers = getNumbersByCategory(category, value);
            
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
            updateCounters();
            saveData();
        }
        
        // 处理分类按钮双击
        function handleCategoryDoubleClick(button) {
            const category = button.dataset.category;
            const value = button.dataset.value;
            
            let numbers = getNumbersByCategory(category, value);
            
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
            updateCounters();
            saveData();
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
            
            selectedList.innerHTML = '';
            if (selectedNumbers.length === 0) {
                selectedList.innerHTML = '<div class="empty-message">暂无已选号码</div>';
            } else {
                selectedNumbers.forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    
                    const numberData = numbersData.find(data => data.num === num);
                    if (numberData) {
                        if (numberData.color === 'red') {
                            listNumber.style.background = 'linear-gradient(135deg, #e74c3c, #c0392b)';
                        } else if (numberData.color === 'green') {
                            listNumber.style.background = 'linear-gradient(135deg, #27ae60, #229954)';
                        } else {
                            listNumber.style.background = 'linear-gradient(135deg, #3498db, #2980b9)';
                        }
                    }
                    
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    selectedList.appendChild(listNumber);
                });
            }
            
            killedList.innerHTML = '';
            if (killedNumbers.length === 0) {
                killedList.innerHTML = '<div class="empty-message">暂无已杀号码</div>';
            } else {
                killedNumbers.forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.style.background = '#7f8c8d';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    killedList.appendChild(listNumber);
                });
            }
            
            document.getElementById('selectedListCount').textContent = selectedNumbers.length;
            document.getElementById('killedListCount').textContent = killedNumbers.length;
        }
        
        // 更新计数器
        function updateCounters() {
            document.getElementById('selectedCount').textContent = selectedNumbers.length;
            document.getElementById('killedCount').textContent = killedNumbers.length;
            const remaining = 49 - selectedNumbers.length - killedNumbers.length;
            document.getElementById('remainingCount').textContent = remaining;
        }
        
        // 生成五组号码动画
        function generateFiveGroups() {
            if (isAnimating) {
                return;
            }
            
            if (selectedNumbers.length < 4) {
                alert('至少需要4个已选号码才能生成组号');
                return;
            }
            
            document.getElementById('groupBtn').classList.add('disabled');
            document.getElementById('groupBtn').disabled = true;
            
            document.getElementById('animationSection').style.display = 'block';
            document.getElementById('groupResultSection').style.display = 'none';
            
            const animationGroupsEl = document.getElementById('animationGroups');
            animationGroupsEl.innerHTML = '';
            
            animationGroupsData = [];
            
            for (let i = 0; i < 5; i++) {
                const groupContainer = document.createElement('div');
                groupContainer.className = 'animation-group';
                groupContainer.id = `animationGroup${i+1}`;
                
                const groupLabel = document.createElement('div');
                groupLabel.className = 'group-label-small';
                groupLabel.textContent = `第${i+1}组:`;
                
                const numbersContainer = document.createElement('div');
                numbersContainer.className = 'animation-numbers';
                numbersContainer.id = `animationNumbers${i+1}`;
                
                for (let j = 0; j < 4; j++) {
                    const numEl = document.createElement('div');
                    numEl.className = 'animation-number rolling';
                    numEl.id = `animationNum${i+1}-${j+1}`;
                    numEl.textContent = '0';
                    numbersContainer.appendChild(numEl);
                    
                    if (j < 3) {
                        const comma = document.createElement('div');
                        comma.className = 'comma';
                        comma.textContent = ',';
                        numbersContainer.appendChild(comma);
                    }
                }
                
                groupContainer.appendChild(groupLabel);
                groupContainer.appendChild(numbersContainer);
                animationGroupsEl.appendChild(groupContainer);
                
                animationGroupsData.push([0, 0, 0, 0]);
            }
            
            const timerEl = document.getElementById('timerValue');
            const progressFill = document.getElementById('progressFill');
            
            isAnimating = true;
            animationStartTime = Date.now();
            
            if (animationInterval) clearInterval(animationInterval);
            if (animationTimeout) clearTimeout(animationTimeout);
            
            function updateAnimation() {
                const currentTime = Date.now();
                const elapsedTime = currentTime - animationStartTime;
                const remainingTime = Math.max(0, animationDuration - elapsedTime);
                
                timerEl.textContent = (remainingTime / 1000).toFixed(2);
                
                const progress = Math.min(100, (elapsedTime / animationDuration) * 100);
                progressFill.style.width = progress + '%';
                
                if (remainingTime <= 0) {
                    endAnimation();
                    return;
                }
                
                for (let i = 0; i < 5; i++) {
                    const shuffled = [...selectedNumbers].sort(() => Math.random() - 0.5);
                    const selectedFour = [];
                    
                    for (let j = 0; j < shuffled.length && selectedFour.length < 4; j++) {
                        if (!selectedFour.includes(shuffled[j])) {
                            selectedFour.push(shuffled[j]);
                        }
                    }
                    
                    while (selectedFour.length < 4) {
                        const randomNum = selectedNumbers[Math.floor(Math.random() * selectedNumbers.length)];
                        if (!selectedFour.includes(randomNum)) {
                            selectedFour.push(randomNum);
                        }
                    }
                    
                    animationGroupsData[i] = selectedFour;
                    
                    for (let j = 0; j < 4; j++) {
                        const numEl = document.getElementById(`animationNum${i+1}-${j+1}`);
                        if (numEl) {
                            numEl.textContent = selectedFour[j];
                        }
                    }
                }
            }
            
            function endAnimation() {
                isAnimating = false;
                
                for (let i = 0; i < 5; i++) {
                    for (let j = 0; j < 4; j++) {
                        const numEl = document.getElementById(`animationNum${i+1}-${j+1}`);
                        if (numEl) {
                            numEl.classList.remove('rolling');
                        }
                    }
                }
                
                clearInterval(animationInterval);
                clearTimeout(animationTimeout);
                
                const finalFiveGroups = [];
                for (let i = 0; i < 5; i++) {
                    const group = animationGroupsData[i].slice().sort((a, b) => a - b);
                    finalFiveGroups.push(group);
                }
                
                finalFiveGroups.forEach(group => {
                    allGroups.push(group);
                });
                
                document.getElementById('animationSection').style.display = 'none';
                
                document.getElementById('groupBtn').classList.remove('disabled');
                document.getElementById('groupBtn').disabled = false;
                
                updateGroupDisplay();
                saveData();
                
                let resultText = '五组号码生成完成:\n';
                finalFiveGroups.forEach((group, index) => {
                    resultText += `第${index+1}组: ${group.join(', ')}\n`;
                });
                alert(resultText);
            }
            
            animationInterval = setInterval(updateAnimation, 50);
            animationTimeout = setTimeout(endAnimation, animationDuration);
            
            updateAnimation();
        }
        
        // 更新组号显示
        function updateGroupDisplay() {
            const groupResult = document.getElementById('groupResult');
            groupResult.innerHTML = '';
            
            if (allGroups.length === 0) {
                document.getElementById('groupResultSection').style.display = 'none';
                return;
            }
            
            document.getElementById('groupResultSection').style.display = 'block';
            document.getElementById('groupCount').textContent = allGroups.length;
            
            allGroups.slice().reverse().forEach((group, index) => {
                const groupRow = document.createElement('div');
                groupRow.className = 'group-row';
                
                const groupLabel = document.createElement('div');
                groupLabel.className = 'group-label';
                groupLabel.textContent = `组${allGroups.length - index}:`;
                
                const groupNumbers = document.createElement('div');
                groupNumbers.className = 'group-numbers';
                groupNumbers.textContent = group.join(', ');
                
                groupRow.appendChild(groupLabel);
                groupRow.appendChild(groupNumbers);
                groupResult.appendChild(groupRow);
            });
        }
        
        // 清空组号
        function clearGroups() {
            allGroups = [];
            updateGroupDisplay();
            saveData();
        }
        
        // 复制组号
        function copyGroups() {
            if (allGroups.length === 0) {
                alert('请先生成组号');
                return;
            }
            
            const groupsText = allGroups.map(group => group.join(', ') + ' 三中三2').join('\n');
            
            navigator.clipboard.writeText(groupsText)
                .then(() => {
                    alert('所有组号已复制到剪贴板:\n' + groupsText);
                })
                .catch(err => {
                    console.error('复制失败: ', err);
                    const textArea = document.createElement('textarea');
                    textArea.value = groupsText;
                    document.body.appendChild(textArea);
                    textArea.select();
                    try {
                        document.execCommand('copy');
                        alert('所有组号已复制到剪贴板:\n' + groupsText);
                    } catch (err) {
                        console.error('备用复制方法也失败: ', err);
                        alert('复制失败，请手动复制: \n' + groupsText);
                    }
                    document.body.removeChild(textArea);
                });
        }
        
        // 全部选中
        function selectAll() {
            const allNumbers = Array.from({length: 49}, (_, i) => i + 1);
            const availableNumbers = allNumbers.filter(num => !killedNumbers.includes(num));
            
            availableNumbers.forEach(num => {
                if (!selectedNumbers.includes(num)) {
                    selectedNumbers.push(num);
                }
            });
            
            updateNumberGrid();
            updateLists();
            updateCounters();
            saveData();
            
            alert(`已成功选中 ${availableNumbers.length} 个号码`);
        }
        
        // 清空所有
        function clearAll() {
            selectedNumbers = [];
            killedNumbers = [];
            updateNumberGrid();
            updateLists();
            updateCounters();
            saveData();
            
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
            
            const numbersToCopy = selectedNumbers.join(', ');
            
            navigator.clipboard.writeText(numbersToCopy)
                .then(() => {
                    alert('已选号码已复制到剪贴板: ' + numbersToCopy);
                })
                .catch(err => {
                    console.error('复制失败: ', err);
                    const textArea = document.createElement('textarea');
                    textArea.value = numbersToCopy;
                    document.body.appendChild(textArea);
                    textArea.select();
                    try {
                        document.execCommand('copy');
                        alert('已选号码已复制到剪贴板: ' + numbersToCopy);
                    } catch (err) {
                        console.error('备用复制方法也失败: ', err);
                        alert('复制失败，请手动复制: ' + numbersToCopy);
                    }
                    document.body.removeChild(textArea);
                });
        }
        
        // 初始化应用
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
