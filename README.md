
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>英単語学習アプリ - レッスン別</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            max-width: 700px;
            width: 90%;
            min-height: 500px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .title {
            font-size: 28px;
            color: #333;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .stats {
            display: flex;
            justify-content: space-around;
            margin-bottom: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
        }

        .stat-item {
            text-align: center;
        }

        .stat-number {
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
            display: block;
        }

        .stat-label {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
        }

        .menu {
            display: flex;
            flex-direction: column;
            gap: 15px;
            position: relative;
            z-index: 1;
        }

        .menu-button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 20px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            position: relative;
            z-index: 2;
        }

        .menu-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        .lesson-selector {
            display: none;
        }

        .lesson-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .lesson-card {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 15px;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            position: relative;
        }

        .lesson-card:hover {
            border-color: #667eea;
            background: #e7f3ff;
            transform: translateY(-2px);
        }

        .lesson-card.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .lesson-card.completed {
            border-color: #28a745;
            background: #d4edda;
        }

        .lesson-card.completed::after {
            content: "✓";
            position: absolute;
            top: 10px;
            right: 15px;
            color: #28a745;
            font-weight: bold;
            font-size: 18px;
        }

        .lesson-number {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .lesson-title {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .lesson-info {
            font-size: 12px;
            color: #666;
            margin-bottom: 10px;
        }

        .lesson-card.selected .lesson-info {
            color: #e9ecef;
        }

        .lesson-progress {
            width: 100%;
            height: 4px;
            background: #e0e0e0;
            border-radius: 2px;
            overflow: hidden;
            margin-top: 10px;
        }

        .lesson-progress-fill {
            height: 100%;
            background: #667eea;
            transition: width 0.3s ease;
        }

        .lesson-card.completed .lesson-progress-fill {
            background: #28a745;
        }

        .quiz-container {
            display: none;
        }

        .quiz-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .lesson-info-header {
            font-size: 14px;
            color: #667eea;
            font-weight: 600;
        }

        .progress-bar {
            width: 60%;
            height: 8px;
            background: #e0e0e0;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            width: 0%;
            transition: width 0.3s ease;
        }

        .question-container {
            text-align: center;
            margin-bottom: 30px;
        }

        .question-text {
            font-size: 32px;
            font-weight: bold;
            color: #333;
            margin-bottom: 10px;
        }

        .question-type {
            font-size: 14px;
            color: #666;
            margin-bottom: 20px;
        }

        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .option {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            padding: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 16px;
        }

        .option:hover {
            background: #e9ecef;
            border-color: #667eea;
        }

        .option.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .option.correct {
            background: #28a745;
            color: white;
            border-color: #28a745;
        }

        .option.incorrect {
            background: #dc3545;
            color: white;
            border-color: #dc3545;
        }

        .quiz-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: #667eea;
            color: white;
        }

        .btn-primary:hover {
            background: #5a6fd8;
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
        }

        .btn-secondary:hover {
            background: #5a6268;
        }

        .result-container {
            display: none;
            text-align: center;
        }

        .result-score {
            font-size: 48px;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 20px;
        }

        .result-text {
            font-size: 20px;
            color: #333;
            margin-bottom: 30px;
        }

        .mistakes-section {
            background: #fff3cd;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .mistakes-title {
            font-size: 18px;
            font-weight: bold;
            color: #856404;
            margin-bottom: 15px;
        }

        .mistake-item {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            margin-bottom: 10px;
            background: white;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
        }

        .hidden {
            display: none;
        }

        .mode-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .mode-button {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            padding: 20px;
            cursor: pointer;
            text-align: center;
            transition: all 0.3s ease;
        }

        .mode-button:hover {
            border-color: #667eea;
            background: #e7f3ff;
        }

        .mode-button.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .mode-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .mode-description {
            font-size: 14px;
            color: #666;
        }

        .mode-button.selected .mode-description {
            color: #e9ecef;
        }

        .back-button {
            background: #6c757d;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            margin-bottom: 20px;
        }

        .back-button:hover {
            background: #5a6268;
        }

        .typing-container {
            text-align: center;
            margin-bottom: 30px;
        }

        .typing-input {
            width: 100%;
            max-width: 400px;
            padding: 15px 20px;
            font-size: 18px;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            outline: none;
            transition: all 0.3s ease;
            margin-bottom: 15px;
        }

        .typing-input:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .typing-input.correct {
            border-color: #28a745;
            background-color: #d4edda;
        }

        .typing-input.incorrect {
            border-color: #dc3545;
            background-color: #f8d7da;
        }

        .typing-feedback {
            font-size: 16px;
            font-weight: 600;
            min-height: 24px;
            margin-bottom: 15px;
        }

        .typing-feedback.correct {
            color: #28a745;
        }

        .typing-feedback.incorrect {
            color: #dc3545;
        }

        .mistakes-list-view {
            display: none;
        }

        .mistakes-quiz-item {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .mistakes-quiz-item:hover {
            border-color: #667eea;
            background: #e7f3ff;
        }

        .mistakes-quiz-item.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .mistake-question {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .mistake-details {
            font-size: 14px;
            color: #666;
        }

        .mistakes-quiz-item.selected .mistake-details {
            color: #e9ecef;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1 class="title">英単語学習アプリ</h1>
            <div class="stats">
                <div class="stat-item">
                    <span class="stat-number" id="totalWords">0</span>
                    <div class="stat-label">学習済み</div>
                </div>
                <div class="stat-item">
                    <span class="stat-number" id="correctRate">0%</span>
                    <div class="stat-label">正解率</div>
                </div>
                <div class="stat-item">
                    <span class="stat-number" id="completedLessons">0</span>
                    <div class="stat-label">完了レッスン</div>
                </div>
            </div>
        </div>

        <div id="mainMenu" class="menu">
            <button class="menu-button" onclick="showLessonSelector()">レッスン選択</button>
            <button class="menu-button" onclick="showModeSelection('test')">全レッスンテスト</button>
            <button class="menu-button" onclick="showMistakesList()" id="mistakesButton">間違いリスト (0)</button>
        </div>

        <div id="lessonSelector" class="lesson-selector">
            <button class="back-button" onclick="showMainMenu()">← メニューに戻る</button>
            <div class="lesson-grid" id="lessonGrid"></div>
        </div>

        <div id="modeSelection" class="hidden">
            <button class="back-button" onclick="goBackFromMode()">← 戻る</button>
            <div class="mode-selector">
                <div class="mode-button" onclick="selectMode('enToJp')">
                    <div class="mode-title">英語 → 日本語</div>
                    <div class="mode-description">英単語を見て日本語を選択</div>
                </div>
                <div class="mode-button" onclick="selectMode('jpToEn')">
                    <div class="mode-title">日本語 → 英語</div>
                    <div class="mode-description">日本語を見て英単語を選択</div>
                </div>
                <div class="mode-button" onclick="selectMode('typing')">
                    <div class="mode-title">タイピング</div>
                    <div class="mode-description">日本語を見て英単語をタイピング</div>
                </div>
            </div>

            <div class="quiz-buttons">
                <button class="btn btn-primary" onclick="startQuiz()">開始</button>
            </div>
        </div>

        <div id="mistakesListView" class="mistakes-list-view">
            <button class="back-button" onclick="showMainMenu()">← メニューに戻る</button>
            <button class="clear-mistakes-btn" onclick="clearMistakes()">リストを消去</button>
            <h2 style="margin-bottom: 20px;">間違いリスト</h2>
            <div id="mistakesListContainer"></div>
            <div class="quiz-buttons" style="margin-top: 20px;">
                <button class="btn btn-primary" id="startMistakesQuizBtn" onclick="startMistakesQuiz()" style="display: none;">選択した問題をテスト</button>
            </div>
        </div>

        <div id="quizContainer" class="quiz-container">
            <div class="quiz-header">
                <button class="btn btn-secondary" onclick="goBackFromQuiz()">戻る</button>
                <div>
                    <div class="lesson-info-header" id="currentLessonInfo"></div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="progressFill"></div>
                    </div>
                </div>
                <span id="questionCounter">1/10</span>
            </div>

            <div class="question-container">
                <div class="question-text" id="questionText"></div>
                <div class="question-type" id="questionType"></div>
            </div>

            <div class="options" id="optionsContainer"></div>
            <div id="typingContainer" class="typing-container" style="display: none;">
                <input type="text" id="typingInput" class="typing-input" placeholder="英単語を入力してください" />
                <div class="typing-feedback" id="typingFeedback"></div>
            </div>

            <div class="quiz-buttons">
                <button class="btn btn-primary" id="nextButton" onclick="nextQuestion()" style="display: none;">次の問題</button>
            </div>
        </div>

        <div id="resultContainer" class="result-container">
            <div class="result-score" id="resultScore">0/10</div>
            <div class="result-text" id="resultText"></div>
            
            <div id="mistakesSection" class="mistakes-section" style="display: none;">
                <div class="mistakes-title">間違えた問題</div>
                <div id="mistakesList"></div>
            </div>

            <div class="quiz-buttons">
                <button class="btn btn-primary" onclick="showMainMenu()">メニューに戻る</button>
                <button class="btn btn-secondary" onclick="restartQuiz()">もう一度</button>
            </div>
        </div>
    </div>

    <script>
        // レッスン別の単語データ
        const lessons = [
        {
                id: 1,
                title: "p16~20",
                theme: "ターゲット①",
                words: [
                    { english: "change", japanese: "を変える；変わる",type:"word" },
                    { english: "learn", japanese: "(を)学ぶ；を身につける",type:"word" },
                    { english: "help", japanese: "(人)を手伝う、助ける",type:"word" },
                    { english: "need", japanese: "を必要とする",type:"word" },
                    { english: "live", japanese: "住んでいる；生きる；暮らす",type:"word" },
                    { english: "ask", japanese: "に頼む；に尋ねる",type:"word" },
                    { english: "enjoy", japanese: "を楽しむ" ,type:"word"},
                    { english: "wait", japanese: "待つ",type:"word" },
                    { english: "cook", japanese: "(を調理する),(食事)を作る",type:"word" },
                    { english: "talk", japanese: "話す",type:"word" },
                    { english: "speak", japanese: "(を)話す" ,type:"word"},
                    { english: "meet", japanese: "(に)会う",type:"word" },
                    { english: "mean", japanese: "のことを指して言う；を意味する" ,type:"word"},
                    { english: "buy", japanese: "を買う",type:"word" },
                    { english: "travel", japanese: "旅行する；(人・乗り物などが)行く,進む",type:"word" },
                    { english: "build", japanese: "を建てる,建設する,作る",type:"word" },
                    { english: "close", japanese: "を閉じる；閉まる",type:"word" },
                    { english: "stay", japanese: "滞在する；とどまる；(ある状態)のままでいる",type:"word" },
                    { english: "move", japanese: "を動かす；動く；引っ越す；を感動させる",type:"word" },
                    { english: "plan", japanese: "を計画する",type:"word" },
                ]
            },
            {
                id: 2,
                title: "p22~26",
                theme: "ターゲット①",
                words: [
                    { english: "write", japanese: "(を)書く",type:"word" },
                    { english: "listen", japanese: "(意識して)聞く",type:"word" },
                    { english: "happen", japanese: "起こる,生じる",type:"word" },
                    { english: "lose", japanese: "を失う,なくす；に負ける",type:"word" },
                    { english: "stand", japanese: "立つ,立っている",type:"word" },
                    { english: "grow", japanose: "育つ；を栽培する；増大する",type:"word" },
                    { english: "sound", japanese: "〜(のように)聞こえる,思える" ,type:"word"},
                    { english: "rain", japanese: "雨が降る",type:"word" },
                    { english: "worry", japanese: "心配する；を心配させる",type:"word" },
                    { english: "teach", japanese: "(を)教える",type:"word" },
                    { english: "hope", japanese: "を望む,期待する" ,type:"word"},
                    { english: "hold", japanese: "を持つ,抱える；を保つ；(会合など)を催す" ,type:"word"},
                    { english: "life", japanese: "一生,生涯；人生；(日常の)生活；命",type:"word" },
                    { english: "thing", japanese: "事；物",type:"word" },
                    { english: "country", japanese: "国；<the~>田舎",type:"word" },
                    { english: "example", japanese: "例,実例",type:"word" },
                    { english: "place", japanese: "場所,所；順位",type:"word" },
                    { english: "part", japanese: "部分；役；役割",type:"word" },
                    { english: "trip", japanese: "旅行" ,type:"word"},
                    { english: "problem", japanese: "問題",type:"word" },
                    { english: "question", japanese: "質問" ,type:"word"},
                    { english: "color", japanese: "色",type:"word" },
                    { english: "point", japanese: "要点；点；得点",type:"word" },
                ]
            },
            {
                id: 3,
                title: "p28~30",
                theme: "ターゲット①",
                words: [
                    { english: "language", japanese: "言語",type:"word" },
                    { english: "word", japanese: "単語,語；言葉",type:"word" },
                    { english: "health", japanese: "健康(状態)",type:"word" },
                    { english: "report", japanese: "報告(書),レポー,ト；報道",type:"word" },
                    { english: "minute", japanese: "(時間の)分；少しの間" ,type:"word"},
                    { english: "reason", japanose: "理由" ,type:"word"},
                    { english: "line", japanese: "路線；線；列,行列" ,type:"word"},
                    { english: "month", japanese: "(暦の)月",type:"word" },
                    { english: "week", japanese: "週",type:"word" },
                    { english: "date", japanese: "日にち；デート" ,type:"word"},
                    { english: "event", japanese: "行事；出来事",type:"word" },
                    { english: "future", japanese: "未来,将来",type:"word" },
                    { english: "design", japanese: "デザイン;設計図" ,type:"word"},
                    { english: "end", japanese: "終わり；端；目的",type:"word" },
                    { english: "computer", japanese: "コンピューター",type:"word" },
                    { english: "plant", japanese: "植物；(製造)工場,発電所",type:"word" },
                ]
            },
            {
                id: 4,
                title: "p32~34",
                theme: "ターゲット①",
                words: [
                    { english: "art", japanese: "芸術",type:"word" },
                    { english: "zhance", japanese: "機会,好機；可能性；偶然",type:"word" },
                    { english: "history", japanese: "歴史",type:"word" },
                    { english: "festival", japanese: "祭り" ,type:"word"},
                    { english: "season", japanese: "季節,時季",type:"word" },
                    { english: "fun", japanose: "楽しみ",type:"word" },
                    { english: "host", japanese: "(催しなどでもてなす側の)主人,主催者",type:"word" },
                    { english: "message", japanese: "伝言,メッセージ",type:"word" },
                    { english: "step", japanese: "段階；歩み,一歩；(階段の)段",type:"word" },
                    { english: "popular", japanese: "人気のある",type:"word" },
                    { english: "most", japanese: "大部分の,たいていの；<the most~>最も~な",type:"word" },
                    { english: "different", japanese: "違う,異なる" ,type:"word"},
                    { english: "such", japanese: "そのような",type:"word" },
                    { english: "last", japanese: "この前の；最後の",type:"word" },
                    { english: "same", japanese: "<the~>同じ,同様の；同一の" ,type:"word"},
                    { english: "great", japanese: "すばらしい;元気な；偉大な；大変な" ,type:"word"},
                    { english: "open", japanese: "開店[営業]している；開いている",type:"word" }
                ]
            },
            {
                id: 5,
                title: "p36~40",
                theme: "ターゲット②",
                words: [
                    { english: "own", japanese: "自分自身の；特有の",type:"word" },
                    { english: "kind", japanese: "親切な；優しい",type:"word" },
                    { english: "difficult", japanese: "難しい" ,type:"word"},
                    { english: "enough", japanese: "十分な" ,type:"word"},
                    { english: "apecial", japanese: "特別な",type:"word" },
                    { english: "famous", japanose: "有名な" ,type:"word"},
                    { english: "bad", japanese: "悪い",type:"word" },
                    { english: "short", japanese: "短い",type:"word" },
                    { english: "useful", japanese: "役に立つ",type:"word" },
                    { english: "afraid", japanese: "恐れて,怖がって",type:"word" },
                    { english: "favorite", japanese: "お気に入りの",type:"word" },
                    { english: "expensive", japanese: "高価な",type:"word" },
                    { english: "carry", japanese: "を運ぶ",type:"word" },
                    { english: "break", japanese: "壊れる,割れる；を壊す,を割る",type:"word" },
                    { english: "arrive", japanese: "到着する",type:"word" },
                    { english: "fall", japanese: "落ちる" ,type:"word"},
                    { english: "miss", japanese:"がいなくて[なくて]さみしく思う",type:"word" },
                    { english: "cover", japanese: "を覆う",type:"word" },
                    { english: "catch", japanese: "を捕まえる",type:"word" },
                    { english: "save", japanese: "を救う；を節約する",type:"word" },
                    { english: "check", japanese: "を点検する,確かめる",type:"word" },
                    { english: "introduce", japanese: "を紹介する；を導入する" ,type:"word"},
                    { english: "join", japanese: "(に)加わる；(集団・組織など)の一員になる",type:"word" },
                ]
            },
            {
                id: 6,
                title: "p42~46",
                theme: "ターゲット②",
                words: [
                    { english: "clean", japanese: "をきれいにする,清掃する",type:"word" },
                    { english: "answer", japanese: "(に)答える；(に)応答する" ,type:"word"},
                    { english: "throw", japanese: "を投げる",type:"word" },
                    { english: "invite", japanese: "を招待する,招く",type:"word" },
                    { english: "pick", japanese: "を摘み取る,つまみ取る；を選び出す",type:"word" },
                    { english: "die", japanose: "死ぬ；枯れる",type:"word" },
                    { english: "return", japanese: "戻る；お返す",type:"word" },
                    { english: "fly", japanese: "飛ぶ；飛行機で行く",type:"word" },
                    { english: "cut", japanese: "を切る",type:"word" },
                    { english: "hit", japanese: "(災害などが)を襲う；をたたく；を打つ",type:"word" },
                    { english: "excuse", japanese: "を許す",type:"word" },
                    { english: "wash", japanese: "を洗う" ,type:"word"},
                    { english: "cry", japanese: "(と)叫ぶ；泣く",type:"word" },
                    { english: "borrow", japanese: "を借りる",type:"word" },
                    { english: "kill", japanese: "を殺す",type:"word" },
                    { english: "push", japanese: "(を)押す",type:"word" },
                    { english: "climb", japanese: "(を)登る,よじ登る" ,type:"word"},
                    { english: "laugh", japanese: "(声を立てて)笑う" ,type:"word"},
                    { english: "smile", japanese: "ほほえむ",type:"word" },
                    { english: "hurry", japanese: "急ぐ；急いで行く",type:"word" },
                    { english: "cheer", japanese: "を元気づける；(に)喝采を送る",type:"word" },
                    { english: "volunteer", japanese: "ボランティア；志願者" ,type:"word"},
                    { english: "side", japanese: "側；わき；(対立する一方の)側" ,type:"word"},
                    { english: "front", japanese: "前,正面；前部",type:"word" },
                ]
            },
            {
                id: 7,
                title: "p48~54",
                theme: "ターゲット②",
                words: [
                    { english: "concert", japanese: "コンサート,演奏会",type:"word" },
                    { english: "fire", japanese: "火事；火" ,type:"word"},
                    { english: "village", japanese: "村",type:"word" },
                    { english: "lesson", japanese: "レッスンん；課；授業；教訓",type:"word" },
                    { english: "light", japanese: "明かり,証明；光",type:"word" },
                    { english: "Internet", japanose: "<the~>インターネット",type:"word" },
                    { english: "weather", japanese: "天気",type:"word" },
                    { english: "voice", japanese: "声；意見",type:"word" },
                    { english: "piece", japanese: "1つ,1枚；部品",type:"word" },
                    { english: "goal", japanese: "目標；ゴール；得点" ,type:"word"},
                    { english: "speech", japanese: "スピーチ,演説",type:"word" },
                    { english: "fan", japanese: "ファン；うちわ；換気扇",type:"word" },
                    { english: "dream", japanese: "夢",type:"word" },
                    { english: "mistake", japanese: "間違い,誤り",type:"word" },
                    { english: "meter", japanese: "メートル;(計量)メーター" ,type:"word"},
                    { english: "land", japanese: "土地；陸；国土",type:"word" },
                    { english: "hundred", japanese: "百",type:"word" },
                    { english: "thousand", japanese: "千",type:"word" },
                    { english: "million", japanese: "百万",type:"word" },
                    { english: "medicine", japanese: "薬；医学",type:"word" },
                    { english: "uniform", japanese: "制服",type:"word" },
                    { english: "heat", japanese: "暑さ；熱",type:"word" },
                    { english: "evening", japanese: "夕方,晩",type:"word" },
                    { english: "noon", japanese: "正午",type:"word" },
                    { english: "holiday", japanese: "休日,祝日",type:"word" },
                    { english: "course", japanese: "講座,課程；進路" ,type:"word"},
                    { english: "rule", japanese: "ルール,規則",type:"word" },
                    { english: "forest", japanese: "森林",type:"word" },
                    { english: "farm", japanese: "農場",type:"word" },
                    { english: "treasure", japanese: "大切な物,宝物",type:"word" },
                    { english: "hole", japanese: "穴",type:"word" },
                    { english: "cloud", japanese: "雲",type:"word" },
                    { english: "phone", japanese: "電話；電話機",type:"word" },

                ]
            },
            {
                id: 8,
                title: "p56~60",
                theme: "ターゲット③",
                words: [
                    { english: "sorry", japanese: "すまなく思って；残念[気の毒]に思って",type:"word" },
                    { english: "careful", japanese: "注意深い",type:"word" },
                    { english: "wonderful", japanese: "すばらしい",type:"word" },
                    { english: "heavy", japanese: "重い",type:"word" },
                    { english: "sick", japanese: "病気の；吐き気がする",type:"word" },
                    { english: "dear", japanese: "親愛なる",type:"word" },
                    { english: "glad", japanese: "うれしい",type:"word" },
                    { english: "dark", japanese: "暗い；(色が)濃い",type:"word" },
                    { english: "sad", japanese: "悲しい",type:"word" },
                    { english: "cute", japanese: "かわいい",type:"word" },
                    { english: "free", japanese: "暇な,時間のある；自由な；無料の",type:"word" },
                    { english: "foreign", japanese: "外国の" ,type:"word"},
                    { english: "low", japanese: "低い",type:"word" },
                    { english: "safe", japanese: "安全な" ,type:"word"},
                    { english: "angry", japanese: "怒った" ,type:"word"},
                    { english: "lucky", japanese: "幸運な",type:"word" },
                    { english: "bright", japanese: "輝いて",type:"word" },
                    { english: "soft", japanese: "柔らかい,明るい" ,type:"word"},
                    { english: "loud", japanese: "大きい" ,type:"word"},
                    { english: "even", japanese: "~でさえ；(比較級を強調して)さらに",type:"word" },
                    { english: "back", japanese: "戻って；後ろに" ,type:"word"},
                    { english: "still", japanese: "まだ,なお；それにもかかわらず",type:"word" },
                    { english: "early", japanese: "早く" ,type:"word"},
                    { english: "soon", japanese: "すぐに,まもなく",type:"word" },
                    
                ]
            },
            {
                id: 9,
                title: "p62~66",
                theme: "ターゲット③",
                words: [
                    { english: "away", japanese: "離れて,遠くへ" ,type:"word"},
                    { english: "almost", japanese: "ほとんど,ほぼ；もう少しで",type:"word" },
                    { english: "together", japanese: "一緒に" ,type:"word"},
                    { english: "maybe", japanese: "もしかすると,おそらく",type:"word" },
                    { english: "once", japanese: "1度[回]；かつて",type:"word" },
                    { english: "else", japanose: "ほかに",type:"word" },
                    { english: "ago", japanese: "(今から)〜前",type:"word" },
                    { english: "straight", japanese: "まっすぐに",type:"word" },
                    { english: "slowly", japanese: "ゆっくりと",type:"word" },
                    { english: "suddently", japanese: "突然,急に",type:"word" },
                    { english: "until", japanese: "⋯するまで",type:"word" },
                    { english: "since", japanese: "⋯したときから",type:"word" },
                    { english: "around", japanese: "〜のあちこちを；〜の周りに[を]",type:"word" },
                    { english: "over", japanese: "〜を超えて,越えて；〜の上[上方に]；〜じゅうを",type:"word" },
                    { english: "without", japanese: "〜なしに",type:"word" },
                    { english: "through", japanese: "〜の間じゅう；〜を通り抜けて；〜の至る所に[を]；(手段・原因)〜を通じて",type:"word" },
                    { english: "between", japanese: "〜の間に[を]",type:"word" },
                    { english: "during", japanese: "〜の間に；〜の間じゅう(ずっと)" ,type:"word"},
                    { english: "behind", japanese: "〜の後ろに；〜の背後に",type:"word" },
                    { english: "along", japanese: "〜に沿って" ,type:"word"},
                    
                ]
            },
            {
                id: 10,
                title: "p68~72",
                theme: "ターゲット③",
                words: [
                    { english: "come from~", japanese: "〜から来ている,〜に由来する；〜の出身である", type: "phrase" },
                    { english: "come true", japanese: "実現する" , type: "phrase"},
                    { english: "cut off~/cut~off", japanese: "〜を切り取る" , type: "phrase"},
                    { english: "do[try]one's best", japanese: "最善[全力]を尽くす", type: "phrase" },
                    { english: "get off(~)", japanese: "(〜(乗り物など)を)降りる", type: "phrase" },
                    { english: "get on(~)", japanose: "(〜(乗り物など)に)乗る" , type: "phrase"},
                    { english: "get to~", japanese: "〜(場所)に到着する,達する", type: "phrase" },
                    { english: "go through~", japanese: "〜を経験する；〜を通り抜ける", type: "phrase" },
                    { english: "grow up", japanese: "成長する,大人になる" , type: "phrase"},
                    { english: "hear of~", japanese: "〜のことを聞き知る,〜のうわさを聞く" , type: "phrase"},
                    { english: "help oneself(to~)", japanese: "(〜を)自由にとって食べる[飲む]", type: "phrase" },
                    { english: "look for~", japanese: "〜を探す", type: "phrase" },
                    { english: "look forward to~", japanese: "〜を楽しみに待つ", type: "phrase" },
                    { english: "look like~", japanese: "〜に似ている" , type: "phrase"},
                    { english: "pick up~/pick~up", japanese: "〜を拾い上げる；〜を引き取る；〜を迎えに行く[来る]" , type: "phrase"},
                    { english: "speak to[with]~", japanese: "〜と話す", type: "phrase" },
                    { english: "take care of~", japanese: "〜の世話をする；〜に気をつける", type: "phrase" },
                    { english: "take off~/take~off", japanese: "〜を脱ぐ；<take offで>離陸する", type: "phrase" },
                    { english: "take part in~", japanese: "〜参加する", type: "phrase" },
                    { english: "think of~", japanese: "〜を思いつく；〜のことを考える", type: "phrase" },
                    { english: "after school", japanese: "放課後に" , type: "phrase"},
                    { english: "all over(~)", japanese: "(〜の)至る所に[で]", type: "phrase" },
                    { english: "all the time", japanese: "いつ(で)も；その間ずっと" , type: "phrase"},
                    { english: "at first", japanese: "最初は,初めのうちは" , type: "phrase"},
                    { english: "at home", japanese: "在宅して；くつろいで" , type: "phrase"},
                    { english: "at last", japanese: "ついに,やっと" , type: "phrase"},
                    { english: "at that time", japanese: "その時に(は),当時(は)", type: "phrase" },
                    { english: "at the same time", japanese: "同時に", type: "phrase" },
                    { english: "for a long time", japanese: "長い間", type: "phrase" },
                    { english: "for the first time", japanese: "初めて", type: "phrase" },
                    { english: "in the end", japanese: "結局；最後に", type: "phrase" },
                    { english: "in the future", japanese: "将来" , type: "phrase"},
                    { english: "in this way", japanese: "このようにして", type: "phrase" },
                ]
            },
            {
                id: 11,
                title: "p74~76",
                theme: "ターゲット③",
                words: [
                    { english: "more than~", japanese: "〜より多い,〜以上", type: "phrase" },
                    { english: "of course", japanese: "もちろん", type: "phrase" },
                    { english: "on one's[the]way(to~)", japanese: "(〜に行く)途中で", type: "phrase" },
                    { english: "over there", japanese: "あそこに[で]" , type: "phrase"},
                    { english: "these days", japanese: "近ごろ(は)", type: "phrase" },
                    { english: "a kind[sort] of~", japanose: "一種の〜,〜のようなもの", type: "phrase" },
                    { english: "a lot of~/lots of~", japanese: "たくさんの〜", type: "phrase" },
                    { english: "A such as B", japanese: "BのようなA,A、例えばB", type: "phrase" },
                    { english: "and so on [forth]", japanese: "〜など" , type: "phrase"},
                    { english: "here is[are]~", japanese: "これが〜です,〜をどうぞ", type: "phrase" },
                    { english: "How about~?", japanese: "〜（して）はいかがですか", type: "phrase" },
                    { english: "no longer~", japanese: "もはや〜ない", type: "phrase" },
                    { english: "not only A but (also) B", japanese: "AだけでなくBも", type: "phrase" },
                    { english: "so ~ that ...", japanese: "とても〜なので⋯", type: "phrase" },
                    { english: "too ~ to do", japanese: "あまりに〜なので⋯できない" , type: "phrase"},
                    { english: "used to do[be~]", japanese: "以前はよく⋯した[〜であった]" , type: "phrase"},
                    { english: "would like[love] to do", japanese: "⋯したいと思う", type: "phrase" },
                ]
            },
            {
                id: 12,
                title: "p78-1",
                theme: "ターゲット③",
                words: [
                    { english: "brain", japanese: "脳",type:"word" },
                    { english: "head", japanese: "頭" ,type:"word"},
                    { english: "face", japanese: "顔" ,type:"word"},
                    { english: "eye", japanese: "目",type:"word" },
                    { english: "nose", japanese: "鼻",type:"word" },
                    { english: "ear", japanose: "耳",type:"word" },
                    { english: "mouth", japanese: "口",type:"word" },
                    { english: "neck", japanese: "首" ,type:"word"},
                    { english: "throat", japanese: "喉" ,type:"word"},
                    { english: "shoulder", japanese: "肩",type:"word" },
                    { english: "chest", japanese: "胸",type:"word" },
                    { english: "elbow", japanese: "肘",type:"word" },
                    { english: "lung", japanese: "肺",type:"word" },
                    { english: "heart", japanese: "心臓",type:"word" },
                    { english: "arm", japanese: "腕",type:"word" },
                    { english: "liver", japanese: "肝臓" ,type:"word"},
                    { english: "waist", japanese: "ウエスト",type:"word" },
                    { english: "stomach", japanese: "胃",type:"word" },
                    { english: "intestine", japanese: "腸",type:"word" },
                    { english: "hand", japanese: "頭" ,type:"word"},
                    { english: "thigh", japanese: "太もも",type:"word" },
                    { english: "knee", japanese: "膝",type:"word" },
                    { english: "leg", japanese: "脚",type:"word" },
                    { english: "foot", japanese: "足" ,type:"word"},
                    { english: "ankle", japanese: "足首",type:"word" },
                    { english: "toe", japanese: "つま先",type:"word" },
                    { english: "heel", japanese: "かかと",type:"word" },
                    { english: "sole", japanese: "足の裏",type:"word" },
                ]
            },
            {
                id: 13,
                title: "p78-2",
                theme: "ターゲット③",
                words: [
                    { english: "hair", japanese: "髪の毛" ,type:"word"},
                    { english: "forehead", japanese: "額",type:"word" },
                    { english: "eyebrow", japanese: "眉",type:"word" },
                    { english: "eyelid", japanese: "まぶた",type:"word" },
                    { english: "eyelash", japanese: "まつ毛",type:"word" },
                    { english: "pupil", japanose: "瞳",type:"word" },
                    { english: "teeth", japanese: "歯",type:"word" },
                    { english: "lips", japanese: "唇",type:"word" },
                    { english: "chin", japanese: "あご",type:"word" },
                    { english: "thumb", japanese: "親指" ,type:"word"},
                    { english: "nail", japanese: "爪",type:"word" },
                    { english: "index finger", japanese: "人差し指",type:"word" },
                    { english: "middle finger", japanese: "中指",type:"word" },
                    { english: "ring finger", japanese: "薬指",type:"word" },
                    { english: "little finger", japanese: "小指",type:"word" },
                    { english: "palm", japanese: "手のひら",type:"word" },
                    { english: "wrist", japanese: "手首",type:"word" },
                ]
            },
            {
                id: 14,
                title: "p80~84",
                theme: "ターゲット④",
                words: [
                    { english: "create", japanese: "を創造する" ,type:"word"},
                    { english: "base", japanese: "の基礎を置く",type:"word" },
                    { english: "repair", japanese: "を修理[修復]する",type:"word" },
                    { english: "fail", japanese: "失敗する；(に)不合格になる",type:"word" },
                    { english: "accept", japanese: "を受け入れる" ,type:"word"},
                    { english: "belong", japanose: "属する",type:"word" },
                    { english: "exchange", japanese: "を交換する；を取り替える；を両替する" ,type:"word"},
                    { english: "complete", japanese: "を完成させる；終える",type:"word" },
                    { english: "treat", japanese: "を扱う；を治療する",type:"word" },
                    { english: "cross", japanese: "(を)横切る；(と)交差する",type:"word" },
                    { english: "hide", japanese: "を隠す；隠れる",type:"word" },
                    { english: "shake", japanese: "を振る；揺れる",type:"word" },
                    { english: "challenge", japanese: "に挑戦する",type:"word" },
                    { english: "connect", japanese: "をつなぐ,つながる；を関連づける",type:"word" },
                    { english: "reply", japanese: "返事をする；応じる；と答える" ,type:"word"},
                    { english: "beat", japanese: "を打ち明かす；(を)(連続して)打つ,たたく；(を)強くかき混ぜる",type:"word" },
                    { english: "share", japanese: "を分かち合う；を共有する,シェアする" ,type:"word"},
                    { english: "observe", japanese: "を観察する；に気づく" ,type:"word"},
                    { english: "mark", japanese: "にしるしをつける；(〜の日)にあたる" ,type:"word"},
                    { english: "burn", japanese: "を焦がす,焦げる；を燃やす,燃える；やけどする,日焼けする",type:"word" },
                    { english: "locate", japanese: "<be locatedで>位置している；(場所・物など)を特定する",type:"word" },
                    { english: "fix", japanese: "を修理する；を固定する；を(明確に)決める",type:"word" },
                ]
            },
            {
                id: 15,
                title: "p86~90",
                theme: "ターゲット④",
                words: [
                    { english: "suit", japanese: "に最適[好都合]である；(人)に似合う",type:"word" },
                    { english: "destroy", japanese: "を破壊する",type:"word" },
                    { english: "control", japanese: "を抑制する,制御する；を支配する,管理する" ,type:"word"},
                    { english: "respond", japanese: "返答する；反応する；だと応答する",type:"word" },
                    { english: "depend", japanese: "当てにする,頼る；〜次第である",type:"word" },
                    { english: "forgive", japanose: "を許す",type:"word" },
                    { english: "attack", japanese: "を襲う；を攻撃する",type:"word" },
                    { english: "sink", japanese: "沈む" ,type:"word"},
                    { english: "appreciate", japanese: "にかんしゃする；(〜のよさを)認める",type:"word" },
                    { english: "feed", japanese: "に食べ物[えさ・肥料]を与える",type:"word" },
                    { english: "success", japanese: "成功したこと[人]；成功",type:"word" },
                    { english: "mystery", japanese: "謎,未知のこと；神秘；推理小説" ,type:"word"},
                    { english: "ceremony", japanese: "式典,儀式",type:"word" },
                    { english: "schedule", japanese: "予定(表)；時刻表；時間割",type:"word" },
                    { english: "damage", japanese: "損害,悪影響" ,type:"word"},
                    { english: "model", japanese: "型；モデル；模型；模範",type:"word" },
                    { english: "search", japanese: "捜索；(データの)検索",type:"word" },
                    { english: "project", japanese: "計画；事業",type:"word" },
                    { english: "form", japanese: "形態；形；用紙",type:"word" },
                    { english: "scene", japanese: "場面；現場；光景",type:"word" },
                    { english: "accident", japanese: "事故；偶然" ,type:"word"},
                    { english: "contact", japanese: "連絡；接触" ,type:"word"},
                ]
            },
            {
                id: 16,
                title: "p92~96",
                theme: "ターゲット④",
                words: [
                    { english: "image", japanese: "イメージ,印象；像,画像",type:"word" },
                    { english: "trust", japanese: "信頼,信用",type:"word" },
                    { english: "quality", japanese: "質,品質；<~ties>(人の)資質",type:"word" },
                    { english: "action", japanese: "行動；行為",type:"word" },
                    { english: "lack", japanese: "不足,ないこと",type:"word" },
                    { english: "spot", japanose: "場所；斑点；汚点" ,type:"word"},
                    { english: "truth", japanese: "真実,本当のこと" ,type:"word"},
                    { english: "effort", japanese: "努力；苦労",type:"word" },
                    { english: "type", japanese: "型,タイプ<one's~>好みのタイプの人",type:"word" },
                    { english: "site", japanese: "敷地,土地；サイト",type:"word" },
                    { english: "tool", japanese: "手段；道具",type:"word" },
                    { english: "couple", japanese: "2つ,2人；1対,1組；カップル,夫婦",type:"word" },
                    { english: "hero", japanese: "ヒーロー,英雄；(男性の)主人公",type:"word" },
                    { english: "courage", japanese: "勇気",type:"word" },
                    { english: "board", japanese: "板,掲示板；委員会",type:"word" },
                    { english: "purpose", japanese: "目的；意図",type:"word" },
                    { english: "waste", japanese: "無駄,浪費；ごみ,廃棄物" ,type:"word"},
                    { english: "shape", japanese: "形；状態；体調" ,type:"word"},
                    { english: "technique", japanese: "技能,技術",type:"word" },
                    { english: "middle", japanese: "<the~>真ん中,中央；中間,最中",type:"word" },
                    { english: "spirit", japanese: "精神,心；魂；<~s>気分",type:"word" },
                    { english: "partner", japanese: "パートナー；仲間；配偶者,同棲者" ,type:"word"},
                    { english: "poopulation", japanese: "人口",type:"word" },
                    { english: "fever", japanese: "熱,発熱；興奮；発狂",type:"word" },
                ]
            },
            {
                id: 17,
                title: "p98",
                theme: "ターゲット④",
                words: [
                    { english: "method", japanese: "方法",type:"word" },
                    { english: "structure", japanese: "構造；建造物",type:"word" },
                    { english: "background", japanese: "経歴,生い立ち；背景事情；背景" ,type:"word"},
                    { english: "combination", japanese: "組み合わせ,結合" ,type:"word"},
                    { english: "official", japanese: "公式の；公の,職務(上)の",type:"word" },
                    { english: "flat", japanese: "平らな,起伏のない；空気の抜けた",type:"word" },
                    { english: "serious", japanese: "申告な；まじめな；本気の",type:"word" },
                    { english: "ordinary", japanese: "普通の；ありふれた" ,type:"word"},
                    
                ]
            },
            {
                id: 18,
                title: "p100~108",
                theme: "ターゲット⑤",
                words: [
                    { english: "private", japanese: "私的な,個人的な；私立の；秘密の",type:"word" },
                    { english: "major", japanese: "重大な；主要な；大きな方の",type:"word" },
                    { english: "classical", japanese: "クラシックの；古典的な",type:"word" },
                    { english: "honest", japanese: "正直な；率直な",type:"word" },
                    { english: "excellent", japanese: "とても優れた；すばらしい；(承諾の返答で)大変結構だ" ,type:"word"},
                    { english: "whole", japanese: "全体の",type:"word" },
                    { english: "central", japanese: "中心(部)の；中心的な" ,type:"word"},
                    { english: "ancient", japanese: "古代の",type:"word" },
                    { english: "fantastic", japanese: "とてもすばらしい；空想的な",type:"word" },
                    { english: "regular", japanese: "定期的な；規則正しい；通常の；普通サイズの",type:"word" },
                    { english: "basic", japanese: "基本的な,初歩的な；重要な,基礎となる",type:"word" },
                    { english: "huge", japanese: "巨大な；莫大な",type:"word" },
                    { english: "empty", japanese: "空の；空いている",type:"word" },
                    { english: "smart", japanese: "頭のよい,賢い",type:"word" },
                    { english: "general", japanese: "大まかな；一般的な；全般的な",type:"word" },
                    { english: "single", japanese: "たった1つ[1人]の；個々の；独身の",type:"word" },
                    { english: "responsible", japanese: "責任のある",type:"word" },
                    { english: "fresh", japanese: "新鮮な,できたての；斬新な；すがすがしい",type:"word" },
                    { english: "familiar", japanese: "熟知している；よく知られている；親しい",type:"word" },
                    { english: "native", japanese: "出生地の,母国の；その土地固有の",type:"word" },
                    { english: "instant", japanose: "即席の；即時の",type:"word" },
                    { english: "lovely", japanose: "すてきな,すばらしい；美しい" ,type:"word"},
                    { english: "clear", japanose: "明白な,はっきりした；澄んだ；晴れた",type:"word" },
                    { english: "convenient", japanose: "都合のよい,便利な" ,type:"word"},
                    { english: "crazy", japanose: "夢中で；ばかげた；いらいらして" ,type:"word"},
                    { english: "funny", japanose: "おかしい,滑稽な；奇妙な" ,type:"word"},
                    { english: "secret", japanose: "秘密の",type:"word" },
                    { english: "remote", japanose: "(距離的・時間的に)(遠く)離れた；かけ離れた" ,type:"word"},
                    { english: "wake", japanose: "目を覚ます；を目覚めさせる",type:"word" },
                    { english: "release", japanose: "を解放する；を公表[公開]する；を(新しく)発売する",type:"word" },
                    { english: "establish", japanose: "を設立[創立]する；(関係)を築く",type:"word" },
                    { english: "examine", japanose: "を調べる" ,type:"word"},
                    { english: "celebrate", japanose: "を祝う",type:"word" },
                    { english: "float", japanose: "(水面・空中を)漂う；浮く",type:"word" },
                    { english: "recommend", japanose: "を推薦する",type:"word" },
                    { english: "supply", japanose: "を供給する",type:"word" },
                ]
            },
            {
                id: 19,
                title: "p110~114",
                theme: "ターゲット⑤",
                words: [
                    { english: "disappear", japanese: "見えなくなる；消滅する",type:"word" },
                    { english: "apologize", japanese: "謝る",type:"word" },
                    { english: "paint", japanese: "を塗る；を描く",type:"word" },
                    { english: "pull", japanese: "(を)引く,引っ張る",type:"word" },
                    { english: "print", japanese: "(を)印刷する；を出版する",type:"word" },
                    { english: "lift", japanose: "を持ち上げる；(持ち)上がる",type:"word" },
                    { english: "separate", japanose: "を分ける；を区別する；を引き離す；別れる",type:"word" },
                    { english: "melt", japanose: "を溶かす,溶ける" ,type:"word"},
                    { english: "strike", japanose: "を強く打つ,ぶつける；当たる；(を)(突然)襲う",type:"word" },
                    { english: "blow", japanose: "を吹き飛ばす；風で飛ぶ；(風が)吹く",type:"word" },
                    { english: "let", japanose: "⋯させてやる,⋯するのを許可する；<let's doで>⋯しよう",type:"word" },
                    { english: "roll", japanose: "転がる,を転がす；を巻く",type:"word" },
                    { english: "recover", japanose: "回復する；を取り戻す",type:"word" },
                    { english: "surround", japanose: "を囲む,取り囲む",type:"word" },
                    { english: "doubt", japanose: "を襲う",type:"word" },
                    { english: "display", japanose: "を展示[陳列]する；(感情・性質など)を表す",type:"word" },
                    { english: "announce", japanose: "を発表する；だと(大声で)告げる,アナウンスする",type:"word" },
                    { english: "support", japanose: "を支持する；を支援する；を支える",type:"word" },
                    { english: "act", japanose: "行動する；振る舞う；(を)演じる" ,type:"word"},
                    { english: "repeat", japanose: "を繰り返して言う[書く]；を繰り返す；(を)復唱する",type:"word" },
                    { english: "count", japanose: "(を)数える；重要である",type:"word" },

                ]
            },
            {
                id: 20,
                title: "p116~120",
                theme: "ターゲット⑤",
                words: [
                    { english: "compare", japanese: "を比べる" ,type:"word"},
                    { english: "shine", japanese: "輝く；を磨く" ,type:"word"},
                    { english: "replace", japanese: "に取って代わる；を取り替える",type:"word" },
                    { english: "reality", japanese: "現実；現実の物",type:"word" },
                    { english: "strength", japanese: "力；強さ",type:"word" },
                    { english: "era", japanese: "時代",type:"word" },
                    { english: "area", japanese: "地域,区域；領域,分野",type:"word" },
                    { english: "respect", japanese: "尊敬；箇所,点",type:"word" },
                    { english: "pressure", japanese: "重圧；圧力" ,type:"word"},
                    { english: "pleasure", japanese: "喜び,楽しみ",type:"word" },
                    { english: "favor", japanese: "親切な行為,手助け；支持" ,type:"word"},
                    { english: "statue", japanese: "像" ,type:"word"},
                    { english: "limit", japanese: "限度,限界；制服",type:"word" },
                    { english: "bottom", japanese: "下部,底",type:"word" },
                    { english: "position", japanese: "立場；位置；姿勢；職",type:"word" },
                    { english: "memory", japanese: "記憶(力)；思い出",type:"word" },
                    { english: "level", japanese: "水準,レベル；程度；(ある基準点からの)高さ",type:"word" },
                    { english: "figure", japanese: "<通例~s>数(値)；数字；姿；図",type:"word" },
                    { english: "direction", japanese: "方向；方針；<~s>指示,説明書" ,type:"word"},
                    { english: "bit", japanese: "少し,少量",type:"word" },
                ]
            },
            {
                id: 21,
                title: "p122~126",
                theme: "ターゲット⑥",
                words: [
                    { english: "contrast", japanese: "対比,差異；(画僧・明暗などの)コントラスト" ,type:"word"},
                    { english: "religion", japanese: "宗教",type:"word" },
                    { english: "harmony", japanese: "調和,一致",type:"word" },
                    { english: "pattern", japanese: "模様,図柄；パターン,型",type:"word" },
                    { english: "stage", japanese: "段階；舞台",type:"word" },
                    { english: "degree", japanese: "程度；(温度・経緯度などの)度；学位",type:"word" },
                    { english: "emergency", japanese: "緊急(事態)",type:"word" },
                    { english: "origin", japanese: "起源；(の)生まれ,家系",type:"word" },
                    { english: "battle", japanese: "戦闘；闘争",type:"word" },
                    { english: "enemy", japanese: "敵",type:"word" },
                    { english: "note", japanese: "メモ,覚書；短信；注釈",type:"word" },
                    { english: "countryside", japanese: "田舎",type:"word" },
                    { english: "contest", japanese: "競技(会),コンテスト",type:"word" },
                    { english: "sort", japanese: "種類",type:"word" },
                    { english: "depth", japanese: "深さ；奥行き" ,type:"word"},
                    { english: "top", japanese: "最高部,頂上",type:"word" },
                    { english: "theme", japanese: "テーマ,主題",type:"word" },
                    { english: "sentence", japanese: "文；判決；刑",type:"word" },
                    { english: "cycle", japanese: "周期；循環",type:"word" },
                    { english: "concept", japanese: "概念",type:"word" },
                    { english: "rhythm", japanese: "リズム",type:"word" },
                    { english: "tradition", japanese: "伝統；しきたり",type:"word" },
                    { english: "theory", japanese: "理論；説,学説",type:"word" },
                    { english: "correct", japanese: "正しい；適切",type:"word" },
                ]
            },
            {
                id: 22,
                title: "p128~132",
                theme: "ターゲット⑥",
                words: [
                    { english: "blank", japanese: "白紙の,空白の,空の",type:"word" },
                    { english: "quiet", japanese: "静かな；平穏な,人気のない",type:"word" },
                    { english: "smooth", japanese: "滑らかな,すべすべした" ,type:"word"},
                    { english: "wet", japanese: "濡れた,湿った；雨降りの" ,type:"word"},
                    { english: "chief", japanese: "最高(位)の；主要な",type:"word" },
                    { english: "raw", japanese: "生の；加工[処理]されていない",type:"word" },
                    { english: "parsonal", japanese: "個人の,個人的な",type:"word" },
                    { english: "double", japanese: "2倍の；二重の；二人用の",type:"word" },
                    { english: "dirty", japanese: "汚れた；不正な",type:"word" },
                    { english: "normal", japanese: "普通の；標準の",type:"word" },
                    { english: "full", japanese: "いっぱいの；満腹の",type:"word" },
                    { english: "simple", japanese: "簡単な；質素な",type:"word" },
                    { english: "equal", japanese: "等しい,同等の；平等の",type:"word" },
                    { english: "quick", japanese: "短時間の；素早い；即時",type:"word" },
                    { english: "rapid", japanese: "急速な；素早い",type:"word" },
                    { english: "idea", japanese: "理想的な",type:"word" },
                    { english: "rough", japanese: "大まかな；粗い；乱暴な",type:"word" },
                    { english: "silent", japanese: "無言の；静かな" ,type:"word"},
                    { english: "violent", japanese: "暴力的な,乱暴な；激しい",type:"word" },
                    { english: "rich", japanese: "豊富な；お金持ちの",type:"word" },
                    { english: "perfect", japanese: "完全な,完璧な；最適の,うってつけの",type:"word" },
                    { english: "weak", japanese: "弱い,弱った；不得手な；(味の)薄い",type:"word" },
                    { english: "upper", japanese: "上の[高い]方の；上位の",type:"word" },
                    { english: "inner", japanese: "内部の,中心部の",type:"word" },
                ]
            },
            {
                id: 23,
                title: "p134~138",
                theme: "ターゲット⑥",
                words: [
                    { english: "awful", japanese: "ひどい；すさまじい",type:"word" },
                    { english: "false", japanese: "間違った；本物でない,偽の",type:"word" },
                    { english: "vivid", japanese: "鮮やかな",type:"word" },
                    { english: "pure", japanese: "純粋な；汚れていない",type:"word" },
                    { english: "minor", japanese: "(比較的)重要ではない,小さい方の" ,type:"word"},
                    { english: "mild", japanese: "(天候が)穏やかな,温暖な；(味が)まろやかな",type:"word" },
                    { english: "admire", japanese: "を賞賛する,に感嘆する；に見とれる",type:"word" },
                    { english: "drop", japanese: "を落とす；落ちる",type:"word" },
                    { english: "reflect", japanese: "を映し出す；を反射する；を熟考する",type:"word" },
                    { english: "dig", japanese: "(を)掘る；を掘り出す",type:"word" },
                    { english: "beg", japanese: "(を)懇願する",type:"word" },
                    { english: "freeze", japanese: "凍る；を凍らせる",type:"word" },
                    { english: "adopt", japanese: "を採用する" ,type:"word"},
                    { english: "measure", japanese: "を測る；(の)寸法がある",type:"word" },
                    { english: "flow", japanese: "流れる",type:"word" },
                    { english: "fulfill", japanese: "を実現させる；(要求・必要など)を満たす；(役割)を果たす",type:"word" },
                    { english: "deliver", japanese: "を配達する；(演説・公演など)をする",type:"word" },
                    { english: "wrap", japanese: "を包む；を巻く",type:"word" },
                    { english: "knock", japanese: "ノックする；にぶつける[ぶつかる]" ,type:"word"},
                    { english: "spell", japanese: "をつづる",type:"word" },
                    { english: "rush", japanese: "急いでいく；をせきたてる",type:"word" },
                    { english: "pray", japanese: "切に願う；祈る",type:"word" },
                    { english: "reject", japanese: "を拒絶する" ,type:"word"},
                    { english: "protest", japanese: "講義する,異議を唱える" ,type:"word"},
                ]
            },
            {
                id: 24,
                title: "p140~144",
                theme: "ターゲット⑦",
                words: [
                    { english: "handle", japanese: "を扱う,処理する；に手を触れる",type:"word" },
                    { english: "disturb", japanese: "を邪魔する；(平穏など)を乱す",type:"word" },
                    { english: "gather", japanese: "を集める；集まる",type:"word" },
                    { english: "copy", japanese: "の写しをとる,コピーする；をまねる" ,type:"word"},
                    { english: "press", japanese: "(を)押す,(を)押しつける",type:"word" },
                    { english: "consist", japanese: "成り立つ；ある" ,type:"word"},
                    { english: "assist", japanese: "を手助けする",type:"word" },
                    { english: "kick", japanese: "を蹴る",type:"word" },
                    { english: "link", japanese: "を結びつける,関連づける",type:"word" },
                    { english: "adjust", japanese: "順応[適応]；を調整する",type:"word" },
                    { english: "defend", japanese: "を守る" ,type:"word"},
                    { english: "shut", japanese: "を閉める,閉じる" ,type:"word"},
                    { english: "bear", japanese: "に耐える；(重さ・負荷)を支える；(子)を産む",type:"word" },
                    { english: "task", japanese: "(するべき)仕事,(困難な)作業,課題" ,type:"word"},
                    { english: "hug", japanese: "ハグ,抱擁",type:"word" },
                    { english: "clue", japanese: "手がかり；ヒント",type:"word" },
                    { english: "percent", japanese: "パーセント" ,type:"word"},
                    { english: "dozen", japanese: "1ダース,12,<~s>たくさん",type:"word" },
                    { english: "ghost", japanese: "幽霊",type:"word" },
                    { english: "error", japanese: "誤り",type:"word" },
                    { english: "trend", japanese: "流行；傾向；動向",type:"word" },
                    { english: "thought", japanese: "考え,思いつき；(one's ~s)意見,~の考え；考えること" ,type:"word"},
                ]
            },
            {
                id: 25,
                title: "p146~150",
                theme: "ターゲット⑦",
                words: [
                    { english: "alarm", japanese: "警報(装置)；目覚まし時計；恐怖",type:"word" },
                    { english: "sample", japanese: "見本,試供品",type:"word" },
                    { english: "shadow", japanese: "(人・物の)影,陰",type:"word" },
                    { english: "shade", japanese: "陰,日陰",type:"word" },
                    { english: "standard", japanese: "基準,標準" ,type:"word"},
                    { english: "hunger", japanese: "飢え；空腹(感)",type:"word" },
                    { english: "appeal", japanese: "訴え,要求；魅力",type:"word" },
                    { english: "harm", japanese: "害,危害；悪意",type:"word" },
                    { english: "pile", japanese: "(同種のものの)山" ,type:"word"},
                    { english: "plenty", japanese: "たくさん",type:"word" },
                    { english: "edge", japanese: "端,縁；刃" ,type:"word"},
                    { english: "poison", japanese: "毒；害悪",type:"word" },
                    { english: "scsle", japanese: "規模；段階,基準；目盛り",type:"word" },
                    { english: "section", japanese: "節；区分；部門" ,type:"word"},
                    { english: "attempt", japanese: "試み",type:"word" },
                    { english: "merit", japanese: "長所，利点",type:"word" },
                    { english: "trick", japanese: "いたずら；たくらみ；手品；トリック",type:"word" },
                    { english: "second", japanese: "少しの間；秒" ,type:"word"},
                    { english: "medium", japanese: "媒体，手段；情報伝達手段",type:"word" },
                    { english: "unit", japanese: "単位；単元",type:"word" },
                    { english: "ambition", japanese: "(強い)願望，野心",type:"word" },
                    { english: "midnight", japanese: "夜の12時",type:"word" },
                ]
            },
            {
                id: 26,
                title: "p151~155",
                theme: "ターゲット⑦",
                words: [
                    {english: "power", japanese: "力；能力；エネルギー；電力" ,type:"word"},
                    {english: "principle", japanese: "信条；原理",type:"word" },
                    {english: "vision", japanese: "展望,理想像；視力",type:"word" },
                    {english: "quarter", japanese: "4分の1；15分",type:"word" },
                    {english: "luck", japanese: "運；幸運" ,type:"word"},
                    {english: "quantity", japanese: "分量,数量；量；多量,多数",type:"word" },
                    {english: "fault", japanese: "責任,罪；欠点",type:"word" },
                    {english: "somehow", japanese: "何とかして；どういうわけか",type:"word" },
                    {english: "forever", japanese: "永遠に；とても長い間",type:"word" },
                    {english: "mostly", japanese: "主に,たいてい",type:"word" },
                    {english: "forward", japanese: "前へ,先に",type:"word" },
                    {english: "nowadays", japanese: "近頃，最近では",type:"word" },
                    {english: "ahead", japanese: "前方に",type:"word" },
                    {english: "apart", japanese: "離れて",type:"word" },
                    {english: "altogether", japanese: "まったく，完全に；前部で；概して" ,type:"word"},
                   
                ]
            },
            {
                id: 27,
                title: "p156~159",
                theme: "ターゲット⑦",
                words: [
                    {english: "throughout", japanese: "〜の間中(ずっと)；〜の至る所に",type:"word" },
                    {english: "beyond", japanese: "〜の向こうに；(限界・範囲)を越えて" ,type:"word"},
                    {english: "toward", japanese: "〜の方向へ，〜に向けて；〜に対する",type:"word" },
                    {english: "within", japanese: "〜以内に；〜の範囲内に",type:"word" },
                    {english: "above", japanese: "〜の上方に；〜より上に[で]",type:"word" },
                    {english: "below", japanese: "〜より下に",type:"word" },
                    {english: "per", japanese: "〜につき，〜あたり",type:"word" },
                    {english: "except", japanese: "〜を除いて",type:"word" },
                    {english: "beside", japanese: "〜のそばに" ,type:"word"},
                    {english: "unlike", japanese: "〜と違って" ,type:"word"},
                    {english: "outside", japanese: "〜の外に[で]",type:"word" },
                    {english: "inside", japanese: "〜の中に[で]",type:"word" },
                    {english: "against", japanese: "〜に反対して；〜に反して；〜に対抗して",type:"word" },
                    {english: "beneath", japanese: "〜の下に[の]",type:"word" },
                    {english: "plus", japanese: "〜を加えて,足すと",type:"word" },
                    {english: "across", japanese: "〜を横切って；〜の向こう側に；〜の至る所に",type:"word" },
                    
                ]
            },
            {
                id: 28,
                title: "p160~165",
                theme: "ターゲット⑧",
                words: [
                    {english: "bring back~/bring~back", japanese: "〜(物)を返す，〜(人)を送り届ける", type: "phrase" },
                    {english: "carry out~/carry~out", japanese: "〜(計画・約束など)を実行する", type: "phrase" },
                    {english: "date back to~", japanese: "〜(ある時期)にさかのぼる", type: "phrase" },
                    {english: "find out (about)~", japanese: "(〜について)(情報などを)知る", type: "phrase" },
                    {english: "get together (with A) (for B)", japanese: "(A(人)と)(Bのことで)集まる，会う", type: "phrase" },
                    {english: "give off~", japanese: "〜(光・におい・熱など)を発する", type: "phrase" },
                    {english: "hand in~/hand~in", japanese: "〜を提出する", type: "phrase" },
                    {english: "hang up(on~)", japanese: "(〜(人)との)電話を切る", type: "phrase" },
                    {english: "hold up~/hold~up", japanese: "〜を(倒れないように)支えている" , type: "phrase"},
                    {english: "live on~", japanese: "〜(金額・収入)で暮らしを立てる", type: "phrase" },
                    {english: "look after~", japanese: "〜の世話をする", type: "phrase" },
                    {english: "look out(for~)", japanese: "(〜に)注意する" , type: "phrase"},
                    {english: "look up~/look~up", japanese: "〜(語句・情報など)を調べる；〜(人)を(久しぶりに)訪ねる", type: "phrase" },
                    {english: "major in~", japanese: "〜を専攻する" , type: "phrase"},
                    {english: "name A after[for] B", japanese: "AにBの名を取って名付ける", type: "phrase" },
                    {english: "put down~/put~down", japanese: "〜を書き留める；〜を置く", type: "phrase" },
                    {english: "see off~/see~off", japanese: "〜(人)を見送る", type: "phrase" },
                    {english: "take[have] a look (at~)", japanese: "(〜を)(ちょっと)見る", type: "phrase" },
                    {english: "take away~/take~away", japanese: "〜を片付ける，持ち帰る，取り上げる" , type: "phrase"},
                    {english: "take one’s time doing", japanese: "…するのに時間をかける", type: "phrase" },
                    {english: "think of A as B", japanese: "AをBと見なす" , type: "phrase"},
                    {english: "be about to do", japanese: "…しようとしている", type: "phrase" },
                    {english: "be born into~", japanese: "〜の家庭に生まれる", type: "phrase" },
                    {english: "be faced with~", japanese: "〜に直面している" , type: "phrase"},
                    {english: "be good at~", japanese: "〜が上手だ", type: "phrase" },
                    {english: "be in need of~", japanese: "〜を必要としている", type: "phrase" },
                    {english: "be made of [from]~", japanese: "〜でできている" , type: "phrase"},
                    {english: "be short of~", japanese: "〜が足りない" , type: "phrase"},
                    {english: "be up to~", japanese: "〜次第[〜の責任]である" , type: "phrase"},
                    {english: "be worried about~", japanese: "〜のことを心配している", type: "phrase" },
                ]
            },
            {
                id: 29,
                title: "p166~170",
                theme: "ターゲット⑧",
                words: [
                    {english: "(all) by oneself", japanese: "ひとりで，独力で；一人きりで", type: "phrase" },
                    {english: "(all) on one’s own", japanese: "(たった)ひとりで；独力で", type: "phrase" },
                    {english: "above all (else)", japanese: "とりわけ，何よりも" , type: "phrase"},
                    {english: "across from~", japanese: "〜の向かい側[反対側]に", type: "phrase" },
                    {english: "after all", japanese: "(予想に反して)結局は，やはり；(理由を補足して)何しろ⋯なのだから", type: "phrase" },
                    {english: "~at a time", japanese: "１度に〜；〜を続けて" , type: "phrase"},
                    {english: "by accident[chance]", japanese: "偶然(に)", type: "phrase" },
                    {english: "by mistake", japanese: "間違って，誤って", type: "phrase" },
                    {english: "day after day", japanese: "来る日も来る日も" , type: "phrase"},
                    {english: "for free", japanese: "無料で", type: "phrase" },
                    {english: "from ~ on", japanese: "〜以降(ずっと)", type: "phrase" },
                    {english: "from time to time", japanese: "時々", type: "phrase" },
                    {english: "in a hurry", japanese: "急いで", type: "phrase" },
                    {english: "in favor of~", japanese: "〜に賛成して，〜を支持して", type: "phrase" },
                    {english: "in order [so as]to do", japanese: "…するために", type: "phrase" },
                    {english: "in the past", japanese: "昔は，過去に" , type: "phrase"},
                    {english: "in those days", japanese: "その当時は" , type: "phrase"},
                    {english: "on purpose", japanese: "故意に，わざと" , type: "phrase"},
                    {english: "over and over (again)", japanese: "何度も", type: "phrase" },
                    {english: "upside down", japanese: "逆さまに" , type: "phrase"},
                    {english: "a large[great] number of~", japanese: "たくさんの〜" , type: "phrase"},
                    {english: "a series of~", japanese: "一連の〜，一続きの〜" , type: "phrase"},
                    {english: "as [so] long as", japanese: "…である限り[間]；⋯でありさえすれば", type: "phrase" },
                    {english: "as soon as⋯", japanese: "…するとすぐに" , type: "phrase"},
                    {english: "had better do", japanese: "…するべきだ，⋯するのがよい", type: "phrase" },
                    {english: "more and more~", japanese: "ますます多くの〜" , type: "phrase"},
                    {english: "not ~ at all", japanese: "まったく〜ではない" , type: "phrase"},
                    {english: "not A but B", japanese: "AではなくてB", type: "phrase" },
                    {english: "the first time⋯", japanese: "初めて…する[した]とき(に)", type: "phrase" },
                    {english: "when it comes to~", japanese: "〜のこととなると，〜に関しては", type: "phrase" }, 
                ]
            },
            {
                id: 30,
                title: "p174~175",
                theme: "ターゲット⑨",
                words: [
                    {english: "parent", japanese: "親",type:"word" },
                    {english: "husband", japanese: "夫",type:"word" },
                    {english: "wife", japanese: "妻" ,type:"word"},
                    {english: "kid", japanese: "子供；若者" ,type:"word"},
                    {english: "twin", japanese: "双子(の１人)",type:"word" },   
                    {english: "relative", japanese: "親戚",type:"word" },
                    {english: "cousin", japanese: "いとこ",type:"word" },
                    {english: "ancestor", japanese: "先祖；原型",type:"word" },
            ]
            },
            {
                id: 31,
                title: "p176~180",
                theme: "ターゲット⑨",
                words: [
                    {english: "job", japanese: "仕事",type:"word" },
                    {english: "work", japanese: "仕事；職場；勉強；作品" ,type:"word"},
                    {english: "occupation", japanese: "職業",type:"word" },
                    {english: "career", japanese: "職業；経歴" ,type:"word"},
                    {english: "businesss", japanese: "仕事，商売；会社，店舗" ,type:"word"},
                    {english: "interview", japanese: "面接；インタビュー" ,type:"word"},
                    {english: "hire", japanese: "を(一時的に)雇う；を賃借りする" ,type:"word"},
                    {english: "retire", japanese: "退職する，引退する",type:"word" },
                    {english: "clerk", japanese: "店員；事務員",type:"word" },
                    {english: "officer", japanese: "警官；役人",type:"word" },
                    {english: "engineer", japanese: "技師，技術者",type:"word" },
                    {english: "artist", japanese: "芸術家；画家",type:"word" },
                    {english: "director", japanese: "監督；管理者",type:"word" },
                    {english: "actor", japanese: "俳優" ,type:"word"},
                    {english: "nurse", japanese: "看護師",type:"word" },
                    {english: "secretary", japanese: "秘書",type:"word" },
                    {english: "agent", japanese: "代行業者；代理人",type:"word" },
                    {english: "civil", japanese: "民間の；市民の；国内の",type:"word" },
                    {english: "mayor", japanese: "市長，町[村]長" ,type:"word"},
                    {english: "chairperson", japanese: "議長，委員長",type:"word" },
                    {english: "professor", japanese: "教授" ,type:"word"},
                    {english: "principal", japanese: "校長",type:"word" },
                    {english: "expert", japanese: "専門家，達人" ,type:"word"},
                    {english: "leader", japanese: "指導者，リーダー",type:"word" },
                    {english: "queen", japanese: "女王；王妃",type:"word" },
                    {english: "prince", japanese: "王子；第一人者",type:"word" },
                ]
            },
             {
                id: 32,
                title: "p181~185",
                theme: "ターゲット⑨",
                words: [
                    {english: "royal", japanese: "国王の，王室の",type:"word" },
                    {english: "slave", japanese: "奴隷",type:"word" },
                    {english: "hall", japanese: "会館，ホール；玄関(の広間)；廊下",type:"word" },
                    {english: "office", japanese: "事務所，会社",type:"word" },
                    {english: "bank", japanese: "銀行；土手；川岸",type:"word" },
                    {english: "apartment", japanese: "アパート，マンション" ,type:"word"},
                    {english: "library", japanese: "図書館；蔵書，コレクション" ,type:"word"},
                    {english: "gym", japanese: "体育館，ジム；体育",type:"word" },
                    {english: "museum", japanese: "博物館，美術館" ,type:"word"},
                    {english: "theater", japanese: "劇場",type:"word" },
                    {english: "studio", japanese: "スタジオ，（映画）撮影所",type:"word" },
                    {english: "stadium", japanese: "競技場，スタジアム" ,type:"word"},
                    {english: "temple", japanese: "寺院，神殿" ,type:"word"},
                    {english: "shrine", japanese: "聖堂，神社；聖地",type:"word" },
                    {english: "castle", japanese: "城" ,type:"word"},
                    {english: "tower", japanese: "塔",type:"word" },
                    {english: "entrance", japanese: "入口，玄関；入学；入場",type:"word" },
                    {english: "exit", japanese: "出口；(プログラムの)終了",type:"word" },
                    {english: "architecture", japanese: "建築；建築様式",type:"word" },
                ]
            },
             {
                id: 33,
                title: "p186~190",
                theme: "ターゲット⑨",
                words: [
                    {english: "avenue", japanese: "大通り，〜街",type:"word" },
                    {english: "block", japanese: "(街の)１区画，ブロック；大きな塊" ,type:"word"},
                    {english: "corner", japanese: "曲がり角；角，隅" ,type:"word"},
                    {english: "intersection", japanese: "交差点" ,type:"word"},
                    {english: "zone", japanese: "地帯，区域",type:"word" },
                    {english: "square", japanese: "(四角い)広場；正方形",type:"word" },
                    {english: "market", japanese: "市場" ,type:"word"},
                    {english: "path", japanese: "小道",type:"word" },
                    {english: "slope", japanese: "坂",type:"word" },
                    {english: "traffic", japanese: "交通(量)",type:"word" },
                    {english: "drive", japanese: "（車を）運転する",type:"word" },
                    {english: "ride", japanese: "(に)乗る，乗っていく",type:"word" },
                    {english: "railroad", japanese: "鉄道",type:"word" },
                    {english: "subway", japanese: "地下鉄" ,type:"word"},
                    {english: "automobile", japanese: "自動車" ,type:"word"},
                    {english: "engine", japanese: "エンジン",type:"word" },
                    {english: "wheel", japanese: "ハンドル；車輪",type:"word" },
                    {english: "license", japanese: "免許(証)；許可",type:"word" },
                    {english: "airport", japanese: "空港" ,type:"word"},
                    {english: "flight", japanese: "定期航空便，フライト",type:"word" },
                    {english: "port", japanese: "港",type:"word" },
                    {english: "canal", japanese: "運河",type:"word" },
                    {english: "key", japanese: "鍵；手がかり",type:"word" },
                    {english: "stair", japanese: "階段" ,type:"word"},
                    {english: "upstairs", japanese: "上の階に［で］",type:"word" },
                ]
            },
             {
                id: 34,
                title: "p191~193",
                theme: "ターゲット⑨",
                words: [
                    {english: "floor", japanese: "床；階" ,type:"word"},
                    {english: "shelf", japanese: "棚" ,type:"word"},
                    {english: "roof", japanese: "屋根；<the~>最高部",type:"word" },
                    {english: "ladder", japanese: "はしご",type:"word" },
                    {english: "yard", japanese: "庭；囲い地；(長さの単位)ヤード，ヤール",type:"word" },
                    {english: "closet", japanese: "クローゼット，収納場所",type:"word" },
                    {english: "refrigerator", japanese: "冷蔵庫",type:"word" },
                    {english: "shower", japanese: "シャワー(室・器具)；シャワー(を浴びること)；にわか雨" ,type:"word"},
                    {english: "housework", japanese: "家事",type:"word" },
                    
                ]
            },
             {
                id: 35,
                title: "p194~200",
                theme: "ターゲット⑩",
                words: [
                     {english: "plastic", japanese: "プラスチック(製)の，ビニール(製)の",type:"word" },
                    {english: "plate", japanese: "皿；料理；表示板" ,type:"word"},
                    {english: "glass", japanese: "グラス，カップ；ガラス；<~es>めがね",type:"word" },
                    {english: "garbage", japanese: "生ごみ，ごみ",type:"word" },
                    {english: "trash", japanese: "ごみ，(紙)くず；<the~>ゴミ箱",type:"word" },
                    {english: "dust", japanese: "ほこり",type:"word" },
                    {english: "trap", japanese: "わな；策略",type:"word" },
                    {english: "brush", japanese: "ブラシ，はけ",type:"word" },
                    {english: "comb", japanese: "くし；くしで髪をとかすこと",type:"word" },
                    {english: "blanket", japanese: "毛布",type:"word" },
                    {english: "sheet", japanese: "(1枚の)紙，紙の1枚；シーツ",type:"word" },
                    {english: "label", japanese: "ラベル，荷札",type:"word" },
                    {english: "envelope", japanese: "封筒" ,type:"word"},
                    {english: "fashion", japanese: "流行(しているもの)" ,type:"word"},
                    {english: "style", japanese: "流行；(服装・髪などの)スタイル；様式；(人の)流儀",type:"word" },
                    {english: "formal", japanese: "正式の；形式ばった",type:"word" },
                    {english: "tight", japanese: "(衣類などが)きつい；(時間・金銭などが)ゆとりのない",type:"word" },
                    {english: "loose", japanese: "(衣類などが)ゆったりした；ゆるい，ゆるんだ",type:"word" },
                    {english: "wear", japanese: "を着ている，身につけている；をすり減る，すり減る",type:"word" },
                    {english: "clothes", japanese: "衣服，衣類",type:"word" },
                    {english: "dress", japanese: "に衣服を着せる；(の)服装をしている",type:"word" },
                    {english: "costume", japanese: "(舞台などの)衣装，仮装；(国民・時代特有の)服装",type:"word" },
                    {english: "tie", japanese: "を結ぶ；を縛る",type:"word" },
                    {english: "sew", japanese: "を縫う，縫い付ける；縫い物をする",type:"word" },
                    {english: "frame", japanese: "<~s>(めがねの)フレーム；枠；額縁",type:"word" },
                    {english: "button", japanese: "(衣類の・機械の)ボタン",type:"word" },
                    {english: "ring", japanese: "指輪；輪；鳴る音",type:"word" },
                    {english: "jewel", japanese: "宝石；<~s>宝飾品",type:"word" },
                    {english: "wallet", japanese: "財布",type:"word" },
                    {english: "mobile", japanese: "携帯電話",type:"word" },
                    {english: "portable", japanese: "持ち運びできる，携帯用の",type:"word" },
                    {english: "umbrella", japanese: "傘",type:"word" },
                ]
            },
             {
                id: 36,
                title: "p201~205",
                theme: "ターゲット⑩",
                words: [
                    {english: "silk", japanese: "絹，絹糸" ,type:"word"},
                    {english: "cotton", japanese: "綿" ,type:"word"},
                    {english: "leather", japanese: "革",type:"word" },
                    {english: "feather", japanese: "羽，羽毛",type:"word" },
                    {english: "meal", japanese: "食事" ,type:"word"},
                    {english: "supper", japanese: "夕食",type:"word" },
                    {english: "snack", japanese: "軽食，おやつ",type:"word" },
                    {english: "dessert", japanese: "デザート",type:"word" },
                    {english: "diet", japanese: "ダイエット；(栄養的観点での)食事",type:"word" },
                    {english: "chopstick", japanese: "<~s>箸",type:"word" },
                    {english: "bite", japanese: "(を)かむ，(に)かみつく；(虫などが)を刺す",type:"word" },
                    {english: "flavor", japanese: "風味，味" ,type:"word"},
                    {english: "delicious", japanese: "とてもおいしい",type:"word" },
                    {english: "bitter", japanese: "苦い；つらい",type:"word" },
                    {english: "sour", japanese: "酸っぱい；すえた",type:"word" },
                    {english: "recipe", japanese: "調理法，レシピ；秘訣",type:"word" },
                    {english: "mix", japanese: "を混ぜる；混ざる",type:"word" },
                ]
            },
             {
                id: 37,
                title: "p206~210",
                theme: "ターゲット⑩",
                words: [
                    {english: "pour", japanese: "注ぐ，かける；(飲み物など)をつぐ，(多量に)流れ出る" ,type:"word"},
                    {english: "fry", japanese: "(油で)炒める，揚げる" ,type:"word"},
                    {english: "boil", japanese: "をゆでる，煮る；を沸かす；沸く",type:"word" },
                    {english: "steam", japanese: "を蒸す",type:"word" },
                    {english: "bake", japanese: "（パンなど）を焼く",type:"word" },
                    {english: "harvest", japanese: "収穫(物)；収穫期" ,type:"word"},
                    {english: "vegetable", japanese: "野菜",type:"word" },
                    {english: "meat", japanese: "肉",type:"word" },
                    {english: "wheat", japanese: "小麦" ,type:"word"},
                    {english: "flour", japanese: "小麦粉" ,type:"word"},
                    {english: "honey", japanese: "ハチミツ",type:"word" },
                    {english: "salt", japanese: "塩",type:"word" },
                    {english: "menu", japanese: "メニュー",type:"word" },
                    {english: "choice", japanese: "選択(の幅・種類)",type:"word" },
                    {english: "service", japanese: "サービス，応対；公益事業；(運行)便",type:"word" },
                    {english: "tip", japanese: "チップ；秘訣" ,type:"word"},
                    {english: "cancel", japanese: "(を)取り消す，中止する",type:"word" },
                    {english: "culture", japanese: "文化，文化活動",type:"word" },
                    {english: "hobby", japanese: "趣味",type:"word" },
                    {english: "amusement", japanese: "楽しみ；おもしろさ；<~s>娯楽",type:"word" },
                    {english: "entertainment", japanese: "娯楽，気晴らし",type:"word" },
                    {english: "collect", japanese: "を集める，収集する",type:"word" },
                    {english: "exhibit", japanese: "を展示する；(感情・能力など)を出す",type:"word" },
                    {english: "instrument", japanese: "楽器；器具",type:"word" },
                    {english: "tune", japanese: "(正しい)音調；曲",type:"word" },
                    {english: "film", japanese: "映画；フィルム",type:"word" },
                ]
            },
             {
                id: 38,
                title: "p211~213",
                theme: "ターゲット⑩",
                words: [
                    {english: "cartoon", japanese: "アニメ(動画)",type:"word" },
                    {english: "comic", japanese: "漫画(雑誌・本)" ,type:"word"},
                    {english: "photograph", japanese: "写真",type:"word" },
                    {english: "portrait", japanese: "肖像画；(詳しい)描写" ,type:"word"},
                    {english: "magic", japanese: "手品；魔法；不思議な力",type:"word" },
                    {english: "tour", japanese: "(周遊)旅行，ツアー；見学",type:"word" },
                    {english: "journey", japanese: "旅行；旅程；(〜への)道のり",type:"word" },
                    {english: "sightseeing", japanese: "観光",type:"word" },
                    
                ]
            },
            { 
                id: 39,
                title: "p214~220",
                theme: "ターゲット⑪",
                words:[   
                    {english: "adventure", japanese: "冒険(旅行)；冒険心" ,type:"word"},
                    {english: "explore", japanese: "(を)探検する；を探求する",type:"word" },
                    {english: "wander", japanese: "(を)歩き回る，ぶらつく" ,type:"word"},
                    {english: "camp", japanese: "キャンプ，合宿；野営地",type:"word" },
                    {english: "tourist", japanese: "観光客，旅行者",type:"word" },
                    {english: "passenger", japanese: "乗客",type:"word" },
                    {english: "guide", japanese: "ガイド，案内人；案内書；指針",type:"word" },
                    {english: "vacation", japanese: "休暇",type:"word" },
                    {english: "souvenir", japanese: "土産，思い出の品",type:"word" },
                    {english: "pack", japanese: "(に)荷物を詰める，(を)荷造りする；を詰め込む",type:"word" },
                    {english: "win", japanese: "(競技など)(に)勝つ；を獲得する",type:"word" },
                    {english: "victory", japanese: "勝利",type:"word" },
                    {english: "record", japanese: "記録，最高記録" ,type:"word"},
                    {english: "score", japanese: "得点，スコア；成績",type:"word" },
                    {english: "prize", japanese: "賞" ,type:"word"},
                    {english: "award", japanese: "賞，賞金",type:"word" },
                    {english: "race", japanese: "競争，レース；人種；民族",type:"word" },
                    {english: "match", japanese: "試合；競争相手；適合する人[物]",type:"word" },
                    {english: "tournament", japanese: "トーナメント" ,type:"word"},
                    {english: "professional", japanese: "プロの；熟練した；専門職の",type:"word" },
                    {english: "athlete", japanese: "運動選手" ,type:"word"},
                    {english: "coach", japanese: "コーチ，指導員",type:"word" },
                    {english: "rival", japanese: "ライバル，競争相手",type:"word" },
                    {english: "train", japanese: "(を)訓練する，トレーニングする",type:"word" },
                    {english: "exercise", japanese: "運動する；(体の部位など)を鍛える" ,type:"word"},
                    {english: "practice", japanese: "(を)練習する；を実践する",type:"word" },
                    {english: "indoor", japanese: "屋内の，室内の",type:"word" },
                    {english: "flag", japanese: "旗，国旗",type:"word" },
                    {english: "nature", japanese: "自然，自然界；性質",type:"word" },
                    {english: "climate", japanese: "気候" ,type:"word"},
                    {english: "forecast", japanese: "予報，予測",type:"word" },
                    
                ]
            },
             {
                id: 40,
                title: "p221~225",
                theme: "ターゲット⑪",
                words: [
                    {english: "temperature", japanese: "温度，気温；体温" ,type:"word"},
                    {english: "wind", japanese: "風" ,type:"word"},
                    {english: "breeze", japanese: "そよ風",type:"word" },
                    {english: "storm", japanese: "嵐" ,type:"word"},
                    {english: "thunder", japanese: "雷，雷鳴",type:"word" },
                    {english: "wave", japanese: "波；(急な)高まり，増加",type:"word" },
                    {english: "ray", japanese: "光線",type:"word" },
                    {english: "sunlight", japanese: "日光",type:"word" },
                    {english: "sunshine", japanese: "日差し，日なた",type:"word" },
                    {english: "sunset", japanese: "日没；夕焼け",type:"word" },
                    {english: "landscape", japanese: "風景，景色",type:"word" },
                    {english: "continent", japanese: "大陸",type:"word" },
                    {english: "ocean", japanese: "海，大洋" ,type:"word"},
                    {english: "island", japanese: "島",type:"word" },
                    {english: "ground", japanese: "地面",type:"word" },
                    {english: "cave", japanese: "洞窟",type:"word" },
                    {english: "bay", japanese: "湾，入り江",type:"word" },
                    {english: "coast", japanese: "海岸，沿岸",type:"word" },
                    {english: "shore", japanese: "岸",type:"word" },
                ]
            },
             {
                id: 41,
                title: "p226~230",
                theme: "ターゲット⑪",
                words: [
                {english: "horizon", japanese: "地平線，水平線",type:"word" },
                {english: "valley", japanese: "谷，盆地",type:"word" },
                {english: "desert", japanese: "砂漠",type:"word" },
                {english: "sand", japanese: "砂；砂地，砂浜",type:"word" },
                {english: "mud", japanese: "泥，ぬかるみ" ,type:"word"},
                {english: "rock", japanese: "岩，岩石；ロック音楽",type:"word" },
                {english: "environment", japanese: "(自然)環境；(生活・社会)環境",type:"word" },
                {english: "recycle", japanese: "を再(生)処理する，リサイクルする" ,type:"word"},
                {english: "pollution", japanese: "汚染，公害",type:"word" },
                {english: "disaster", japanese: "(大)災害",type:"word" },
                {english: "earthquake", japanese: "地震" ,type:"word"},
                {english: "flood", japanese: "洪水" ,type:"word"},
                {english: "rescue", japanese: "を救助する",type:"word" },
                {english: "creature", japanese: "生き物，動物",type:"word" },
                {english: "species", japanese: "種(しゅ)",type:"word" },
                {english: "wild", japanese: "野生の；自然のままの；荒々しい" ,type:"word"},
                {english: "wildlife", japanese: "野生動物",type:"word" },
                {english: "insect", japanese: "昆虫",type:"word" },
                {english: "dinosaur", japanese: "恐竜",type:"word" },
                {english: "hunt", japanese: "狩りをする；を狩る；を探し求める" ,type:"word"},
                {english: "bark", japanese: "吠える" ,type:"word"},
                {english: "nest", japanese: "巣" ,type:"word"},
                {english: "wood", japanese: "森，林；木材",type:"word" },
                {english: "bush", japanese: "茂み；低木",type:"word" },
                {english: "branch", japanese: "枝；支店",type:"word" },
                {english: "root", japanese: "根",type:"word" },   
                ]
            },
             {
                id: 42,
                title: "p231~235",
                theme: "ターゲット⑪",
                words: [
                {english: "grass", japanese: "芝生，草",type:"word" },
                {english: "leaf", japanese: "葉",type:"word" },
                {english: "bloom", japanese: "開花(期)；(観賞用の)花",type:"word" },
                {english: "seed", japanese: "種",type:"word" },
                {english: "human", japanese: "人間の；人間らしい",type:"word" },
                {english: "person", japanese: "人，人間；⋯する人",type:"word" },
                {english: "people", japanese: "人々；国民，民族",type:"word" },
                {english: "crowd", japanese: "群衆，人混み",type:"word" },
                {english: "generation", japanese: "世代(の人々)",type:"word" },
                {english: "male", japanese: "男性の，雄の",type:"word" },
                {english: "female", japanese: "女性の，雌の",type:"word" },
                {english: "gender", japanese: "ジェンダー，性",type:"word" },
                {english: "neighbor", japanese: "隣人；隣国",type:"word" },
                {english: "stranger", japanese: "（その土地に）不案内な人；見知らぬ人",type:"word" },
                {english: "birth", japanese: "誕生；出産",type:"word" },
                {english: "childhood", japanese: "子供時代",type:"word" },  
                ]
            },
             {
                id: 43,
                title: "p236~240",
                theme: "ターゲット⑪",
                words: [
                {english: "youth", japanese: "青年時代；若さ",type:"word" },
                {english: "teenager", japanese: "ティーンエイジャー，10代の若者",type:"word" },
                {english: "adult", japanese: "大人，成人",type:"word" },
                {english: "junior", japanese: "年少者；下位の人，後輩",type:"word" },
                {english: "senior", japanese: "年長者，高齢者；上位の人，先輩",type:"word" },
                {english: "elderly", japanese: "年配の",type:"word" },
                {english: "dead", japanese: "死んでいる，枯れた；機能しない",type:"word" },
                {english: "age", japanese: "年齢；時代，時期",type:"word" },
                {english: "physical", japanese: "身体の，肉体の；物質の，物理的な；物理学の",type:"word" },
                {english: "condition", japanese: "状態，体調；<~s>状況，環境",type:"word" },
                {english: "function", japanese: "機能，働き",type:"word" },
                {english: "sight", japanese: "視力；見ること；視界；光景",type:"word" },
                {english: "weight", japanese: "体重；重さ",type:"word" },
                {english: "fat", japanese: "太った；脂肪の多い",type:"word" },
                {english: "thin", japanese: "やせた；細い；薄い",type:"word" },
                {english: "slim", japanese: "ほっそりした，スリムな；わずかな",type:"word" },
                {english: "ugly", japanese: "醜い，不格好な",type:"word" },
                {english: "thirsty", japanese: "喉の渇いた",type:"word" },
                {english: "tear", japanese: "<~s>涙",type:"word" },
                {english: "sweat", japanese: "汗(をかいている状態)；スエットスーツ" ,type:"word"},
                {english: "hospital", japanese: "病院",type:"word" },
                {english: "ambulance", japanese: "救急車",type:"word" },
                ]
            },
             {
                id: 44,
                title: "p241~245",
                theme: "ターゲット⑪",
                words: [
                    {english: "wheelchair", japanese: "車椅子",type:"word" },
                    {english: "patient", japanese: "患者",type:"word" },
                    {english: "disease", japanese: "病気" ,type:"word"},
                    {english: "illness", japanese: "病気(の状態)",type:"word" },
                    {english: "ill", japanese: "病気で，気分が悪い",type:"word" },
                    {english: "pain", japanese: "痛み；苦痛",type:"word" },
                    {english: "injure", japanese: "を傷つける，痛める",type:"word" },
                    {english: "headache", japanese: "頭痛；悩みの種" ,type:"word"},
                    {english: "cancer", japanese: "癌",type:"word" },
                    {english: "breathe", japanese: "呼吸する；を吸い込む",type:"word" },
                    {english: "touch", japanese: "に触れる；を感動させる",type:"word" },
                    {english: "pat", japanese: "（手のひらで）を(軽く)たたく",type:"word" },
                    {english: "shout", japanese: "(を)叫ぶ，大声で話す",type:"word" },
                    {english: "scream", japanese: "金切り声を出す",type:"word" },
                    {english: "whisper", japanese: "(を)ささやく，小声で話す",type:"word" },
                    {english: "bow", japanese: "おじぎをする，頭を下げる",type:"word" },
                    {english: "bend", japanese: "かがむ；(体の一部)を曲げる" ,type:"word"},
                ]
            },
             {
                id: 45,
                title: "p246~253",
                theme: "ターゲット⑪",
                words: [
                    {english: "forehead", japanese: "額",type:"word" },
                    {english: "cheek", japanese: "頬" ,type:"word"},
                    {english: "lip", japanese: "唇",type:"word" },
                    {english: "tooth", japanese: "歯",type:"word" },
                    {english: "throat", japanese: "喉",type:"word" },
                    {english: "shoulder", japanese: "肩",type:"word" },
                    {english: "chest", japanese: "胸",type:"word" },
                    {english: "elbow", japanese: "肘" ,type:"word"},
                    {english: "finger", japanese: "（手の）指",type:"word" },
                    {english: "thumb", japanese: "（手の）親指",type:"word" },
                    {english: "nail", japanese: "爪；くぎ",type:"word" },
                    {english: "toe", japanese: "（足の）指，つま先",type:"word" },
                    {english: "ankle", japanese: "足首，くるぶし",type:"word" },
                    {english: "skin", japanese: "皮膚，肌；皮" ,type:"word"},
                    {english: "brain", japanese: "脳；頭脳；優秀な人",type:"word" },
                    {english: "heart", japanese: "心臓；心；<the~>中心",type:"word" },
                    {english: "stomach", japanese: "胃；腹部" ,type:"word"},
                    {english: "blood", japanese: "血液；血統" ,type:"word"},
                    {english: "bone", japanese: "骨",type:"word" },
                    {english: "muscle", japanese: "筋肉",type:"word" },
                    {english: "emotion", japanese: "感情，感動",type:"word" },
                    {english: "mind", japanese: "心，精神；意見",type:"word" },
                    {english: "mental", japanese: "心の，精神の",type:"word" },
                    {english: "pleasant", japanese: "楽しい；好感のある",type:"word" },
                    {english: "suffer", japanese: "苦しむ；(苦痛など)を経験する",type:"word" },
                    {english: "upset", japanese: "取り乱して，動転して" ,type:"word"},
                    {english: "nervous", japanese: "心配して，緊張して；神経質な；神経の",type:"word" },
                    {english: "lonely", japanese: "孤独な，ひとりぼっちの",type:"word" },
                    {english: "shocked", japanese: "衝撃[ショック]を受けた",type:"word" },
                    {english: "stress", japanese: "(精神的)ストレス；(語・音声の)強勢",type:"word" },
                    {english: "mad", japanese: "怒って；ばかげた",type:"word" },
                ]
            },
             {
                id: 46,
                title: "p254~258",
                theme: "ターゲット⑫",
                words: [
                    {english: "anger", japanese: "怒り",type:"word" },
                    {english: "joy", japanese: "喜び",type:"word" },
                    {english: "relaxed", japanese: "くつろいだ",type:"word" },
                    {english: "fear", japanese: "恐怖；不安；恐れ，懸念",type:"word" },
                    {english: "panic", japanese: "パニック，恐怖心",type:"word" },
                    {english: "character", japanese: "性格；特徴；登場人物；文字",type:"word" },
                    {english: "humor", japanese: "ユーモア" ,type:"word"},
                    {english: "frank", japanese: "率直な",type:"word" },
                    {english: "cheerful", japanese: "元気な，陽気な；心地よい" ,type:"word"},
                    {english: "friendly", japanese: "親切な，好意的な；友好的な；仲のよい",type:"word" },
                    {english: "gentle", japanese: "優しい；穏やかな",type:"word" },
                    {english: "calm", japanese: "落ち着いた；穏やかな",type:"word" },
                    {english: "lively", japanese: "元気な，活発な",type:"word" },
                    {english: "shy", japanese: "恥ずかしがりの",type:"word" },
                    {english: "strict", japanese: "厳しい，厳格な",type:"word" },
                    {english: "positive", japanese: "前向きの，積極的な；肯定的な，有益な",type:"word" },
                    {english: "negative", japanese: "悲観的な，消極的な；否定的な，否定の",type:"word" },
                    {english: "active", japanese: "活動的な，活発な；積極[自発]的な；活動中の",type:"word" },
                    {english: "lazy", japanese: "怠惰な；のんびりした",type:"word" },
                    {english: "communication", japanese: "コミュニケーション，意思疎通；<~s>通信手段",type:"word" },
                    {english: "greet", japanese: "に挨拶する，を出迎える",type:"word" },
                    {english: "conversation", japanese: "会話，おしゃべり",type:"word" },
                    {english: "chat", japanese: "おしゃべりする；チャットする",type:"word" },
                    {english: "text", japanese: "(ショート)メッセージを送る",type:"word" },
                    {english: "e-mail", japanese: "E[電子]メール",type:"word" },
                ]
            },
             {
                id: 47,
                title: "p260~264",
                theme: "ターゲット⑫",
                words: [
                    {english: "address", japanese: "住所，アドレス；演説",type:"word" },
                    {english: "translate", japanese: "(を)翻訳する",type:"word" },
                    {english: "argue", japanese: "口論する，言い争う" ,type:"word"},
                    {english: "claim", japanese: "主張する；を要求する" ,type:"word"},
                    {english: "insist", japanese: "(を)強く主張する；を要求する" ,type:"word"},
                    {english: "praise", japanese: "を褒める，賞賛する",type:"word" },
                    {english: "debate", japanese: "討論，ディベート",type:"word" },
                    {english: "blame", japanese: "を非難する；のせい[責任]だとする",type:"word" },
                    {english: "joke", japanese: "冗談",type:"word" },
                    {english: "pronounce", japanese: "を(正しく)発音する",type:"word" },
                    {english: "express", japanese: "を言い表す",type:"word" },
                    {english: "state", japanese: "をはっきりと述べる，表明する",type:"word" },
                    {english: "define", japanese: "を定義する；を明確にする",type:"word" },
                    {english: "describe", japanese: "の特徴を述べる；だと表現する，称する",type:"word" },
                    {english: "refer", japanese: "に言及する；を参照する",type:"word" },
                    {english: "predict", japanese: "を予測[予言]する",type:"word" },
                    {english: "comment", japanese: "論評，コメント" ,type:"word"},
                    {english: "term", japanese: "(専門)用語；学期；期間",type:"word" },
                    {english: "publish", japanese: "を出版する；を掲載する",type:"word" },
                    {english: "novel", japanese: "(短編)小説",type:"word" },
                    {english: "fiction", japanese: "フィクション，小説；作り事",type:"word" },
                    {english: "essay", japanese: "小論文，レポート；エッセイ，評論",type:"word" },
                    {english: "newspaper", japanese: "新聞",type:"word" },
                    {english: "magazine", japanese: "雑誌" ,type:"word"},
                    {english: "journal", japanese: "専門誌；定期刊行物",type:"word" },
                ]
            },
             {
                id: 48,
                title: "p266~270",
                theme: "ターゲット⑫",
                words: [
                    {english: "article", japanese: "記事；品物；冠詞",type:"word" },
                    {english: "title", japanese: "題名，タイトル；敬称，肩書き",type:"word" },
                    {english: "poem", japanese: "(1編の)詩",type:"word" },
                    {english: "tale", japanese: "話，物語",type:"word" },
                    {english: "chapter", japanese: "章",type:"word" },
                    {english: "education", japanese: "教育",type:"word" },
                    {english: "knowledge", japanese: "知識" ,type:"word"},
                    {english: "intelligent", japanese: "知能の高い；知能のある",type:"word" },
                    {english: "logic", japanese: "論理，論法；論理学",type:"word" },
                    {english: "talent", japanese: "才能；才能のある人々",type:"word" },
                    {english: "master", japanese: "を習得する" ,type:"word"},
                    {english: "solve", japanese: "を解く，解答する；を解決する",type:"word" },
                    {english: "review", japanese: "(を)復習する；を論評する",type:"word" },
                    {english: "textbook", japanese: "教科書",type:"word" },
                    {english: "dictionary", japanese: "辞書",type:"word" },
                    {english: "lecture", japanese: "講義；説教" ,type:"word"},
                    {english: "subject", japanese: "科目；主題，話題；主語",type:"word" },
                    {english: "mathematics", japanese: "数学；計算",type:"word" },
                    {english: "biology", japanese: "生物学",type:"word" },
                    {english: "elementary", japanese: "初等の；初歩の",type:"word" },
                    {english: "college", japanese: "(単科)大学；専門学校",type:"word" },
                    {english: "university", japanese: "(総合)大学",type:"word" },
                    {english: "scholar", japanese: "学者",type:"word" },
                    {english: "enter", japanese: "に入学する，加入する；(に)入る",type:"word" },
                    {english: "attend", japanese: "(に)出席する，(学校など)に通う；注意を向ける",type:"word" },
                    {english: "absent", japanese: "欠席の",type:"word" },
                ]
            },
             {
                id: 49,
                title: "p272~276",
                theme: "ターゲット⑬",
                words: [
                    {english: "graduate", japanese: "卒業する",type:"word" },
                    {english: "grade", japanese: "成績，評点；学年；等級",type:"word" },
                    {english: "quiz", japanese: "小テスト；(テレビなどの)クイズ",type:"word" },
                    {english: "homework", japanese: "宿題",type:"word" },
                    {english: "science", japanese: "科学；理科",type:"word" },
                    {english: "chemical", japanese: "化学(上)の",type:"word" },
                    {english: "experiment", japanese: "実験",type:"word" },
                    {english: "element", japanese: "元素；要素，要因",type:"word" },
                    {english: "oxygen", japanese: "酸素",type:"word" },
                    {english: "technology", japanese: "科学技術，テクノロジー",type:"word" },
                    {english: "advance", japanese: "進歩；前進",type:"word" },
                    {english: "machine", japanese: "機械(装置)" ,type:"word"},
                    {english: "automatic", japanese: "自動(式)の" ,type:"word"},
                    {english: "invent", japanese: "を発明する",type:"word" },
                    {english: "operate", japanese: "を操作する；作動する；手術する",type:"word" },
                    {english: "artificial", japanese: "人工の",type:"word" },
                    {english: "web", japanese: "ウェブ；クモの巣",type:"word" },
                    {english: "material", japanese: "材料，原料；資料；生地",type:"word" },
                    {english: "resource", japanese: "資源；資料",type:"word" },
                    {english: "energy", japanese: "エネルギー；活力" ,type:"word"},
                    {english: "electricity", japanese: "電気，電力" ,type:"word"},
                    {english: "battery", japanese: "バッテリー，電池",type:"word" },
                    {english: "oil", japanese: "石油，原油；(食用)油，オイル",type:"word" },
                    {english: "gas", japanese: "ガス；ガソリン；気体",type:"word" },
                    {english: "coal", japanese: "石炭" ,type:"word"},
                    {english: "metal", japanese: "金属",type:"word" },
                ]
            },
             {
                id: 50,
                title: "p278~282",
                theme: "ターゲット⑬",
                words: [
                    {english: "steel", japanese: "鋼鉄",type:"word" },
                    {english: "nuclear", japanese: "核エネルギーの，原子力の；核兵器の",type:"word" },
                    {english: "universe", japanese: "宇宙；全世界" ,type:"word"},
                    {english: "planet", japanese: "惑星；地球，世界",type:"word" },
                    {english: "astronaut", japanese: "宇宙飛行士",type:"word" },
                    {english: "earth", japanese: "地球，地面，地上",type:"word" },
                    {english: "cash", japanese: "現金",type:"word" },
                    {english: "earn", japanese: "(働いて)(お金を)を得る；(名声など)を得る",type:"word" },
                    {english: "reward", japanese: "報酬，ほうび",type:"word" },
                    {english: "income", japanese: "収入，所得" ,type:"word"},
                    {english: "budget", japanese: "予算，経費",type:"word" },
                    {english: "tax", japanese: "税金",type:"word" },
                    {english: "consume", japanese: "を消費する",type:"word" },
                    {english: "benefit", japanese: "利益，恩恵",type:"word" },
                    {english: "wealth", japanese: "富，財産",type:"word" },
                    {english: "price", japanese: "価格",type:"word" },
                    {english: "cheap", japanese: "安い；安っぽい" ,type:"word"},
                    {english: "reasonable", japanese: "（値段が）手頃な；道理をわきまえた；筋の通った",type:"word" },
                    {english: "sale", japanese: "特売；販売；売上",type:"word" },
                    {english: "charge", japanese: "（サービスへの）料金；責任；告発",type:"word" },
                    {english: "advertisement", japanese: "広告，宣伝" ,type:"word"},
                    {english: "commercial", japanese: "営利[商業]的な；商業(上)の",type:"word" },
                    {english: "trade", japanese: "貿易，取引；交換",type:"word" },
                    {english: "import", japanese: "を輸入する",type:"word" },
                    {english: "export", japanese: "を輸出する",type:"word" },
                    {english: "factory", japanese: "工場" ,type:"word"},
                ]
            },
             {
                id: 51,
                title: "p284~288",
                theme: "ターゲット⑬",
                words: [
                    {english: "agriculture", japanese: "農業" ,type:"word"},
                    {english: "society", japanese: "社会；協会；社交界",type:"word" },
                    {english: "community", japanese: "地域社会；共同体",type:"word" },
                    {english: "organization", japanese: "組織，団体" ,type:"word"},
                    {english: "committee", japanese: "委員会，委員" ,type:"word"},
                    {english: "charity", japanese: "慈善行為[事業]；慈善団体",type:"word" },
                    {english: "citizen", japanese: "国民，市民",type:"word" },
                    {english: "duty", japanese: "義務；職務；関税",type:"word" },
                    {english: "law", japanese: "法律；〜法" ,type:"word"},
                    {english: "judge", japanese: "裁判官；審査員",type:"word" },
                    {english: "court", japanese: "法廷，裁判所；(運動施設の)コート",type:"word" },
                    {english: "guard", japanese: "警戒，見張り；警備員；防護物" },
                    {english: "arrest", japanese: "を逮捕する",type:"word" },
                    {english: "punish", japanese: "を罰する",type:"word" },
                    {english: "crime", japanese: "犯罪",type:"word" },
                    {english: "murder", japanese: "殺人(事件)",type:"word" },
                    {english: "shoot", japanese: "（銃で）(を)撃つ；シュートする",type:"word" },
                    {english: "steal", japanese: "を盗む",type:"word" },
                    {english: "rob", japanese: "を襲って奪う",type:"word" },
                    {english: "thief", japanese: "泥棒",type:"word" },
                    {english: "victim", japanese: "犠牲[被害]者",type:"word" },
                    {english: "drug", japanese: "薬物，麻薬；薬" ,type:"word"},
                    {english: "poverty", japanese: "貧乏，貧困" ,type:"word"},
                    {english: "government", japanese: "政府；政治",type:"word" },
                    {english: "policy", japanese: "政策，方針；主義",type:"word" },
                    {english: "nation", japanese: "国家，国；国民" ,type:"word"},
                ]
            },
             {
                id: 52,
                title: "p290~294",
                theme: "ターゲット⑭",
                words: [
                    {english: "capital", japanese: "首都；中心部；大文字；資本",type:"word" },
                    {english: "international", japanese: "国際的な，国家間の",type:"word" },
                    {english: "global", japanese: "全世界の，地球全体の",type:"word" },
                    {english: "election", japanese: "選挙",type:"word" },
                    {english: "vote", japanese: "投票をする；を投票で決める",type:"word" },
                    {english: "president", japanese: "大統領，総統；社長；会長",type:"word" },
                    {english: "liberty", japanese: "自由",type:"word" },
                    {english: "fight", japanese: "(と)戦う；(と)けんかする",type:"word" },
                    {english: "war", japanese: "戦争(状態)；争い，戦い",type:"word" },
                    {english: "military", japanese: "軍(隊)の，軍用の",type:"word" },
                    {english: "army", japanese: "軍隊，陸軍",type:"word" },
                    {english: "soldier", japanese: "兵士；軍人",type:"word" },
                    {english: "weapon", japanese: "武器，兵器" ,type:"word"},
                    {english: "bomb", japanese: "爆弾",type:"word" },
                    {english: "break out", japanese: "突然起こる，勃発する", type: "phrase" },
                    {english: "come across~", japanese: "〜に(偶然)会う，〜をふと見つける" , type: "phrase"},
                    {english: "come out", japanese: "発売される;現れる", type: "phrase" },
                    {english: "come up with~", japanese: "〜を思いつく", type: "phrase" },
                    {english: "count on [upon]~", japanese: "〜に頼る，〜を当てにする", type: "phrase" },
                    {english: "cut down [back] on~", japanese: "〜を減らす" , type: "phrase"},
                    {english: "die out", japanese: "（消えて）なくなる，絶滅する", type: "phrase" },
                    {english: "drop in (on~)", japanese: "(〜を)ちょっと訪れる" , type: "phrase"},
                    {english: "feel free to do", japanese: "自由に[遠慮なく]…する", type: "phrase" },
                    {english: "get along (with~)", japanese: "仲よくやっている；うまくやっていく", type: "phrase" },
                    {english: "get out of~", japanese: "〜から逃れる；〜をやめる", type: "phrase" },
                ]
            },
             {
                id: 53,
                title: "p296~300",
                theme: "ターゲット⑭",
                words: [
                    {english: "get over~", japanese: "〜から回復する，立ち直る；〜を克服する", type: "phrase" },
                    {english: "get through~", japanese: "〜を切り抜ける；〜を終える", type: "phrase" },
                    {english: "give way (to~)", japanese: "譲歩する，屈する；〜に取って代わられる", type: "phrase" },
                    {english: "go along with~", japanese: "〜に賛成する，を支持する" , type: "phrase"},
                    {english: "head for~", japanese: "〜に向かう", type: "phrase" },
                    {english: "keep [bear]~ in mind", japanese: "〜を心に留めておく", type: "phrase" },
                    {english: "keep [stay] in touch (with~)", japanese: "(〜と)連絡を取り続ける", type: "phrase" },
                    {english: "learn (how) to do", japanese: "…できるようになる，⋯の仕方を習う[覚える]" , type: "phrase"},
                    {english: "look back (on~)", japanese: "(〜を)回想する，振り返る", type: "phrase" },
                    {english: "look into~", japanese: "〜を調査する；〜を覗き込む", type: "phrase" },
                    {english: "look over~/look~over", japanese: "〜をざっと調べる，〜に目を通す", type: "phrase" },
                    {english: "look up to~", japanese: "〜を尊敬する", type: "phrase" },
                    {english: "make a difference (to~)", japanese: "(〜にとって)影響がある，重要である", type: "phrase" },
                    {english: "make [earn] a [one's]living", japanese: "生計を立てる", type: "phrase" },
                    {english: "make up(~)/make~up", japanese: "〜を構成する" , type: "phrase"},
                    {english: "pass away", japanese: "亡くなる；滅びる" , type: "phrase"},
                    {english: "point out~/put~out", japanese: "〜を指摘する", type: "phrase" },
                    {english: "put off~/put~off", japanese: "〜を延期する", type: "phrase" },
                    {english: "put together~/put~together", japanese: "〜を組み立てる，まとめ上げる", type: "phrase" },
                    {english: "put up with~", japanese: "〜を我慢する" , type: "phrase"},
                    {english: "run after~", japanese: "〜を追いかける", type: "phrase" },
                    {english: "run away (from~)", japanese: "(〜から)逃げる", type: "phrase" },
                    {english: "run out of~", japanese: "〜を使い果たす", type: "phrase" },
                    {english: "stand for~", japanese: "〜を意味する，〜の略称である", type: "phrase" },
                    {english: "stand out", japanese: "ずば抜けている；目立つ" , type: "phrase"},
                    {english: "take A for B", japanese: "AをBだと思う[思い込む]", type: "phrase" },
                    {english: "take place", japanese: "起こる，行われる" , type: "phrase"},
                    {english: "take up~/take~up", japanese: "〜を始める；〜に就く；〜を取り上げる", type: "phrase" },
                    {english: "work on~", japanese: "〜に取り組む；〜に働きかける", type: "phrase" },
                    {english: "all at once", japanese: "（予期せず）突然；いっせいに", type: "phrase" },
                    {english: "all the way", japanese: "はるばる，ずっと", type: "phrase" },
                    {english: "along with~", japanese: "〜と一緒に，〜に加えて", type: "phrase" },
                ]
            },
             {
                id: 54,
                title: "p302~304",
                theme: "ターゲット⑭",
                words: [
                    {english: "at least", japanese: "少なくとも", type: "phrase" },
                    {english: "at (the) most", japanese: "せいぜい，多くても" , type: "phrase"},
                    {english: "by way of~", japanese: "〜を通って；〜の手[手段]で" , type: "phrase"},
                    {english: "for some time", japanese: "かなり長い間；しばらくの間" , type: "phrase"},
                    {english: "face to face (with)", japanese: "面と向かって；直面して" , type: "phrase"},
                    {english: "first of all", japanese: "まず第一に；何よりもまず", type: "phrase" },
                    {english: "in advance", japanese: "あらかじめ，〜前に；前金で", type: "phrase" },
                    {english: "in all", japanese: "全部で，合計で", type: "phrase" },
                    {english: "in place of~/in ~'s place", japanese: "〜の代わりに", type: "phrase" },
                    {english: "in return (for~)", japanese: "お返しに", type: "phrase" },
                    {english: "in the long run", japanese: "長い目で見れば，結局は", type: "phrase" },
                    {english: "in time (for~)", japanese: "間に合うように，遅れずに", type: "phrase" },
                    {english: "on sale", japanese: "販売されて；特売で", type: "phrase" },
                    {english: "on time", japanese: "時間通りに，定刻に" , type: "phrase"},
                    {english: "one by one", japanese: "１つ[１人]ずつ" , type: "phrase"},
                    {english: "out of the question", japanese: "論外で，不可能で", type: "phrase" },
                    {english: "side by side (with~)", japanese: "（横に）並んで；一緒に" , type: "phrase"},
                ]
            },
             {
                id: 55,
                title: "p306",
                theme: "ターゲット⑭",
                words: [
                    {english: "circle", japanese: "丸・円(形)",type:"word" },
                    {english: "semicircle", japanese: "半円(形)" ,type:"word"},
                    {english: "oval", japanese: "楕円(形)",type:"word" },
                    {english: "triangle", japanese: "三角(形)",type:"word" },
                    {english: "square", japanese: "四角(形)",type:"word" },
                    {english: "rectangle", japanese: "長方形" ,type:"word"},
                    {english: "pentagon", japanese: "五角形",type:"word" },
                    {english: "haxagon", japanese: "六角形",type:"word" },
                    {english: "octagon", japanese: "八角形" ,type:"word"},
                    {english: "round/sphere", japanese: "球",type:"word" },
                    {english: "hemisphere", japanese: "半球",type:"word" },
                    {english: "cylinder", japanese: "円柱",type:"word" },
                    {english: "cube", japanese: "立方体",type:"word" },
                    {english: "pyramid", japanese: "三角錐",type:"word" },
                    {english: "cone", japanese: "円錐",type:"word" },
                ]
            },
        ];

      // データを保存
        function saveData() {
            try {
                localStorage.setItem('vocabAppStats', JSON.stringify(stats));
                localStorage.setItem('vocabAppMistakes', JSON.stringify(savedMistakes));
                localStorage.setItem('vocabAppCompletedLessons', JSON.stringify([...completedLessons]));
            } catch (error) {
                console.log('データの保存に失敗しました:', error);
                // localStorageが使用できない場合のフォールバック（メモリ保存）
                window.vocabAppData = {
                    stats: JSON.stringify(stats),
                    mistakes: JSON.stringify(savedMistakes),
                    completedLessons: JSON.stringify([...completedLessons])
                };
            }
        }

        // 統計情報を読み込み
        function loadStats() {
            document.getElementById('totalWords').textContent = stats.totalWords || 0;
            document.getElementById('correctRate').textContent = (stats.correctRate || 0) + '%';
            document.getElementById('completedLessons').textContent = completedLessons.size || 0;
            document.getElementById('mistakesButton').textContent = `間違いリスト (${savedMistakes.length})`;
        }

        // 統計情報を保存
        function saveStats(totalWords, correctAnswers) {
            stats.totalWords = (stats.totalWords || 0) + totalWords;
            stats.totalAnswers = (stats.totalAnswers || 0) + totalWords;
            stats.correctAnswers = (stats.correctAnswers || 0) + correctAnswers;
            stats.correctRate = stats.totalAnswers > 0 ? Math.round((stats.correctAnswers / stats.totalAnswers) * 100) : 0;
            saveData();
            loadStats();
        }

        // グローバル変数
        let currentMode = 'enToJp';
        let currentLesson = null;
        let currentQuiz = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let mistakes = [];
        let isAnswered = false;
        let selectedQuizData = [];
        let selectedMistakeItems = [];
        let isFromMistakes = false;

        // データ管理
        let stats = {
            totalWords: 0,
            correctRate: 0,
            completedLessons: 0,
            totalAnswers: 0,
            correctAnswers: 0
        };
        let savedMistakes = [];
        let completedLessons = new Set(); // 完了したレッスンのIDを保存

        // データを読み込み
        function loadData() {
            try {
                // localStorageからデータを読み込み
                const savedStats = localStorage.getItem('vocabAppStats');
                if (savedStats) {
                    stats = JSON.parse(savedStats);
                }
                
                const savedMistakesData = localStorage.getItem('vocabAppMistakes');
                if (savedMistakesData) {
                    savedMistakes = JSON.parse(savedMistakesData);
                }
                
                const savedCompletedLessons = localStorage.getItem('vocabAppCompletedLessons');
                if (savedCompletedLessons) {
                    completedLessons = new Set(JSON.parse(savedCompletedLessons));
                }
            } catch (error) {
                console.log('データの読み込みに失敗しました:', error);
                // フォールバック: 初期値を設定
                stats = {
                    totalWords: 0,
                    correctRate: 0,
                    completedLessons: 0,
                    totalAnswers: 0,
                    correctAnswers: 0
                };
                savedMistakes = [];
                completedLessons = new Set();
            }
        }

        // データを保存
        function saveData() {
            try {
                localStorage.setItem('vocabAppStats', JSON.stringify(stats));
                localStorage.setItem('vocabAppMistakes', JSON.stringify(savedMistakes));
                localStorage.setItem('vocabAppCompletedLessons', JSON.stringify([...completedLessons]));
            } catch (error) {
                console.log('データの保存に失敗しました:', error);
                // localStorageが使用できない場合のフォールバック
                window.vocabAppData = {
                    stats: JSON.stringify(stats),
                    mistakes: JSON.stringify(savedMistakes),
                    completedLessons: JSON.stringify([...completedLessons])
                };
            }
        }

        // 統計情報を読み込み
        function loadStats() {
            document.getElementById('totalWords').textContent = stats.totalWords || 0;
            document.getElementById('correctRate').textContent = (stats.correctRate || 0) + '%';
            document.getElementById('completedLessons').textContent = completedLessons.size || 0;
            document.getElementById('mistakesButton').textContent = `間違いリスト (${savedMistakes.length})`;
        }

        // 統計情報を保存
        function saveStats(totalWords, correctAnswers) {
            stats.totalWords = (stats.totalWords || 0) + totalWords;
            stats.totalAnswers = (stats.totalAnswers || 0) + totalWords;
            stats.correctAnswers = (stats.correctAnswers || 0) + correctAnswers;
            stats.correctRate = stats.totalAnswers > 0 ? Math.round((stats.correctAnswers / stats.totalAnswers) * 100) : 0;
            saveData();
            loadStats();
        }

        // レッスン完了を記録
        function markLessonAsCompleted(lessonId) {
            if (!completedLessons.has(lessonId)) {
                completedLessons.add(lessonId);
                stats.completedLessons = completedLessons.size;
                saveData();
                loadStats();
            }
        }

        // レッスンが完了しているかチェック
        function isLessonCompleted(lessonId) {
            return completedLessons.has(lessonId);
        }

        // 間違いをストレージから削除
        function removeMistakeFromStorage(questionText, correctAnswer) {
            savedMistakes = savedMistakes.filter(mistake => 
                !(mistake.question === questionText && mistake.correctAnswer === correctAnswer)
            );
            saveData();
            loadStats();
        }

        // クイズの状態を完全にリセット
        function resetQuizState() {
            currentQuiz = [];
            currentQuestionIndex = 0;
            score = 0;
            mistakes = [];
            isAnswered = false;
            selectedQuizData = [];
            selectedMistakeItems = [];
            currentLesson = null;
            currentMode = 'enToJp';
            isFromMistakes = false;
        }

        // 全ての画面を非表示にする
        function hideAllScreens() {
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('lessonSelector').style.display = 'none';
            document.getElementById('modeSelection').classList.add('hidden');
            document.getElementById('quizContainer').style.display = 'none';
            document.getElementById('resultContainer').style.display = 'none';
            document.getElementById('mistakesListView').style.display = 'none';
        }

        // メインメニューを表示
        function showMainMenu() {
            resetQuizState();
            hideAllScreens();
            document.getElementById('mainMenu').style.display = 'flex';
            loadStats();
        }

        // レッスン選択画面を表示
        function showLessonSelector() {
            resetQuizState();
            hideAllScreens();
            document.getElementById('lessonSelector').style.display = 'block';
            renderLessons();
        }

        // レッスン一覧を描画
        function renderLessons() {
            const grid = document.getElementById('lessonGrid');
            grid.innerHTML = '';
            
            lessons.forEach(lesson => {
                const card = document.createElement('div');
                card.className = 'lesson-card';
                
                // 完了状態をチェック
                if (isLessonCompleted(lesson.id)) {
                    card.classList.add('completed');
                }
                
                card.onclick = function() { selectLesson(lesson.id, this); };
                
                const wordsCount = lesson.words.length;
                const phraseCount = lesson.words.filter(w => w.type === 'phrase').length;
                const wordCount = lesson.words.filter(w => w.type === 'word').length;
                
                // 進捗率を計算（完了していれば100%）
                const progressWidth = isLessonCompleted(lesson.id) ? 100 : 0;
                
                card.innerHTML = `
                    <div class="lesson-number">レッスン ${lesson.id}</div>
                    <div class="lesson-title">${lesson.title}</div>
                    <div class="lesson-info">${lesson.theme}</div>
                    <div class="lesson-info">単語: ${wordCount}個 / 熟語: ${phraseCount}個</div>
                    <div class="lesson-progress">
                        <div class="lesson-progress-fill" style="width: ${progressWidth}%"></div>
                    </div>
                `;
                
                grid.appendChild(card);
            });
        }

        // レッスンを選択
        function selectLesson(lessonId, cardElement) {
            currentLesson = lessons.find(l => l.id === lessonId);
            
            // 選択状態を更新
            document.querySelectorAll('.lesson-card').forEach(card => {
                card.classList.remove('selected');
            });
            
            // クリックされたカードを選択状態にする
            if (cardElement) {
                cardElement.classList.add('selected');
            }
            
            showModeSelection('lesson');
        }

        // モード選択画面を表示
        function showModeSelection(type) {
            hideAllScreens();
            document.getElementById('modeSelection').classList.remove('hidden');
            
            // モードボタンの選択状態をリセット
            document.querySelectorAll('.mode-button').forEach(btn => {
                btn.classList.remove('selected');
            });
            
            // 全レッスンテストの場合
            if (type === 'test') {
                currentLesson = null;
                selectedQuizData = lessons.flatMap(lesson => lesson.words);
                isFromMistakes = false;
            } else if (type === 'mistakes') {
                isFromMistakes = true;
            }
        }

        // モードを選択
        function selectMode(mode) {
            currentMode = mode;
            
            // 選択状態を更新
            document.querySelectorAll('.mode-button').forEach(btn => {
                btn.classList.remove('selected');
            });
            event.target.closest('.mode-button').classList.add('selected');
        }

        // モード選択から戻る
        function goBackFromMode() {
            if (currentLesson) {
                showLessonSelector();
            } else if (isFromMistakes) {
                showMistakesList();
            } else {
                showMainMenu();
            }
        }

        // 間違いリストを表示
        function showMistakesList() {
            resetQuizState();
            hideAllScreens();
            document.getElementById('mistakesListView').style.display = 'block';
            
            const container = document.getElementById('mistakesListContainer');
            container.innerHTML = '';
            
            if (savedMistakes.length === 0) {
                container.innerHTML = '<p style="text-align: center; color: #666; margin: 20px 0;">間違いリストは空です。</p>';
                document.getElementById('startMistakesQuizBtn').style.display = 'none';
                return;
            }
            
            selectedMistakeItems = [];
            
            savedMistakes.forEach((mistake, index) => {
                const item = document.createElement('div');
                item.className = 'mistakes-quiz-item';
                item.onclick = () => toggleMistakeSelection(index, item);
                
                const typeLabel = mistake.type === 'phrase' ? '[熟語]' : '[単語]';
                item.innerHTML = `
                    <div class="mistake-question">${typeLabel} ${mistake.question}</div>
                    <div class="mistake-details">
                        間違えた答え: ${mistake.userAnswer} → 正解: ${mistake.correctAnswer}
                    </div>
                `;
                
                container.appendChild(item);
            });
            
            updateMistakesQuizButton();
        }

        // 間違いアイテムの選択を切り替え
        function toggleMistakeSelection(index, element) {
            const mistake = savedMistakes[index];
            
            if (selectedMistakeItems.some(item => item.question === mistake.question && item.correctAnswer === mistake.correctAnswer)) {
                selectedMistakeItems = selectedMistakeItems.filter(item => 
                    !(item.question === mistake.question && item.correctAnswer === mistake.correctAnswer)
                );
                element.classList.remove('selected');
            } else {
                selectedMistakeItems.push(mistake);
                element.classList.add('selected');
            }
            
            updateMistakesQuizButton();
        }

        // 間違いクイズボタンの表示を更新
        function updateMistakesQuizButton() {
            const btn = document.getElementById('startMistakesQuizBtn');
            if (selectedMistakeItems.length > 0) {
                btn.style.display = 'inline-block';
                btn.textContent = `選択した問題をテスト (${selectedMistakeItems.length}問)`;
            } else {
                btn.style.display = 'none';
            }
        }

        // 間違いクイズを開始
        function startMistakesQuiz() {
            if (selectedMistakeItems.length === 0) {
                alert('問題を選択してください');
                return;
            }
            
            showModeSelection('mistakes');
        }

        // クイズを開始
        function startQuiz() {
            // モードが選択されているかチェック
            if (!document.querySelector('.mode-button.selected')) {
                alert('モードを選択してください');
                return;
            }

            // クイズデータを準備
            if (isFromMistakes) {
                selectedQuizData = selectedMistakeItems.map(mistake => ({
                    english: mistake.correctAnswer,
                    japanese: mistake.question,
                    type: mistake.type
                }));
            } else if (currentLesson) {
                selectedQuizData = [...currentLesson.words];
            } else {
                selectedQuizData = lessons.flatMap(lesson => lesson.words);
            }

            // タイピングモードの場合は単語のみに絞る
            if (currentMode === 'typing') {
                selectedQuizData = selectedQuizData.filter(item => item.type === 'word');
                if (selectedQuizData.length === 0) {
                    alert('このレッスンにはタイピング対象の単語がありません');
                    return;
                }
            }

            // クイズデータをシャッフル
            currentQuiz = shuffleArray([...selectedQuizData]).slice(0, Math.min(10, selectedQuizData.length));
            currentQuestionIndex = 0;
            score = 0;
            mistakes = [];
            isAnswered = false;

            hideAllScreens();
            document.getElementById('quizContainer').style.display = 'block';
            
            updateQuizHeader();
            showQuestion();
        }

        // 配列をシャッフル
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // クイズヘッダーを更新
        function updateQuizHeader() {
            let lessonInfo;
            if (isFromMistakes) {
                lessonInfo = '間違いリスト復習';
            } else if (currentLesson) {
                lessonInfo = `${currentLesson.title} - ${currentLesson.theme}`;
            } else {
                lessonInfo = '全レッスンテスト';
            }
            document.getElementById('currentLessonInfo').textContent = lessonInfo;
            
            const progress = ((currentQuestionIndex) / currentQuiz.length) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
            document.getElementById('questionCounter').textContent = 
                `${currentQuestionIndex + 1}/${currentQuiz.length}`;
        }

        // 問題を表示
        function showQuestion() {
            if (currentQuestionIndex >= currentQuiz.length) {
                showResult();
                return;
            }

            const question = currentQuiz[currentQuestionIndex];
            isAnswered = false;

            // UI要素をリセット
            document.getElementById('optionsContainer').style.display = 'grid';
            document.getElementById('typingContainer').style.display = 'none';
            document.getElementById('nextButton').style.display = 'none';

            if (currentMode === 'typing') {
                showTypingQuestion(question);
            } else {
                showMultipleChoiceQuestion(question);
            }

            updateQuizHeader();
        }

        // タイピング問題を表示
        function showTypingQuestion(question) {
            document.getElementById('optionsContainer').style.display = 'none';
            document.getElementById('typingContainer').style.display = 'block';
            
            document.getElementById('questionText').textContent = question.japanese;
            document.getElementById('questionType').textContent = 'タイピング問題';
            
            const input = document.getElementById('typingInput');
            const feedback = document.getElementById('typingFeedback');
            
            input.value = '';
            input.className = 'typing-input';
            feedback.textContent = '';
            feedback.className = 'typing-feedback';
            input.focus();
            
            // Enterキーでの送信
            input.onkeypress = function(e) {
                if (e.key === 'Enter' && !isAnswered) {
                    checkTypingAnswer();
                }
            };
        }

        // タイピング答えをチェック
        function checkTypingAnswer() {
            if (isAnswered) return;
            
            const question = currentQuiz[currentQuestionIndex];
            const userAnswer = document.getElementById('typingInput').value.trim().toLowerCase();
            const correctAnswer = question.english.toLowerCase();
            
            const input = document.getElementById('typingInput');
            const feedback = document.getElementById('typingFeedback');
            
            isAnswered = true;
            
            if (userAnswer === correctAnswer) {
                score++;
                input.className = 'typing-input correct';
                feedback.textContent = '正解！';
                feedback.className = 'typing-feedback correct';
                
                // 間違いリストから削除
                if (isFromMistakes) {
                    removeMistakeFromStorage(question.japanese, question.english);
                }
            } else {
                input.className = 'typing-input incorrect';
                feedback.textContent = `不正解。正解: ${question.english}`;
                feedback.className = 'typing-feedback incorrect';
                mistakes.push({
                    question: question.japanese,
                    userAnswer: userAnswer,
                    correctAnswer: question.english,
                    type: question.type
                });
            }
            
            document.getElementById('nextButton').style.display = 'inline-block';
        }

        // 選択問題を表示
        function showMultipleChoiceQuestion(question) {
            const isEnToJp = currentMode === 'enToJp';
            
            document.getElementById('questionText').textContent = 
                isEnToJp ? question.english : question.japanese;
            document.getElementById('questionType').textContent = 
                isEnToJp ? '日本語の意味を選んでください' : '英語を選んでください';

            // 選択肢を生成
            const options = generateOptions(question, isEnToJp);
            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';

            options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectOption(index, option, question);
                optionsContainer.appendChild(optionElement);
            });
        }

        // 選択肢を生成（改良版）
        function generateOptions(correctQuestion, isEnToJp) {
            const correctAnswer = isEnToJp ? correctQuestion.japanese : correctQuestion.english;
            
            // 全レッスンから候補を収集
            const allWords = lessons.flatMap(lesson => lesson.words);
            
            // 現在の問題を除外
            const candidates = allWords.filter(word => 
                word.english !== correctQuestion.english && 
                word.japanese !== correctQuestion.japanese
            );
            
            // 同じタイプ（単語/熟語）を優先的に選択
            const sameTypeCandidates = candidates.filter(word => word.type === correctQuestion.type);
            const otherTypeCandidates = candidates.filter(word => word.type !== correctQuestion.type);
            
            // まずは同じタイプから選択、足りない場合は他のタイプからも選択
            let selectedCandidates = [];
            const shuffledSameType = shuffleArray([...sameTypeCandidates]);
            const shuffledOtherType = shuffleArray([...otherTypeCandidates]);
            
            // 最大3つの間違いオプションを生成
            selectedCandidates = [...shuffledSameType, ...shuffledOtherType].slice(0, 3);
            
            // 選択肢が足りない場合のフォールバック
            if (selectedCandidates.length < 3) {
                const fallbackOptions = [
                    isEnToJp ? "該当なし" : "no match",
                    isEnToJp ? "不明" : "unknown",
                    isEnToJp ? "その他" : "other"
                ];
                
                while (selectedCandidates.length < 3) {
                    selectedCandidates.push({
                        english: fallbackOptions[selectedCandidates.length],
                        japanese: fallbackOptions[selectedCandidates.length]
                    });
                }
            }
            
            const wrongOptions = selectedCandidates.map(candidate => 
                isEnToJp ? candidate.japanese : candidate.english
            );
            
            // 正解と間違いオプションを混ぜてシャッフル
            const options = [correctAnswer, ...wrongOptions];
            return shuffleArray(options);
        }

        // 選択肢を選択
        function selectOption(index, selectedAnswer, question) {
            if (isAnswered) return;
            
            isAnswered = true;
            const isEnToJp = currentMode === 'enToJp';
            const correctAnswer = isEnToJp ? question.japanese : question.english;
            
            const options = document.querySelectorAll('.option');
            
            options.forEach((option, i) => {
                if (i === index) {
                    option.classList.add('selected');
                    if (selectedAnswer === correctAnswer) {
                        option.classList.add('correct');
                        score++;
                        
                        // 間違いリストから削除
                        if (isFromMistakes) {
                            const questionText = isEnToJp ? question.english : question.japanese;
                            removeMistakeFromStorage(questionText, correctAnswer);
                        }
                    } else {
                        option.classList.add('incorrect');
                        mistakes.push({
                            question: isEnToJp ? question.english : question.japanese,
                            userAnswer: selectedAnswer,
                            correctAnswer: correctAnswer,
                            type: question.type
                        });
                    }
                } else if (option.textContent === correctAnswer) {
                    option.classList.add('correct');
                }
            });
            
            document.getElementById('nextButton').style.display = 'inline-block';
        }

        // 次の問題へ
        function nextQuestion() {
            currentQuestionIndex++;
            showQuestion();
        }

        // クイズから戻る
        function goBackFromQuiz() {
            resetQuizState();
            showMainMenu();
        }

        // 結果を表示
        function showResult() {
            hideAllScreens();
            document.getElementById('resultContainer').style.display = 'block';
            
            const percentage = Math.round((score / currentQuiz.length) * 100);
            document.getElementById('resultScore').textContent = `${score}/${currentQuiz.length}`;
            
            let resultText = '';
            if (percentage >= 90) {
                resultText = '素晴らしい！完璧に近い成績です！';
            } else if (percentage >= 70) {
                resultText = 'よくできました！この調子で頑張りましょう！';
            } else if (percentage >= 50) {
                resultText = 'まずまずの成績です。もう少し練習してみましょう。';
            } else {
                resultText = '復習が必要です。間違えた問題を確認しましょう。';
            }
            document.getElementById('resultText').textContent = resultText;
            
            // レッスン完了判定（80%以上で完了とする）
            if (currentLesson && percentage >= 80) {
                markLessonAsCompleted(currentLesson.id);
                if (!completedLessons.has(currentLesson.id)) {
                    resultText += '\n🎉 レッスン完了！';
                    document.getElementById('resultText').textContent = resultText;
                }
            }
            
            // 間違いリストを表示
            if (mistakes.length > 0) {
                document.getElementById('mistakesSection').style.display = 'block';
                const mistakesList = document.getElementById('mistakesList');
                mistakesList.innerHTML = '';
                
                mistakes.forEach(mistake => {
                    const mistakeElement = document.createElement('div');
                    mistakeElement.className = 'mistake-item';
                    const typeLabel = mistake.type === 'phrase' ? '[熟語]' : '[単語]';
                    mistakeElement.innerHTML = `
                        <div>
                            <strong>${typeLabel} ${mistake.question}</strong><br>
                            <span style="color: #dc3545;">あなたの答え: ${mistake.userAnswer}</span>
                        </div>
                        <div style="color: #28a745; font-weight: bold;">
                            正解: ${mistake.correctAnswer}
                        </div>
                    `;
                    mistakesList.appendChild(mistakeElement);
                });
                
                // 間違いを保存（間違いリストからの復習でない場合のみ）
                if (!isFromMistakes) {
                    mistakes.forEach(mistake => {
                        if (!savedMistakes.some(saved => 
                            saved.question === mistake.question && 
                            saved.correctAnswer === mistake.correctAnswer)) {
                            savedMistakes.push(mistake);
                        }
                    });
                    saveData();
                }
            } else {
                document.getElementById('mistakesSection').style.display = 'none';
            }
            
            // 統計を保存
            saveStats(currentQuiz.length, score);
        }

        // クイズを再開
        function restartQuiz() {
            startQuiz();
        }

        // 間違いリストを消去
        function clearMistakes() {
            if (confirm('間違いリストを全て消去しますか？')) {
                savedMistakes = [];
                saveData();
                loadStats();
                showMistakesList(); // リストを再表示
                alert('間違いリストを消去しました。');
            }
        }

        // 初期化
        document.addEventListener('DOMContentLoaded', function() {
            loadData();
            showMainMenu();
        });
    </script>
</body>
</html>
