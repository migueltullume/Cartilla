<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🎲 Cartilla de BINGO</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            color: white;
        }

        .container {
            width: 100%;
            max-width: 500px;
        }

        .header {
            text-align: center;
            margin: 20px 0;
        }

        .header h1 {
            font-size: 2.5em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 10px;
        }

        .header p {
            font-size: 1em;
            opacity: 0.9;
        }

        /* Pantalla de inicio */
        #inicio-screen {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
        }

        .btn-grande {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            border: none;
            color: white;
            padding: 18px 40px;
            font-size: 1.2em;
            border-radius: 15px;
            margin: 10px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            transition: transform 0.2s, box-shadow 0.2s;
            font-weight: bold;
            width: 100%;
            max-width: 300px;
        }

        .btn-grande:active {
            transform: scale(0.95);
            box-shadow: 0 2px 8px rgba(0,0,0,0.3);
        }

        .btn-secundario {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        }

        /* Selector de números */
        #selector-screen {
            display: none;
        }

        .columna-letra {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
        }

        .columna-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .letra-titulo {
            font-size: 1.8em;
            font-weight: bold;
            padding: 10px 20px;
            border-radius: 10px;
            color: white;
        }

        .contador-columna {
            font-size: 1.1em;
            font-weight: bold;
            padding: 8px 15px;
            background: rgba(255,255,255,0.2);
            border-radius: 8px;
        }

        .contador-columna.completo {
            background: rgba(76, 175, 80, 0.4);
            animation: pulse 0.5s;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .numeros-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
        }

        .numero-btn {
            aspect-ratio: 1;
            background: rgba(255,255,255,0.2);
            border: 2px solid rgba(255,255,255,0.3);
            color: white;
            font-size: 1.1em;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .numero-btn:active {
            transform: scale(0.9);
        }

        .numero-btn.seleccionado {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            border-color: white;
            box-shadow: 0 0 15px rgba(240, 147, 251, 0.6);
        }

        .numero-btn:disabled {
            opacity: 0.3;
            cursor: not-allowed;
        }

        .aviso-centro {
            text-align: center;
            font-size: 0.9em;
            margin-top: 8px;
            padding: 8px;
            background: rgba(255, 193, 7, 0.3);
            border-radius: 8px;
            font-weight: bold;
        }

        .contador-total {
            text-align: center;
            font-size: 1.3em;
            margin: 15px 0;
            padding: 15px;
            background: rgba(255,255,255,0.15);
            border-radius: 12px;
            font-weight: bold;
        }

        /* Cartilla de juego */
        #juego-screen {
            display: none;
        }

        .cartilla {
            background: rgba(255,255,255,0.95);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        .bingo-header {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 5px;
            margin-bottom: 10px;
        }

        .letra {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            text-align: center;
            padding: 15px;
            font-size: 1.8em;
            font-weight: bold;
            border-radius: 10px;
        }

        .cartilla-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
        }

        .casilla {
            aspect-ratio: 1;
            background: white;
            border: 3px solid #667eea;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5em;
            font-weight: bold;
            color: #333;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .casilla:active {
            transform: scale(0.9);
        }

        .casilla.marcada {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            border-color: #f5576c;
            box-shadow: 0 0 20px rgba(245, 87, 108, 0.5);
        }

        .casilla.marcada::after {
            content: '✓';
            position: absolute;
            font-size: 2em;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .casilla.libre {
            background: linear-gradient(135deg, #ffd89b 0%, #19547b 100%);
            color: white;
            cursor: default;
        }

        .controles {
            margin-top: 20px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .btn-control {
            flex: 1;
            min-width: 140px;
            padding: 15px;
            border: none;
            border-radius: 12px;
            font-size: 1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn-control:active {
            transform: scale(0.95);
        }

        .btn-nueva {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-limpiar {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }

        .creditos {
            text-align: center;
            margin-top: 20px;
            padding: 5px;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            font-size: 0.85em;
            font-weight: bold;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .nombre-jugador {
            text-align: center;
            font-size: 1.3em;
            font-style: italic;
            font-weight: bold;
            color: #ffd700;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.4);
            margin: 10px 0;
            text-transform: uppercase;
        }

        .input-nombre {
            width: 100%;
            max-width: 300px;
            padding: 15px;
            font-size: 1.1em;
            border: 3px solid rgba(255,255,255,0.3);
            border-radius: 12px;
            background: rgba(255,255,255,0.9);
            color: #333;
            text-align: center;
            font-weight: bold;
            margin: 15px 0;
        }

        .input-nombre:focus {
            outline: none;
            border-color: #f5576c;
            box-shadow: 0 0 15px rgba(245, 87, 108, 0.5);
        }

        /* Modal de victoria */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .modal-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            animation: bounce 0.5s;
        }

        @keyframes bounce {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        .modal-content h2 {
            color: #f5576c;
            font-size: 3em;
            margin-bottom: 20px;
        }

        .modal-content p {
            color: #333;
            font-size: 1.2em;
            margin-bottom: 30px;
        }

        .btn-cerrar-modal {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 12px;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
        }

        /* Animación de confeti */
        @keyframes confetti {
            0% { transform: translateY(0) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
        }

        .instrucciones {
            background: rgba(255,255,255,0.1);
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
            font-size: 0.9em;
            line-height: 1.6;
        }

        .letra-b { background: linear-gradient(135deg, #e74c3c, #c0392b); }
        .letra-i { background: linear-gradient(135deg, #3498db, #2980b9); }
        .letra-n { background: linear-gradient(135deg, #27ae60, #229954); }
        .letra-g { background: linear-gradient(135deg, #f39c12, #d68910); }
        .letra-o { background: linear-gradient(135deg, #9b59b6, #8e44ad); }
    </style>
</head>
<body>
    <div class="container">
        <!-- Pantalla de inicio -->
        <div id="inicio-screen">
            <div class="header">
                <h1>🎲 CARTILLA DE BINGO</h1>
                <p>Elige cómo crear tu cartilla</p>
            </div>

            <div class="instrucciones">
                📋 <strong>Instrucciones:</strong><br>
                • Selecciona 24 números o genera una cartilla aleatoria<br>
                • Toca los números cuando salgan para marcarlos<br>
                • Completa una línea para ganar BINGO<br>
                • El centro es LIBRE (ya está marcado)
            </div>

            <input type="text" id="input-nombre" class="input-nombre" placeholder="Ingresa tu nombre" maxlength="20">

            <button class="btn-grande" onclick="mostrarSelector()">
                🎯 Elegir Mis Números
            </button>
            <button class="btn-grande btn-secundario" onclick="generarCartillaAleatoria()">
                🎲 Cartilla Aleatoria
            </button>
        </div>

        <!-- Pantalla de selección de números -->
        <div id="selector-screen">
            <div class="header">
                <h2>Selecciona Tus Números</h2>
                <div class="contador-total">
                    Total: <span id="contador-total">0</span> / 24
                </div>
            </div>

            <!-- Columna B -->
            <div class="columna-letra">
                <div class="columna-header">
                    <div class="letra-titulo letra-b">B (1-15)</div>
                    <div class="contador-columna" id="contador-b">0/5</div>
                </div>
                <div class="numeros-grid" id="numeros-b"></div>
            </div>

            <!-- Columna I -->
            <div class="columna-letra">
                <div class="columna-header">
                    <div class="letra-titulo letra-i">I (16-30)</div>
                    <div class="contador-columna" id="contador-i">0/5</div>
                </div>
                <div class="numeros-grid" id="numeros-i"></div>
            </div>

            <!-- Columna N -->
            <div class="columna-letra">
                <div class="columna-header">
                    <div class="letra-titulo letra-n">N (31-45)</div>
                    <div class="contador-columna" id="contador-n">0/4</div>
                </div>
                <div class="numeros-grid" id="numeros-n"></div>
                <div class="aviso-centro">⭐ Centro será LIBRE</div>
            </div>

            <!-- Columna G -->
            <div class="columna-letra">
                <div class="columna-header">
                    <div class="letra-titulo letra-g">G (46-60)</div>
                    <div class="contador-columna" id="contador-g">0/5</div>
                </div>
                <div class="numeros-grid" id="numeros-g"></div>
            </div>

            <!-- Columna O -->
            <div class="columna-letra">
                <div class="columna-header">
                    <div class="letra-titulo letra-o">O (61-75)</div>
                    <div class="contador-columna" id="contador-o">0/5</div>
                </div>
                <div class="numeros-grid" id="numeros-o"></div>
            </div>

            <button class="btn-grande" id="btn-confirmar" onclick="confirmarSeleccion()" disabled>
                ✓ Confirmar Cartilla
            </button>
            <button class="btn-grande btn-secundario" onclick="volverInicio()">
                ← Volver
            </button>
        </div>

        <!-- Pantalla de juego -->
        <div id="juego-screen">
            <div class="header">
                <h1>🎲 MI CARTILLA</h1>
                <div class="nombre-jugador" id="nombre-jugador"></div>
            </div>

            <div class="cartilla">
                <div class="bingo-header">
                    <div class="letra letra-b">B</div>
                    <div class="letra letra-i">I</div>
                    <div class="letra letra-n">N</div>
                    <div class="letra letra-g">G</div>
                    <div class="letra letra-o">O</div>
                </div>

                <div class="cartilla-grid" id="cartilla-grid"></div>
            </div>

            <div class="controles">
                <button class="btn-control btn-limpiar" onclick="limpiarMarcas()">
                    🔄 Limpiar Marcas
                </button>
                <button class="btn-control btn-nueva" onclick="nuevaCartilla()">
                    🎲 Nueva Cartilla
                </button>
            </div>

            <div class="creditos">
                Creditos: Miguel Túllume
            </div>
        </div>
    </div>

    <!-- Modal de victoria -->
    <div class="modal" id="modal-bingo">
        <div class="modal-content">
            <h2>🎉 ¡BINGO! 🎉</h2>
            <p id="mensaje-bingo">¡Felicitaciones!</p>
            <button class="btn-cerrar-modal" onclick="cerrarModal()">
                Continuar Jugando
            </button>
        </div>
    </div>

    <script>
        let numerosSeleccionados = {
            b: [],
            i: [],
            n: [],
            g: [],
            o: []
        };
        let cartilla = [];
        let nombreJugador = '';
        let columnasCompletadas = new Set(); // Registrar columnas ya anunciadas
        let apagonAnunciado = false; // Registrar si ya se anunció el apagón

        const limites = {
            b: { min: 1, max: 15, cantidad: 5 },
            i: { min: 16, max: 30, cantidad: 5 },
            n: { min: 31, max: 45, cantidad: 4 }, // Solo 4 porque centro es LIBRE
            g: { min: 46, max: 60, cantidad: 5 },
            o: { min: 61, max: 75, cantidad: 5 }
        };

        // Crear selector de números por columna
        function crearSelector() {
            Object.keys(limites).forEach(letra => {
                const config = limites[letra];
                const contenedor = document.getElementById(`numeros-${letra}`);
                contenedor.innerHTML = '';
                
                for (let i = config.min; i <= config.max; i++) {
                    const btn = document.createElement('button');
                    btn.className = 'numero-btn';
                    btn.textContent = i;
                    btn.onclick = () => toggleNumero(letra, i, btn);
                    contenedor.appendChild(btn);
                }
            });
        }

        // Toggle selección de número
        function toggleNumero(letra, num, btn) {
            const config = limites[letra];
            const seleccionados = numerosSeleccionados[letra];
            
            if (seleccionados.includes(num)) {
                // Deseleccionar
                numerosSeleccionados[letra] = seleccionados.filter(n => n !== num);
                btn.classList.remove('seleccionado');
            } else {
                // Seleccionar solo si no se alcanzó el límite
                if (seleccionados.length < config.cantidad) {
                    numerosSeleccionados[letra].push(num);
                    btn.classList.add('seleccionado');
                }
            }
            
            actualizarContadores();
        }

        function actualizarContadores() {
            let total = 0;
            
            Object.keys(limites).forEach(letra => {
                const cantidad = numerosSeleccionados[letra].length;
                const limite = limites[letra].cantidad;
                total += cantidad;
                
                const contador = document.getElementById(`contador-${letra}`);
                contador.textContent = `${cantidad}/${limite}`;
                
                // Marcar como completo
                if (cantidad === limite) {
                    contador.classList.add('completo');
                } else {
                    contador.classList.remove('completo');
                }
            });
            
            // Actualizar contador total
            document.getElementById('contador-total').textContent = total;
            
            // Habilitar botón solo si se completaron todos
            document.getElementById('btn-confirmar').disabled = total !== 24;
        }

        function mostrarSelector() {
            const nombre = document.getElementById('input-nombre').value.trim();
            if (!nombre) {
                alert('¡Por favor ingresa tu nombre!');
                document.getElementById('input-nombre').focus();
                return;
            }
            nombreJugador = nombre;
            document.getElementById('inicio-screen').style.display = 'none';
            document.getElementById('selector-screen').style.display = 'block';
            crearSelector();
        }

        function volverInicio() {
            document.getElementById('selector-screen').style.display = 'none';
            document.getElementById('inicio-screen').style.display = 'block';
            numerosSeleccionados = { b: [], i: [], n: [], g: [], o: [] };
            document.getElementById('input-nombre').value = nombreJugador;
        }

        function confirmarSeleccion() {
            // Combinar todos los números seleccionados
            const total = numerosSeleccionados.b.length + 
                         numerosSeleccionados.i.length + 
                         numerosSeleccionados.n.length + 
                         numerosSeleccionados.g.length + 
                         numerosSeleccionados.o.length;
            
            if (total === 24) {
                crearCartilla();
                mostrarJuego();
            }
        }

        function generarCartillaAleatoria() {
            const nombre = document.getElementById('input-nombre').value.trim();
            if (!nombre) {
                alert('¡Por favor ingresa tu nombre!');
                document.getElementById('input-nombre').focus();
                return;
            }
            nombreJugador = nombre;
            
            // Limpiar selección previa
            numerosSeleccionados = { b: [], i: [], n: [], g: [], o: [] };
            
            const rangos = {
                b: [1, 15, 5],    // min, max, cantidad
                i: [16, 30, 5],
                n: [31, 45, 4],   // Solo 4 porque centro es LIBRE
                g: [46, 60, 5],
                o: [61, 75, 5]
            };

            // Generar números aleatorios para cada columna
            Object.keys(rangos).forEach(letra => {
                const [min, max, cantidad] = rangos[letra];
                const disponibles = [];
                
                for (let i = min; i <= max; i++) {
                    disponibles.push(i);
                }
                
                // Mezclar y tomar los primeros
                disponibles.sort(() => Math.random() - 0.5);
                numerosSeleccionados[letra] = disponibles.slice(0, cantidad);
            });

            crearCartilla();
            mostrarJuego();
        }

        function crearCartilla() {
            cartilla = [];
            const grid = document.getElementById('cartilla-grid');
            grid.innerHTML = '';
            
            // Resetear los anuncios al crear nueva cartilla
            columnasCompletadas.clear();
            apagonAnunciado = false;

            // Organizar números por columnas
            const columnas = {
                b: numerosSeleccionados.b.sort((a, b) => a - b),
                i: numerosSeleccionados.i.sort((a, b) => a - b),
                n: numerosSeleccionados.n.sort((a, b) => a - b),
                g: numerosSeleccionados.g.sort((a, b) => a - b),
                o: numerosSeleccionados.o.sort((a, b) => a - b)
            };

            // Crear grid 5x5
            for (let fila = 0; fila < 5; fila++) {
                for (let col = 0; col < 5; col++) {
                    const casilla = document.createElement('div');
                    casilla.className = 'casilla';
                    
                    // Centro es LIBRE
                    if (fila === 2 && col === 2) {
                        casilla.textContent = '★';
                        casilla.classList.add('libre', 'marcada');
                        casilla.dataset.libre = 'true';
                        cartilla.push({ numero: 'LIBRE', marcada: true, libre: true });
                    } else {
                        // Obtener el número correspondiente de la columna
                        const letras = ['b', 'i', 'n', 'g', 'o'];
                        const letra = letras[col];
                        const columnaNums = columnas[letra];
                        
                        // Para la columna N, ajustar el índice
                        let idx = fila;
                        if (col === 2 && fila > 2) {
                            idx = fila - 1; // Saltar el centro
                        }
                        
                        const numero = columnaNums[idx];
                        casilla.textContent = numero;
                        casilla.onclick = () => marcarCasilla(fila * 5 + col);
                        cartilla.push({ numero: numero, marcada: false, libre: false });
                    }
                    
                    grid.appendChild(casilla);
                }
            }
        }

        function mostrarJuego() {
            document.getElementById('inicio-screen').style.display = 'none';
            document.getElementById('selector-screen').style.display = 'none';
            document.getElementById('juego-screen').style.display = 'block';
            document.getElementById('nombre-jugador').textContent = nombreJugador;
        }

        function marcarCasilla(index) {
            if (cartilla[index].libre) return;
            
            cartilla[index].marcada = !cartilla[index].marcada;
            
            const casillas = document.querySelectorAll('.casilla');
            casillas[index].classList.toggle('marcada');
            
            // Vibración fuerte al marcar/desmarcar
            if (navigator.vibrate) {
                try {
                    const resultado = navigator.vibrate([200, 100, 200]);
                    console.log('Vibración ejecutada:', resultado);
                } catch(e) {
                    console.error('Error al vibrar:', e);
                    alert('Error de vibración: ' + e.message);
                }
            } else {
                console.warn('Vibración no soportada por el navegador');
                // Mostrar alerta solo la primera vez
                if (!window.vibracionAdvertida) {
                    alert('Tu navegador no soporta vibración. Usa Chrome en Android.');
                    window.vibracionAdvertida = true;
                }
            }
            
            verificarBingo();
        }

        function verificarBingo() {
            // Verificar columnas
            for (let i = 0; i < 5; i++) {
                const columna = [0, 1, 2, 3, 4].map(j => cartilla[j * 5 + i]);
                if (columna.every(c => c.marcada)) {
                    const letras = ['B', 'I', 'N', 'G', 'O'];
                    const letraColumna = letras[i];
                    
                    // Solo anunciar si no se ha anunciado antes
                    if (!columnasCompletadas.has(i)) {
                        columnasCompletadas.add(i);
                        mostrarBingo(`¡Columna ${letraColumna} completa!`);
                        return;
                    }
                }
            }

            // Verificar cartilla completa (APAGÓN)
            if (cartilla.every(c => c.marcada) && !apagonAnunciado) {
                apagonAnunciado = true;
                mostrarBingo('¡APAGÓN! ¡CARTILLA COMPLETA! 🏆');
            }
        }

        function mostrarBingo(mensaje) {
            // Vibrar si está disponible
            if (navigator.vibrate) {
                navigator.vibrate([200, 100, 200, 100, 200]);
            }

            document.getElementById('mensaje-bingo').textContent = mensaje;
            document.getElementById('modal-bingo').style.display = 'flex';

            // Reproducir sonido si está disponible
            try {
                const audio = new Audio('data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhBSuBzvLZiTYIFmS57OihUBALTqXh8LRlHQU2j9fyz3wvBSl+yvDcjkEMEl+37+6nVxULQ53e8cJtIgcue8zx4I9DEhJfu+/uq1kVDEOe3vHEbSIHLXrJ8d2MPVBSL2pQ')
                audio.play();
            } catch(e) {}
        }

        function cerrarModal() {
            document.getElementById('modal-bingo').style.display = 'none';
        }

        function limpiarMarcas() {
            cartilla.forEach((c, i) => {
                if (!c.libre) {
                    c.marcada = false;
                }
            });
            
            const casillas = document.querySelectorAll('.casilla');
            casillas.forEach((c, i) => {
                if (!cartilla[i].libre) {
                    c.classList.remove('marcada');
                }
            });
            
            // Resetear los anuncios
            columnasCompletadas.clear();
            apagonAnunciado = false;
        }

        function nuevaCartilla() {
            if (confirm('¿Estás seguro de crear una nueva cartilla?')) {
                document.getElementById('juego-screen').style.display = 'none';
                document.getElementById('inicio-screen').style.display = 'block';
                numerosSeleccionados = { b: [], i: [], n: [], g: [], o: [] };
                cartilla = [];
                document.getElementById('input-nombre').value = '';
                nombreJugador = '';
            }
        }

        // Inicializar
        window.onload = () => {
            // Evitar zoom en doble tap
            document.addEventListener('dblclick', (e) => {
                e.preventDefault();
            });
        };
    </script>
</body>
</html>
