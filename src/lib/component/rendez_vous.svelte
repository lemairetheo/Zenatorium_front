<script>
    import { onMount } from 'svelte';
    import {fade} from "svelte/transition";

    let rendezvous = [];
    let jours = [];
    let reservation = false
    let date_selected = []
    let email
    let nom_prenom
    let phone
    let commentaire
    let invalid = false
    let validation = false
    const date = new Date();
    const today = new Date().toISOString();

    async function fetchRendezvous() {
        try {
            const response = await fetch('https://api-zenatorium.vercel.app/rendezvous', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ date: date })
            });
            rendezvous = await response.json();
            console.log(rendezvous)
            updateCreneaux(); // Mettre à jour les créneaux une fois les rendez-vous chargés
        } catch (error) {
            console.error("Erreur lors de la récupération des rendez-vous:", error);
        }
    }

    function updateCreneaux() {
        jours = []; // Réinitialisez les jours
        let number_day = 4
        if (window.innerWidth < 500)
            number_day = 1
        for (let i = 0; i < number_day; i++) {
            const jour = new Date(date);
            jour.setHours(0, 0, 0, 0);
            jour.setDate(jour.getDate() + i);

            const plageDuJour = plagesHoraires[jour.getDay()] || [];
            const creneaux = [];

            plageDuJour.forEach(plage => {
                for (let h = plage.start; h < plage.end; h++) {
                    const heure = h;
                    const estReserve = rendezvous.some(rdv => isRdvOnCreneau(rdv, jour, heure));
                    creneaux.push({ heure: formatHeure(heure), estReserve });
                }
            });

            jours.push({ date: jour, creneaux });
        }
    }

    onMount(() => {
        fetchRendezvous();
    });

    const plagesHoraires = {
        1: [{ start: 18, end: 20 }], // Lundi
        2: [{ start: 18, end: 20 }], // Mardi
        3: [{ start: 9, end: 12 }, { start: 18, end: 20 }], // Mercredi
        4: [], // Jeudi (Fermé)
        5: [{ start: 18, end: 20 }], // Vendredi
        6: [{ start: 9, end: 19 }], // Samedi
        0: [{ start: 9, end: 19 }], // Dimanche
    };

    function formatHeure(heure) {
        return heure < 10 ? '0' + heure + ':00' : heure + ':00';
    }

    function isRdvOnCreneau(rdv, jour, heure) {
        const rdvDate = new Date(rdv.date._seconds * 1000);
        const rdvHeure = rdv.heure.padStart(2, '0') + ':00';

        return rdvDate.toDateString() === jour.toDateString() && rdvHeure === formatHeure(heure);
    }

    function formaterDate(date) {
        return date.toLocaleDateString('fr-FR', {
            weekday: 'long',
            day: 'numeric',
            month: 'long',
            year: 'numeric'
        });
    }

    async function verif_and_valid() {
        if (email === undefined || phone === undefined || nom_prenom === undefined) {
            return invalid = true;
        }
        if(commentaire === undefined)
            commentaire = "aucun"
        invalid = false
        try {
            const today = new Date().toISOString();
            const response = await fetch('https://api-zenatorium.vercel.app/reserved_crenaux', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ date: date_selected[1].date, heure : date_selected[0].heure, email : email, programme : "aucun", telephone : phone, commentaire: commentaire, nom_prenom : nom_prenom })
            });
            console.log(response.json)
        } catch (error) {
            console.error("Erreur lors de la validation du rendez-vous:", error);
        }

        reservation = false;
        await fetchRendezvous();
        validation = true;
    }
</script>

{#if validation}
    <div transition:fade={{duration:800}} class="fixed z-50 w-screen top-0 h-screen bg-black bg-opacity-50">
        <div class="absolute flex flex-col justify-center items-center top-0 right-0 shadow-2xl border-solid border-green-600 border-4 rounded-2xl bottom-0 left-0 m-auto h-fit w-fit px-10  py-5 bg-white z-50">
            <svg on:click={() => validation = false} class=" absolute top-2 right-4 transition-all w-[15px] hover:scale-125" xmlns="http://www.w3.org/2000/svg" width="28.68" height="28.681" viewBox="0 0 28.68 28.681">
                <path id="np_cross_1769549_000000" d="M48.291,23.836,36.762,35.365,25.233,23.836l-1.4,1.4L35.365,36.762,23.836,48.291l1.4,1.4L36.762,38.159,48.291,49.688l1.4-1.4L38.159,36.762,49.688,25.233Z" transform="translate(-22.422 -22.422)" fill="black" stroke="#043d6e" stroke-width="2" />
            </svg>

            <h1 class="my-font text-2xl font-bold text-green-600 ">
                Merci Pour votre reservation !
            </h1>

            <h2 class="my-font text-xl font-medium text-green-600">
                Vous allez recevoir un mail recapitulatif !
            </h2>

        </div>
    </div>

{/if}

{#if reservation}
    <div transition:fade={{duration:800}} class="fixed z-[50] w-screen h-screen bg-black top-0 bg-opacity-50">
        <div class="absolute flex flex-col justify-center items-center top-0 right-0 shadow-2xl border-solid border-black border-4 rounded-2xl bottom-0 left-0 m-auto h-fit w-fit px-10  py-5 bg-white z-50">
            <svg on:click={() => reservation = false} class=" absolute top-2 right-4 transition-all w-[15px] hover:scale-125" xmlns="http://www.w3.org/2000/svg" width="28.68" height="28.681" viewBox="0 0 28.68 28.681">
                <path id="np_cross_1769549_000000" d="M48.291,23.836,36.762,35.365,25.233,23.836l-1.4,1.4L35.365,36.762,23.836,48.291l1.4,1.4L36.762,38.159,48.291,49.688l1.4-1.4L38.159,36.762,49.688,25.233Z" transform="translate(-22.422 -22.422)" fill="black" stroke="#043d6e" stroke-width="2" />
            </svg>
            <h1 class="my-font text-2xl font-bold text-center">
                Prise de rendez vous :
            </h1>

            <h2 class="my-font mt-4 text-xl">
                le {formaterDate(date_selected[1].date)} à {date_selected[0].heure}h
            </h2>

            <div class="flex flex-col justify-center items-end">
                <div class="flex mt-5 flex-row justify-center items-center">
                    <label>Email :</label>
                    <input bind:value={email} type="email" class="px-3 ml-2 border-solid border-2 border-black rounded-2xl outline-0 py-2" placeholder="Exemple@zenatorium.fr">
                </div>
                <div class="flex mt-5 flex-row justify-center items-center">
                    <label>Nom et prénom :</label>
                    <input bind:value={nom_prenom} type="text" class="px-3 ml-2 border-solid border-2 border-black rounded-2xl outline-0 py-2" placeholder="Victor Hugo">
                </div>
                <div class="flex mt-5 flex-row justify-center items-center">
                    <label>Numéro de téléphone :</label>
                    <input bind:value={phone} type="tel" class="px-3 ml-2 border-solid border-2 border-black rounded-2xl outline-0 py-2" placeholder="06 75 09 63 72">
                </div>
                <div class="flex mt-5 flex-row justify-center items-center">
                    <label>Commentaire :</label>
                    <input bind:value={commentaire} type="text" class="px-3 ml-2 border-solid border-2 border-black rounded-2xl outline-0 py-2" placeholder="Information supplémentaire">
                </div>
            </div>

            {#if invalid}
                <p class="text-red-600 my-font text-[14px] italic mt-3">
                    Veuillez remplir tout les champ obligation<br> (Email, Nom et prénom, Numéro de telephone)
                </p>
            {/if}

            <button class:mt-1={invalid} on:click={() => verif_and_valid()} class="w-[70%] text-white my-font font-bold px-2 mt-5 rounded-xl py-3 text-xl bg-blue-600 hover:scale-110 transition-all ">
                Valider mon rendez-vous
            </button>

        </div>
    </div>

{/if}

<div class="flex flex-row justify-center h-screen min-h-screen md:h-screen w-screen mt-20 gap-4 mb-20">
    <div class="h-full flex flex-col ml-4 justify-center items-center">
        <svg on:click={() => { date > new Date(today) ? date.setDate(date.getDate() - 1) : 0; fetchRendezvous()}} xmlns="http://www.w3.org/2000/svg" width="33" height="33" fill="white" class="bi bi-caret-left-fill" viewBox="0 0 16 16">
            <path d="m3.86 8.753 5.482 4.796c.646.566 1.658.106 1.658-.753V3.204a1 1 0 0 0-1.659-.753l-5.48 4.796a1 1 0 0 0 0 1.506z"/>
        </svg>
    </div>

    {#each jours as jour}
        <div class="border-2 border-white border-solid rounded-2xl p-4">
            <h2 class="text-2xl font-bold my-font text-white text-center">{formaterDate(jour.date)}</h2>
            {#if jour.creneaux.length > 0}
                <ul>
                    {#each jour.creneaux as creneau, i}
                        {#if i > 0}
                            <div class="w-[90%] h-[2px] bg-white rounded-full"></div>
                        {/if}

                        <li class="{creneau.estReserve ? 'text-white' : 'text-white'} mt-3 mb-3 text-center my-font text-xl">
                            {creneau.heure} - <button on:click={() => {console.log(creneau.estReserve);creneau.estReserve ? 0 : reservation = true; creneau.estReserve ? 0 : date_selected[0] = creneau; creneau.estReserve ? 0 : date_selected[1] = jour}} class="{creneau.estReserve ? 'bg-red-600 cursor-not-allowed' : 'bg-blue-600 hover:scale-110'}  px-2 py-2 rounded-2xl  transition-all"> {creneau.estReserve ? 'Indisponible' : 'Disponible'} </button>
                        </li>


                    {/each}
                </ul>
            {:else}
                <div class="flex flex-col justify-center items-center h-full">
                    <h1 class="my-font text-4xl text-center font-bold text-white">
                        Fermé
                    </h1>
                </div>
            {/if}
        </div>
    {/each}
    <div class="h-full flex flex-col ml-4 justify-center items-center">
        <svg on:click={() => {date.setDate(date.getDate() + 1); fetchRendezvous()}} xmlns="http://www.w3.org/2000/svg" width="33" height="33" fill="white" class="bi bi-caret-right-fill" viewBox="0 0 16 16">
            <path d="m12.14 8.753-5.482 4.796c-.646.566-1.658.106-1.658-.753V3.204a1 1 0 0 1 1.659-.753l5.48 4.796a1 1 0 0 1 0 1.506z"/>
        </svg>
    </div>

</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;700&display=swap');

    .my-font {
        font-family: 'Quicksand', sans-serif;
    }
</style>

