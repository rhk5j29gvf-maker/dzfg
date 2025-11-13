<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>挑码助手 - 优化版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            padding: 10px;
            max-width: 500px;
            margin: 0 auto;
        }
        
        .title {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            padding: 15px 0;
            color: #d81e06;
            background-color: white;
            border-radius: 8px 8px 0 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .numbers-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            padding: 15px;
            background-color: white;
            border-radius: 0 0 8px 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            min-height: 300px;
        }
        
        .number-item {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            font-weight: bold;
            cursor: pointer;
            position: relative;
            transition: all 0.2s;
            box-shadow: 0 2px 3px rgba(0,0,0,0.2);
        }
        
        .number-item.red {
            background: radial-gradient(circle at 30% 30%, #ff6b6b, #d81e06);
        }
        
        .number-item.blue {
            background: radial-gradient(circle at 30% 30%, #6bc6ff, #1e6bd8);
        }
        
        .number-item.green {
            background: radial-gradient(circle at 30% 30%, #6bff9e, #06d84e);
        }
        
        .number-item.selected {
            opacity: 0.3;
        }
        
        .number-item.hidden {
            opacity: 0.1;
        }
        
        .number {
            font-size: 16px;
        }
        
        .zodiac {
            font-size: 10px;
            margin-top: 2px;
        }
        
        .selected-hidden-section {
            display: flex;
            justify-content: space-between;
            margin: 15px 0;
            gap: 10px;
        }
        
        .selected-box, .hidden-box {
            flex: 1;
            background: white;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            min-height: 60px;
        }
        
        .box-title {
            font-size: 14px;
            color: #666;
            margin-bottom: 5px;
        }
        
        .selected-numbers, .hidden-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        .selected-number, .hidden-number {
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 12px;
            color: white;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .selected-number:hover, .hidden-number:hover {
            transform: scale(1.1);
        }
        
        .selected-number.red {
            background-color: #d81e06;
        }
        
        .selected-number.blue {
            background-color: #1e6bd8;
        }
        
        .selected-number.green {
            background-color: #06d84e;
        }
        
        .hidden-number {
            background: #888;
            color: white;
        }
        
        .random-buttons {
            display: flex;
            justify-content: space-between;
            gap: 10px;
            margin: 15px 0;
        }
        
        .random-btn {
            flex: 1;
            padding: 12px 5px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 14px;
        }
        
        .random-select {
            background-color: #4caf50;
            color: white;
        }
        
        .random-special {
            background-color: #9c27b0;
            color: white;
            flex: 1.2;
        }
        
        .random-kill {
            background-color: #ff5722;
            color: white;
        }
        
        .random-btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
        
        .zodiac-buttons {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .zodiac-btn {
            padding: 8px 5px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 14px;
            background-color: #f0f0f0;
        }
        
        .zodiac-btn:hover {
            background-color: #e0e0e0;
            transform: translateY(-2px);
        }
        
        .function-buttons {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 15px 0;
        }
        
        .btn {
            padding: 12px 5px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 14px;
        }
        
        .btn-generate {
            background-color: #1e6bd8;
            color: white;
        }
        
        .btn-clear {
            background-color: #ff9800;
            color: white;
        }
        
        .btn-copy {
            background-color: #06d84e;
            color: white;
        }
        
        .btn-generate-10 {
            background-color: #d81e06;
            color: white;
        }
        
        .btn-generate-20 {
            background-color: #9c27b0;
            color: white;
        }
        
        .btn-copy-groups {
            background-color: #795548;
            color: white;
        }
        
        .btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
        
        .generated-numbers {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-bottom: 15px;
            min-height: 150px;
        }
        
        .generated-item {
            padding: 8px 0;
            border-bottom: 1px solid #f0f0f0;
            font-size: 16px;
        }
        
        .zodiac-section {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-bottom: 15px;
        }
        
        .info-title {
            font-size: 16px;
            font-weight: bold;
            margin-bottom: 10px;
            color: #d81e06;
            text-align: center;
        }
        
        .zodiac-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        
        .zodiac-item {
            text-align: center;
            padding: 8px;
            border-radius: 4px;
            background-color: #f9f9f9;
        }
        
        .zodiac-name {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .zodiac-numbers {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 2px;
        }
        
        .zodiac-number {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 10px;
            color: white;
        }
        
        .zodiac-number.red {
            background-color: #d81e06;
        }
        
        .zodiac-number.blue {
            background-color: #1e6bd8;
        }
        
        .zodiac-number.green {
            background-color: #06d84e;
        }
        
        .footer {
            text-align: center;
            padding: 15px;
            color: #666;
            font-size: 12px;
        }
    </style>
</head>
<body>
    <div class="title">挑码助手 - 优化版</div>
    
    <div class="numbers-grid" id="numbersGrid">
        <!-- 号码球将通过JavaScript动态生成 -->
    </div>
    
    <div class="selected-hidden-section">
        <div class="selected-box">
            <div class="box-title">已选号码</div>
            <div class="selected-numbers" id="selectedNumbers"></div>
        </div>
        <div class="hidden-box">
            <div class="box-title">以杀号码</div>
            <div class="hidden-numbers" id="hiddenNumbers"></div>
        </div>
    </div>
    
    <!-- 修改后的随机按钮区域 -->
    <div class="random-buttons">
        <button class="random-btn random-select" id="randomSelect">随缘中</button>
        <button class="random-btn random-special" id="randomSpecial">大中特中</button>
        <button class="random-btn random-kill" id="randomKill">随缘杀</button>
    </div>
    
    <div class="zodiac-buttons" id="zodiacButtons">
        <!-- 生肖按钮将通过JavaScript动态生成 -->
    </div>
    
    <div class="function-buttons">
        <!-- 第一行 -->
        <button class="btn btn-clear" id="clearAll">清空全部</button>
        <button class="btn btn-copy" id="copySelected">复制选中</button>
        <button class="btn btn-copy-groups" id="copyGroups">复制组号</button>
        
        <!-- 第二行 -->
        <button class="btn btn-generate" id="generate5">生成五组</button>
        <button class="btn btn-generate-10" id="generate10">生成十组</button>
        <button class="btn btn-generate-20" id="generate20">生成二十组</button>
    </div>
    
    <div class="generated-numbers" id="generatedNumbers">
        <!-- 生成的号码将显示在这里 -->
    </div>
    
    <div class="zodiac-section">
        <div class="info-title">2025蛇年(十二生肖对照)</div>
        <div class="zodiac-grid" id="zodiacGrid">
            <!-- 生肖将通过JavaScript动态生成 -->
        </div>
    </div>

    <div class="footer">
        <p>挑码助手 - 专业彩票选号工具</p>
        <p>© 2025 挑码助手. 仅供娱乐参考</p>
    </div>

    <script>
        // 生肖对应关系 - 根据您提供的图片信息
        const zodiacMap = {
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
        
        // 波色对应关系 - 根据您提供的图片信息
        const colorMap = {
            'red': [1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46],
            'blue': [3, 4, 9, 10, 14, 15, 20, 25, 26, 31, 36, 37, 41, 42, 47, 48],
            'green': [5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49]
        };
        
        // 获取号码对应的生肖
        function getZodiac(number) {
            for (const [zodiac, numbers] of Object.entries(zodiacMap)) {
                if (numbers.includes(number)) {
                    return zodiac;
                }
            }
            return '';
        }
        
        // 获取号码颜色
        function getColor(number) {
            for (const [color, numbers] of Object.entries(colorMap)) {
                if (numbers.includes(number)) {
                    return color;
                }
            }
            return 'red';
        }
        
        // 随机打乱数组
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        // 初始化元素
        const numbersGrid = document.getElementById('numbersGrid');
        const selectedNumbers = document.getElementById('selectedNumbers');
        const hiddenNumbers = document.getElementById('hiddenNumbers');
        const generatedNumbers = document.getElementById('generatedNumbers');
        const zodiacGrid = document.getElementById('zodiacGrid');
        const zodiacButtons = document.getElementById('zodiacButtons');
        
        let selected = [];
        let hidden = [];
        
        // 创建49个号码球
        for (let i = 1; i <= 49; i++) {
            const numberItem = document.createElement('div');
            numberItem.className = `number-item ${getColor(i)}`;
            numberItem.innerHTML = `
                <div class="number">${i.toString().padStart(2, '0')}</div>
                <div class="zodiac">${getZodiac(i)}</div>
            `;
            numberItem.dataset.number = i;
            
            // 单击选择号码
            numberItem.addEventListener('click', function() {
                const num = parseInt(this.dataset.number);
                
                // 如果号码在已选列表中，则取消选择
                if (selected.includes(num)) {
                    selected = selected.filter(n => n !== num);
                    this.classList.remove('selected');
                } 
                // 如果号码在隐藏列表中，则取消隐藏
                else if (hidden.includes(num)) {
                    hidden = hidden.filter(n => n !== num);
                    this.classList.remove('hidden');
                } 
                // 否则选择号码
                else {
                    selected.push(num);
                    this.classList.add('selected');
                }
                
                updateSelectedNumbers();
                updateHiddenNumbers();
            });
            
            // 双击隐藏号码
            numberItem.addEventListener('dblclick', function() {
                const num = parseInt(this.dataset.number);
                
                // 如果号码在隐藏列表中，则取消隐藏
                if (hidden.includes(num)) {
                    hidden = hidden.filter(n => n !== num);
                    this.classList.remove('hidden');
                } 
                // 如果号码在已选列表中，则取消选择并隐藏
                else if (selected.includes(num)) {
                    selected = selected.filter(n => n !== num);
                    this.classList.remove('selected');
                    hidden.push(num);
                    this.classList.add('hidden');
                } 
                // 否则隐藏号码
                else {
                    hidden.push(num);
                    this.classList.add('hidden');
                }
                
                updateSelectedNumbers();
                updateHiddenNumbers();
            });
            
            numbersGrid.appendChild(numberItem);
        }
        
        // 创建十二生肖按钮
        const zodiacOrder = ['鼠', '牛', '虎', '兔', '龙', '蛇', '马', '羊', '猴', '鸡', '狗', '猪'];
        zodiacOrder.forEach(zodiac => {
            const zodiacBtn = document.createElement('button');
            zodiacBtn.className = 'zodiac-btn';
            zodiacBtn.textContent = zodiac;
            zodiacBtn.dataset.zodiac = zodiac;
            
            // 单击选择生肖对应号码
            zodiacBtn.addEventListener('click', function() {
                const zodiac = this.dataset.zodiac;
                const numbers = zodiacMap[zodiac];
                
                // 选择对应生肖的所有号码
                numbers.forEach(num => {
                    if (!selected.includes(num) && !hidden.includes(num)) {
                        selected.push(num);
                        document.querySelector(`.number-item[data-number="${num}"]`).classList.add('selected');
                    }
                });
                
                updateSelectedNumbers();
            });
            
            // 双击隐藏生肖对应号码
            zodiacBtn.addEventListener('dblclick', function() {
                const zodiac = this.dataset.zodiac;
                const numbers = zodiacMap[zodiac];
                
                // 隐藏对应生肖的所有号码
                numbers.forEach(num => {
                    if (!hidden.includes(num)) {
                        // 如果号码已选中，先取消选择
                        if (selected.includes(num)) {
                            selected = selected.filter(n => n !== num);
                        }
                        hidden.push(num);
                        document.querySelector(`.number-item[data-number="${num}"]`).classList.add('hidden');
                    }
                });
                
                updateHiddenNumbers();
                updateSelectedNumbers();
            });
            
            zodiacButtons.appendChild(zodiacBtn);
        });
        
        // 创建生肖对照表
        for (const [zodiac, numbers] of Object.entries(zodiacMap)) {
            const zodiacItem = document.createElement('div');
            zodiacItem.className = 'zodiac-item';
            
            let conflict = '';
            if (zodiac === '蛇') conflict = '[冲猪]';
            else if (zodiac === '龙') conflict = '[冲狗]';
            else if (zodiac === '兔') conflict = '[冲鸡]';
            else if (zodiac === '虎') conflict = '[冲猴]';
            else if (zodiac === '牛') conflict = '[冲羊]';
            else if (zodiac === '鼠') conflict = '[冲马]';
            else if (zodiac === '猪') conflict = '[冲蛇]';
            else if (zodiac === '狗') conflict = '[冲龙]';
            else if (zodiac === '鸡') conflict = '[冲兔]';
            else if (zodiac === '猴') conflict = '[冲虎]';
            else if (zodiac === '羊') conflict = '[冲牛]';
            else if (zodiac === '马') conflict = '[冲鼠]';
            
            zodiacItem.innerHTML = `
                <div class="zodiac-name">${zodiac}${conflict}</div>
                <div class="zodiac-numbers">
                    ${numbers.map(num => `<div class="zodiac-number ${getColor(num)}">${num.toString().padStart(2, '0')}</div>`).join('')}
                </div>
            `;
            
            zodiacGrid.appendChild(zodiacItem);
        }
        
        // 更新已选号码显示
        function updateSelectedNumbers() {
            selectedNumbers.innerHTML = '';
            selected.sort((a, b) => a - b).forEach(num => {
                const span = document.createElement('span');
                span.className = `selected-number ${getColor(num)}`;
                span.textContent = num.toString().padStart(2, '0');
                span.dataset.number = num;
                
                // 单击已选号码可以返回选号区
                span.addEventListener('click', function() {
                    const num = parseInt(this.dataset.number);
                    selected = selected.filter(n => n !== num);
                    
                    // 恢复号码球显示
                    const numberItem = document.querySelector(`.number-item[data-number="${num}"]`);
                    numberItem.classList.remove('selected');
                    
                    updateSelectedNumbers();
                });
                
                selectedNumbers.appendChild(span);
            });
        }
        
        // 更新隐藏号码显示
        function updateHiddenNumbers() {
            hiddenNumbers.innerHTML = '';
            hidden.sort((a, b) => a - b).forEach(num => {
                const span = document.createElement('span');
                span.className = 'hidden-number';
                span.textContent = num.toString().padStart(2, '0');
                span.dataset.number = num;
                
                // 单击隐藏号码可以返回选号区
                span.addEventListener('click', function() {
                    const num = parseInt(this.dataset.number);
                    hidden = hidden.filter(n => n !== num);
                    
                    // 恢复号码球显示
                    const numberItem = document.querySelector(`.number-item[data-number="${num}"]`);
                    numberItem.classList.remove('hidden');
                    
                    updateHiddenNumbers();
                });
                
                hiddenNumbers.appendChild(span);
            });
        }
        
        // 生成随机号码组
        function generateGroups(count) {
            generatedNumbers.innerHTML = '';
            
            // 创建可用号码池（排除隐藏号码）
            let availableNumbers = Array.from({length: 49}, (_, i) => i + 1)
                .filter(num => !hidden.includes(num));
            
            // 如果选择了号码，则只从已选号码中生成
            if (selected.length > 0) {
                availableNumbers = availableNumbers.filter(num => selected.includes(num));
            }
            
            // 生成指定数量的组
            for (let i = 0; i < count; i++) {
                // 打乱数组
                const shuffled = shuffleArray([...availableNumbers]);
                
                // 取前三个号码
                const group = shuffled.slice(0, 3).sort((a, b) => a - b);
                
                // 创建显示元素
                const groupElement = document.createElement('div');
                groupElement.className = 'generated-item';
                groupElement.textContent = `${group.map(num => num.toString().padStart(2, '0')).join(',')} 三中三2`;
                
                generatedNumbers.appendChild(groupElement);
            }
        }
        
        // 清空所有选择
        function clearAllSelections() {
            selected = [];
            hidden = [];
            
            // 清除所有选中和隐藏状态
            document.querySelectorAll('.number-item').forEach(item => {
                item.classList.remove('selected');
                item.classList.remove('hidden');
            });
            
            updateSelectedNumbers();
            updateHiddenNumbers();
            generatedNumbers.innerHTML = '';
        }
        
        // 复制选中号码
        function copySelectedNumbers() {
            if (selected.length === 0) {
                alert('请先选择号码');
                return;
            }
            
            const text = selected.sort((a, b) => a - b)
                .map(num => num.toString().padStart(2, '0'))
                .join(',');
            
            // 使用现代复制API
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text)
                    .then(() => alert('已复制选中号码: ' + text))
                    .catch(err => {
                        // 降级方案
                        copyTextFallback(text);
                    });
            } else {
                // 降级方案
                copyTextFallback(text);
            }
        }
        
        // 复制生成的组号
        function copyGeneratedGroups() {
            const groups = Array.from(generatedNumbers.children)
                .map(el => el.textContent)
                .join('\n');
            
            if (!groups) {
                alert('请先生成号码组');
                return;
            }
            
            // 使用现代复制API
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(groups)
                    .then(() => alert('已复制所有组号'))
                    .catch(err => {
                        // 降级方案
                        copyTextFallback(groups);
                    });
            } else {
                // 降级方案
                copyTextFallback(groups);
            }
        }
        
        // 降级复制方案
        function copyTextFallback(text) {
            const textArea = document.createElement('textarea');
            textArea.value = text;
            textArea.style.position = 'fixed';
            textArea.style.left = '-999999px';
            textArea.style.top = '-999999px';
            document.body.appendChild(textArea);
            textArea.focus();
            textArea.select();
            
            try {
                const successful = document.execCommand('copy');
                if (successful) {
                    alert('已复制内容');
                } else {
                    alert('复制失败，请手动复制');
                }
            } catch (err) {
                alert('复制失败，请手动复制: ' + text);
            }
            
            document.body.removeChild(textArea);
        }
        
        // 随缘杀 - 随机选择10个号码到以杀列表
        function randomKill() {
            // 获取所有可用号码（排除已选和已杀号码）
            const availableNumbers = Array.from({length: 49}, (_, i) => i + 1)
                .filter(num => !selected.includes(num) && !hidden.includes(num));
            
            if (availableNumbers.length < 10) {
                alert('可用号码不足10个，请先清空部分选择');
                return;
            }
            
            // 打乱数组并取前10个
            const shuffled = shuffleArray([...availableNumbers]);
            const randomNumbers = shuffled.slice(0, 10);
            
            // 添加到隐藏列表
            randomNumbers.forEach(num => {
                hidden.push(num);
                document.querySelector(`.number-item[data-number="${num}"]`).classList.add('hidden');
            });
            
            updateHiddenNumbers();
        }
        
        // 随缘中 - 随机选择10个号码到已选列表
        function randomSelect() {
            // 获取所有可用号码（排除已选和已杀号码）
            const availableNumbers = Array.from({length: 49}, (_, i) => i + 1)
                .filter(num => !selected.includes(num) && !hidden.includes(num));
            
            if (availableNumbers.length < 10) {
                alert('可用号码不足10个，请先清空部分选择');
                return;
            }
            
            // 打乱数组并取前10个
            const shuffled = shuffleArray([...availableNumbers]);
            const randomNumbers = shuffled.slice(0, 10);
            
            // 添加到已选列表
            randomNumbers.forEach(num => {
                selected.push(num);
                document.querySelector(`.number-item[data-number="${num}"]`).classList.add('selected');
            });
            
            updateSelectedNumbers();
        }
        
        // 大中特中 - 随机选择24个号码到已选列表
        function randomSpecial() {
            // 获取所有可用号码（排除已选和已杀号码）
            const availableNumbers = Array.from({length: 49}, (_, i) => i + 1)
                .filter(num => !selected.includes(num) && !hidden.includes(num));
            
            if (availableNumbers.length < 24) {
                alert('可用号码不足24个，请先清空部分选择');
                return;
            }
            
            // 打乱数组并取前24个
            const shuffled = shuffleArray([...availableNumbers]);
            const randomNumbers = shuffled.slice(0, 24);
            
            // 添加到已选列表
            randomNumbers.forEach(num => {
                selected.push(num);
                document.querySelector(`.number-item[data-number="${num}"]`).classList.add('selected');
            });
            
            updateSelectedNumbers();
        }
        
        // 事件监听
        document.getElementById('generate5').addEventListener('click', () => generateGroups(5));
        document.getElementById('generate10').addEventListener('click', () => generateGroups(10));
        document.getElementById('generate20').addEventListener('click', () => generateGroups(20));
        document.getElementById('copySelected').addEventListener('click', copySelectedNumbers);
        document.getElementById('copyGroups').addEventListener('click', copyGeneratedGroups);
        document.getElementById('clearAll').addEventListener('click', clearAllSelections);
        document.getElementById('randomKill').addEventListener('click', randomKill);
        document.getElementById('randomSelect').addEventListener('click', randomSelect);
        document.getElementById('randomSpecial').addEventListener('click', randomSpecial);
        
        // 初始化显示
        updateSelectedNumbers();
        updateHiddenNumbers();
    </script>
</body>
</html>
