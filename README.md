<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Полноэкранное адаптивное видео</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            height: 100%;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            color: #fff;
        }

        .video-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            overflow: hidden;
        }

        #background-video {
            position: absolute;
            top: 50%;
            left: 50%;
            min-width: 100%;
            min-height: 100%;
            width: auto;
            height: auto;
            transform: translate(-50%, -50%);
            object-fit: cover;
        }

        /* Затемнение видео для лучшей читаемости текста */
        .video-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.3);
            z-index: 0;
        }

        .content {
            position: relative;
            z-index: 1;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
        }

        h1 {
            font-size: 3.5rem;
            margin-bottom: 20px;
            text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.7);
        }

        p {
            font-size: 1.5rem;
            max-width: 800px;
            line-height: 1.6;
            margin-bottom: 30px;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.7);
        }

        .controls {
            margin-top: 20px;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
        }

        button {
            padding: 12px 24px;
            font-size: 1rem;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            border: 2px solid white;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        button:hover {
            background-color: rgba(255, 255, 255, 0.9);
            color: #000;
        }

        .video-info {
            position: absolute;
            bottom: 20px;
            left: 0;
            width: 100%;
            text-align: center;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.8);
            padding: 0 20px;
        }

        /* Адаптивные стили для разных размеров экрана */
        @media (max-width: 1200px) {
            h1 {
                font-size: 3rem;
            }
            
            p {
                font-size: 1.3rem;
            }
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 2.5rem;
            }
            
            p {
                font-size: 1.1rem;
                max-width: 90%;
            }
            
            button {
                padding: 10px 20px;
                font-size: 0.9rem;
            }
        }

        @media (max-width: 480px) {
            h1 {
                font-size: 2rem;
            }
            
            p {
                font-size: 1rem;
            }
            
            .controls {
                flex-direction: column;
                align-items: center;
            }
            
            button {
                width: 80%;
                max-width: 250px;
            }
        }

        @media (max-height: 600px) {
            h1 {
                font-size: 2rem;
                margin-bottom: 10px;
            }
            
            p {
                font-size: 1rem;
                margin-bottom: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="video-container">
        <video id="background-video" autoplay muted playsinline>
            <!-- Здесь можно заменить src на ссылку на ваше видео -->
            <source src="https://assets.mixkit.co/videos/preview/mixkit-sunset-over-a-lake-1486-large.mp4" type="video/mp4">
            Ваш браузер не поддерживает видео элемент.
        </video>
        <div class="video-overlay"></div>
    </div>

    <div class="content">
        <h1>Полноэкранное адаптивное видео</h1>
        <p>Этот сайт полностью адаптирован под все разрешения экрана. Видео автоматически воспроизводится на полный экран и зацикливается по окончании. Попробуйте изменить размер окна браузера, чтобы увидеть адаптацию.</p>
        
        <div class="controls">
            <button id="play-pause-btn">Пауза</button>
            <button id="mute-unmute-btn">Включить звук</button>
            <button id="restart-btn">Перезапустить видео</button>
        </div>
    </div>

    <div class="video-info">
        Видео автоматически воспроизводится и зацикливается. Текущее состояние: <span id="video-status">воспроизведение</span>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const video = document.getElementById('background-video');
            const playPauseBtn = document.getElementById('play-pause-btn');
            const muteUnmuteBtn = document.getElementById('mute-unmute-btn');
            const restartBtn = document.getElementById('restart-btn');
            const videoStatus = document.getElementById('video-status');
            
            // Настройка видео для автоматического воспроизведения и зацикливания
            video.loop = true;
            video.muted = true; // Автовоспроизведение работает только с muted
            
            // Обработчик завершения видео (на всякий случай, если loop не сработает)
            video.addEventListener('ended', function() {
                video.currentTime = 0;
                video.play();
                videoStatus.textContent = 'воспроизведение (зациклено)';
            });
            
            // Обработчик начала воспроизведения
            video.addEventListener('play', function() {
                videoStatus.textContent = 'воспроизведение';
            });
            
            // Обработчик паузы
            video.addEventListener('pause', function() {
                videoStatus.textContent = 'на паузе';
            });
            
            // Кнопка пауза/воспроизведение
            playPauseBtn.addEventListener('click', function() {
                if (video.paused) {
                    video.play();
                    playPauseBtn.textContent = 'Пауза';
                } else {
                    video.pause();
                    playPauseBtn.textContent = 'Воспроизвести';
                }
            });
            
            // Кнопка включения/выключения звука
            muteUnmuteBtn.addEventListener('click', function() {
                if (video.muted) {
                    video.muted = false;
                    muteUnmuteBtn.textContent = 'Выключить звук';
                } else {
                    video.muted = true;
                    muteUnmuteBtn.textContent = 'Включить звук';
                }
            });
            
            // Кнопка перезапуска видео
            restartBtn.addEventListener('click', function() {
                video.currentTime = 0;
                video.play();
                playPauseBtn.textContent = 'Пауза';
            });
            
            // Адаптация размера видео при изменении размера окна
            window.addEventListener('resize', function() {
                // Видео само адаптируется благодаря CSS
                // Но можно добавить дополнительную логику при необходимости
            });
            
            // Попытка автоматического воспроизведения (на случай, если autoplay не сработал)
            function attemptAutoPlay() {
                const playPromise = video.play();
                
                if (playPromise !== undefined) {
                    playPromise.then(_ => {
                        console.log('Автовоспроизведение успешно');
                    }).catch(error => {
                        console.log('Автовоспроизведение не удалось: ', error);
                        // Показываем сообщение пользователю
                        videoStatus.textContent = 'нажмите кнопку воспроизведения';
                        playPauseBtn.textContent = 'Воспроизвести';
                    });
                }
            }
            
            // Попробуем воспроизвести видео при загрузке
            attemptAutoPlay();
            
            // Альтернативная попытка воспроизведения при взаимодействии с пользователем
            document.addEventListener('click', function initVideoPlay() {
                if (video.paused) {
                    attemptAutoPlay();
                }
                // Удаляем обработчик после первого клика
                document.removeEventListener('click', initVideoPlay);
            });
        });
    </script>
</body>
</html>
