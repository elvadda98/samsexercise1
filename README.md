<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nuovo Gioco di Vocabolario Italiano</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #4a6fa5 0%, #2a3f5f 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4a6fa5, #2a3f5f);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4a6fa5, #2a3f5f);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74,111,165,0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(74,111,165,0.2);
        }

        .word-bank h3 {
            color: #2c3e50;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4a6fa5, #2a3f5f);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(74,111,165,0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(74,111,165,0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #2c3e50;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #4a6fa5;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(74,111,165,0.2);
        }

        .option.selected {
            background: #4a6fa5;
            color: white;
            border-color: #4a6fa5;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #2c3e50;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #4a6fa5;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(74,111,165,0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #4a6fa5;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #4a6fa5, #2a3f5f);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(74,111,165,0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74,111,165,0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #4a6fa5, #2a3f5f);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(74,111,165,0.3);
            z-index: 1000;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Punteggio: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1>📚 Nuovo Vocabolario Italiano</h1>
            <p>Impara e pratica queste nuove parole italiane!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('fill-gaps')">Riempire gli Spazi</button>
            <button class="nav-btn" onclick="showSection('matching')">Abbinare i Significati</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Scelta Multipla</button>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section active">
            <div class="word-bank">
                <h3>📝 Banca Parole - Scegli tra queste parole:</h3>
                <div class="word-options">
                    <span class="word-option">paese</span>
                    <span class="word-option">solo</span>
                    <span class="word-option">marito</span>
                    <span class="word-option">sposato</span>
                    <span class="word-option">invitato</span>
                    <span class="word-option">isola</span>
                    <span class="word-option">officina</span>
                    <span class="word-option">di solito</span>
                    <span class="word-option">pensare</span>
                    <span class="word-option">campagna</span>
                    <span class="word-option">salito</span>
                    <span class="word-option">come mai</span>
                </div>
            </div>

            <div class="question">
                <h3>Domanda 1:</h3>
                <p>Mio <input type="text" class="fill-blank" data-answer="marito" placeholder="risposta"> lavora come ingegnere.</p>
                <div class="feedback" id="feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 2:</h3>
                <p>Sono <input type="text" class="fill-blank" data-answer="sposato" placeholder="risposta"> da cinque anni.</p>
                <div class="feedback" id="feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 3:</h3>
                <p>Il mio amico è un <input type="text" class="fill-blank" data-answer="invitato" placeholder="risposta"> al matrimonio.</p>
                <div class="feedback" id="feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 4:</h3>
                <p>La Sicilia è un'<input type="text" class="fill-blank" data-answer="isola" placeholder="risposta"> bellissima.</p>
                <div class="feedback" id="feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 5:</h3>
                <p>Mio padre ha un'<input type="text" class="fill-blank" data-answer="officina" placeholder="risposta"> meccanica.</p>
                <div class="feedback" id="feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 6:</h3>
                <p><input type="text" class="fill-blank" data-answer="di solito" placeholder="risposta"> vado in palestra il martedì e il giovedì.</p>
                <div class="feedback" id="feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 7:</h3>
                <p>Devo <input type="text" class="fill-blank" data-answer="pensare" placeholder="risposta"> a una soluzione.</p>
                <div class="feedback" id="feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 8:</h3>
                <p>Amo la vita in <input type="text" class="fill-blank" data-answer="campagna" placeholder="risposta">, è molto tranquilla.</p>
                <div class="feedback" id="feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 9:</h3>
                <p>Sono <input type="text" class="fill-blank" data-answer="salito" placeholder="risposta"> sul treno pochi minuti fa.</p>
                <div class="feedback" id="feedback-9" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 10:</h3>
                <p><input type="text" class="fill-blank" data-answer="come mai" placeholder="risposta"> non sei venuto alla festa ieri sera?</p>
                <div class="feedback" id="feedback-10" style="display: none;"></div>
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Controlla le Risposte</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #2c3e50;">Abbina le parole italiane con le loro traduzioni in inglese</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Parole Italiane</h4>
                    <div class="match-item" data-word="paese" onclick="selectMatch(this)">paese</div>
                    <div class="match-item" data-word="solo" onclick="selectMatch(this)">solo</div>
                    <div class="match-item" data-word="marito" onclick="selectMatch(this)">marito</div>
                    <div class="match-item" data-word="sposato" onclick="selectMatch(this)">sposato</div>
                    <div class="match-item" data-word="invitato" onclick="selectMatch(this)">invitato</div>
                    <div class="match-item" data-word="isola" onclick="selectMatch(this)">isola</div>
                    <div class="match-item" data-word="officina" onclick="selectMatch(this)">officina</div>
                    <div class="match-item" data-word="di solito" onclick="selectMatch(this)">di solito</div>
                    <div class="match-item" data-word="pensare" onclick="selectMatch(this)">pensare</div>
                    <div class="match-item" data-word="campagna" onclick="selectMatch(this)">campagna</div>
                    <div class="match-item" data-word="salito" onclick="selectMatch(this)">salito</div>
                    <div class="match-item" data-word="come mai" onclick="selectMatch(this)">come mai</div>
                </div>
                <div class="match-column">
                    <h4>Traduzioni Inglesi</h4>
                    <div class="match-item" data-meaning="solo" onclick="selectMatch(this)">alone/only</div>
                    <div class="match-item" data-meaning="sposato" onclick="selectMatch(this)">married</div>
                    <div class="match-item" data-meaning="campagna" onclick="selectMatch(this)">countryside</div>
                    <div class="match-item" data-meaning="paese" onclick="selectMatch(this)">town</div>
                    <div class="match-item" data-meaning="invitato" onclick="selectMatch(this)">wedding guest</div>
                    <div class="match-item" data-meaning="officina" onclick="selectMatch(this)">workshop</div>
                    <div class="match-item" data-meaning="pensare" onclick="selectMatch(this)">to think</div>
                    <div class="match-item" data-meaning="isola" onclick="selectMatch(this)">island</div>
                    <div class="match-item" data-meaning="di solito" onclick="selectMatch(this)">usually</div>
                    <div class="match-item" data-meaning="marito" onclick="selectMatch(this)">husband</div>
                    <div class="match-item" data-meaning="salito" onclick="selectMatch(this)">gone up</div>
                    <div class="match-item" data-meaning="come mai" onclick="selectMatch(this)">how come</div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Domanda 1: Cosa significa "paese" in questo contesto?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">country</div>
                    <div class="option" onclick="selectOption(this, true)">town</div>
                    <div class="option" onclick="selectOption(this, false)">village</div>
                    <div class="option" onclick="selectOption(this, false)">nation</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 2: Come si dice "husband" in italiano?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">padre</div>
                    <div class="option" onclick="selectOption(this, true)">marito</div>
                    <div class="option" onclick="selectOption(this, false)">fratello</div>
                    <div class="option" onclick="selectOption(this, false)">amico</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 3: Cosa significa "sposato"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">engaged</div>
                    <div class="option" onclick="selectOption(this, true)">married</div>
                    <div class="option" onclick="selectOption(this, false)">divorced</div>
                    <div class="option" onclick="selectOption(this, false)">single</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 4: Come si dice "wedding guest" in italiano?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">sposo</div>
                    <div class="option" onclick="selectOption(this, true)">invitato</div>
                    <div class="option" onclick="selectOption(this, false)">testimone</div>
                    <div class="option" onclick="selectOption(this, false)">ospite</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 5: Cosa significa "isola"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">beach</div>
                    <div class="option" onclick="selectOption(this, true)">island</div>
                    <div class="option" onclick="selectOption(this, false)">mountain</div>
                    <div class="option" onclick="selectOption(this, false)">lake</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 6: Come si dice "workshop" in italiano?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">ufficio</div>
                    <div class="option" onclick="selectOption(this, true)">officina</div>
                    <div class="option" onclick="selectOption(this, false)">fabbrica</div>
                    <div class="option" onclick="selectOption(this, false)">negozio</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 7: Cosa significa "di solito"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">sometimes</div>
                    <div class="option" onclick="selectOption(this, true)">usually</div>
                    <div class="option" onclick="selectOption(this, false)">never</div>
                    <div class="option" onclick="selectOption(this, false)">always</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 8: Come si dice "to think" in italiano?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">parlare</div>
                    <div class="option" onclick="selectOption(this, true)">pensare</div>
                    <div class="option" onclick="selectOption(this, false)">credere</div>
                    <div class="option" onclick="selectOption(this, false)">sapere</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 9: Cosa significa "campagna" in questo contesto?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">beach</div>
                    <div class="option" onclick="selectOption(this, true)">countryside</div>
                    <div class="option" onclick="selectOption(this, false)">mountain</div>
                    <div class="option" onclick="selectOption(this, false)">city</div>
                </div>
                <div class="feedback" id="mc-feedback-9" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 10: Come si dice "how come" in italiano?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">perché</div>
                    <div class="option" onclick="selectOption(this, true)">come mai</div>
                    <div class="option" onclick="selectOption(this, false)">quando</div>
                    <div class="option" onclick="selectOption(this, false)">dove</div>
                </div>
                <div class="feedback" id="mc-feedback-10" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
            });
            
            // Show feedback for each question
            for (let i = 1; i <= 10; i++) {
                const feedback = document.getElementById(`feedback-${i}`);
                const blank = document.querySelectorAll('.fill-blank')[i - 1];
                
                if (blank.classList.contains('correct')) {
                    feedback.textContent = '✅ Corretto!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Sbagliato. La risposta corretta è: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            }
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Abbinamento corretto!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 12) {
                    feedback.textContent = '🎉 Congratulazioni! Hai abbinato tutte le parole correttamente!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Abbinamento errato. Riprova.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Corretto!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Sbagliato. Riprova.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
    </script>
</body>
</html>
