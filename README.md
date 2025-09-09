# quiz
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🧬 중학생 생물학 퀴즈</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #e8f5e8 0%, #b8e6b8 50%, #87ceeb 100%);
            min-height: 100vh;
            overflow-x: hidden;
            position: relative;
        }

        .floating-cells {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }

        .cell {
            position: absolute;
            font-size: 24px;
            animation: float 8s infinite linear;
            opacity: 0.3;
        }

        @keyframes float {
            0% {
                transform: translateY(100vh) rotate(0deg);
            }
            100% {
                transform: translateY(-100px) rotate(360deg);
            }
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            position: relative;
            z-index: 2;
        }

        .header {
            text-align: center;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
        }

        .title {
            font-size: 2.5rem;
            color: #2d5016;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        .subtitle {
            font-size: 1.2rem;
            color: #4a7c59;
            margin-bottom: 20px;
        }

        .progress-container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin: 20px 0;
        }

        .plant-progress {
            font-size: 3rem;
            transition: all 0.5s ease;
        }

        .score {
            font-size: 1.5rem;
            color: #2d5016;
            font-weight: bold;
        }

        .difficulty-selector {
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            text-align: center;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
        }

        .difficulty-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .difficulty-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
        }

        .easy { background: #90EE90; color: #2d5016; }
        .medium { background: #87CEEB; color: #1e3a8a; }
        .hard { background: #DDA0DD; color: #4c1d95; }

        .difficulty-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }

        .difficulty-btn.active {
            transform: scale(1.05);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        .quiz-container {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
            display: none;
        }

        .quiz-container.active {
            display: block;
            animation: slideIn 0.5s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .question {
            font-size: 1.4rem;
            color: #2d5016;
            margin-bottom: 25px;
            line-height: 1.6;
            text-align: center;
        }

        .options {
            display: grid;
            gap: 15px;
            margin-bottom: 25px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #87ceeb;
            border-radius: 15px;
            background: #f0f8ff;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.1rem;
            text-align: center;
        }

        .option:hover {
            background: #e6f3ff;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .option.correct {
            background: #90EE90;
            border-color: #32CD32;
            color: #2d5016;
        }

        .option.incorrect {
            background: #FFB6C1;
            border-color: #FF69B4;
            color: #8b0000;
        }

        .feedback {
            margin: 20px 0;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            font-weight: bold;
            display: none;
        }

        .feedback.correct {
            background: #90EE90;
            color: #2d5016;
        }

        .feedback.incorrect {
            background: #FFB6C1;
            color: #8b0000;
        }

        .next-btn {
            background: #32CD32;
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 1.1rem;
            cursor: pointer;
            display: none;
            margin: 0 auto;
            transition: all 0.3s ease;
        }

        .next-btn:hover {
            background: #228B22;
            transform: translateY(-2px);
        }

        .completion-screen {
            text-align: center;
            display: none;
            animation: celebration 1s ease;
        }

        @keyframes celebration {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .badge {
            font-size: 4rem;
            margin: 20px 0;
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }

        .floating-leaves {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 10;
        }

        .leaf {
            position: absolute;
            font-size: 30px;
            animation: fall 3s linear infinite;
        }

        @keyframes fall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }

        .restart-btn {
            background: #4169E1;
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 1.2rem;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s ease;
        }

        .restart-btn:hover {
            background: #0000CD;
            transform: translateY(-2px);
        }

        @media (max-width: 768px) {
            .container { padding: 15px; }
            .title { font-size: 2rem; }
            .difficulty-buttons { flex-direction: column; align-items: center; }
            .plant-progress { font-size: 2.5rem; }
        }
    </style>
</head>
<body>
    <div class="floating-cells" id="floatingCells"></div>
    <div class="floating-leaves" id="floatingLeaves"></div>

    <div class="container">
        <div class="header">
            <h1 class="title">🧬 중학생 생물학 퀴즈</h1>
            <p class="subtitle">세포, 광합성, 동물 분류를 배워보세요!</p>
            
            <div class="progress-container">
                <div class="plant-progress" id="plantProgress">🌱</div>
                <div class="score" id="scoreDisplay">점수: 0/0</div>
            </div>
        </div>

        <div class="difficulty-selector" id="difficultySelector">
            <h3 style="color: #2d5016; margin-bottom: 20px;">🎯 난이도를 선택하세요</h3>
            <div class="difficulty-buttons">
                <button class="difficulty-btn easy" onclick="selectDifficulty('easy')">
                    🌱 쉬움 (초급)
                </button>
                <button class="difficulty-btn medium" onclick="selectDifficulty('medium')">
                    🌿 보통 (중급)
                </button>
                <button class="difficulty-btn hard" onclick="selectDifficulty('hard')">
                    🌳 어려움 (고급)
                </button>
            </div>
        </div>

        <div class="quiz-container" id="quizContainer">
            <div class="question" id="question"></div>
            <div class="options" id="options"></div>
            <div class="feedback" id="feedback"></div>
            <button class="next-btn" id="nextBtn" onclick="nextQuestion()">다음 문제 ➡️</button>
        </div>

        <div class="completion-screen" id="completionScreen">
            <h2 style="color: #2d5016; margin-bottom: 20px;">🎉 퀴즈 완료!</h2>
            <div class="badge">🔬🧪🧬</div>
            <h3 style="color: #4a7c59;">주니어 생물학자 배지 획득!</h3>
            <p id="finalScore" style="font-size: 1.3rem; margin: 20px 0; color: #2d5016;"></p>
            <button class="restart-btn" onclick="restartQuiz()">🔄 다시 시작</button>
        </div>
    </div>

    <script>
        const questions = {
            easy: [
                {
                    question: "🦠 동물 세포와 식물 세포의 가장 큰 차이점은 무엇인가요?",
                    options: ["크기", "색깔", "세포벽의 유무", "모양"],
                    correct: 2,
                    explanation: "식물 세포는 세포벽이 있어 단단하지만, 동물 세포는 세포벽이 없어요! 🌱"
                },
                {
                    question: "🌱 광합성이 일어나는 세포 소기관은?",
                    options: ["미토콘드리아", "엽록체", "핵", "리보솜"],
                    correct: 1,
                    explanation: "엽록체에서 햇빛을 이용해 광합성이 일어나요! ☀️🍃"
                },
                {
                    question: "🐟 물고기는 어떤 동물 분류에 속하나요?",
                    options: ["포유류", "조류", "어류", "파충류"],
                    correct: 2,
                    explanation: "물고기는 아가미로 숨쉬는 어류예요! 🐠"
                },
                {
                    question: "🧫 세포의 '컨트롤 센터' 역할을 하는 부분은?",
                    options: ["세포막", "핵", "세포질", "세포벽"],
                    correct: 1,
                    explanation: "핵은 세포의 모든 활동을 조절하는 중요한 부분이에요! 🎯"
                },
                {
                    question: "🌿 광합성의 결과로 만들어지는 기체는?",
                    options: ["이산화탄소", "산소", "질소", "수소"],
                    correct: 1,
                    explanation: "광합성으로 산소가 만들어져서 우리가 숨쉴 수 있어요! 💨"
                }
            ],
            medium: [
                {
                    question: "🔬 세포막의 주요 기능은 무엇인가요?",
                    options: ["에너지 생산", "물질의 출입 조절", "유전정보 저장", "단백질 합성"],
                    correct: 1,
                    explanation: "세포막은 필요한 물질만 들어오고 나가게 조절해요! 🚪"
                },
                {
                    question: "🌱 광합성 과정에서 필요한 것들을 모두 고르세요:",
                    options: ["햇빛 + 물 + 이산화탄소", "햇빛 + 산소 + 물", "물 + 산소 + 포도당", "이산화탄소 + 산소 + 햇빛"],
                    correct: 0,
                    explanation: "햇빛, 물, 이산화탄소가 모두 필요해요! ☀️💧🌬️"
                },
                {
                    question: "🦅 조류의 특징이 아닌 것은?",
                    options: ["깃털이 있다", "알을 낳는다", "체온이 일정하다", "아가미로 숨쉰다"],
                    correct: 3,
                    explanation: "새는 폐로 숨쉬어요! 아가미는 물고기의 특징이죠 🐦"
                },
                {
                    question: "🧬 DNA가 들어있는 곳은?",
                    options: ["세포질", "핵", "세포막", "엽록체"],
                    correct: 1,
                    explanation: "DNA는 핵 안에 있어서 유전정보를 보관해요! 📚"
                },
                {
                    question: "🦠 미토콘드리아의 주요 역할은?",
                    options: ["광합성", "에너지 생산", "물질 운반", "세포 분열"],
                    correct: 1,
                    explanation: "미토콘드리아는 세포의 '발전소'예요! ⚡"
                }
            ],
            hard: [
                {
                    question: "🔬 원핵세포와 진핵세포의 가장 중요한 차이점은?",
                    options: ["크기의 차이", "막으로 둘러싸인 핵의 유무", "세포벽의 유무", "DNA의 유무"],
                    correct: 1,
                    explanation: "진핵세포는 핵막이 있지만, 원핵세포는 핵막이 없어요! 🧫"
                },
                {
                    question: "🌿 광합성의 명반응이 일어나는 곳은?",
                    options: ["스트로마", "틸라코이드", "핵", "미토콘드리아"],
                    correct: 1,
                    explanation: "틸라코이드에서 빛에너지를 화학에너지로 바꿔요! ⚡🌱"
                },
                {
                    question: "🐸 양서류의 특징으로 옳은 것은?",
                    options: ["평생 아가미로만 숨쉰다", "변온동물이다", "깃털이 있다", "태생으로 새끼를 낳는다"],
                    correct: 1,
                    explanation: "개구리 같은 양서류는 주변 온도에 따라 체온이 변해요! 🌡️"
                },
                {
                    question: "🧬 단백질 합성이 일어나는 세포 소기관은?",
                    options: ["골지체", "리보솜", "중심체", "액포"],
                    correct: 1,
                    explanation: "리보솜에서 아미노산을 연결해 단백질을 만들어요! 🔗"
                },
                {
                    question: "🦠 세포 호흡과 광합성의 관계는?",
                    options: ["같은 과정이다", "정반대 과정이다", "관련이 없다", "순서대로 일어난다"],
                    correct: 1,
                    explanation: "세포호흡은 산소를 쓰고 이산화탄소를 만들고, 광합성은 그 반대예요! ↔️"
                }
            ]
        };

        let currentDifficulty = '';
        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let answered = false;

        function createFloatingCells() {
            const container = document.getElementById('floatingCells');
            const cells = ['🔬🦠', '🧫', '🧬', '🦠'];
            
            setInterval(() => {
                const cell = document.createElement('div');
                cell.className = 'cell';
                cell.textContent = cells[Math.floor(Math.random() * cells.length)];
                cell.style.left = Math.random() * 100 + '%';
                cell.style.animationDuration = (Math.random() * 5 + 5) + 's';
                container.appendChild(cell);
                
                setTimeout(() => {
                    if (cell.parentNode) {
                        cell.parentNode.removeChild(cell);
                    }
                }, 10000);
            }, 2000);
        }

        function selectDifficulty(difficulty) {
            currentDifficulty = difficulty;
            currentQuestions = [...questions[difficulty]];
            currentQuestionIndex = 0;
            score = 0;
            answered = false;

            // 버튼 활성화 표시
            document.querySelectorAll('.difficulty-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');

            // 화면 전환
            setTimeout(() => {
                document.getElementById('difficultySelector').style.display = 'none';
                document.getElementById('quizContainer').classList.add('active');
                showQuestion();
            }, 500);
        }

        function showQuestion() {
            if (currentQuestionIndex >= currentQuestions.length) {
                showCompletion();
                return;
            }

            const question = currentQuestions[currentQuestionIndex];
            document.getElementById('question').textContent = question.question;
            
            const optionsContainer = document.getElementById('options');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectAnswer(index);
                optionsContainer.appendChild(optionElement);
            });

            document.getElementById('feedback').style.display = 'none';
            document.getElementById('nextBtn').style.display = 'none';
            answered = false;

            updateScore();
        }

        function selectAnswer(selectedIndex) {
            if (answered) return;
            answered = true;

            const question = currentQuestions[currentQuestionIndex];
            const options = document.querySelectorAll('.option');
            const feedback = document.getElementById('feedback');

            options.forEach((option, index) => {
                if (index === question.correct) {
                    option.classList.add('correct');
                } else if (index === selectedIndex && index !== question.correct) {
                    option.classList.add('incorrect');
                }
                option.style.pointerEvents = 'none';
            });

            if (selectedIndex === question.correct) {
                score++;
                feedback.className = 'feedback correct';
                feedback.textContent = '🎉 정답입니다! ' + question.explanation;
                updatePlantProgress();
            } else {
                feedback.className = 'feedback incorrect';
                feedback.textContent = '❌ 틀렸습니다. ' + question.explanation;
            }

            feedback.style.display = 'block';
            document.getElementById('nextBtn').style.display = 'block';
            updateScore();
        }

        function nextQuestion() {
            currentQuestionIndex++;
            showQuestion();
        }

        function updateScore() {
            document.getElementById('scoreDisplay').textContent = 
                `점수: ${score}/${currentQuestionIndex + 1}`;
        }

        function updatePlantProgress() {
            const plantElement = document.getElementById('plantProgress');
            if (score <= 2) {
                plantElement.textContent = '🌱';
            } else if (score <= 4) {
                plantElement.textContent = '🌿';
            } else {
                plantElement.textContent = '🌳';
            }
        }

        function showCompletion() {
            document.getElementById('quizContainer').style.display = 'none';
            document.getElementById('completionScreen').style.display = 'block';
            
            const percentage = Math.round((score / currentQuestions.length) * 100);
            document.getElementById('finalScore').textContent = 
                `최종 점수: ${score}/${currentQuestions.length} (${percentage}%)`;

            // 축하 잎사귀 애니메이션
            createCelebrationLeaves();
        }

        function createCelebrationLeaves() {
            const container = document.getElementById('floatingLeaves');
            const leaves = ['🍃', '🌿', '🍀'];
            
            for (let i = 0; i < 20; i++) {
                setTimeout(() => {
                    const leaf = document.createElement('div');
                    leaf.className = 'leaf';
                    leaf.textContent = leaves[Math.floor(Math.random() * leaves.length)];
                    leaf.style.left = Math.random() * 100 + '%';
                    leaf.style.animationDelay = Math.random() * 2 + 's';
                    container.appendChild(leaf);
                    
                    setTimeout(() => {
                        if (leaf.parentNode) {
                            leaf.parentNode.removeChild(leaf);
                        }
                    }, 3000);
                }, i * 200);
            }
        }

        function restartQuiz() {
            document.getElementById('completionScreen').style.display = 'none';
            document.getElementById('difficultySelector').style.display = 'block';
            document.getElementById('quizContainer').classList.remove('active');
            
            // 초기화
            currentDifficulty = '';
            currentQuestions = [];
            currentQuestionIndex = 0;
            score = 0;
            answered = false;
            
            document.getElementById('plantProgress').textContent = '🌱';
            document.getElementById('scoreDisplay').textContent = '점수: 0/0';
            
            // 버튼 활성화 해제
            document.querySelectorAll('.difficulty-btn').forEach(btn => {
                btn.classList.remove('active');
            });
        }

        // 페이지 로드 시 실행
        window.onload = function() {
            createFloatingCells();
        };
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97c326bd10d5326f',t:'MTc1NzM4Mzg0OC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
