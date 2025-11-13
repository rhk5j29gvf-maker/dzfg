<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-title" content="挑码助手">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>挑码助手 - 专业彩票选号工具</title>
    <link rel="apple-touch-icon" href="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTkyIiBoZWlnaHQ9IjE5MiIgdmlld0JveD0iMCAwIDE5MiAxOTIiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIxOTIiIGhlaWdodD0iMTkyIiByeD0iMjQiIGZpbGw9IiNEMDFGMDYiLz4KPHN2ZyB3aWR0aD0iOTYiIGhlaWdodD0iOTYiIHZpZXdCb3g9IjAgMCA5NiA5NiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTQ4IDI0QzM0LjcyNSAyNCAyNCAzNC43MjUgMjQgNDhWMzZDMTYgNDAgOCA1MiA4IDY0QzggNzYgMTYgODggMjQgOTJWODBDMjQgOTMuMjU0OCAyNi43NDUyIDk2IDMwIDk2SDY2QzY5LjI1NDggOTYgNzIgOTMuMjU0OCA3MiA5MlY4MEM3MiA3NiA2NCA2NCA1NiA2MEM2NCA1NiA3MiA0NCA3MiAzMkM3MiAyMCA2NCA4IDU2IDRWNDhDNTYgMzQuNzI1IDQ1LjI3NSAyNCAzMiAyNEg0OFoiIGZpbGw9IndoaXRlIi8+Cjwvc3ZnPgo8L3N2Zz4K">
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
            -webkit-user-select: none;
            -webkit-touch-callout: none;
            -webkit-tap-highlight-color: transparent;
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
        
        .install-prompt {
            background: #e8f4ff;
            border-left: 4px solid #1e6bd8;
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
            font-size: 14px;
            display: flex;
            align-items: center;
        }
        
        .install-prompt .icon {
            font-size: 20px;
            margin-right: 10px;
            color: #1e6bd8;
        }
        
        .numbers-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            padding: 15px;
            background-color: white;
            border-radius: 0 0 8px 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
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
            box-shadow: 0 0 0 3px #ffcc00;
        }
        
        .number-item.hidden {
            background: #888 !important;
            opacity: 0.5;
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
        
        .function-buttons {
            display: flex;
            justify-content: space-between;
            margin: 15px 0;
            gap: 10px;
        }
        
        .btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .btn-generate {
            background-color: #1e6bd8;
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
        
        .btn-copy-groups {
            width: 100%;
            padding: 12px;
            background-color: #ff9800;
            color: white;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 20px;
        }
        
        .info-section {
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
        
        .info-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }
        
        .info-item {
            padding: 8px;
            border-radius: 4px;
            font-size: 12px;
        }
        
        .info-item.red {
            background-color: #ffebee;
            border-left: 3px solid #d81e06;
        }
        
        .info-item.blue {
            background-color: #e3f2fd;
            border-left: 3px solid #1e6bd8;
        }
        
        .info-item.green {
            background-color: #e8f5e9;
            border-left: 3px solid #06d84e;
        }
        
        .info-item-title {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .info-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 3px;
        }
        
        .info-number {
            padding: 1px 4px;
            border-radius: 3px;
            font-size: 10px;
        }
        
        .info-number.red {
            background-color: #ffcdd2;
        }
        
        .info-number.blue {
            background-color: #bbdefb;
        }
        
        .info-number.green {
            background-color: #c8e6c9;
        }
        
        .zodiac-section {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-bottom: 15px;
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
        
        .property-section {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-bottom: 15px;
        }
        
        .property-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        
        .property-item {
            padding: 8px;
            border-radius: 4px;
            background-color: #f9f9f9;
            font-size: 12px;
        }
        
        .property-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: #1e6bd8;
        }
        
        .property-content {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        .property-zodiac {
            padding: 2px 6px;
            background-color: #e3f2fd;
            border-radius: 10px;
            font-size: 11px;
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
    <div class="title">挑码助手</div>
    
    <div class="install-prompt">
        <div class="icon">⬇️</div>
        <div>点击分享按钮 → "添加到主屏幕"，即可像应用一样使用</div>
    </div>
    
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
    
    <div class="function-buttons">
        <button class="btn btn-generate" id="generate5">生成五组</button>
        <button class="btn btn-copy" id="copySelected">复制选中</button>
        <button class="btn btn-generate-10" id="generate10">生成十组</button>
    </div>
    
    <div class="generated-numbers" id="generatedNumbers">
        <!-- 生成的号码将显示在这里 -->
    </div>
    
    <button class="btn-copy-groups" id="copyGroups">复制组号</button>
    
    <div class="zodiac-section">
        <div class="info-title">2025蛇年(十二生肖对照)</div>
        <div class="zodiac-grid" id="zodiacGrid">
            <!-- 生肖将通过JavaScript动态生成 -->
        </div>
    </div>
    
    <div class="info-section">
        <div class="info-title">波色对照</div>
        <div class="info-grid">
            <div class="info-item red">
                <div class="info-item-title">红波</div>
                <div class="info-numbers">
                    <span class="info-number red">01</span>
                    <span class="info-number red">02</span>
                    <span class="info-number red">07</span>
                    <span class="info-number red">08</span>
                    <span class="info-number red">12</span>
                    <span class="info-number red">13</span>
                    <span class="info-number red">18</span>
                    <span class="info-number red">19</span>
                    <span class="info-number red">23</span>
                    <span class="info-number red">24</span>
                    <span class="info-number red">29</span>
                    <span class="info-number red">30</span>
                    <span class="info-number red">34</span>
                    <span class="info-number red">35</span>
                    <span class="info-number red">40</span>
                    <span class="info-number red">45</span>
                    <span class="info-number red">46</span>
                </div>
            </div>
            <div class="info-item blue">
                <div class="info-item-title">蓝波</div>
                <div class="info-numbers">
                    <span class="info-number blue">03</span>
                    <span class="info-number blue">04</span>
                    <span class="info-number blue">09</span>
                    <span class="info-number blue">10</span>
                    <span class="info-number blue">14</span>
                    <span class="info-number blue">15</span>
                    <span class="info-number blue">20</span>
                    <span class="info-number blue">25</span>
                    <span class="info-number blue">26</span>
                    <span class="info-number blue">31</span>
                    <span class="info-number blue">36</span>
                    <span class="info-number blue">37</span>
                    <span class="info-number blue">41</span>
                    <span class="info-number blue">42</span>
                    <span class="info-number blue">47</span>
                    <span class="info-number blue">48</span>
                </div>
            </div>
            <div class="info-item green">
                <div class="info-item-title">绿波</div>
                <div class="info-numbers">
                    <span class="info-number green">05</span>
                    <span class="info-number green">06</span>
                    <span class="info-number green">11</span>
                    <span class="info-number green">16</span>
                    <span class="info-number green">17</span>
                    <span class="info-number green">21</span>
                    <span class="info-number green">22</span>
                    <span class="info-number green">27</span>
                    <span class="info-number green">28</span>
                    <span class="info-number green">32</span>
                    <span class="info-number green">33</span>
                    <span class="info-number green">38</span>
                    <span class="info-number green">39</span>
                    <span class="info-number green">43</span>
                    <span class="info-number green">44</span>
                    <span class="info-number green">49</span>
                </div>
            </div>
        </div>
    </div>
    
    <div class="info-section">
        <div class="info-title">五行对照</div>
        <div class="info-grid">
            <div class="info-item" style="background-color: #fff3e0; border-left: 3px solid #ff9800;">
                <div class="info-item-title">金</div>
                <div class="info-numbers">
                    <span class="info-number" style="background-color: #ffe0b2;">03</span>
                    <span class="info-number" style="background-color: #ffe0b2;">04</span>
                    <span class="info-number" style="background-color: #ffe0b2;">11</span>
                    <span class="info-number" style="background-color: #ffe0b2;">12</span>
                    <span class="info-number" style="background-color: #ffe0b2;">25</span>
                    <span class="info-number" style="background-color: #ffe0b2;">26</span>
                    <span class="info-number" style="background-color: #ffe0b2;">33</span>
                    <span class="info-number" style="background-color: #ffe0b2;">34</span>
                    <span class="info-number" style="background-color: #ffe0b2;">41</span>
                    <span class="info-number" style="background-color: #ffe0b2;">42</span>
                </div>
            </div>
            <div class="info-item" style="background-color: #e8f5e8; border-left: 3px solid #4caf50;">
                <div class="info-item-title">木</div>
                <div class="info-numbers">
                    <span class="info-number" style="background-color: #c8e6c9;">07</span>
                    <span class="info-number" style="background-color: #c8e6c9;">08</span>
                    <span class="info-number" style="background-color: #c8e6c9;">15</span>
                    <span class="info-number" style="background-color: #c8e6c9;">16</span>
                    <span class="info-number" style="background-color: #c8e6c9;">23</span>
                    <span class="info-number" style="background-color: #c8e6c9;">24</span>
                    <span class="info-number" style="background-color: #c8e6c9;">37</span>
                    <span class="info-number" style="background-color: #c8e6c9;">38</span>
                    <span class="info-number" style="background-color: #c8e6c9;">45</span>
                    <span class="info-number" style="background-color: #c8e6c9;">46</span>
                </div>
            </div>
            <div class="info-item" style="background-color: #e3f2fd; border-left: 3px solid #2196f3;">
                <div class="info-item-title">水</div>
                <div class="info-numbers">
                    <span class="info-number" style="background-color: #bbdefb;">13</span>
                    <span class="info-number" style="background-color: #bbdefb;">14</span>
                    <span class="info-number" style="background-color: #bbdefb;">21</span>
                    <span class="info-number" style="background-color: #bbdefb;">22</span>
                    <span class="info-number" style="background-color: #bbdefb;">29</span>
                    <span class="info-number" style="background-color: #bbdefb;">30</span>
                    <span class="info-number" style="background-color: #bbdefb;">43</span>
                    <span class="info-number" style="background-color: #bbdefb;">44</span>
                </div>
            </div>
            <div class="info-item" style="background-color: #ffebee; border-left: 3px solid #f44336;">
                <div class="info-item-title">火</div>
                <div class="info-numbers">
                    <span class="info-number" style="background-color: #ffcdd2;">01</span>
                    <span class="info-number" style="background-color: #ffcdd2;">02</span>
                    <span class="info-number" style="background-color: #ffcdd2;">09</span>
                    <span class="info-number" style="background-color: #ffcdd2;">10</span>
                    <span class="info-number" style="background-color: #ffcdd2;">17</span>
                    <span class="info-number" style="background-color: #ffcdd2;">18</span>
                    <span class="info-number" style="background-color: #ffcdd2;">31</span>
                    <span class="info-number" style="background-color: #ffcdd2;">32</span>
                    <span class="info-number" style="background-color: #ffcdd2;">39</span>
                    <span class="info-number" style="background-color: #ffcdd2;">40</span>
                    <span class="info-number" style="background-color: #ffcdd2;">47</span>
                    <span class="info-number" style="background-color: #ffcdd2;">48</span>
                </div>
            </div>
            <div class="info-item" style="background-color: #efebe9; border-left: 3px solid #795548;">
                <div class="info-item-title">土</div>
                <div class="info-numbers">
                    <span class="info-number" style="background-color: #d7ccc8;">05</span>
                    <span class="info-number" style="background-color: #d7ccc8;">06</span>
                    <span class="info-number" style="background-color: #d7ccc8;">19</span>
                    <span class="info-number" style="background-color: #d7ccc8;">20</span>
                    <span class="info-number" style="background-color: #d7ccc8;">27</span>
                    <span class="info-number" style="background-color: #d7ccc8;">28</span>
                    <span class="info-number" style="background-color: #d7ccc8;">35</span>
                    <span class="info-number" style="background-color: #d7ccc8;">36</span>
                    <span class="info-number" style="background-color: #d7ccc8;">49</span>
                </div>
            </div>
        </div>
    </div>
    
    <div class="property-section">
        <div class="info-title">生肖属性</div>
        <div class="property-grid">
            <div class="property-item">
                <div class="property-title">家禽</div>
                <div class="property-content">
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">鸡</span>
                    <span class="property-zodiac">狗</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">野兽</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">猴</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">吉美</div>
                <div class="property-content">
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">鸡</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">凶丑</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">猴</span>
                    <span class="property-zodiac">狗</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">阴性</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">狗</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">阳性</div>
                <div class="property-content">
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">猴</span>
                    <span class="property-zodiac">鸡</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">单笔</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">鸡</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">双笔</div>
                <div class="property-content">
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">猴</span>
                    <span class="property-zodiac">狗</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">天肖</div>
                <div class="property-content">
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">猴</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">地肖</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">鸡</span>
                    <span class="property-zodiac">狗</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">藏中</div>
                <div class="property-content">
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">猴</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">白边</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">鸡</span>
                    <span class="property-zodiac">狗</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">女肖</div>
                <div class="property-content">
                    <span class="property-zodiac">兔</span>
                    <span class="property-zodiac">蛇</span>
                    <span class="property-zodiac">羊</span>
                    <span class="property-zodiac">鸡</span>
                    <span class="property-zodiac">猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">男肖</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠</span>
                    <span class="property-zodiac">牛</span>
                    <span class="property-zodiac">虎</span>
                    <span class="property-zodiac">龙</span>
                    <span class="property-zodiac">马</span>
                    <span class="property-zodiac">猴</span>
                    <span class="property-zodiac">狗</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">三合</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠龙猴</span>
                    <span class="property-zodiac">牛蛇鸡</span>
                    <span class="property-zodiac">虎马狗</span>
                    <span class="property-zodiac">兔羊猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">六合</div>
                <div class="property-content">
                    <span class="property-zodiac">鼠牛</span>
                    <span class="property-zodiac">龙鸡</span>
                    <span class="property-zodiac">虎猪</span>
                    <span class="property-zodiac">蛇猴</span>
                    <span class="property-zodiac">兔狗</span>
                    <span class="property-zodiac">马羊</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">琴棋书画</div>
                <div class="property-content">
                    <span class="property-zodiac">琴:兔蛇鸡</span>
                    <span class="property-zodiac">棋:鼠牛狗</span>
                    <span class="property-zodiac">书:虎龙马</span>
                    <span class="property-zodiac">画:羊猴猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">四季</div>
                <div class="property-content">
                    <span class="property-zodiac">春:虎兔龙</span>
                    <span class="property-zodiac">夏:蛇马羊</span>
                    <span class="property-zodiac">秋:猴鸡狗</span>
                    <span class="property-zodiac">冬:鼠牛猪</span>
                </div>
            </div>
            <div class="property-item">
                <div class="property-title">红蓝绿肖</div>
                <div class="property-content">
                    <span class="property-zodiac">红:马兔鼠鸡</span>
                    <span class="property-zodiac">蓝:蛇虎猪猴</span>
                    <span class="property-zodiac">绿:羊龙牛狗</span>
                </div>
            </div>
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
            '虎': [4, 16, 28, 40],
            '猪': [7, 19, 31, 43],
            '猴': [10, 22, 34, 46],
            '龙': [2, 14, 26, 38],
            '牛': [5, 17, 29, 41],
            '狗': [8, 20, 32, 44],
            '羊': [11, 23, 35, 47],
            '兔': [3, 15, 27, 39],
            '鼠': [6, 18, 30, 42],
            '鸡': [9, 21, 33, 45],
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
        
        // 初始化号码球
        const numbersGrid = document.getElementById('numbersGrid');
        const selectedNumbers = document.getElementById('selectedNumbers');
        const hiddenNumbers = document.getElementById('hiddenNumbers');
        const generatedNumbers = document.getElementById('generatedNumbers');
        const zodiacGrid = document.getElementById('zodiacGrid');
        
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
            
            // 单击选择/取消选择
            numberItem.addEventListener('click', function() {
                const num = parseInt(this.dataset.number);
                if (selected.includes(num)) {
                    selected = selected.filter(n => n !== num);
                    this.classList.remove('selected');
                } else {
                    selected.push(num);
                    this.classList.add('selected');
                }
                updateSelectedNumbers();
            });
            
            // 双击隐藏/显示
            numberItem.addEventListener('dblclick', function() {
                const num = parseInt(this.dataset.number);
                if (hidden.includes(num)) {
                    hidden = hidden.filter(n => n !== num);
                    this.classList.remove('hidden');
                } else {
                    hidden.push(num);
                    this.classList.add('hidden');
                }
                updateHiddenNumbers();
            });
            
            numbersGrid.appendChild(numberItem);
        }
        
        // 创建生肖对照表
        for (const [zodiac, numbers] of Object.entries(zodiacMap)) {
            const zodiacItem = document.createElement('div');
            zodiacItem.className = 'zodiac-item';
            
            let conflict = '';
            if (zodiac === '蛇') conflict = '[冲猪]';
            else if (zodiac === '虎') conflict = '[冲猴]';
            else if (zodiac === '猪') conflict = '[冲蛇]';
            else if (zodiac === '猴') conflict = '[冲虎]';
            else if (zodiac === '龙') conflict = '[冲狗]';
            else if (zodiac === '牛') conflict = '[冲羊]';
            else if (zodiac === '狗') conflict = '[冲龙]';
            else if (zodiac === '羊') conflict = '[冲牛]';
            else if (zodiac === '兔') conflict = '[冲鸡]';
            else if (zodiac === '鼠') conflict = '[冲马]';
            else if (zodiac === '鸡') conflict = '[冲兔]';
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
                const shuffled = [...availableNumbers].sort(() => Math.random() - 0.5);
                
                // 取前三个号码
                const group = shuffled.slice(0, 3).sort((a, b) => a - b);
                
                // 创建显示元素
                const groupElement = document.createElement('div');
                groupElement.className = 'generated-item';
                groupElement.textContent = `${group.map(num => num.toString().padStart(2, '0')).join(',')} 三中三2`;
                
                generatedNumbers.appendChild(groupElement);
            }
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
        
        // 事件监听
        document.getElementById('generate5').addEventListener('click', () => generateGroups(5));
        document.getElementById('generate10').addEventListener('click', () => generateGroups(10));
        document.getElementById('copySelected').addEventListener('click', copySelectedNumbers);
        document.getElementById('copyGroups').addEventListener('click', copyGeneratedGroups);
        
        // 初始化显示
        updateSelectedNumbers();
        updateHiddenNumbers();
        
        // 检测是否支持添加到主屏幕
        window.addEventListener('beforeinstallprompt', (e) => {
            // 防止默认提示
            e.preventDefault();
            console.log('可以安装为应用');
        });
    </script>
</body>
</html>
