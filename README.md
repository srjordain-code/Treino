# <!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Projeto 8 Meses - Híbrido</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        body { background-color: #0f172a; color: white; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .card { background-color: #1e293b; border-radius: 15px; padding: 15px; margin-bottom: 15px; border: 1px solid #334155; }
        .accent { color: #22c55e; }
        .progress-bar { background: #334155; border-radius: 10px; height: 10px; overflow: hidden; }
        .progress-fill { background: #22c55e; height: 100%; width: 5%; }
        .hidden { display: none; }
        .training-img { max-height: 250px; object-fit: contain; margin: 0 auto; display: block; border-radius: 10px; }
    </style>
</head>
<body class="p-4 pb-24">

    <div id="dashboard" class="">
        <div class="flex justify-between items-center mb-6">
            <div>
                <h1 class="text-2xl font-bold">Olá, Campeão! 👋</h1>
                <p class="text-gray-400 text-sm">Foco: 96kg → 80kg</p>
            </div>
            <div class="text-right">
                <span class="text-xs text-gray-400">Progresso 8 meses</span>
                <div class="progress-bar w-24 mt-1">
                    <div class="progress-fill"></div>
                </div>
            </div>
        </div>

        <div class="card flex justify-between items-center">
            <div>
                <h3 class="font-bold"><i class="fas fa-tint accent mr-2"></i> Meta de Água</h3>
                <p class="text-xs text-gray-400" id="water-status">0ml / 4000ml</p>
            </div>
            <div class="flex space-x-2">
                <button onclick="addWater()" class="bg-blue-600 px-3 py-1 rounded">+ 500ml</button>
            </div>
        </div>

        <h2 class="text-lg font-bold mb-3 accent">🍎 Dieta (190g Proteína)</h2>
        <div class="card space-y-3">
            <label class="flex items-center space-x-3">
                <input type="checkbox" id="checkCafe" class="w-5 h-5 rounded accent" onchange="saveProgress()">
                <span>Café: 3 ovos + Aveia + Fruta</span>
            </label>
            <label class="flex items-center space-x-3">
                <input type="checkbox" id="checkAlmoco" class="w-5 h-5 rounded" onchange="saveProgress()">
                <span>Almoço: 150g Proteína + 100g Carbo + Salada</span>
            </label>
            <label class="flex items-center space-x-3">
                <input type="checkbox" id="checkLanche" class="w-5 h-5 rounded" onchange="saveProgress()">
                <span>Lanche: Whey + 1 Fruta</span>
            </label>
            <label class="flex items-center space-x-3">
                <input type="checkbox" id="checkJantar" class="w-5 h-5 rounded" onchange="saveProgress()">
                <span>Jantar: 150g Proteína + Vegetais</span>
            </label>
            <label class="flex items-center space-x-3">
                <input type="checkbox" id="checkSuplementos" class="w-5 h-5 rounded text-green-500" onchange="saveProgress()">
                <span>Creatina + Ômega 3 (Tomados?)</span>
            </label>
        </div>

        <h2 class="text-lg font-bold mb-3 accent">🏋️ Treino do Dia</h2>
        <div id="treino-content" class="card">
            <p class="text-sm italic text-gray-300 mb-4">Selecione o dia na barra inferior para ver o treino.</p>
        </div>

        <div class="card bg-slate-800">
            <h3 class="font-bold mb-2"><i class="fas fa-shopping-cart mr-2"></i> Lista de Compras</h3>
            <p class="text-xs text-gray-400">Frango, Ovos, Patinho, Tilápia, Aveia, Batata Doce, Brócolis, Whey, Creatina.</p>
        </div>
    </div>

    <div id="training-mode" class="hidden fixed inset-0 bg-slate-900 p-4 flex flex-col justify-between">
        <div class="text-center mb-4">
            <h2 class="text-xl font-bold accent" id="training-day-title"></h2>
            <p class="text-lg font-semibold mt-2" id="current-exercise-name"></p>
        </div>
        <div class="flex-grow flex flex-col justify-center items-center mb-4">
            <img id="exercise-image" src="" alt="Exercício" class="training-img mb-4">
            <p class="text-sm text-center px-4" id="exercise-description"></p>
        </div>
        <div class="mb-4 text-center">
            <span class="text-3xl font-bold" id="timer">00:00</span>
        </div>
        <div class="flex justify-between space-x-4">
            <button onclick="prevExercise()" class="bg-gray-700 text-white px-4 py-2 rounded-lg flex-1"><i class="fas fa-arrow-left mr-2"></i> Anterior</button>
            <button onclick="nextExercise()" class="bg-green-600 text-white px-4 py-2 rounded-lg flex-1">Próximo <i class="fas fa-arrow-right ml-2"></i></button>
        </div>
        <button onclick="exitTrainingMode()" class="bg-red-700 text-white px-4 py-2 rounded-lg mt-4 w-full">Sair do Treino</button>
    </div>

    <div class="fixed bottom-0 left-0 right-0 bg-slate-900 border-t border-slate-700 p-2 flex justify-around text-[10px] uppercase font-bold z-10">
        <button onclick="setTreino('Segunda')" class="flex flex-col items-center p-2"><i class="fas fa-dumbbell text-lg mb-1"></i>SEG</button>
        <button onclick="setTreino('Terça')" class="flex flex-col items-center p-2 text-green-500"><i class="fas fa-running text-lg mb-1"></i>TER</button>
        <button onclick="setTreino('Quarta')" class="flex flex-col items-center p-2"><i class="fas fa-child text-lg mb-1"></i>QUA</button>
        <button onclick="setTreino('Quinta')" class="flex flex-col items-center p-2"><i class="fas fa-dumbbell text-lg mb-1"></i>QUI</button>
        <button onclick="setTreino('Sexta')" class="flex flex-col items-center p-2 text-blue-400"><i class="fas fa-sync text-lg mb-1"></i>FULL</button>
        <button onclick="setTreino('Sábado')" class="flex flex-col items-center p-2 text-orange-400"><i class="fas fa-road text-lg mb-1"></i>SÁB</button>
    </div>

    <script>
        // URLs de imagens (use links diretos para GIFs ou JPGs)
        // Substitua por imagens reais de alta qualidade se possível!
        const imageUrls = {
            'Puxada Frente': 'https://i.imgur.com/rM7Y14A.gif', // Puxada (Lat Pulldown)
            'Supino Reto': 'https://i.imgur.com/5D64h8R.gif', // Supino Reto
            'Remada Baixa': 'https://i.imgur.com/0oI49d5.gif', // Remada Baixa
            'Elevação Lateral': 'https://i.imgur.com/IymuM1x.gif', // Elevação Lateral
            'Tríceps Corda': 'https://i.imgur.com/i9zD37w.gif', // Tríceps Corda
            'Agachamento': 'https://i.imgur.com/yU2K2dE.gif', // Agachamento Livre
            'Stiff': 'https://i.imgur.com/gK0K19o.gif', // Stiff
            'Leg Press': 'https://i.imgur.com/r93x84L.gif', // Leg Press
            'Panturrilha': 'https://i.imgur.com/XF8gYgS.gif', // Panturrilha
            'Corrida Tiros': 'https://i.imgur.com/W2d5rW6.gif', // Corrida de tiros
            'Corrida Longão': 'https://i.imgur.com/7s1H7QJ.gif', // Corrida longa
            'Abdominal Prancha': 'https://i.imgur.com/4q6sX4S.gif', // Prancha
            'Desenvolvimento Halteres': 'https://i.imgur.com/7q3Z0r7.gif', // Desenvolvimento Halteres
            'Rosca Direta': 'https://i.imgur.com/E1L2tG8.gif', // Rosca Direta
            'Mobilidade': 'https://i.imgur.com/p1Q9tK1.gif', // Exemplo de mobilidade
        };

        const treinos = {
            'Segunda': {
                title: 'Força Superior (Push)',
                exercises: [
                    { name: 'Supino Reto', sets: '3x10', desc: 'Deite-se no banco, pés no chão. Desça a barra até perto do peito e empurre. Controle o movimento.', img: imageUrls['Supino Reto'] },
                    { name: 'Desenvolvimento Halteres', sets: '3x12', desc: 'Sentado ou em pé, halteres na altura dos ombros. Empurre para cima e volte controlando.', img: imageUrls['Desenvolvimento Halteres'] },
                    { name: 'Elevação Lateral', sets: '3x15', desc: 'Mantenha os braços levemente flexionados, eleve os halteres lateralmente até a altura dos ombros. Não use impulso.', img: imageUrls['Elevação Lateral'] },
                    { name: 'Tríceps Corda', sets: '3x12', desc: 'Use a polia alta, puxe a corda para baixo estendendo os cotovelos. Foque na contração do tríceps.', img: imageUrls['Tríceps Corda'] },
                ]
            },
            'Terça': {
                title: 'Corrida (Tiros)',
                exercises: [
                    { name: 'Aquecimento', sets: '5min', desc: 'Caminhada ou trote bem leve para preparar o corpo.', img: imageUrls['Corrida Tiros'] },
                    { name: 'Tiros de 1min', sets: '8x (1min Forte / 1min Caminha)', desc: 'Alterne 1 minuto correndo em ritmo forte com 1 minuto caminhando para recuperar.', img: imageUrls['Corrida Tiros'] },
                    { name: 'Volta à calma', sets: '5min', desc: 'Caminhada leve para normalizar a frequência cardíaca.', img: imageUrls['Corrida Tiros'] },
                ]
            },
            'Quarta': {
                title: 'Inferiores (Leg Day)',
                exercises: [
                    { name: 'Agachamento', sets: '3x10', desc: 'Pés na largura dos ombros, agache como se fosse sentar numa cadeira. Mantenha a coluna reta e o peito aberto.', img: imageUrls['Agachamento'] },
                    { name: 'Stiff (Halteres ou Barra)', sets: '3x12', desc: 'Desça o peso rente às pernas, empurrando o quadril para trás. Sinta alongar o posterior da coxa.', img: imageUrls['Stiff'] },
                    { name: 'Leg Press 45º', sets: '3x12', desc: 'Mantenha os pés na plataforma, desça até as coxas encostarem no abdômen. Empurre de volta sem travar os joelhos.', img: imageUrls['Leg Press'] },
                    { name: 'Panturrilha', sets: '4x20', desc: 'Em pé, eleve os calcanhares o máximo que puder. Sinta a panturrilha queimar.', img: imageUrls['Panturrilha'] },
                ]
            },
            'Quinta': {
                title: 'Força Superior (Pull)',
                exercises: [
                    { name: 'Puxada Frente (Pulley)', sets: '3x10', desc: 'Puxe a barra em direção ao peito, contraindo as costas. Sinta as escápulas "esmagarem" atrás.', img: imageUrls['Puxada Frente'] },
                    { name: 'Remada Curvada', sets: '3x12', desc: 'Incline o tronco, puxe a barra em direção ao umbigo. Mantenha o abdômen contraído.', img: imageUrls['Remada Baixa'] }, // Usando a imagem de Remada Baixa
                    { name: 'Rosca Direta', sets: '3x12', desc: 'Com barra ou halteres, levante o peso flexionando os cotovelos. Foque no bíceps, sem balançar o corpo.', img: imageUrls['Rosca Direta'] },
                    { name: 'Abdominal Prancha', sets: '3x45s', desc: 'Mantenha o corpo reto como uma tábua, apoiado nos antebraços e ponta dos pés. Não curve a lombar.', img: imageUrls['Abdominal Prancha'] },
                ]
            },
            'Sexta': {
                title: 'Mobilidade e Full Body',
                exercises: [
                    { name: 'Aquecimento Articular', sets: '10 min', desc: 'Rotações de braços, pernas, tornozelos para lubrificar as articulações.', img: imageUrls['Mobilidade'] },
                    { name: 'Agachamento com Peso Corporal', sets: '3x15', desc: 'Foco na técnica e amplitude, sem carga extra.', img: imageUrls['Agachamento'] },
                    { name: 'Flexões de Braço', sets: '3xMax', desc: 'Apoie os joelhos se necessário. Mantenha o corpo reto.', img: 'https://i.imgur.com/uGzJ0uR.gif' }, // Imagem para flexões
                    { name: 'Prancha Lateral', sets: '3x30s (cada lado)', desc: 'Foco na estabilidade do core e oblíquos.', img: 'https://i.imgur.com/P0V2k1B.gif' }, // Imagem para prancha lateral
                ]
            },
            'Sábado': {
                title: 'Corrida (Longão)',
                exercises: [
                    { name: 'Trote Contínuo', sets: '40 minutos', desc: 'Corra em ritmo constante e confortável, onde você consegue conversar.', img: imageUrls['Corrida Longão'] },
                    { name: 'Alongamento pós-corrida', sets: '10 minutos', desc: 'Alongue principalmente quadríceps, isquiotibiais e panturrilhas.', img: imageUrls['Mobilidade'] },
                ]
            }
        };

        let currentTrainingDay = '';
        let currentExerciseIndex = 0;
        let timerInterval;

        // Funções de salvamento e carregamento de progresso (LocalStorage)
        function saveProgress() {
            localStorage.setItem('waterCount', waterCount);
            localStorage.setItem('checkCafe', document.getElementById('checkCafe').checked);
            localStorage.setItem('checkAlmoco', document.getElementById('checkAlmoco').checked);
            localStorage.setItem('checkLanche', document.getElementById('checkLanche').checked);
            localStorage.setItem('checkJantar', document.getElementById('checkJantar').checked);
            localStorage.setItem('checkSuplementos', document.getElementById('checkSuplementos').checked);
            updateWaterStatus(); // Atualiza o status da água após salvar
        }

        function loadProgress() {
            waterCount = parseInt(localStorage.getItem('waterCount')) || 0;
            document.getElementById('checkCafe').checked = localStorage.getItem('checkCafe') === 'true';
            document.getElementById('checkAlmoco').checked = localStorage.getItem('checkAlmoco') === 'true';
            document.getElementById('checkLanche').checked = localStorage.getItem('checkLanche') === 'true';
            document.getElementById('checkJantar').checked = localStorage.getItem('checkJantar') === 'true';
            document.getElementById('checkSuplementos').checked = localStorage.getItem('checkSuplementos') === 'true';
            updateWaterStatus(); // Atualiza o status da água ao carregar
        }

        function updateWaterStatus() {
            document.getElementById('water-status').textContent = `${waterCount}ml / 4000ml`;
        }

        let waterCount = 0;
        function addWater() {
            waterCount += 500;
            if (waterCount > 4000) waterCount = 4000;
            saveProgress(); // Salva o progresso da água
            updateWaterStatus(); // Atualiza o status no display
        }
        
        // Funções do Modo de Treino
        function setTreino(day) {
            currentTrainingDay = day;
            const treinoData = treinos[day];
            let htmlContent = `
                <h4 class='font-bold text-green-500'>${treinoData.title}</h4>
                <ul class='mt-2 text-sm space-y-2'>
            `;
            treinoData.exercises.forEach(ex => {
                htmlContent += `<li>• ${ex.name} (${ex.sets})</li>`;
            });
            htmlContent += `
                </ul>
                <button onclick="startTrainingMode('${day}')" class="btn-check mt-4 bg-green-600 text-white block w-full p-3 rounded-lg font-bold">
                    <i class="fas fa-play mr-2"></i> Começar Treino
                </button>
            `;
            document.getElementById('treino-content').innerHTML = htmlContent;
        }

        function startTrainingMode(day) {
            currentTrainingDay = day;
            currentExerciseIndex = 0;
            document.getElementById('dashboard').classList.add('hidden');
            document.getElementById('training-mode').classList.remove('hidden');
            displayExercise();
        }

        function displayExercise() {
            const treinoData = treinos[currentTrainingDay];
            const exercise = treinoData.exercises[currentExerciseIndex];

            document.getElementById('training-day-title').textContent = treinoData.title;
            document.getElementById('current-exercise-name').textContent = `${exercise.name} (${exercise.sets})`;
            document.getElementById('exercise-image').src = exercise.img;
            document.getElementById('exercise-description').textContent = exercise.desc;

            // Reinicia o timer para cada exercício
            clearInterval(timerInterval);
            let seconds = 0;
            document.getElementById('timer').textContent = '00:00';
            timerInterval = setInterval(() => {
                seconds++;
                const minutes = Math.floor(seconds / 60);
                const remainingSeconds = seconds % 60;
                document.getElementById('timer').textContent = 
                    `${minutes < 10 ? '0' : ''}${minutes}:${remainingSeconds < 10 ? '0' : ''}${remainingSeconds}`;
            }, 1000);
        }

        function nextExercise() {
            const treinoData = treinos[currentTrainingDay];
            if (currentExerciseIndex < treinoData.exercises.length - 1) {
                currentExerciseIndex++;
                displayExercise();
            } else {
                alert('Treino concluído! Parabéns!');
                exitTrainingMode();
            }
        }

        function prevExercise() {
            if (currentExerciseIndex > 0) {
                currentExerciseIndex--;
                displayExercise();
            }
        }

        function exitTrainingMode() {
            clearInterval(timerInterval); // Para o timer
            document.getElementById('training-mode').classList.add('hidden');
            document.getElementById('dashboard').classList.remove('hidden');
            setTreino(currentTrainingDay); // Volta a mostrar o treino do dia no dashboard
        }

        // Carrega o progresso ao iniciar o aplicativo
        window.onload = loadProgress;

        // Define o treino do dia atual ao carregar
        const today = new Date();
        const dayNames = ['Domingo', 'Segunda', 'Terça', 'Quarta', 'Quinta', 'Sexta', 'Sábado'];
        const currentDayName = dayNames[today.getDay()];
        if (treinos[currentDayName]) {
            setTreino(currentDayName);
        } else {
            document.getElementById('treino-content').innerHTML = "<p class='text-sm italic text-gray-300'>Sem treino agendado para hoje. Escolha um dia abaixo!</p>";
        }

    </script>
</body>
</html>
