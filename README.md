<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>六合彩智能选号助手 - 专业号码分析工具</title>
    <meta name="description" content="专业的六合彩选号助手，提供智能组号、号码分析、历史数据参考等功能">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Microsoft YaHei', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            line-height: 1.6;
            min-height: 100vh;
        }
        
        .container {
            max-width: 100%;
            margin: 0 auto;
            background: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }
        
        .header {
            background: linear-gradient(135deg, #e53935, #d32f2f);
            color: white;
            text-align: center;
            padding: 15px 20px;
            position: relative;
            box-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }
        
        .header h1 {
            font-size: 1.4rem;
            font-weight: 700;
            margin: 0;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }
        
        .section {
            margin-bottom: 0;
        }
        
        .section-title {
            background: linear-gradient(135deg, #f5f5f5, #e0e0e0);
            padding: 10px 15px;
            font-size: 0.9rem;
            color: #555;
            border-bottom: 1px solid #ddd;
            font-weight: 600;
        }
        
        .number-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            padding: 10px;
            background: #fafafa;
        }
        
        .number-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            aspect-ratio: 1;
            border-radius: 50%;
            font-weight: bold;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            user-select: none;
            position: relative;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        .number-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: left 0.5s;
        }
        
        .number-item:hover::before {
            left: 100%;
        }
        
        .number-item.red {
            background: radial-gradient(circle at 30% 30%, #ff6b6b, #d32f2f);
            color: white;
        }
        
        .number-item.blue {
            background: radial-gradient(circle at 30% 30%, #74b9ff, #0984e3);
            color: white;
        }
        
        .number-item.green {
            background: radial-gradient(circle at 30% 30%, #55efc4, #00b894);
            color: white;
        }
        
        .number-item.selected {
            box-shadow: 0 0 0 3px #ff9800, 0 0 20px rgba(255, 152, 0, 0.6);
            transform: scale(0.95);
            animation: pulse 2s infinite;
        }
        
        .number-item.hidden {
            opacity: 0.3;
            filter: grayscale(1);
            transform: scale(0.85);
        }
        
        .zodiac-label {
            font-size: 0.5rem;
            margin-top: 2px;
            font-weight: normal;
            opacity: 0.9;
        }
        
        .stats-section {
            background: #fff3e0;
            border: 1px solid #ffb74d;
            border-radius: 10px;
            margin: 10px;
            padding: 12px;
            display: none;
        }
        
        .stats-title {
            font-size: 0.85rem;
            font-weight: bold;
            margin-bottom: 8px;
            color: #f57c00;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .stats-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }
        
        .stat-item {
            background: linear-gradient(135deg, #ffb74d, #f57c00);
            color: white;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        
        .action-bar {
            display: flex;
            padding: 10px;
            gap: 8px;
            background: white;
            border-top: 1px solid #e0e0e0;
            flex-wrap: wrap;
        }
        
        .action-btn {
            flex: 1;
            min-width: 120px;
            padding: 12px 15px;
            text-align: center;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            border: none;
            font-size: 0.85rem;
            position: relative;
            overflow: hidden;
        }
        
        .action-btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: rgba(255,255,255,0.3);
            border-radius: 50%;
            transition: all 0.3s;
            transform: translate(-50%, -50%);
        }
        
        .action-btn:active::before {
            width: 100px;
            height: 100px;
        }
        
        .clear-btn {
            background: linear-gradient(135deg, #78909c, #546e7a);
            color: white;
        }
        
        .generate-btn {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
            color: white;
        }
        
        .analysis-btn {
            background: linear-gradient(135deg, #2196f3, #1976d2);
            color: white;
        }
        
        .result-section {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: white;
        }
        
        .result-header {
            background: linear-gradient(135deg, #74b9ff, #0984e3);
            color: white;
            padding: 12px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .result-content {
            flex: 1;
            padding: 15px;
            background: #f8f9fa;
            overflow-y: auto;
            min-height: 200px;
        }
        
        .result-group {
            background: white;
            margin-bottom: 12px;
            padding: 12px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            border-left: 4px solid #74b9ff;
            transition: transform 0.3s;
        }
        
        .result-group:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }
        
        .result-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            align-items: center;
        }
        
        .result-number {
            background: linear-gradient(135deg, #74b9ff, #0984e3);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        
        .result-number.special {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
        }
        
        .empty-result {
            text-align: center;
            color: #999;
            padding: 40px 20px;
            font-size: 0.9rem;
        }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 3px #ff9800, 0 0 20px rgba(255, 152, 0, 0.6); }
            50% { box-shadow: 0 0 0 5px #ff9800, 0 0 30px rgba(255, 152, 0, 0.8); }
            100% { box-shadow: 0 0 0 3px #ff9800, 0 0 20px rgba(255, 152, 0, 0.6); }
        }
        
        @media (max-width: 480px) {
            .number-grid {
                grid-template-columns: repeat(7, 1fr);
                gap: 4px;
                padding: 8px;
            }
            
            .number-item {
                font-size: 0.7rem;
            }
            
            .action-bar {
                flex-direction: column;
            }
            
            .action-btn {
                min-width: auto;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎯 六合彩智能选号助手</h1>
        </div>
        
        <div class="section">
            <div class="section-title">🎲 选择号码 (1-49)</div>
            <div class="number-grid" id="numberGrid"></div>
        </div>
        
        <div class="stats-section" id="statsSection">
            <div class="stats-title">
                <span>📊 当前统计</span>
                <span id="selectedCount">已选: 0 个</span>
            </div>
            <div class="stats-grid" id="statsGrid"></div>
        </div>
        
        <div class="action-bar">
            <button class="action-btn clear-btn" id="clearBtn">🗑️ 清空</button>
            <button class="action-btn analysis-btn" id="analyzeBtn">📈 分析</button>
            <button class="action-btn generate-btn" id="generate5">🎲 生成5组</button>
        </div>
        
        <div class="action-bar">
            <button class="action-btn generate-btn" id="generate10">🎯 生成10组</button>
            <button class="action-btn generate-btn" id="generate20">🔥 生成20组</button>
            <button class="action-btn copy-btn" id="copyResultBtn">📋 复制结果</button>
        </div>
        
        <div class="result-section">
            <div class="result-header">
                <span>📋 组号结果</span>
                <span id="resultCount">共 0 组</span>
            </div>
            <div class="result-content" id="resultContent">
                <div class="empty-result">
                    <p>🎯 请选择号码，然后点击生成按钮</p>
                    <p style="font-size: 0.8rem; margin-top: 10px; color: #ccc;">建议选择6-15个号码获得最佳效果</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 初始化变量
            const elements = {
                numberGrid: document.getElementById('numberGrid'),
                resultContent: document.getElementById('resultContent'),
                selectedCount: document.getElementById('selectedCount'),
                resultCount: document.getElementById('resultCount'),
                statsSection: document.getElementById('statsSection'),
                statsGrid: document.getElementById('statsGrid'),
                clearBtn: document.getElementById('clearBtn'),
                analyzeBtn: document.getElementById('analyzeBtn'),
                copyResultBtn: document.getElementById('copyResultBtn'),
                generate5: document.getElementById('generate5'),
                generate10: document.getElementById('generate10'),
                generate20: document.getElementById('generate20')
            };
            
            let selectedNumbers = new Set();
            let killNumbers = new Set();
            let hiddenNumbers = new Set();
            let currentResults = [];
            
            // 配置数据
            const config = {
                colors: {
                    red: [1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46],
                    blue: [3, 4, 9, 10, 14, 15, 20, 25, 26, 31, 36, 37, 41, 42, 47, 48],
                    green: [5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49]
                },
                zodiacs: {
                    '鼠': [6, 18, 30, 42], '牛': [5, 17, 29, 41], '虎': [4, 16, 28, 40],
                    '兔': [3, 15, 27, 39], '龙': [2, 14, 26, 38], '蛇': [1, 13, 25, 37, 49],
                    '马': [12, 24, 36, 48], '羊': [11, 23, 35, 47], '猴': [10, 22, 34, 46],
                    '鸡': [9, 21, 33, 45], '狗': [8, 20, 32, 44], '猪': [7, 19, 31, 43]
                }
            };
            
            // 初始化号码网格
            function initializeNumberGrid() {
                elements.numberGrid.innerHTML = '';
                
                for (let i = 1; i <= 49; i++) {
                    const numberItem = createNumberItem(i);
                    elements.numberGrid.appendChild(numberItem);
                }
            }
            
            // 创建号码元素
            function createNumberItem(number) {
                const item = document.createElement('div');
                item.className = 'number-item';
                
                // 设置颜色
                if (config.colors.red.includes(number)) item.classList.add('red');
                else if (config.colors.blue.includes(number)) item.classList.add('blue');
                else if (config.colors.green.includes(number)) item.classList.add('green');
                
                // 设置生肖
                const zodiac = Object.keys(config.zodiacs).find(z => 
                    config.zodiacs[z].includes(number)
                ) || '';
                
                item.innerHTML = `
                    <span>${number.toString().padStart(2, '0')}</span>
                    <span class="zodiac-label">${zodiac}</span>
                `;
                
                item.dataset.number = number;
                setupNumberInteractions(item);
                
                return item;
            }
            
            // 设置号码交互
            function setupNumberInteractions(item) {
                let clickTimer = null;
                
                item.addEventListener('click', function() {
                    if (clickTimer) {
                        clearTimeout(clickTimer);
                        handleDoubleClick(this);
                        clickTimer = null;
                    } else {
                        clickTimer = setTimeout(() => {
                            handleSingleClick(this);
                            clickTimer = null;
                        }, 300);
                    }
                });
                
                // 触摸事件
                let touchTimer;
                item.addEventListener('touchstart', function(e) {
                    touchTimer = setTimeout(() => {
                        handleLongPress(this);
                        e.preventDefault();
                    }, 500);
                });
                
                item.addEventListener('touchend', () => clearTimeout(touchTimer));
                item.addEventListener('touchmove', () => clearTimeout(touchTimer));
            }
            
            // 处理单击
            function handleSingleClick(element) {
                const num = parseInt(element.dataset.number);
                if (killNumbers.has(num) || hiddenNumbers.has(num)) return;
                
                if (selectedNumbers.has(num)) {
                    selectedNumbers.delete(num);
                    element.classList.remove('selected');
                } else {
                    selectedNumbers.add(num);
                    element.classList.add('selected');
                }
                updateDisplay();
            }
            
            // 处理双击
            function handleDoubleClick(element) {
                const num = parseInt(element.dataset.number);
                if (!hiddenNumbers.has(num)) {
                    hiddenNumbers.add(num);
                    selectedNumbers.delete(num);
                    killNumbers.delete(num);
                    element.classList.add('hidden');
                    element.classList.remove('selected');
                } else {
                    hiddenNumbers.delete(num);
                    element.classList.remove('hidden');
                }
                updateDisplay();
            }
            
            // 处理长按
            function handleLongPress(element) {
                const num = parseInt(element.dataset.number);
                if (!killNumbers.has(num)) {
                    killNumbers.add(num);
                    selectedNumbers.delete(num);
                    hiddenNumbers.delete(num);
                    element.classList.add('hidden');
                    element.classList.remove('selected');
                } else {
                    killNumbers.delete(num);
                    element.classList.remove('hidden');
                }
                updateDisplay();
            }
            
            // 更新显示
            function updateDisplay() {
                updateStats();
                updateResultsCount();
            }
            
            // 更新统计信息
            function updateStats() {
                elements.selectedCount.textContent = `已选: ${selectedNumbers.size} 个`;
                elements.statsSection.style.display = selectedNumbers.size > 0 ? 'block' : 'none';
                
                if (selectedNumbers.size === 0) return;
                
                const stats = {
                    '红波': 0, '蓝波': 0, '绿波': 0,
                    '总号码': selectedNumbers.size
                };
                
                selectedNumbers.forEach(num => {
                    if (config.colors.red.includes(num)) stats['红波']++;
                    else if (config.colors.blue.includes(num)) stats['蓝波']++;
                    else if (config.colors.green.includes(num)) stats['绿波']++;
                });
                
                elements.statsGrid.innerHTML = Object.entries(stats)
                    .map(([key, value]) => 
                        `<div class="stat-item">${key}: ${value}</div>`
                    ).join('');
            }
            
            // 更新结果计数
            function updateResultsCount() {
                elements.resultCount.textContent = `共 ${currentResults.length} 组`;
            }
            
            // 生成组号 - 增强随机性版本
            function generateGroups(count) {
                if (selectedNumbers.size < 3) {
                    showMessage('请至少选择3个号码', 'error');
                    return;
                }
                
                const availableNumbers = Array.from(selectedNumbers)
                    .filter(num => !killNumbers.has(num) && !hiddenNumbers.has(num));
                
                if (availableNumbers.length < 3) {
                    showMessage('可用号码不足3个', 'error');
                    return;
                }
                
                const results = [];
                const usedCombinations = new Set();
                const numberUsage = new Map();
                
                // 初始化使用次数
                availableNumbers.forEach(num => numberUsage.set(num, 0));
                
                for (let i = 0; i < count; i++) {
                    let group;
                    let attempts = 0;
                    const maxAttempts = 100;
                    
                    do {
                        group = generateRandomGroup(availableNumbers, numberUsage, i);
                        attempts++;
                    } while (attempts < maxAttempts && 
                           (usedCombinations.has(group.join(',')) || 
                            new Set(group).size !== 3));
                    
                    if (group && new Set(group).size === 3) {
                        // 更新使用次数
                        group.forEach(num => {
                            numberUsage.set(num, (numberUsage.get(num) || 0) + 1);
                        });
                        
                        usedCombinations.add(group.join(','));
                        results.push({
                            numbers: group.sort((a, b) => a - b),
                            index: i + 1
                        });
                    }
                }
                
                currentResults = results;
                displayResults(results);
                showMessage(`成功生成 ${results.length} 组号码`, 'success');
            }
            
            // 生成随机组号（多种算法）
            function generateRandomGroup(numbers, numberUsage, groupIndex) {
                const algorithms = [
                    // 算法1: 加权随机（基于使用次数）
                    (arr, usage) => {
                        const weighted = arr.map(num => {
                            const usageCount = usage.get(num) || 0;
                            const weight = 1 / (usageCount + 1);
                            return { num, weight: weight * (1 + Math.random() * 0.3) };
                        });
                        
                        const totalWeight = weighted.reduce((sum, item) => sum + item.weight, 0);
                        const selected = [];
                        
                        while (selected.length < 3 && weighted.length > 0) {
                            let random = Math.random() * totalWeight;
                            for (let i = 0; i < weighted.length; i++) {
                                random -= weighted[i].weight;
                                if (random <= 0) {
                                    selected.push(weighted[i].num);
                                    totalWeight -= weighted[i].weight;
                                    weighted.splice(i, 1);
                                    break;
                                }
                            }
                        }
                        
                        return selected.slice(0, 3);
                    },
                    
                    // 算法2: Fisher-Yates 洗牌
                    (arr) => {
                        const shuffled = [...arr];
                        for (let i = shuffled.length - 1; i > 0; i--) {
                            const j = Math.floor(Math.random() * (i + 1));
                            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
                        }
                        return shuffled.slice(0, 3);
                    },
                    
                    // 算法3: 基于时间的随机
                    (arr) => {
                        const timestamp = Date.now();
                        return [...arr]
                            .map(num => [(num * timestamp + groupIndex) % 1000, num])
                            .sort((a, b) => a[0] - b[0])
                            .slice(0, 3)
                            .map(item => item[1]);
                    }
                ];
                
                // 轮换使用不同算法
                const algorithm = algorithms[groupIndex % algorithms.length];
                return algorithm(numbers, numberUsage);
            }
            
            // 显示结果
            function displayResults(results) {
                if (results.length === 0) {
                    elements.resultContent.innerHTML = `
                        <div class="empty-result">
                            <p>❌ 无法生成有效的组号</p>
                            <p style="font-size: 0.8rem; margin-top: 10px; color: #ccc;">请尝试选择更多号码或调整杀号设置</p>
                        </div>
                    `;
                    return;
                }
                
                elements.resultContent.innerHTML = results.map(result => `
                    <div class="result-group">
                        <div class="result-numbers">
                            ${result.numbers.map(num => 
                                `<div class="result-number">${num.toString().padStart(2, '0')}</div>`
                            ).join('')}
                            <div class="result-number special">三中三</div>
                        </div>
                    </div>
                `).join('');
            }
            
            // 显示消息
            function showMessage(message, type = 'info') {
                const colors = {
                    error: '#f44336',
                    success: '#4caf50',
                    info: '#2196f3'
                };
                
                // 简单的消息提示
                alert(message);
            }
            
            // 复制结果
            function copyResults() {
                if (currentResults.length === 0) {
                    showMessage('没有可复制的内容', 'error');
                    return;
                }
                
                const text = currentResults.map(result => 
                    result.numbers.map(num => num.toString().padStart(2, '0')).join(',') + ',三中三'
                ).join('\n');
                
                navigator.clipboard.writeText(text)
                    .then(() => showMessage('✅ 组号已复制到剪贴板', 'success'))
                    .catch(err => {
                        console.error('复制失败:', err);
                        showMessage('❌ 复制失败，请手动选择复制', 'error');
                    });
            }
            
            // 清空所有
            function clearAll() {
                selectedNumbers.clear();
                killNumbers.clear();
                hiddenNumbers.clear();
                currentResults = [];
                
                document.querySelectorAll('.number-item').forEach(item => {
                    item.classList.remove('selected', 'hidden');
                });
                
                updateDisplay();
                elements.resultContent.innerHTML = `
                    <div class="empty-result">
                        <p>🎯 请选择号码，然后点击生成按钮</p>
                        <p style="font-size: 0.8rem; margin-top: 10px; color: #ccc;">建议选择6-15个号码获得最佳效果</p>
                    </div>
                `;
                
                showMessage('已清空所有选择', 'info');
            }
            
            // 分析功能
            function analyzeNumbers() {
                if (selectedNumbers.size === 0) {
                    showMessage('请先选择要分析的号码', 'error');
                    return;
                }
                
                const analysis = {
                    total: selectedNumbers.size,
                    red: 0, blue: 0, green: 0,
                    zodiacs: {}
                };
                
                selectedNumbers.forEach(num => {
                    if (config.colors.red.includes(num)) analysis.red++;
                    else if (config.colors.blue.includes(num)) analysis.blue++;
                    else if (config.colors.green.includes(num)) analysis.green++;
                    
                    const zodiac = Object.keys(config.zodiacs).find(z => 
                        config.zodiacs[z].includes(num)
                    );
                    if (zodiac) {
                        analysis.zodiacs[zodiac] = (analysis.zodiacs[zodiac] || 0) + 1;
                    }
                });
                
                const message = `
分析结果：
📊 总号码: ${analysis.total}个
🔴 红波: ${analysis.red}个
🔵 蓝波: ${analysis.blue}个  
🟢 绿波: ${analysis.green}个
🐭 生肖分布: ${Object.entries(analysis.zodiacs)
    .map(([z, c]) => `${z}${c}个`).join(' ')}
                `.trim();
                
                showMessage(message, 'info');
            }
            
            // 绑定事件
            function bindEvents() {
                elements.clearBtn.addEventListener('click', clearAll);
                elements.analyzeBtn.addEventListener('click', analyzeNumbers);
                elements.copyResultBtn.addEventListener('click', copyResults);
                elements.generate5.addEventListener('click', () => generateGroups(5));
                elements.generate10.addEventListener('click', () => generateGroups(10));
                elements.generate20.addEventListener('click', () => generateGroups(20));
                
                // 键盘快捷键
                document.addEventListener('keydown', (e) => {
                    if (e.ctrlKey || e.metaKey) {
                        switch(e.key) {
                            case '1': e.preventDefault(); generateGroups(5); break;
                            case '2': e.preventDefault(); generateGroups(10); break;
                            case '3': e.preventDefault(); generateGroups(20); break;
                            case 'c': e.preventDefault(); copyResults(); break;
                            case 'a': e.preventDefault(); analyzeNumbers(); break;
                            case 'Delete': e.preventDefault(); clearAll(); break;
                        }
                    }
                });
            }
            
            // 初始化
            function init() {
                initializeNumberGrid();
                bindEvents();
                updateDisplay();
                
                // 显示欢迎信息
                setTimeout(() => {
                    showMessage('🎉 欢迎使用六合彩智能选号助手！\n\n💡 使用提示：\n• 单击：选择/取消号码\n• 双击：隐藏/显示号码\n• 长按：添加到杀号区\n• 快捷键：Ctrl+1/2/3 快速生成', 'info');
                }, 1000);
            }
            
            // 启动应用
            init();
        });
    </script>
</body>
</html>
