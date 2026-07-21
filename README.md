<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BioGen - Plataforma de Evaluación Interactiva</title>
    <style>
        :root {
            --primary: #10b981;
            --primary-dark: #059669;
            --primary-light: #d1fae5;
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
            --correct-color: #22c55e;
            --correct-bg: rgba(34, 197, 94, 0.15);
            --incorrect-color: #ef4444;
            --incorrect-bg: rgba(239, 68, 68, 0.15);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 16px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.5);
            width: 100%;
            max-width: 680px;
            padding: 32px;
            position: relative;
            overflow: hidden;
        }

        .container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #10b981, #6366f1);
        }

        .header {
            margin-bottom: 24px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 1.3rem;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .user-tag {
            font-size: 0.85rem;
            background: #334155;
            padding: 4px 12px;
            border-radius: 20px;
            color: var(--text-muted);
        }

        /* Forms / Login */
        .auth-container {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .input-group label {
            font-size: 0.9rem;
            color: var(--text-muted);
        }

        .input-field {
            background: #0f172a;
            border: 1px solid var(--border-color);
            padding: 12px 16px;
            border-radius: 8px;
            color: white;
            font-size: 1rem;
            outline: none;
        }

        .input-field:focus {
            border-color: var(--primary);
        }

        /* Buttons */
        .btn {
            background-color: var(--primary);
            color: #0f172a;
            border: none;
            border-radius: 8px;
            padding: 12px 24px;
            font-size: 0.95rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.2s ease;
            width: 100%;
        }

        .btn:hover {
            background-color: var(--primary-dark);
            color: white;
        }

        .btn:disabled {
            background-color: #334155;
            color: #64748b;
            cursor: not-allowed;
        }

        /* Quiz Layout */
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #0f172a;
            border-radius: 4px;
            margin-top: 10px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #10b981, #3b82f6);
            width: 0%;
            transition: width 0.3s ease;
        }

        .question-text {
            font-size: 1.15rem;
            margin: 20px 0;
            line-height: 1.5;
        }

        .options-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 20px;
        }

        .option-btn {
            background-color: #0f172a;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 14px 18px;
            font-size: 0.95rem;
            color: var(--text-main);
            text-align: left;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .option-btn:hover {
            border-color: var(--primary);
            background-color: #1e293b;
        }

        .option-btn.selected {
            border-color: var(--primary);
            background-color: rgba(16, 185, 129, 0.1);
        }

        .option-btn.correct {
            background-color: var(--correct-bg);
            border-color: var(--correct-color);
            color: #4ade80;
        }

        .option-btn.incorrect {
            background-color: var(--incorrect-bg);
            border-color: var(--incorrect-color);
            color: #f87171;
        }

        .explanation-box {
            background-color: #0f172a;
            border-left: 4px solid var(--primary);
            border-radius: 4px;
            padding: 16px;
            margin-bottom: 20px;
            font-size: 0.92rem;
            line-height: 1.5;
        }

        /* Results & Stats */
        .stats-card {
            background: #0f172a;
            border-radius: 8px;
            padding: 20px;
            margin: 15px 0;
            display: flex;
            justify-content: space-around;
            text-align: center;
        }

        .stat-item .val {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--primary);
        }

        .stat-item .lbl {
            font-size: 0.8rem;
            color: var(--text-muted);
        }

        .history-list {
            margin: 15px 0;
            max-height: 150px;
            overflow-y: auto;
            text-align: left;
        }

        .history-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 12px;
            border-bottom: 1px solid var(--border-color);
            font-size: 0.85rem;
        }

        .trend-up { color: #4ade80; }
        .trend-down { color: #f87171; }

        .hide { display: none; }
    </style>
</head>
<body>

    <div class="container">
        <!-- Pantalla de Login -->
        <div id="login-screen">
            <div class="header">
                <h1>🧬 BioGen Login</h1>
            </div>
            <div class="auth-container">
                <div class="input-group">
                    <label for="username">Nombre de Usuario / Estudiante:</label>
                    <input type="text" id="username" class="input-field" placeholder="Ej: Isaac" autocomplete="off">
                </div>
                <button class="btn" onclick="iniciarSesion()">Ingresar al Test</button>
            </div>
        </div>

        <!-- Pantalla del Test -->
        <div id="quiz-screen" class="hide">
            <div class="header">
                <h1>🧬 Evaluación Genética</h1>
                <span class="user-tag" id="user-display"></span>
            </div>

            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>

            <div class="question-text" id="question-text">Cargando...</div>
            <div class="options-container" id="options-container"></div>

            <div id="explanation-box" class="explanation-box hide">
                <strong id="explanation-title" style="display:block; margin-bottom:5px;"></strong>
                <span id="explanation-text"></span>
            </div>

            <div style="display: flex; justify-content: space-between; align-items: center;">
                <span id="question-counter" style="color: var(--text-muted); font-size: 0.85rem;"></span>
                <button class="btn" id="next-btn" style="width: auto;" disabled>Comprobar</button>
            </div>
        </div>

        <!-- Pantalla de Resultados e Histórico -->
        <div id="result-screen" class="hide" style="text-align: center;">
            <div class="header">
                <h1>📊 Resultados Obtenidos</h1>
            </div>

            <div class="stats-card">
                <div class="stat-item">
                    <div class="val" id="current-score">0/10</div>
                    <div class="lbl">Nota Actual</div>
                </div>
                <div class="stat-item">
                    <div class="val" id="trend-indicator">-</div>
                    <div class="lbl">Tendencia</div>
                </div>
            </div>

            <h3 style="font-size: 0.95rem; color: var(--text-muted); text-align: left; margin-top: 10px;">Historial de Intentos</h3>
            <div class="history-list" id="history-list"></div>

            <button class="btn" onclick="iniciarTest()" style="margin-top: 15px;">Intentar Nuevamente</button>
            <button class="btn" onclick="cerrarSesion()" style="background: transparent; color: var(--text-muted); border: 1px solid var(--border-color); margin-top: 8px;">Cerrar Sesión</button>
        </div>
    </div>

    <script>
        const bancoBase = [
            {
                pregunta: "¿En qué etapa del ciclo celular ocurre la replicación del ADN?",
                opciones: ["Fase S", "Fase G1", "Fase G2", "Mitosis"],
                correcta: "Fase S",
                explicacion: "La fase S (Síntesis) duplica el material genético previo a la división celular."
            },
            {
                pregunta: "¿Cuál es la secuencia complementaria de ADN para 5'-TACGATCATAT-3'?",
                opciones: ["3'-ATGCTAGTATA-5'", "3'-UACGAUCAUAU-5'", "5'-ATGCTAGTATA-3'", "3'-TACGATCATAT-5'"],
                correcta: "3'-ATGCTAGTATA-5'",
                explicacion: "Complementariedad antiparalela: A-T, C-G en dirección 5' a 3'."
            },
            {
                pregunta: "En la primera ley de Mendel, ¿qué proporción fenotípica se espera de Aa x Aa?",
                opciones: ["3:1", "1:2:1", "1:1", "9:3:3:1"],
                correcta: "3:1",
                explicacion: "Los genotipos 1 AA y 2 Aa expresan el fenotipo dominante (3) frente a 1 aa (1)."
            },
            {
                pregunta: "¿En qué fase de la meiosis se separan los cromosomas homólogos?",
                opciones: ["Anafase I", "Anafase II", "Metafase I", "Profase II"],
                correcta: "Anafase I",
                explicacion: "Anafase I separa cromosomas homólogos. Anafase II separa cromátidas hermanas."
            },
            {
                pregunta: "Si un ARNm tiene 336 nucleótidos, ¿cuántos aminoácidos tiene la proteína?",
                opciones: ["111", "112", "110", "335"],
                correcta: "111",
                explicacion: "(336 / 3) = 112 codones. Restando el codón de parada final = 111 aminoácidos."
            },
            {
                pregunta: "¿Qué aminoácido es el primero en la síntesis de proteínas eucariotas?",
                opciones: ["Metionina", "Alanina", "Lisina", "Leucina"],
                correcta: "Metionina",
                explicacion: "El codón de inicio 5'-AUG-3' codifica siempre para Metionina."
            },
            {
                pregunta: "En la replicación, la hebra sintetizada de forma discontinua se denomina:",
                opciones: ["Hebra retrasada", "Hebra líder", "Hebra molde", "Hebra codificante"],
                correcta: "Hebra retrasada",
                explicacion: "Se sintetiza en fragmentos de Okazaki en sentido 5'->3'."
            },
            {
                pregunta: "Razón fenotípica esperada de un cruce de prueba Tt x tt:",
                opciones: ["1 Tt : 1 tt", "3 Tt : 1 tt", "Todos Tt", "1 TT : 2 Tt : 1 tt"],
                correcta: "1 Tt : 1 tt",
                explicacion: "Produce 50% heterocigotos dominantes y 50% homocigotos recesivos."
            },
            {
                pregunta: "¿En qué etapa se obtiene la mejor resolución para un cariotipo?",
                opciones: ["Metafase", "Profase", "Anafase", "Fase G0"],
                correcta: "Metafase",
                explicacion: "Los cromosomas alcanzan su máximo nivel de condensación en el plano ecuatorial."
            },
            {
                pregunta: "Si el cruce entre flor roja y blanca da flores rosadas, es un caso de:",
                opciones: ["Dominancia incompleta", "Codominancia", "Herencia ligada al sexo", "Dominancia completa"],
                correcta: "Dominancia incompleta",
                explicacion: "El heterocigoto expresa un fenotipo intermedio entre ambos progenitores."
            }
        ];

        let usuarioActual = "";
        let preguntasSeleccionadas = [];
        let indiceActual = 0;
        let puntaje = 0;
        let opcionSeleccionada = null;
        let mostrandoExplicacion = false;

        function mezclar(arr) {
            return [...arr].sort(() => Math.random() - 0.5);
        }

        // Sistema de Login y Storage
        function iniciarSesion() {
            const input = document.getElementById("username").value.trim();
            if (!input) return;
            usuarioActual = input;
            document.getElementById("user-display").innerText = usuarioActual;

            document.getElementById("login-screen").classList.add("hide");
            iniciarTest();
        }

        function cerrarSesion() {
            usuarioActual = "";
            document.getElementById("username").value = "";
            document.getElementById("result-screen").classList.add("hide");
            document.getElementById("login-screen").classList.remove("hide");
        }

        function iniciarTest() {
            preguntasSeleccionadas = mezclar(bancoBase).slice(0, 10).map(q => ({
                ...q,
                opciones: mezclar(q.opciones)
            }));

            indiceActual = 0;
            puntaje = 0;

            document.getElementById("result-screen").classList.add("hide");
            document.getElementById("quiz-screen").classList.remove("hide");
            cargarPregunta();
        }

        function cargarPregunta() {
            opcionSeleccionada = null;
            mostrandoExplicacion = false;

            const nextBtn = document.getElementById("next-btn");
            nextBtn.disabled = true;
            nextBtn.innerText = "Comprobar";

            document.getElementById("explanation-box").classList.add("hide");

            const q = preguntasSeleccionadas[indiceActual];
            document.getElementById("question-text").innerText = `${indiceActual + 1}. ${q.pregunta}`;
            document.getElementById("question-counter").innerText = `Pregunta ${indiceActual + 1} de ${preguntasSeleccionadas.length}`;
            document.getElementById("progress").style.width = `${((indiceActual + 1) / preguntasSeleccionadas.length) * 100}%`;

            const container = document.getElementById("options-container");
            container.innerHTML = "";

            q.opciones.forEach(opc => {
                const btn = document.createElement("button");
                btn.className = "option-btn";
                btn.innerText = opc;
                btn.onclick = () => seleccionar(opc, btn);
                container.appendChild(btn);
            });
        }

        function seleccionar(opc, btn) {
            if (mostrandoExplicacion) return;

            document.querySelectorAll(".option-btn").forEach(b => b.classList.remove("selected"));
            btn.classList.add("selected");
            opcionSeleccionada = opc;
            document.getElementById("next-btn").disabled = false;
        }

        document.getElementById("next-btn").onclick = function() {
            if (!mostrandoExplicacion) {
                mostrandoExplicacion = true;
                const q = preguntasSeleccionadas[indiceActual];
                const esCorrecta = opcionSeleccionada === q.correcta;

                if (esCorrecta) puntaje++;

                document.querySelectorAll(".option-btn").forEach(btn => {
                    btn.disabled = true;
                    if (btn.innerText === q.correcta) btn.classList.add("correct");
                    if (btn.innerText === opcionSeleccionada && !esCorrecta) btn.classList.add("incorrect");
                });

                const expBox = document.getElementById("explanation-box");
                document.getElementById("explanation-title").innerText = esCorrecta ? "¡Correcto!" : "Incorrecto";
                document.getElementById("explanation-title").style.color = esCorrecta ? "#4ade80" : "#f87171";
                document.getElementById("explanation-text").innerText = q.explicacion;
                expBox.classList.remove("hide");

                this.innerText = (indiceActual === preguntasSeleccionadas.length - 1) ? "Ver Resultados" : "Siguiente";
            } else {
                indiceActual++;
                if (indiceActual < preguntasSeleccionadas.length) {
                    cargarPregunta();
                } else {
                    guardarYMostrarResultados();
                }
            }
        };

        function guardarYMostrarResultados() {
            document.getElementById("quiz-screen").classList.add("hide");
            document.getElementById("result-screen").classList.remove("hide");

            // Guardar en localStorage
            const claveStorage = `biogen_history_${usuarioActual}`;
            let historial = JSON.parse(localStorage.getItem(claveStorage)) || [];
            
            historial.push({
                fecha: new Date().toLocaleDateString() + " " + new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}),
                puntaje: puntaje
            });

            localStorage.setItem(claveStorage, JSON.stringify(historial));

            // Rendimiento e Indicador de tendencia
            document.getElementById("current-score").innerText = `${puntaje}/10`;
            
            const trendEl = document.getElementById("trend-indicator");
            if (historial.length > 1) {
                const prev = historial[historial.length - 2].puntaje;
                if (puntaje > prev) {
                    trendEl.innerHTML = `<span class="trend-up">▲ Mejora (+${puntaje - prev})</span>`;
                } else if (puntaje < prev) {
                    trendEl.innerHTML = `<span class="trend-down">▼ Bajó (${puntaje - prev})</span>`;
                } else {
                    trendEl.innerHTML = `<span>= Mantuvo</span>`;
                }
            } else {
                trendEl.innerText = "Primer intento";
            }

            // Renderizar Lista Histórica
            const listEl = document.getElementById("history-list");
            listEl.innerHTML = "";
            historial.slice().reverse().forEach((item, idx) => {
                const div = document.createElement("div");
                div.className = "history-item";
                div.innerHTML = `<span>Intento ${historial.length - idx} (${item.fecha})</span> <strong>${item.puntaje}/10</strong>`;
                listEl.appendChild(div);
            });
        }
    </script>
</body>
</html>
