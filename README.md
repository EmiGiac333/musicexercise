<!DOCTYPE html>

<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title></title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="">
</head>

<body>
    <!--[if lt IE 7]>
            <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="#">upgrade your browser</a> to improve your experience.</p>
        <![endif]-->

    <script src="" async defer></script>
</body>

</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
    <main class="container mt-5">
        <div class="row">
            <div class="col-lg-8 col-md-6 col-sm-12">
                <header>
                    <h1 id="songTitle" class="display-4">Select a Song</h1>
                </header>
                <article id="songDescription" class="text-justify">
                    <p>Select a song to see its description here.</p>
                </article><img id="albumCover" src="" class="card-img-top" alt="Album Cover">
                
            </div>
            <div class="col-lg-4 col-md-6 col-sm-12">
                <div class="card" style="width: 20rem;">
                    <audio id="musicPlayer" src="kn.mp3" hidden></audio>
                    <img id="songGif" src="" alt="Song Gif" style="width: 100%; max-width: 400px; margin-top: 20px;">
                    <div class="card-body">
                        <h5 class="card-title">Music Player</h5>
                        <select id="trackSelect" class="form-select" onchange="changeTrack()">
                            <option value="Knocking_on_Heavens_Door.mp3" data-title="Guns N' Roses - Knockin' on Heaven's Door" data-description="Si tratta di una reinterpretazione in chiave hard rock dell'omonimo brano portato al successo nel 1973 dal cantautore statunitense Bob Dylan. Già dal 1987 il gruppo proponeva la propria versione nei loro concerti, incidendone una versione in studio per il film Giorni di tuono.
                            Al momento della sua pubblicazione, il singolo raggiunse la seconda posizione nella classifica britannica dei singoli." data-image="Album.jpg" data-gif="GunsNRoses.gif">Knockin' on Heaven's Door - Guns N' Roses</option>
                            <option value="American_Idiot.mp3" data-title="Green Day- American Idiot" data-description="American Idiot è un singolo del gruppo musicale statunitense Green Day, pubblicato il 31 agosto 2004 come primo estratto dall'album omonimo.
                             Il titolo è una storpiatura dileggiante di American Idol, programma televisivo molto noto trasmesso negli Stati Uniti d'America. Il testo è una polemica verso l'allora presidente statunitense George W. Bush e la sua politica che, secondo il gruppo, è basata sui media e l'alienazione, tentando un lavaggio del cervello al popolo." data-image="Green_Day.png" data-gif="American_idiot.gif">American Idiot - Green Day</option>
                            <option value="Losing_My_Religion.mp3" data-title="R.E.M. - Losing my Religion" data-description="Losing My Religion è un singolo del gruppo musicale statunitense R.E.M., pubblicato il 19 febbraio 1991 come primo estratto dal settimo album in studio Out of Time. La canzone è opera del chitarrista Peter Buck, il quale la compose mentre stava guardando la televisione con in mano un mandolino appena acquistato, cercando in qualche modo di impararlo da solo. Il riff della canzone è infatti eseguito proprio con il mandolino." data-image="Losing_my_religion.jpg" data-gif="rem_losing_my_religion.gif">Losing My Religion - R.E.M.</option>
                        </select>
                        <div class="d-flex justify-content-center mt-3">
                            <button id="playButton" class="btn btn-primary">
                                <i id="playPauseIcon" class="bi bi-play-circle"></i>
                            </button>
                        </div>
                        <div class="volume-control-container mt-3">
                            <i class="bi bi-volume-down-fill"></i>
                            <input type="range" id="volumeControl" class="form-range volume-slider" min="0" max="1" step="0.1" value="0.5">
                            <i class="bi bi-volume-up-fill"></i>
                        </div>
                        <div class="progress mt-3" style="height: 5px;">
                            <div class="progress-bar" role="progressbar" style="width: 0%;" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" id="progressBar"></div>
                        </div>
                        <div class="d-flex justify-content-between mt-2">
                            <span id="currentTime">0:00</span>
                            <span id="totalDuration">0:00</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <script>
        const audio = document.getElementById('musicPlayer');
        const trackSelect = document.getElementById('trackSelect');
        const songTitle = document.getElementById('songTitle');
        const songDescription = document.getElementById('songDescription');
        const albumCover = document.getElementById('albumCover');
        const songGif = document.getElementById('songGif');
        let isPlaying = false;

        document.getElementById('playButton').addEventListener('click', () => {
            if (isPlaying) {
                audio.pause();
            } else {
                audio.play();
            }
            isPlaying = !isPlaying;
            document.getElementById('playPauseIcon').classList.toggle('bi-play-circle');
            document.getElementById('playPauseIcon').classList.toggle('bi-pause-circle');
        });

        audio.addEventListener('loadedmetadata', () => {
            document.getElementById('totalDuration').textContent = formatTime(audio.duration);
        });

        audio.addEventListener('timeupdate', () => {
            const progress = (audio.currentTime / audio.duration) * 100;
            document.getElementById('progressBar').style.width = `${progress}%`;
            document.getElementById('currentTime').textContent = formatTime(audio.currentTime);
        });

        function changeTrack() {
            const selectedOption = trackSelect.options[trackSelect.selectedIndex];
            audio.src = selectedOption.value;
            audio.load();
            songTitle.textContent = selectedOption.getAttribute('data-title');
            songDescription.textContent = selectedOption.getAttribute('data-description');
            albumCover.src = selectedOption.getAttribute('data-image');
            songGif.src = selectedOption.getAttribute('data-gif');
            if (isPlaying) audio.play();
        }

        function formatTime(time) {
            const minutes = Math.floor(time / 60);
            const seconds = Math.floor(time % 60);
            return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
        }
    </script>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.min.js"
        integrity="sha384-0pUGZvbkm6XF6gxjEnlmuGrJXVbNuzT9qBBavbLwCsOGabYfZo0T0to5eqruptLy"
        crossorigin="anonymous"></script>
</body>
</html>
