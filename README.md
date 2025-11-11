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
        
        .header {
            background: linear-gradient(to right, #e53935, #d32f2f);
            color: white;
            text-align: center;
            padding: 12px 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        .section {
            margin-bottom: 15px;
        }
        
        .section-title {
            background-color: #f0f0f0;
            padding: 8px 15px;
            font-size: 0.9rem;
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
            border-radius: 8px;
            font-weight: bold;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.2s;
            user-select: none;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
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
            box-shadow: 0 0 0 2px #ff9800;
            transform: scale(0.95);
        }
        
        .number-item.hidden {
            opacity: 0.3;
            filter: grayscale(0.8);
        }
        
        .zodiac-label {
            font-size: 0.5rem;
            margin-top: 2px;
        }
        
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
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            font-size: 0.85rem;
        }
        
        .clear-btn {
            background: #f0f0f0;
            color: #666;
        }
        
        .generate-btn {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
            color: white;
        }
        
        .result-section {
            flex: 1;
            border-top: 1px solid #e0e0e0;
        }
        
        .result-title {
            background-color: #f0f0f0;
            padding: 8px 15px;
            font-size: 0.85rem;
            color: #666;
            display: flex;
            justify-content: space-between;
        }
        
        .result-content {
            padding: 15px;
            min-height: 150px;
        }
        
        @media (max-width: 480px) {
            .number-grid {
                grid-template-columns: repeat(5, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>挑码助手</h1>
        </div>
        
        <div class="section">
            <div class="section-title">选择号码 (1-49)</div>
            <div class="number-grid" id="numberGrid"></div>
        </div>
        
        <div class="action-bar">
            <div class="action-btn clear-btn" id="clearBtn">清空</div>
            <div class="action-btn copy-btn" id="copyBtn">复制结果</div>
            <div class="action-btn generate-btn" id="generate5">生成5组</div>
        </div>
        
        <div class="action-bar">
            <div class="action-btn generate-btn" id="generate10">生成10组</div>
            <div class="action-btn generate-btn" id="generate20">生成20组</div>
        </div>
        
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
            const copyBtn = document.getElementById('copyBtn');
            const generate5 = document.getElementById('generate5');
            const generate10 = document.getElementById('generate10');
            const generate20 = document.getElementById('generate20');
            
            let selectedNumbers = new Set();
            let hiddenNumbers = new Set();
            
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
                zodiacSpan.textContent = Object.entries(zodiacNumbers)
                    .find(([_, nums]) => nums.includes(i))?.[0] || '';
                
                numberItem.appendChild(numberSpan);
                numberItem.appendChild(zodiacSpan);
                
                numberItem.dataset.number = i;
                
                // 点击事件
                numberItem.addEventListener('click', function() {
                    const num = parseInt(this.dataset.number);
                    if (hiddenNumbers.has(num)) return;
                    
                    if (selectedNumbers.has(num)) {
                        selectedNumbers.delete(num);
                        this.classList.remove('selected');
                    } else {
                        selectedNumbers.add(num);
                        this.classList.add('selected');
                    }
                    updateSelectedCount();
                });
                
                // 长按隐藏
                let longPressTimer;
                numberItem.addEventListener('touchstart', function(e) {
                    longPressTimer = setTimeout(() => {
                        const num = parseInt(this.dataset.number);
                        if (!hiddenNumbers.has(num)) {
                            hiddenNumbers.add(num);
                            selectedNumbers.delete(num);
                            this.classList.add('hidden');
                            this.classList.remove('selected');
                        } else {
                            hiddenNumbers.delete(num);
                            this.classList.remove('hidden');
                        }
                        updateSelectedCount();
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
            
            function updateSelectedCount() {
                selectedCount.textContent = `已选: ${selectedNumbers.size} 个号码`;
            }
            
            clearBtn.addEventListener('click', function() {
                selectedNumbers.clear();
                hiddenNumbers.clear();
                document.querySelectorAll('.number-item').forEach(item => {
                    item.classList.remove('selected', 'hidden');
                });
                updateSelectedCount();
                resultContent.innerHTML = '<p style="text-align: center; color: #999; padding: 20px;">请选择号码，然后点击上方按钮生成组号</p>';
            });
            
            function generateGroups(count) {
                if (selectedNumbers.size < 3) {
                    alert('请至少选择3个号码');
                    return;
                }
                
                const availableNumbers = Array.from(selectedNumbers)
                    .filter(num => !hiddenNumbers.has(num));
                
                if (availableNumbers.length < 3) {
                    alert('可用号码不足3个');
                    return;
                }
                
                let resultHTML = '';
                for (let i = 0; i < count; i++) {
                    const shuffled = [...availableNumbers].sort(() => 0.5 - Math.random());
                    const group = shuffled.slice(0, 3).sort((a, b) => a - b);
                    
                    resultHTML += `
                        <div style="margin-bottom: 15px; padding: 10px; background: white; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.1);">
                            <div style="display: flex; flex-wrap: wrap; gap: 8px; align-items: center;">
                                ${group.map(num => 
                                    `<div style="background: #f0f0f0; padding: 5px 10px; border-radius: 4px; font-size: 0.85rem;">
                                        ${num.toString().padStart(2, '0')}
                                    </div>`
                                ).join('')}
                                <div style="background: #f0f0f0; padding: 5px 10px; border-radius: 4px; font-size: 0.85rem;">
                                    三中三2
                                </div>
                            </div>
                        </div>
                    `;
                }
                
                resultContent.innerHTML = resultHTML;
            }
            
            generate5.addEventListener('click', () => generateGroups(5));
            generate10.addEventListener('click', () => generateGroups(10));
            generate20.addEventListener('click', () => generateGroups(20));
            
            copyBtn.addEventListener('click', function() {
                if (!resultContent.textContent.includes('三中三')) {
                    alert('请先生成组号');
                    return;
                }
                
                const textToCopy = Array.from(resultContent.querySelectorAll('div > div'))
                    .map(group => {
                        return Array.from(group.querySelectorAll('div'))
                            .map(item => item.textContent.trim())
                            .join(',');
                    })
                    .join('\n');
                
                navigator.clipboard.writeText(textToCopy)
                    .then(() => alert('已复制到剪贴板'))
                    .catch(() => {
                        const textArea = document.createElement('textarea');
                        textArea.value = textToCopy;
                        document.body.appendChild(textArea);
                        textArea.select();
                        try {
                            document.execCommand('copy');
                            alert('已复制到剪贴板');
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
