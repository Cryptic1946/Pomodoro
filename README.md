<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Timer</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;700&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Raleway', sans-serif;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
            background: url('https://i.redd.it/i16ws44adrub1.jpg') no-repeat center center/cover;
        }

        .mode {
            font-size: 2rem;
            font-weight: bold;
            color: white;
            margin-bottom: 20px;
            text-shadow: 2px 2px 6px rgba(0, 0, 0, 0.5);
        }

        .time {
            font-size: 6rem;
            font-weight: 700;
            color: white;
            text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.6);
            margin-bottom: 30px;
        }

        .controls {
            display: flex;
            gap: 15px;
            margin-bottom: 10px;
        }

        .controls button {
            padding: 12px 20px;
            border: 2px solid white;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.2rem;
            font-weight: bold;
            color: white;
            background: transparent;
            transition: 0.3s ease-in-out;
        }

        .controls button:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        /* Music Toggle */
        .music-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
        }

        .music-btn {
            padding: 10px 15px;
            border: 2px solid white;
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.2);
            color: white;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.3s ease-in-out;
            min-width: 120px;
            text-align: center;
        }

        .music-btn:hover {
            background: rgba(255, 255, 255, 0.4);
        }

        /* Hide YouTube player */
        #youtubePlayer {
            position: absolute;
            width: 0;
            height: 0;
            visibility: hidden;
        }
    </style>
</head>
<body>

    <div class="mode">Pomodoro</div>
    <div class="time">25:00</div>
    
    <div class="controls">
        <button class="pomodoro">Pomodoro</button>
        <button class="short-break">Short Break</button>
        <button class="long-break">Long Break</button>
    </div>

    <div class="controls">
        <button class="start">Start</button>
        <button class="pause">Pause</button>
        <button class="reset">Reset</button>
    </div>

    <!-- Music Toggle -->
    <div class="music-container">
        <button class="music-btn" id="musicToggle" onclick="toggleMusic()">Turn Music ON</button>
    </div>

    <!-- YouTube Iframe (Hidden) -->
    <iframe id="youtubePlayer" src="https://www.youtube.com/embed/BVdHrTfO2PE?autoplay=0&loop=1" allow="autoplay"></iframe>

    <script>
        let timeLeft = 1500; // Default: 25 minutes (Pomodoro)
        let timer;
        let isRunning = false;
        let mode = "pomodoro"; // 'pomodoro', 'short-break', 'long-break'
        const timeDisplay = document.querySelector('.time');
        const modeDisplay = document.querySelector('.mode');
        const youtubePlayer = document.getElementById("youtubePlayer");
        const musicToggle = document.getElementById("musicToggle");

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            timeDisplay.textContent = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
        }

        function switchMode(newMode) {
            clearInterval(timer);
            isRunning = false;
            mode = newMode;

            if (newMode === 'pomodoro') {
                timeLeft = 1500;
                modeDisplay.textContent = "Pomodoro";
            } else if (newMode === 'short-break') {
                timeLeft = 300;
                modeDisplay.textContent = "Short Break";
            } else if (newMode === 'long-break') {
                timeLeft = 600;
                modeDisplay.textContent = "Long Break";
            }
            updateTimerDisplay();
        }

        function startTimer() {
            if (!isRunning) {
                isRunning = true;
                timer = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(timer);
                        isRunning = false;

                        // Auto-switch logic (NO SOUND)
                        if (mode === 'pomodoro') {
                            switchMode('short-break');
                            startTimer();
                        } else if (mode === 'short-break') {
                            switchMode('pomodoro');
                            startTimer();
                        } else {
                            alert("Long break is over! Time to focus.");
                        }
                    }
                }, 1000);
            }
        }

        function pauseTimer() {
            clearInterval(timer);
            isRunning = false;
        }

        function resetTimer() {
            switchMode('pomodoro');
        }

        document.querySelector('.pomodoro').addEventListener('click', () => switchMode('pomodoro'));
        document.querySelector('.short-break').addEventListener('click', () => switchMode('short-break'));
        document.querySelector('.long-break').addEventListener('click', () => switchMode('long-break'));
        document.querySelector('.start').addEventListener('click', startTimer);
        document.querySelector('.pause').addEventListener('click', pauseTimer);
        document.querySelector('.reset').addEventListener('click', resetTimer);

        updateTimerDisplay();

        // Music Player Function
        let musicPlaying = false;
        function toggleMusic() {
            if (!musicPlaying) {
                youtubePlayer.src = "https://www.youtube.com/embed/BVdHrTfO2PE?autoplay=1&loop=1"; 
                musicToggle.textContent = "Turn Music OFF";
                musicPlaying = true;
            } else {
                youtubePlayer.src = "https://www.youtube.com/embed/BVdHrTfO2PE?autoplay=0"; 
                musicToggle.textContent = "Turn Music ON";
                musicPlaying = false;
            }
        }
    </script>

</body>
</html>
