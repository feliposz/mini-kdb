---
title: Youtube-DL
--- 

## Download 

Link: <https://github.com/ytdl-org/youtube-dl>

## Ações

Baixar playlist extraindo som em MP3 (excluir vídeo!)

    youtube-dl.exe --extract-audio --audio-format mp3 --yes-playlist --no-overwrites --continue https://www.youtube.com/watch?v=XXXXXXXXXX

Baixar extraindo som em MP3 (excluir vídeo!)

    youtube-dl.exe -x --audio-format mp3 https://www.youtube.com/watch?v=XXXXXXXXXX

Baixar extraindo som em MP3 com legendas em português e qualidade média (480p)

    youtube-dl.exe --keep-video --extract-audio --audio-format mp3 --sub-lang ptBR --write-sub --no-overwrites --continue --format "bestvideo[height<=480]+bestaudio/best[height<=480]" https://www.crunchyroll.com/pt-br/folktales-from-japan/episode-1-the-old-man-who-made-the-dead-trees-blossom-the-man-who-bought-dreams-the-rat-sutra-594953

Baixar playlist completa extraindo som em MP3 com legendas em japonês e qualidade média (480p)

    youtube-dl.exe --keep-video --extract-audio --audio-format mp3 --sub-lang ja --write-sub --yes-playlist --no-overwrites --continue --format "bestvideo[height<=480]+bestaudio/best[height<=480]" --ignore-errors https://www.youtube.com/playlist?list=XXXXXXXXXX

Listar legendas disponíveis (não baixar!)

    youtube-dl.exe  --skip-download --list-subs https://www.youtube.com/watch?v=XXXXXXXXXX

Baixar somente as legendas:

    youtube-dl.exe  --skip-download --write-sub --sub-lang ptBR https://www.crunchyroll.com/pt-br/XXXXXXXXXX/episode-XXXXXXXXXX

Usando cabeçalhos (viki)
 
    youtube-dl.exe --add-header "x-viki-app-ver: 2.2.5.1428709186" --add-header "x-viki-as-id: 100005a" --keep-video --extract-audio --audio-format mp3 --no-overwrites --continue --ignore-errors https://www.viki.com/videos/XXXXXXXXXX

## ffmpeg

Redimensionar vídeo:
  
    ffmpeg.exe -vf scale="848:480" -i entrada.mp4 saida.mp4
