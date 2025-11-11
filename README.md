<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>挑码助手 - 六合彩选号工具</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: #f5f5f5;
            color: #333;
            padding: 0;
            line-height: 1.5;
        }
        
        .container {
            max-width: 100%;
            margin: 0 auto;
            background: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        /* 顶部标题栏 */
        .header {
            background: linear-gradient(to right, #e53935, #d32f2f);
            color: white;
            text-align: center;
            padding: 12px 15px;
            position: relative;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        .header h1 {
            font-size: 1.3rem;
            font-weight: 600;
        }
        
        /* 数字选择区域 */
        .section {
            margin-bottom: 10px;
        }
        
        .section-title {
            background-color: #f0f0f0;
            padding: 6px 12px;
            font-size: 0.85rem;
            color: #666;
            border-bottom: 1px solid #e0e0e0;
        }
        
        .number-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 4px;
            padding: 8px;
        }
        
        .number-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            aspect-ratio: 1;
            border-radius: 6px;
            font-weight: bold;
            font-size: 0.75rem;
            cursor: pointer;
            transition: all 0.2s;
            user-select: none;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            position: relative;
            padding: 3px;
        }
        
        .number-item.red {
            background: linear-gradient(135deg, #ff5252, #d32f2f);
            color: white;
        }
        
        .number-item.blue {
            background: linear-gradient(135deg, #448aff, #2962ff);
            color: white;
        }
        
        .number-item.green {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
            color: white;
        }
        
        .number-item.selected {
            box-shadow: 0 0 0 2px #ff9800, 0 0 10px rgba(255, 152, 0, 0.7);
            transform: scale(0.95);
        }
        
        .number-item.hidden {
            opacity: 0.3;
            filter: grayscale(0.8);
        }
        
        .zodiac-label {
            font-size: 0.5rem;
            margin-top: 1px;
            font-weight: normal;
        }
        
        /* 杀号区 */
        .kill-section {
            background: #fff8e1;
            border: 1px solid #ffd54f;
            border-radius: 6px;
            margin: 8px;
            padding: 8px;
        }
        
        .kill-title {
            font-size: 0.85rem;
            font-weight: bold;
            margin-bottom: 6px;
            color: #ff9800;
        }
        
        .kill-numbers-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
        }
        
        .kill-number {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        
        /* 已选号码区域 */
        .selected-numbers-section {
            background: #e8f5e9;
            border: 1px solid #4caf50;
            border-radius: 6px;
            margin: 8px;
            padding: 8px;
        }
        
        .selected-numbers-title {
            font-size: 0.85rem;
            font-weight: bold;
            margin-bottom: 6px;
            color: #2e7d32;
        }
        
        .selected-numbers-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
        }
        
        .selected-number {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        
        /* 隐藏号码区域 */
        .hidden-numbers-section {
            background: #f5f5f5;
            border: 1px solid #bdbdbd;
            border-radius: 6px;
            margin: 8px;
            padding: 8px;
        }
        
        .hidden-numbers-title {
            font-size: 0.85rem;
            font-weight: bold;
            margin-bottom: 6px;
            color: #616161;
        }
        
        .hidden-numbers-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
        }
        
        .hidden-number {
            background: linear-gradient(135deg, #9e9e9e, #757575);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        
        /* 结果区域 */
        .result-section {
            flex: 1;
            display: flex;
            flex-direction: column;
            border-top: 1px solid #e0e0e0;
        }
        
        .result-title {
            background-color: #f0f0f0;
            padding: 6px 12px;
            font-size: 0.85rem;
            color: #666;
            display: flex;
            justify-content: space-between;
        }
        
        .result-content {
            flex: 1;
            padding: 12px;
            background: #f9f9f9;
            overflow-y: auto;
            min-height: 120px;
        }
        
        .result-group {
            margin-bottom: 10px;
            padding: 8px;
            background: white;
            border-radius: 6px;
            box-shadow: 0 1px 2px rgba(0,0,0,0.1);
        }
        
        .result-group h3 {
            font-size: 0.85rem;
            margin-bottom: 6px;
            color: #d32f2f;
        }
        
        .result-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            align-items: center;
        }
        
        .result-number {
            background: #f0f0f0;
            padding: 3px 8px;
            border-radius: 3px;
            font-size: 0.75rem;
        }
        
        /* 底部操作栏 */
        .action-bar {
            display: flex;
            padding: 8px;
            gap: 8px;
            background: white;
            border-top: 1px solid #e0e0e0;
        }
        
        .action-btn {
            flex: 1;
            padding: 10px;
            text-align: center;
            border-radius: 5px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.85rem;
        }
        
        .clear-btn {
            background: #f0f0f0;
            color: #666;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
            color: white;
        }
        
        .copy-selected-btn {
            background: linear-gradient(135deg, #2196f3, #1976d2);
            color: white;
        }
        
        .generate-btn {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
        }
        
        /* 响应式调整 */
        @media (max-width: 480px) {
            .number-grid {
                grid-template-columns: repeat(7, 1fr);
                gap: 3px;
                padding: 6px;
            }
            
            .number-item {
                font-size: 0.7rem;
                border-radius: 4px;
            }
            
            .zodiac-label {
                font-size: 0.45rem;
            }
            
            .action-bar {
                flex-wrap: wrap;
            }
            
            .action-btn {
                flex: 1 0 45%;
                margin: 2px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 顶部标题栏 -->
        <div class="header">
            <h1>挑码助手</h1>
        </div>
        
        <!-- 数字选择区域 -->
        <div class="section">
            <div class="section-title">选择号码 (1-49)</div>
            <div class="number-grid" id="numberGrid"></div>
        </div>
        
        <!-- 杀号区 -->
        <div class="kill-section" id="killSection" style="display: none;">
            <div class="kill-title">杀号区 (不需要的号码)</div>
            <div class="kill-numbers-grid" id="killNumbersGrid"></div>
        </div>
        
        <!-- 已选号码区域 -->
        <div class="selected-numbers-section" id="selectedNumbersSection" style="display: none;">
            <div class="selected-numbers-title">已选号码</div>
            <div class="selected-numbers-grid" id="selectedNumbersGrid"></div>
        </div>
        
        <!-- 隐藏号码区域 -->
        <div class="hidden-numbers-section" id="hiddenNumbersSection" style="display: none;">
            <div class="hidden-numbers-title">隐藏号码</div>
            <div class="hidden-numbers-grid" id="hiddenNumbersGrid"></div>
        </div>
        
        <!-- 操作按钮 -->
        <div class="action-bar">
            <div class="action-btn clear-btn" id="clearBtn">清空</div>
            <div class="action-btn copy-selected-btn" id="copySelectedBtn">复制已选</div>
            <div class="action-btn copy-btn" id="copyResultBtn">复制组号</div>
        </div>
        
        <div class="action-bar">
            <div class="action-btn generate-btn" id="generate5">生成5组</div>
            <div class="action-btn generate-btn" id="generate10">生成10组</div>
            <div class="action-btn generate-btn" id="generate20">生成20组</div>
        </div>
        
        <!-- 结果展示区域 -->
        <div class="result-section">
            <div class="result-title">
                <span>组号结果</span>
                <span id="selectedCount">已选: 0 个号码</span>
            </div>
            <div class="result-content" id="resultContent">
                <p style="text-align: center; color: #999; padding: 20px;">请选择号码，然后点击上方按钮生成组号</p>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const numberGrid = document.getElementById('numberGrid');
            const resultContent = document.getElementById('resultContent');
            const selectedCount = document.getElementById('selectedCount');
            const clearBtn = document.getElementById('clearBtn');
            const copySelectedBtn = document.getElementById('copySelectedBtn');
            const copyResultBtn = document.getElementById('copyResultBtn');
            const generate5 = document.getElementById('generate5');
            const generate10 = document.getElementById('generate10');
            const generate20 = document.getElementById('generate20');
            const selectedNumbersSection = document.getElementById('selectedNumbersSection');
            const selectedNumbersGrid = document.getElementById('selectedNumbersGrid');
            const killSection = document.getElementById('killSection');
            const killNumbersGrid = document.getElementById('killNumbersGrid');
            const hiddenNumbersSection = document.getElementById('hiddenNumbersSection');
            const hiddenNumbersGrid = document.getElementById('hiddenNumbersGrid');
            
            // 存储选中的数字、杀号和隐藏号码
            let selectedNumbers = new Set();
            let killNumbers = new Set();
            let hiddenNumbers = new Set();
            
            // 生肖与号码对应关系
            const zodiacNumbers = {
                '鼠': [6, 18, 30, 42],
                '牛': [5, 17, 29, 41],
                '虎': [4, 16, 28, 40],
                '兔': [3, 15, 27, 39],
                '龙': [2, 14, 26, 38],
                '蛇': [1, 13, 25, 37, 49],
                '马': [12, 24, 36, 48],
                '羊': [11, 23, 35, 47],
                '猴': [10, 22, 34, 46],
                '鸡': [9, 21, 33, 45],
                '狗': [8, 20, 32, 44],
                '猪': [7, 19, 31, 43]
            };
            
            // 号码与生肖对应关系
            const numberZodiacs = {};
            for (const [zodiac, numbers] of Object.entries(zodiacNumbers)) {
                numbers.forEach(num => {
                    numberZodiacs[num] = zodiac;
                });
            }
            
            // 波色对应关系
            const colorNumbers = {
                'red': [1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46],
                'blue': [3, 4, 9, 10, 14, 15, 20, 25, 26, 31, 36, 37, 41, 42, 47, 48],
                'green': [5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49]
            };
            
            // 创建1-49的数字
            for (let i = 1; i <= 49; i++) {
                const numberItem = document.createElement('div');
                numberItem.className = 'number-item';
                
                // 设置颜色
                if (colorNumbers.red.includes(i)) {
                    numberItem.classList.add('red');
                } else if (colorNumbers.blue.includes(i)) {
                    numberItem.classList.add('blue');
                } else if (colorNumbers.green.includes(i)) {
                    numberItem.classList.add('green');
                }
                
                // 创建数字和生肖标签
                const numberSpan = document.createElement('span');
                numberSpan.textContent = i.toString().padStart(2, '0');
                
                const zodiacSpan = document.createElement('span');
                zodiacSpan.className = 'zodiac-label';
                zodiacSpan.textContent = numberZodiacs[i] || '';
                
                numberItem.appendChild(numberSpan);
                numberItem.appendChild(zodiacSpan);
                
                numberItem.dataset.number = i;
                
                // 双击隐藏号码
                let clickTimer = null;
                numberItem.addEventListener('click', function() {
                    if (clickTimer) {
                        clearTimeout(clickTimer);
                        // 双击事件 - 隐藏号码
                        const num = parseInt(this.dataset.number);
                        if (!hiddenNumbers.has(num)) {
                            hiddenNumbers.add(num);
                            selectedNumbers.delete(num);
                            killNumbers.delete(num);
                            this.classList.add('hidden');
                            this.classList.remove('selected');
                        } else {
                            hiddenNumbers.delete(num);
                            this.classList.remove('hidden');
                        }
                        updateSelectedCount();
                        updateSelectedNumbersDisplay();
                        updateKillNumbersDisplay();
                        updateHiddenNumbersDisplay();
                        clickTimer = null;
                    } else {
                        clickTimer = setTimeout(() => {
                            // 单击事件 - 选择号码
                            const num = parseInt(this.dataset.number);
                            if (hiddenNumbers.has(num) || killNumbers.has(num)) {
                                return; // 隐藏或杀号区的号码不能直接选择
                            }
                            
                            if (selectedNumbers.has(num)) {
                                selectedNumbers.delete(num);
                                this.classList.remove('selected');
                            } else {
                                selectedNumbers.add(num);
                                this.classList.add('selected');
                            }
                            updateSelectedCount();
                            updateSelectedNumbersDisplay();
                            clickTimer = null;
                        }, 300);
                    }
                });
                
                // 长按添加到杀号区
                let longPressTimer;
                numberItem.addEventListener('touchstart', function(e) {
                    longPressTimer = setTimeout(() => {
                        const num = parseInt(this.dataset.number);
                        if (!killNumbers.has(num)) {
                            killNumbers.add(num);
                            selectedNumbers.delete(num);
                            hiddenNumbers.delete(num);
                            this.classList.add('hidden');
                            this.classList.remove('selected');
                        } else {
                            killNumbers.delete(num);
                            this.classList.remove('hidden');
                        }
                        updateSelectedCount();
                        updateSelectedNumbersDisplay();
                        updateKillNumbersDisplay();
                        updateHiddenNumbersDisplay();
                        e.preventDefault();
                    }, 500);
                });
                
                numberItem.addEventListener('touchend', function() {
                    clearTimeout(longPressTimer);
                });
                
                numberItem.addEventListener('touchmove', function() {
                    clearTimeout(longPressTimer);
                });
                
                numberGrid.appendChild(numberItem);
            }
            
            // 更新已选号码显示
            function updateSelectedNumbersDisplay() {
                selectedNumbersGrid.innerHTML = '';
                
                if (selectedNumbers.size === 0) {
                    selectedNumbersSection.style.display = 'none';
                    return;
                }
                
                selectedNumbersSection.style.display = 'block';
                
                // 显示已选号码
                Array.from(selectedNumbers).sort((a, b) => a - b).forEach(num => {
                    const selectedNumber = document.createElement('div');
                    selectedNumber.className = 'selected-number';
                    selectedNumber.textContent = num.toString().padStart(2, '0');
                    selectedNumbersGrid.appendChild(selectedNumber);
                });
            }
            
            // 更新杀号区显示
            function updateKillNumbersDisplay() {
                killNumbersGrid.innerHTML = '';
                
                if (killNumbers.size === 0) {
                    killSection.style.display = 'none';
                    return;
                }
                
                killSection.style.display = 'block';
                
                // 显示杀号
                Array.from(killNumbers).sort((a, b) => a - b).forEach(num => {
                    const killNumber = document.createElement('div');
                    killNumber.className = 'kill-number';
                    killNumber.textContent = num.toString().padStart(2, '0');
                    killNumbersGrid.appendChild(killNumber);
                });
            }
            
            // 更新隐藏号码显示
            function updateHiddenNumbersDisplay() {
                hiddenNumbersGrid.innerHTML = '';
                
                if (hiddenNumbers.size === 0) {
                    hiddenNumbersSection.style.display = 'none';
                    return;
                }
                
                hiddenNumbersSection.style.display = 'block';
                
                // 显示隐藏号码
                Array.from(hiddenNumbers).sort((a, b) => a - b).forEach(num => {
                    const hiddenNumber = document.createElement('div');
                    hiddenNumber.className = 'hidden-number';
                    hiddenNumber.textContent = num.toString().padStart(2, '0');
                    hiddenNumbersGrid.appendChild(hiddenNumber);
                });
            }
            
            // 更新选中计数
            function updateSelectedCount() {
                selectedCount.textContent = `已选: ${selectedNumbers.size} 个号码`;
            }
            
            // 清空按钮
            clearBtn.addEventListener('click', function() {
                selectedNumbers.clear();
                killNumbers.clear();
                hiddenNumbers.clear();
                
                document.querySelectorAll('.number-item').forEach(item => {
                    item.classList.remove('selected', 'hidden');
                });
                
                updateSelectedCount();
                updateSelectedNumbersDisplay();
                updateKillNumbersDisplay();
                updateHiddenNumbersDisplay();
                resultContent.innerHTML = '<p style="text-align: center; color: #999; padding: 20px;">请选择号码，然后点击上方按钮生成组号</p>';
            });
            
            // 生成随机组号
            function generateGroups(count) {
                if (selectedNumbers.size < 3) {
                    alert('请至少选择3个号码');
                    return;
                }
                
                // 过滤掉杀号和隐藏的号码
                const availableNumbers = Array.from(selectedNumbers).filter(num => 
                    !killNumbers.has(num) && !hiddenNumbers.has(num)
                );
                
                if (availableNumbers.length < 3) {
                    alert('可用号码不足3个，请选择更多号码或减少杀号/隐藏号码');
                    return;
                }
                
                let resultHTML = '';
                
                for (let i = 0; i < count; i++) {
                    // 随机打乱数组并取前3个
                    const shuffled = [...availableNumbers].sort(() => 0.5 - Math.random());
                    const group = shuffled.slice(0, 3).sort((a, b) => a - b);
                    
                    resultHTML += `
                        <div class="result-group">
                            <div class="result-numbers">
                                ${group.map(num => 
                                    `<div class="result-number">${num.toString().padStart(2, '0')}</div>`
                                ).join('')}
                                <div class="result-number">三中三2</div>
                            </div>
                        </div>
                    `;
                }
                
                resultContent.innerHTML = resultHTML;
            }
            
            // 绑定生成按钮
            generate5.addEventListener('click', () => generateGroups(5));
            generate10.addEventListener('click', () => generateGroups(10));
            generate20.addEventListener('click', () => generateGroups(20));
            
            // 复制已选号码
            copySelectedBtn.addEventListener('click', function() {
                if (selectedNumbers.size === 0) {
                    alert('请先选择号码');
                    return;
                }
                
                const selectedNumbersArray = Array.from(selectedNumbers).sort((a, b) => a - b);
                const textToCopy = selectedNumbersArray.map(num => num.toString().padStart(2, '0')).join(',');
                
                navigator.clipboard.writeText(textToCopy)
                    .then(() => alert('已选号码已复制到剪贴板'))
                    .catch(() => {
                        const textArea = document.createElement('textarea');
                        textArea.value = textToCopy;
                        document.body.appendChild(textArea);
                        textArea.select();
                        try {
                            document.execCommand('copy');
                            alert('已选号码已复制到剪贴板');
                        } catch (err) {
                            alert('复制失败，请手动复制');
                        }
                        document.body.removeChild(textArea);
                    });
            });
            
            // 复制组号结果
            copyResultBtn.addEventListener('click', function() {
                if (resultContent.textContent.includes('请选择')) {
                    alert('请先生成组号');
                    return;
                }
                
                const textToCopy = Array.from(resultContent.querySelectorAll('.result-group'))
                    .map((group) => {
                        const numbers = Array.from(group.querySelectorAll('.result-number'))
                            .map(item => item.textContent)
                            .filter(text => !text.includes('三中三'))
                            .join(',');
                        return `${numbers},三中三2`;
                    })
                    .join('\n');
                
                navigator.clipboard.writeText(textToCopy)
                    .then(() => alert('组号结果已复制到剪贴板'))
                    .catch(() => {
                        const textArea = document.createElement('textarea');
                        textArea.value = textToCopy;
                        document.body.appendChild(textArea);
                        textArea.select();
                        try {
                            document.execCommand('copy');
                            alert('组号结果已复制到剪贴板');
                        } catch (err) {
                            alert('复制失败，请手动复制');
                        }
                        document.body.removeChild(textArea);
                    });
            });
        });
    </script>
</body>
</html>
