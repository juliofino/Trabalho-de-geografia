<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo Interativo: O Ciclo das Rochas</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chosen Palette: Earthy Harmony -->
    <!-- Application Structure Plan: A single-page, vertical-scrolling app. It starts with an overview of the game (objectives and rules). Then, it presents an interactive diagram of the rock cycle to provide educational context, as the game's goal is to teach this. Finally, it culminates in an interactive quiz using the questions from the source document. This structure guides the user from the 'what' (the game) to the 'why' (the science) and finally to the 'test' (the quiz), creating a logical and engaging learning flow. -->
    <!-- Visualization & Content Choices: 1. Game Info: Presented in clear cards using structured HTML/CSS for readability. Goal: Inform. Interaction: None. 2. Rock Cycle Diagram: A custom, interactive diagram built with HTML/CSS and JS. Goal: Organize & Explain. Interaction: Clicking a rock type reveals a description, encouraging exploration. 3. Quiz: The static question list is converted into a dynamic quiz. Goal: Engage & Test. Interaction: Users select answers and get immediate feedback, with score tracking. This transforms passive content into an active learning tool. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .card {
            background-color: #fdfaf6;
            border: 1px solid #e2d8ce;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .btn {
            transition: background-color 0.3s ease, transform 0.2s ease;
        }
        .btn:hover {
            transform: scale(1.05);
        }
        .answer-option {
            border: 2px solid transparent;
        }
        .correct-answer {
            background-color: #d1fae5;
            border-color: #10b981;
        }
        .incorrect-answer {
            background-color: #fee2e2;
            border-color: #ef4444;
        }
        .rock-cycle-arrow {
            position: absolute;
            background-color: #a8a29e;
            height: 2px;
            transform-origin: left center;
        }
        .rock-cycle-arrow::after {
            content: '';
            position: absolute;
            right: -1px;
            top: -4px;
            border-top: 5px solid transparent;
            border-bottom: 5px solid transparent;
            border-left: 8px solid #a8a29e;
        }
    </style>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
</head>
<body class="bg-[#fdfaf6] text-[#4a4a4a]">

    <div class="container mx-auto p-4 md:p-8 max-w-5xl">

        <header class="text-center mb-12">
            <h1 class="text-4xl md:text-5xl font-bold text-[#d97706]">A Jornada das Rochas</h1>
            <p class="text-lg text-gray-600 mt-2">Uma aventura interativa pelo ciclo das rochas</p>
        </header>

        <main>
            <section id="game-info" class="mb-16">
                <h2 class="text-3xl font-bold text-center mb-8 text-[#78716c]">Sobre o Jogo</h2>
                <div class="grid md:grid-cols-2 gap-8">
                    <div class="card p-6 rounded-lg">
                        <h3 class="text-2xl font-semibold mb-4 text-[#d97706]">🎯 Objetivos</h3>
                        <ul class="list-disc list-inside space-y-2 text-gray-700">
                            <li>Ensinar sobre os três tipos de rochas: ígneas, sedimentares e metamórficas.</li>
                            <li>Ajudar a entender o ciclo das rochas de forma divertida e interativa.</li>
                        </ul>
                    </div>
                    <div class="card p-6 rounded-lg">
                        <h3 class="text-2xl font-semibold mb-4 text-[#d97706]">📜 Regras Simples</h3>
                        <ul class="space-y-3 text-gray-700">
                            <li class="flex items-start"><span class="mr-3 text-xl">🎲</span> Jogue o dado e avance no tabuleiro.</li>
                            <li class="flex items-start"><span class="mr-3 text-xl">❓</span> Caia em uma casa de pergunta e responda a uma questão.</li>
                            <li class="flex items-start"><span class="mr-3 text-xl">🏆</span> Acerte para ganhar uma Ficha-Rocha do tipo correspondente.</li>
                            <li class="flex items-start"><span class="mr-3 text-xl">🏁</span> O primeiro a coletar as 3 fichas diferentes e chegar ao final, vence!</li>
                        </ul>
                    </div>
                </div>
            </section>

            <section id="rock-cycle" class="mb-16">
                <h2 class="text-3xl font-bold text-center mb-4 text-[#78716c]">Explore o Ciclo das Rochas</h2>
                 <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">O jogo se baseia no ciclo das rochas, um processo contínuo em que as rochas se transformam de um tipo para outro. Clique em cada tipo de rocha abaixo para aprender mais sobre ela.</p>
                <div class="relative w-full max-w-lg mx-auto h-96 md:h-[450px]">
                    <div id="igneous-node" class="rock-node absolute top-0 left-1/2 -translate-x-1/2" onclick="showInfo('ígnea')">Ígnea</div>
                    <div id="sedimentary-node" class="rock-node absolute bottom-0 left-0" onclick="showInfo('sedimentar')">Sedimentar</div>
                    <div id="metamorphic-node" class="rock-node absolute bottom-0 right-0" onclick="showInfo('metamórfica')">Metamórfica</div>
                </div>
                <div id="info-box" class="mt-8 p-6 bg-white rounded-lg shadow-md min-h-[100px] text-center flex items-center justify-center transition-opacity duration-500 opacity-0">
                    <p id="info-text" class="text-lg text-gray-700"></p>
                </div>
            </section>

            <section id="quiz-section">
                <h2 class="text-3xl font-bold text-center mb-8 text-[#78716c]">Teste seu Conhecimento</h2>
                <div class="card max-w-2xl mx-auto p-6 md:p-8 rounded-lg">
                    <div id="quiz-container">
                        <div class="flex justify-between items-center mb-4">
                            <p class="text-sm font-semibold text-gray-600">Questão <span id="question-number">1</span> de <span id="total-questions">8</span></p>
                            <p class="text-sm font-semibold text-gray-600">Pontuação: <span id="score">0</span></p>
                        </div>
                        <div id="progress-bar" class="w-full bg-gray-200 rounded-full h-2.5 mb-6">
                            <div id="progress" class="bg-[#d97706] h-2.5 rounded-full" style="width: 0%"></div>
                        </div>
                        <h3 id="question-text" class="text-xl md:text-2xl font-semibold mb-6 min-h-[60px]"></h3>
                        <div id="answer-options" class="grid grid-cols-1 gap-4">
                        </div>
                    </div>
                    <div id="result-container" class="hidden text-center">
                        <h3 class="text-3xl font-bold text-[#d97706] mb-4">Quiz Finalizado!</h3>
                        <p class="text-xl mb-6">Sua pontuação final é: <span id="final-score" class="font-bold"></span></p>
                        <button id="restart-quiz" class="btn bg-[#d97706] text-white font-bold py-2 px-6 rounded-lg hover:bg-[#b45309]">Jogar Novamente</button>
                    </div>
                </div>
            </section>
        </main>

    </div>

    <script>
        const quizData = [
            { question: "Como se forma uma rocha ígnea?", options: ["Quando o magma esfria e endurece", "Quando a água escava uma montanha", "Quando plantas viram pedra"], answer: 0, type: "ígnea" },
            { question: "Qual dessas é uma rocha sedimentar?", options: ["Mármore", "Arenito", "Granito"], answer: 1, type: "sedimentar" },
            { question: "O que transforma uma rocha comum em metamórfica?", options: ["Sol e vento", "Água e sal", "Pressão e calor"], answer: 2, type: "metamórfica" },
            { question: "O que pode acontecer com uma rocha ígnea com o tempo?", options: ["Ela some sozinha", "Vira areia mágica", "Se transforma em outra rocha"], answer: 2, type: "ciclo" },
            { question: "Qual dessas rochas é formada por pressão e calor?", options: ["Basalto", "Mármore", "Arenito"], answer: 1, type: "metamórfica" },
            { question: "O que forma as rochas sedimentares?", options: ["Lava derretida", "Partículas de outras rochas", "Gelo"], answer: 1, type: "sedimentar" },
            { question: "Qual é a principal característica das rochas ígneas?", options: ["Camadas visíveis", "Brilho metálico", "Formadas por resfriamento do magma"], answer: 2, type: "ígnea" },
            { question: "O que pode transformar uma rocha sedimentar em metamórfica?", options: ["Tempo e vento", "Pressão e calor", "Água da chuva"], answer: 1, type: "metamórfica" },
        ];

        const rockInfo = {
            'ígnea': 'Rochas Ígneas (ou magmáticas) são formadas pelo resfriamento e solidificação do magma. O granito e o basalto são exemplos comuns.',
            'sedimentar': 'Rochas Sedimentares são formadas pela compactação e cimentação de sedimentos (partículas de outras rochas, matéria orgânica, etc.). O arenito é um bom exemplo.',
            'metamórfica': 'Rochas Metamórficas surgem da transformação de outras rochas sob intensa pressão e calor, sem que cheguem a derreter. O mármore (formado a partir do calcário) é um exemplo.'
        };

        let currentQuestionIndex = 0;
        let score = 0;

        const questionNumberEl = document.getElementById('question-number');
        const totalQuestionsEl = document.getElementById('total-questions');
        const scoreEl = document.getElementById('score');
        const questionTextEl = document.getElementById('question-text');
        const answerOptionsEl = document.getElementById('answer-options');
        const quizContainer = document.getElementById('quiz-container');
        const resultContainer = document.getElementById('result-container');
        const finalScoreEl = document.getElementById('final-score');
        const restartQuizBtn = document.getElementById('restart-quiz');
        const progressBar = document.getElementById('progress');

        function startQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            quizContainer.classList.remove('hidden');
            resultContainer.classList.add('hidden');
            showQuestion();
        }

        function showQuestion() {
            updateProgress();
            const currentQuestion = quizData[currentQuestionIndex];
            questionNumberEl.textContent = currentQuestionIndex + 1;
            totalQuestionsEl.textContent = quizData.length;
            scoreEl.textContent = score;
            questionTextEl.textContent = currentQuestion.question;
            answerOptionsEl.innerHTML = '';

            currentQuestion.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('btn', 'w-full', 'p-4', 'rounded-lg', 'text-left', 'bg-white', 'hover:bg-amber-50', 'border-2', 'border-gray-200');
                button.onclick = () => selectAnswer(index);
                answerOptionsEl.appendChild(button);
            });
        }

        function selectAnswer(selectedIndex) {
            const currentQuestion = quizData[currentQuestionIndex];
            const buttons = answerOptionsEl.getElementsByTagName('button');
            
            Array.from(buttons).forEach(button => button.disabled = true);

            if (selectedIndex === currentQuestion.answer) {
                score++;
                buttons[selectedIndex].classList.add('correct-answer');
            } else {
                buttons[selectedIndex].classList.add('incorrect-answer');
                buttons[currentQuestion.answer].classList.add('correct-answer');
            }
            
            scoreEl.textContent = score;

            setTimeout(() => {
                currentQuestionIndex++;
                if (currentQuestionIndex < quizData.length) {
                    showQuestion();
                } else {
                    showResults();
                }
            }, 1500);
        }

        function showResults() {
            quizContainer.classList.add('hidden');
            resultContainer.classList.remove('hidden');
            finalScoreEl.textContent = `${score} de ${quizData.length}`;
        }
        
        function updateProgress() {
            const progressPercentage = ((currentQuestionIndex) / quizData.length) * 100;
            progressBar.style.width = `${progressPercentage}%`;
        }

        restartQuizBtn.addEventListener('click', startQuiz);

        document.addEventListener('DOMContentLoaded', startQuiz);

        function showInfo(rockType) {
            const infoBox = document.getElementById('info-box');
            const infoText = document.getElementById('info-text');
            
            infoText.textContent = rockInfo[rockType];
            infoBox.classList.remove('opacity-0');
        }

        function createArrow(from, to, container) {
            const fromEl = document.getElementById(from);
            const toEl = document.getElementById(to);
            const arrow = document.createElement('div');
            arrow.classList.add('rock-cycle-arrow');

            const fromRect = fromEl.getBoundingClientRect();
            const toRect = toEl.getBoundingClientRect();
            const containerRect = container.getBoundingClientRect();

            const fromX = fromRect.left + fromRect.width / 2 - containerRect.left;
            const fromY = fromRect.top + fromRect.height / 2 - containerRect.top;
            const toX = toRect.left + toRect.width / 2 - containerRect.left;
            const toY = toRect.top + toRect.height / 2 - containerRect.top;

            const angle = Math.atan2(toY - fromY, toX - fromX) * 180 / Math.PI;
            const length = Math.sqrt(Math.pow(toX - fromX, 2) + Math.pow(toY - fromY, 2));

            arrow.style.width = `${length}px`;
            arrow.style.left = `${fromX}px`;
            arrow.style.top = `${fromY}px`;
            arrow.style.transform = `rotate(${angle}deg)`;
            
            container.appendChild(arrow);
        }

        window.addEventListener('load', () => {
            const container = document.querySelector('.relative.w-full');
            const nodes = ['igneous-node', 'sedimentary-node', 'metamorphic-node'];
            
            const rockNodes = document.querySelectorAll('.rock-node');
            rockNodes.forEach(node => {
                node.classList.add('cursor-pointer', 'p-4', 'rounded-full', 'bg-[#f59e0b]', 'text-white', 'font-bold', 'text-center', 'shadow-lg', 'w-32', 'h-32', 'flex', 'items-center', 'justify-center', 'transition-transform', 'hover:scale-110');
            });
            
            setTimeout(() => {
                createArrow('igneous-node', 'sedimentary-node', container);
                createArrow('sedimentary-node', 'metamorphic-node', container);
                createArrow('metamorphic-node', 'igneous-node', container);
            }, 100);
        });
        
    </script>
</body>
</html>
