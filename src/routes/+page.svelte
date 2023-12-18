<script>
    import { onMount } from "svelte";
    import {fly, fade} from "svelte/transition";
    import Phrise_chrono from "$lib/component/Phrise_chrono.svelte";
    import Rendez_vous from "$lib/component/rendez_vous.svelte";

    let count = 0;
    let interval;
    let visible = false;
    let blur = false;
    let state = 0;
    let display = 0
    let tab = [
        ["/affiche abysse.jpg", "https://www.youtube-nocookie.com/embed/oHccvPoky3o?si=sjreXG9smsTZxfFY"],
        ["/affiche amerindien.jpg", "https://www.youtube-nocookie.com/embed/1S8Fdc662Ac?si=sv52pJSVsNh8Cf7T"],
        ["/affiche arbres.jpg", "https://www.youtube-nocookie.com/embed/VviXYU5BaVs?si=Ka0shUm4GhD7z3Js"],
        ["/affiche asimania.jpg", "https://www.youtube-nocookie.com/embed/911dhjfSANk?si=ynowLsqGw11nylXN"],
        ["/affiche bestioles.jpg", "https://www.youtube-nocookie.com/embed/wTHA5S7CS6g?si=wQfAt873xaFNpGL0"],
        ["/affiche bird.jpg", "https://www.youtube-nocookie.com/embed/S61ryAqywhQ?si=_XnrJDkxTipxaSLp"],
        ["/affiche bivouac.jpg", "https://www.youtube-nocookie.com/embed/pXQbjSrglPs?si=jBXrjQR_xcjRchSg"],
        ["/affiche circus.jpg", "https://www.youtube-nocookie.com/embed/xMrV7CRgxTw?si=Lcs0b_jY2rtxGrSa"],
        ["/affiche doudou&peluche.jpg", "https://www.youtube-nocookie.com/embed/ahwTBwX3G6Y?si=9NTuU6uSKek3UlsU"],
        ["/affiche element.jpg", "https://www.youtube-nocookie.com/embed/repEMzPeaso?si=6_FGEJRdLMxiKuCF"],
        ["/affiche fleurs.jpg", "https://www.youtube-nocookie.com/embed/WJmnyDfmJEg?si=AqiGVqSjVQKzXkCb"],
        ["/affiche licorne.jpg", "https://www.youtube-nocookie.com/embed/AytFCXDx-lc?si=DYuPILrJdCTBRkW3"],
        ["/affiche humanis.jpg", "https://www.youtube-nocookie.com/embed/iQA7ObIbGjw?si=nkvDcr5etvJTXRXO"],
        ["/affiche la terre vue du ciel.jpg", "https://www.youtube-nocookie.com/embed/MfEwDkQdLv4?si=pZ-0eSvCspT-Znaw"],
        ["/Affiche lions ours.jpg", "https://www.youtube-nocookie.com/embed/myfzmaQRdOU?si=OdQ0FjGZlRZwmv0t"],
        ["/affiche matou.jpg", "https://www.youtube-nocookie.com/embed/HaRqlGq30i0?si=azTaekmlGwmhgl4g"],
        ["/affiche monumental.jpg", "https://www.youtube-nocookie.com/embed/-QvtYKkO2bs?si=K6iDM4ez8dN3B6ND"],
        ["/affiche nuance.jpg", "https://www.youtube-nocookie.com/embed/MZMotEfpcsI?si=wgj-EEWkqm8Vr1Pt"],
        ["/affiche reve.jpg", "https://www.youtube-nocookie.com/embed/1d00_heyclA?si=HN5UFlyylavvKH7E"],
        ["/affiche savane.jpg", "https://www.youtube-nocookie.com/embed/PobEQPFRWoA?si=4OltPZPNVm7cnQcF"],
        ["/affiche tableaux.jpg", "https://www.youtube-nocookie.com/embed/m8dYjD6HDnw?si=hUdUQ4qFx0yYnGUr"],
        ["/affiche univers.jpg", "https://www.youtube-nocookie.com/embed/1ArYCo7iVoA?si=B6ADhb6hzTtK4nrK"],
        ["/affiche visage.jpg", "https://www.youtube-nocookie.com/embed/S7aryrWUSAY?si=TMUXwMc_5I8WaQs2"]
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

    function startCounter() {
        interval = setInterval(() => {
            count += 1;
        }, 1000);
    }

    function stopCounter() {
        clearInterval(interval);
    }

    onMount(() => {
        startCounter();
        visible = true;
        return () => {
            stopCounter();
        };


    });

    function change_window(link) {-
        window.location.replace('/'+link);
    }
    function blur_anim() {
        blur = true;
    }

    function state_change(number) {
        state = number
    }

    $: count === 1 && blur_anim();
    $: count === 1 && state_change(1)
    $:count === 2 && state_change(2)
</script>

{#if visible}
    <div class="h-screen flex flex-col items-center justify-center w-screen relative ">
        <img class:blur-sm={blur} src="/img.png" class="w-screen h-screen object-cover absolute -z-10"/>
        {#if state >= 1}
            <h1 in:fly={{y : -300, duration: 800}} class="my-font  md:-mt-[30%] text-center md:leading-[78px] font-bold text-[35px] md:text-[70px] blur-none text-white">
                Découvrez le<br>
                <span class="text-[43px] md:text-[83px]">Zen'Atorium</span>
            </h1>

            <div class="md:hidden flex flex-col justify-center items-center mt-10">
                <div in:fly={{y: 800, duration: 800}} on:click={() => display = 1} style="text-shadow: 1px 1px 6px black;" class=" text-center relative w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    Découvrir<br>
                    les programmes
                </div>
                <img in:fly={{y: 800, duration: 800, delay:150}} src="/test-ying.png" class="w-20 h-20 mt-5 mb-5" alt="ying-yang"/>
                <div in:fly={{y: 800, duration: 800, delay:300}} on:click={() => display = 2} style="text-shadow: 1px 1px 6px black;" class=" relative mt-[-8%] w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    Le concept
                </div>
                <img in:fly={{y: 800, duration: 800, delay:450}} src="/test-ying.png" class="w-20 h-20 mt-5 mb-5" alt="ying-yang"/>
                <div in:fly={{y: 800, duration: 800, delay:600}} on:click={() => display = 3} style="text-shadow: 1px 1px 6px black;" class="text-center relative w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    Les tarifs
                    & <br>prendre<br> rendez vous<br>
                </div>
            </div>
        {/if}
        {#if state >= 2}
            <div class="hidden md:flex absolute left-0 right-0 top-[20%] md:overflow-x-hidden overflow-scroll bottom-0  flex-row transition-all justify-center items-center gap-x-36">
                <div in:fly={{y: 800, duration: 800}} on:click={() => display = 1} style="text-shadow: 1px 1px 6px black;" class="h-64 text-center relative w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    <img src="/test-ying.png" class="object-center -z-10 object-cover w-full h-full group-hover:rotate-[240deg] duration-[800ms] transition-all absolute" alt="Le concept"/>
                    Découvrir<br>
                    les programmes
                </div>
                <div in:fly={{y: 800, duration: 800, delay:300}} on:click={() => display = 2} style="text-shadow: 1px 1px 6px black;" class="h-64 relative mt-[-8%] w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    <img src="/test-ying.png" class="object-center -z-10 object-cover w-full h-full group-hover:rotate-[240deg] duration-[800ms] transition-all absolute" alt="Le concept"/>
                    Le concept
                </div>
                <div in:fly={{y: 800, duration: 800, delay:600}} on:click={() => display = 3} style="text-shadow: 1px 1px 6px black;" class="h-64 text-center relative w-64 flex flex-col justify-center group items-center font-dancing duration-[800ms] font-bold cursor-pointer text-white text-[26px] hover:scale-125 transition-all rounded-full">
                    <img src="/test-ying.png" class="object-center -z-10 object-cover w-full h-full group-hover:rotate-[240deg] duration-[800ms] transition-all absolute" alt="Le concept"/>
                    Les tarifs
                    & <br>prendre<br> rendez vous<br>
                </div>
            </div>
        {/if}
    </div>
{/if}

{#if display === 1}
    <div transition:fly={{x:1500, duration: 1000}} class="absolute z-10 top-0 left-0">
        <img class:blur-sm={blur} src="/img.png" class="w-screen h-screen object-cover absolute -z-10"/>
        <svg on:click={() => display = 0} xmlns="http://www.w3.org/2000/svg" width="63" height="63" fill="white" class="bi bi-arrow-left-circle-fill hover:scale-125 absolute top-10 transition-all left-6 z-50" viewBox="0 0 16 16">
            <path d="M8 0a8 8 0 1 0 0 16A8 8 0 0 0 8 0m3.5 7.5a.5.5 0 0 1 0 1H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5z"/>
        </svg>
        <div class="min-h-screen w-screen mybg flex flex-col md:flex-row md:flex-wrap justify-center pt-10 items-center">
            {#if display_video === true}
                <div class=" z-50 top-[5%] fixed w-[90%] h-[90%]">
                    <div  class="md:h-[5%] w-full bg-black flex flex-col justify-end items-end pr-10">
                        <svg on:click={() => {display_video = false; pauseVideo()}} class="cursor-pointer" xmlns="http://www.w3.org/2000/svg" width="26.946" height="26.946" viewBox="0 0 26.946 26.946" >
                            <g transform="translate(13.473 -19.837) rotate(45)"><path d="M18,7.5V39.607" transform="translate(5.553)" fill="none" stroke="#fff" stroke-linecap="round" stroke-linejoin="round" stroke-width="3" />
                                <path d="M7.5,18H39.607" transform="translate(0 5.553)" fill="none" stroke="#fff" stroke-linecap="round" stroke-linejoin="round" stroke-width="3"/>
                            </g>
                        </svg>
                    </div>
                    <iframe id="videoFrame" class="w-[100%] h-[95%]" src="{tab[index_video][1]}" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
                </div>
            {/if}
            {#each tab as prog, i}
                <div on:click={() => {display_video = true; index_video = i}} on:mouseleave={() => display_mess = false} on:mouseenter={() => {display_mess = true; index_mouseenter = i}} class="md:w-[30%] w-[80%]  hover:scale-125 transition-all group mb-10 h-[200px] relative flex flex-col justify-center items-center bg-cover bg-center">
                    <img src="{prog[0]}" class="object-contain object-center w-full h-full absolute group-hover:blur-sm"/>
                    {#if display_mess === true && i === index_mouseenter}
                        <h1 in:fade={{duration: 800}} class="absolute flex flex-col justify-center items-center font-dancing text-[28px] text-white " style="text-shadow: 3px 1px 17px #000000;">
                            Découvrir le programme
                        </h1>

                    {/if}
                </div>
            {/each}
        </div>
    </div>
{/if}

{#if display === 2}
    <div transition:fly={{x:1500, duration: 1000}} class="absolute z-10 top-0 left-0">
        <img class:blur-sm={blur} src="/img.png" class="w-screen h-screen object-cover absolute -z-10"/>
        <svg on:click={() => display = 0} xmlns="http://www.w3.org/2000/svg" width="63" height="63" fill="white" class="bi bi-arrow-left-circle-fill hover:scale-125 absolute top-3 md:top-10 transition-all left-6 z-50" viewBox="0 0 16 16">
            <path d="M8 0a8 8 0 1 0 0 16A8 8 0 0 0 8 0m3.5 7.5a.5.5 0 0 1 0 1H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5z"/>
        </svg>
        <div class="w-full flex flex-col justify-center mybg">
            <h1 class="my-font  text-4xl mt-20 text-center font-bold text-white">
                Découvrez une Nouvelle Dimension<br> de Détente et Bien-Être
            </h1>
            <Phrise_chrono/>
        </div>

    </div>
{/if}

{#if display === 3}
    <div transition:fly={{x:1500, duration: 1000}} class="absolute z-10 top-0 left-0 mybg">
        <svg on:click={() => display = 0} xmlns="http://www.w3.org/2000/svg" width="63" height="63" fill="white" class="bi bi-arrow-left-circle-fill hover:scale-125 absolute top-3 md:top-10 transition-all left-6 z-50" viewBox="0 0 16 16">
            <path d="M8 0a8 8 0 1 0 0 16A8 8 0 0 0 8 0m3.5 7.5a.5.5 0 0 1 0 1H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5z"/>
        </svg>
        <div class="w-screen h-full flex flex-col justify-center items-center">
            <h1 class="my-font text-center text-4xl text-white font-bold mt-20 md:mt-10">
                Comment venir au Zen'atorium
            </h1>
            <div class=" w-full flex md:w-[80%] mt-4 flex-col md:flex-row justify-center gap-4 items-center">
                <h2 class="text-white text-center my-font text-2xl w-[80%] md:w-[30%]">
                    Le Zen'atorium se trouve au 39 rue de la croix rouge à sailly les cambrai 59554
                </h2>
                <iframe class="w-[80%]" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2554.5518484510512!2d3.178686776972284!3d50.188214971543246!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x47c2bb6e53092759%3A0x12b39db52546e74f!2sZen&#39;atorium!5e0!3m2!1sfr!2sfr!4v1702725481607!5m2!1sfr!2sfr" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
            </div>
            <h2 class="my-font text-2xl text-white text-center mt-10 md:w-[60%] w-[80%]">
                Une séance de relaxation au Zen'atorium est proposée <b>au tarif de 20 €.</b>
                <br><br>
                Nous offrons également un <b>programme de parrainage</b>. Lorsque vous parrainez un ami, celui-ci bénéficie d'une <b>remise de 5 €</b> sur sa prochaine séance. En guise de remerciement, <b>vous recevrez aussi une réduction de 5 € </b>pour votre prochaine visite !
                <br><br>
                Pour parrainer quelqu'un, rien de plus simple ! Lors de votre réservation, il vous suffit d'indiquer dans l'espace commentaire l'adresse e-mail de la personne qui vous a parrainé.
            </h2>

            <h1 class="my-font text-3xl text-center text-white font-bold mt-10">
                N'attendez plus est prenez dès maintenant rendez vous au Zen'atorium !
            </h1>
            <Rendez_vous/>
        </div>

    </div>
{/if}

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;700&display=swap');

    .my-font {
        font-family: 'Quicksand', sans-serif;
    }

    .mybg {
        background: hsla(288, 100%, 34%, 1);
        background: linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
        background: -moz-linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
        background: -webkit-linear-gradient(135deg, hsla(288, 100%, 34%, 1) 0%, hsla(260, 87%, 56%, 1) 51%, hsla(231, 100%, 36%, 1) 100%);
    }
</style>

