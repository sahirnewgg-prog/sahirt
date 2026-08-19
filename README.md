<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HackerCraft - Batería & Ruido</title>
    <style>
        /* ===== ESTILO MINECRAFT + HACKER ===== */
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a0f0a;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Press Start 2P', monospace;
            image-rendering: pixelated;
            padding: 16px;
        }

        /* Panel principal con textura de piedra pixelada */
        .panel {
            background: #2d2d2d;
            padding: 30px 25px;
            border: 8px solid #5a5a5a;
            border-radius: 20px;
            box-shadow: 0 0 0 4px #1a1a1a, 0 0 0 8px #3d3d3d, 0 0 30px #00ff41;
            max-width: 700px;
            width: 100%;
            transition: box-shadow 0.2s;
            image-rendering: pixelated;
            background-image: 
                linear-gradient(45deg, #2d2d2d 25%, #3a3a3a 25%, #3a3a3a 50%, #2d2d2d 50%, #2d2d2d 75%, #3a3a3a 75%, #3a3a3a 100%);
            background-size: 16px 16px;
        }

        /* Título retro */
        h1 {
            text-align: center;
            color: #00ff41;
            text-shadow: 0 0 10px #00ff41, 0 0 20px #00aa22;
            font-size: 1.8rem;
            letter-spacing: 4px;
            margin-bottom: 25px;
            word-break: break-word;
        }
        h1 span {
            color: #f0f0f0;
            text-shadow: 0 0 8px #aaa;
        }

        /* Tarjetas de datos */
        .card {
            background: #1e261e;
            border: 4px solid #4caf50;
            border-radius: 16px;
            padding: 20px 18px;
            margin-bottom: 22px;
            box-shadow: inset 0 0 15px #0f3f0f, 0 4px 0 #0a2a0a;
            transition: 0.1s linear;
            image-rendering: pixelated;
        }

        .card-title {
            color: #aaffaa;
            font-size: 0.9rem;
            text-shadow: 0 0 5px #00aa33;
            margin-bottom: 14px;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .card-title i {
            font-size: 1.8rem;
            filter: drop-shadow(0 0 5px #0f0);
        }

        /* Valor principal */
        .value {
            color: #e0ffe0;
            font-size: 2.8rem;
            font-weight: bold;
            text-shadow: 0 0 12px #00ff41;
            letter-spacing: 2px;
        }
        .unit {
            color: #88cc88;
            font-size: 1.2rem;
            margin-left: 8px;
        }

        /* Barra de progreso estilo Minecraft */
        .bar-container {
            background: #0c1a0c;
            border: 4px solid #3d6b3d;
            border-radius: 30px;
            height: 32px;
            margin-top: 14px;
            padding: 3px;
            box-shadow: inset 0 0 10px #0a1f0a;
        }
        .bar-fill {
            height: 100%;
            border-radius: 30px;
            background: #3cff7a;
            background: linear-gradient(90deg, #3cff7a, #8cff8c);
            width: 0%;
            transition: width 0.15s ease-out;
            box-shadow: 0 0 20px #3cff7a;
            image-rendering: pixelated;
        }

        /* Medidor de ruido con aguja (estilo texto) */
        .noise-level {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 12px;
        }
        .noise-value {
            font-size: 2.6rem;
            color: #ffdd77;
            text-shadow: 0 0 15px #ffaa33;
        }
        .noise-label {
            color: #bbbb99;
            font-size: 0.7rem;
        }

        /* Microestado */
        .status-badge {
            background: #0a1a0a;
            border: 2px solid #3c8c3c;
            border-radius: 40px;
            padding: 10px 18px;
            color: #8f8;
            font-size: 0.7rem;
            display: inline-block;
            margin-top: 8px;
        }

        /* Footer */
        .footer {
            text-align: center;
            color: #4f6f4f;
            font-size: 0.6rem;
            margin-top: 20px;
            border-top: 2px dashed #3a5a3a;
            padding-top: 18px;
            letter-spacing: 2px;
        }

        /* Responsive */
        @media (max-width: 500px) {
            h1 { font-size: 1.3rem; }
            .value { font-size: 2rem; }
            .noise-value { font-size: 2rem; }
            .panel { padding: 20px 12px; }
        }
    </style>
</head>
<body>

<div class="panel">
    <h1>⚡ <span>Hacker</span>Craft <span>⚡</span></h1>

    <!-- BATERÍA -->
    <div class="card">
        <div class="card-title">
            <span>🔋</span> BATERÍA
            <span style="margin-left: auto; font-size: 0.6rem; color: #8a8;">NIVEL</span>
        </div>
        <div>
            <span class="value" id="batteryPercent">--</span><span class="unit">%</span>
            <span style="color:#8a8; font-size:0.8rem; margin-left: 10px;" id="batteryStatus">(cargando...)</span>
        </div>
        <div class="bar-container">
            <div class="bar-fill" id="batteryBar" style="width: 0%;"></div>
        </div>
    </div>

    <!-- RUIDO (dB) -->
    <div class="card">
        <div class="card-title">
            <span>🎤</span> RUIDO AMBIENTE
            <span style="margin-left: auto; font-size: 0.6rem; color: #8a8;">dB SPL</span>
        </div>
        <div class="noise-level">
            <span class="noise-value" id="dbValue">--</span><span class="unit">dB</span>
            <span class="noise-label" id="dbDescription">Esperando micrófono...</span>
        </div>
        <div class="bar-container">
            <div class="bar-fill" id="dbBar" style="width: 0%; background: linear-gradient(90deg, #ffaa33, #ff6633);"></div>
        </div>
        <div style="display: flex; justify-content: space-between; font-size:0.6rem; color:#6a8a6a; margin-top:6px;">
            <span>🔇 0 dB</span>
            <span>🔊 100 dB</span>
        </div>
        <div class="status-badge" id="micStatus">🎙️ Micrófono inactivo</div>
    </div>

    <div class="footer">
        ⚡ [ HACKER MODE ] ⚡  •  🟢 SISTEMA ACTIVO
    </div>
</div>

<script>
    (function() {
        "use strict";

        // ===== ELEMENTOS DOM =====
        const batteryPercent = document.getElementById('batteryPercent');
        const batteryStatus = document.getElementById('batteryStatus');
        const batteryBar = document.getElementById('batteryBar');

        const dbValue = document.getElementById('dbValue');
        const dbDescription = document.getElementById('dbDescription');
        const dbBar = document.getElementById('dbBar');
        const micStatus = document.getElementById('micStatus');

        // ===== 1. BATERÍA (API) =====
        function updateBattery() {
            if (!navigator.getBattery) {
                batteryPercent.textContent = '?';
                batteryStatus.textContent = '(no soportado)';
                batteryBar.style.width = '50%';
                batteryBar.style.background = '#aaaaaa';
                return;
            }

            navigator.getBattery().then(battery => {
                function setBatteryUI() {
                    const level = Math.round(battery.level * 100);
                    batteryPercent.textContent = level;
                    batteryBar.style.width = level + '%';

                    let statusText = '';
                    let color = '#3cff7a';
                    if (battery.charging) {
                        statusText = '⚡ CARGANDO';
                        color = '#66ff99';
                    } else {
                        statusText = '🔋 EN USO';
                        if (level < 20) color = '#ff6644';
                        else if (level < 50) color = '#ffaa33';
                        else color = '#3cff7a';
                    }
                    batteryStatus.textContent = statusText;
                    batteryBar.style.background = `linear-gradient(90deg, ${color}, #8cff8c)`;
                }

                setBatteryUI();
                battery.addEventListener('levelchange', setBatteryUI);
                battery.addEventListener('chargingchange', setBatteryUI);
            }).catch(() => {
                batteryPercent.textContent = 'ERR';
                batteryStatus.textContent = '(error)';
            });
        }

        // ===== 2. RUIDO (decibelios) con getUserMedia =====
        let audioContext = null;
        let analyser = null;
        let dataArray = null;
        let animationId = null;
        let isMicActive = false;

        function startNoiseMeter() {
            if (isMicActive) return;

            if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
                micStatus.textContent = '❌ Micrófono no soportado';
                dbValue.textContent = '--';
                dbDescription.textContent = 'Sin soporte';
                return;
            }

            navigator.mediaDevices.getUserMedia({ audio: true, video: false })
                .then(stream => {
                    audioContext = new (window.AudioContext || window.webkitAudioContext)();
                    analyser = audioContext.createAnalyser();
                    analyser.fftSize = 512;
                    analyser.smoothingTimeConstant = 0.8;
                    const source = audioContext.createMediaStreamSource(stream);
                    source.connect(analyser);
                    dataArray = new Uint8Array(analyser.frequencyBinCount);

                    isMicActive = true;
                    micStatus.innerHTML = '🎙️ Micrófono activo';
                    micStatus.style.borderColor = '#66ff66';
                    dbDescription.textContent = 'Midiendo...';

                    // Empezar a actualizar dB
                    if (animationId) cancelAnimationFrame(animationId);
                    updateDecibels();
                })
                .catch(err => {
                    console.warn('Error micrófono:', err);
                    micStatus.textContent = '❌ Permiso denegado';
                    dbValue.textContent = '--';
                    dbDescription.textContent = 'Sin acceso';
                });
        }

        function updateDecibels() {
            if (!analyser || !isMicActive) {
                // Si no hay mic, intentar reiniciar (por si se perdió)
                if (!isMicActive) {
                    dbValue.textContent = '--';
                    dbBar.style.width = '0%';
                }
                animationId = requestAnimationFrame(updateDecibels);
                return;
            }

            analyser.getByteTimeDomainData(dataArray);
            let sum = 0;
            for (let i = 0; i < dataArray.length; i++) {
                const value = (dataArray[i] - 128) / 128;  // normalizado -1 a 1
                sum += value * value;
            }
            const rms = Math.sqrt(sum / dataArray.length);
            // Convertir a decibelios (escala aproximada 0-100 dB)
            let db = 0;
            if (rms > 0.001) {
                db = Math.max(0, Math.min(100, 20 * Math.log10(rms) + 90));
            } else {
                db = 0;
            }
            db = Math.round(db);

            // Mostrar
            dbValue.textContent = db;
            dbBar.style.width = db + '%';

            // Descripción textual
            let desc = '';
            if (db < 15) desc = '🔇 Silencio';
            else if (db < 35) desc = '🤫 Susurro';
            else if (db < 55) desc = '🔊 Conversación';
            else if (db < 75) desc = '📢 Ruidoso';
            else if (db < 90) desc = '⚠️ Muy alto';
            else desc = '🚨 PELIGRO (alto)';
            dbDescription.textContent = desc;

            // Color de barra según dB
            let color = '#ffaa33';
            if (db < 30) color = '#66dd88';
            else if (db < 60) color = '#ffbb33';
            else if (db < 80) color = '#ff8833';
            else color = '#ff3333';
            dbBar.style.background = `linear-gradient(90deg, ${color}, #ff6633)`;

            animationId = requestAnimationFrame(updateDecibels);
        }

        // ===== Iniciar al cargar la página =====
        updateBattery();

        // Iniciar el micrófono cuando el usuario interactúe (por políticas de navegador)
        // Lo lanzamos al hacer clic en cualquier parte del panel (mejor experiencia)
        const panel = document.querySelector('.panel');
        panel.addEventListener('click', () => {
            if (!isMicActive) {
                startNoiseMeter();
            } else if (audioContext && audioContext.state === 'suspended') {
                audioContext.resume();
            }
        });

        // También intentar automáticamente al cargar (algunos navegadores lo permiten)
        window.addEventListener('load', () => {
            // Pequeño delay para que el DOM esté listo
            setTimeout(() => {
                startNoiseMeter();
            }, 400);
        });

        // Si el usuario toca en móvil, activar
        document.addEventListener('touchstart', () => {
            if (!isMicActive) startNoiseMeter();
        });

        // ===== Limpieza =====
        window.addEventListener('beforeunload', () => {
            if (animationId) cancelAnimationFrame(animationId);
            if (audioContext && audioContext.state !== 'closed') {
                audioContext.close();
            }
        });

    })();
</script>

</body>
</html>