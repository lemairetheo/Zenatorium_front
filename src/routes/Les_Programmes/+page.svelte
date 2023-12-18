<script>
    import {fade} from "svelte/transition";

    let tab = [
        ["/img.png", "https://www.youtube-nocookie.com/embed/9W4l7RS49IM?si=rflcXiwfXwegygz0&amp;controls=0&amp;start=22"],
        ["/img.png", "https://www.youtube-nocookie.com/embed/dQw4w9WgXcQ?si=mBq5dNlwiDv5s7ac&amp;controls=0&amp;start=0"],
        ["/img.png", "https://www.youtube-nocookie.com/embed/dQw4w9WgXcQ?si=mBq5dNlwiDv5s7ac&amp;controls=0&amp;start=0"]
    ]

    let display_mess = false
    let display_video = false
    let index_mouseenter = 0;
    let index_video = 0

    function pauseVideo() {
        // Obtenez l'élément iframe par son ID
        var iframe = document.getElementById('videoFrame');
        if (iframe) {
            var player = iframe.contentWindow;
            player.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*');
        }
    }

</script>



<style>
    .mybg {
        background: hsla(288, 100%, 34%, 1);
        background: linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
        background: -moz-linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
        background: -webkit-linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
    }
</style>
