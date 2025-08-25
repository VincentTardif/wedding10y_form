<script>
import toast, {Toaster} from 'svelte-french-toast'

let lastname= '';
let isPresent = false; // État de la case à cocher
let isAbsent = false; // État de la case à cocher
let adultNb= '';
let childNb='';
let saturday12= false;
let saturday19= false;
let sundayYes= false;
let sundayNo= false;
let accommodation = false; // État de la case à cocher
let noAccommodation = false; // État de la case à cocher
let message='';

// Fonction pour envoyer les données
async function handleSubmit(event) {
  event.preventDefault();
  try {
    const response = await fetch(import.meta.env.VITE_API_BASE_URL + "items/wedding", {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        lastname,
        isPresent,
        isAbsent,
        adultNb,
        childNb,
        accommodation,
        noAccommodation,
        message
      }),
    });
    if (response.ok) {
        toast('Réponse envoyée. A bientôt.\n\nStéphanie et Vincent',
        {
            style: 'background-color: white; text-align: center; font-weight: 700; color: #ab9861',
            position: "top-center",
            duration: 10000,   // Durée en millisecondes (10 secondes)
                }
        )
        // Réinitialiser les champs du formulaire
        lastname = '';    
        adultNb = '';
        childNb = '';
        isPresent = false;
        isAbsent = false;
        accommodation = false;
        noAccommodation = false;
        message = '';

        // Rediriger en haut de la page
        window.scrollTo(0, 0);
    } else {
            console.error('Erreur lors de l\'envoi');
            toast.error("Erreur !!! Contacte-nous directement. Merci.", {
                position: "top-center",
                duration: 10000,  // Durée en millisecondes (10 secondes)            
            });
    }
  } catch (error) {
    console.error('Erreur API:', error);
    toast.error("Une erreur est survenue", { // <-- Correction de la syntaxe de `toast.error`
      position: "top-center",
      duration: 10000,
    });
  }
}
// Fonction pour gérer le changement d'état de la case à cocher Présent
function handleIsPresentChange() {
  if (isPresent) {
    isAbsent = false;
  }
}
// Fonction pour gérer le changement d'état de la case à cocher Absent
function handleIsAbsentChange() {
  if (isAbsent) {
    isPresent = false;
  }
}
// Fonctions pour gérer le changement d'état pour l'hébergement
function handleAccommodationChange() {
  if (accommodation) {
    noAccommodation = false;
  }
} 
function handleNoAccommodationChange() {
  if (noAccommodation) {
    accommodation = false;
  }
}
// Fonctions pour gérer le changement d'état pour le samedi (12h ou 19h)
function handleSaturday12Change() {
  if (saturday12) {
    saturday19 = false;
  }
}  
function handleSaturday19Change() {
  if (saturday19) {
    saturday12= false;
  }
}  
// Fonctions pour gérer le changement d'état pour le dimanche
function handleSundayYesChange() {
  if (sundayYes) {
    sundayNo = false;
  }
}  
function handleSundayNoChange() {
  if (sundayNo) {
    sundayYes = false;
  }
}  
</script>

<Toaster />
<main>
    <section class="form-header">   
    <h2>
        Alors ? Tu seras là ?
    </h2>
    </section>
    <form  on:submit={handleSubmit}>
        <div class="lastname">
            <label for="lastname">Nom</label>
            <input type="text"
            id="lastname"
            name="lastname"
            bind:value={lastname}
            required
            />
        </div>
        <section class="presence">
            <div class="answer">
                <label for="present">Présent(es)</label>
                <input type="checkbox"
                id="present"
                name="present"
                bind:checked={isPresent}
                on:change={handleIsPresentChange}

                />
            </div>
                <div class="answer">
                <label for="absent">Absent(es)</label>
                <input type="checkbox"
                id="absent"
                name="absent"
                bind:checked={isAbsent}
                on:change={handleIsAbsentChange}
                />
            </div>
        </section>
        {#if isPresent} 
    <section class="participants-container">
        <div class="participants">
            <label for="adult">Adultes</label>
            <input type="tel"
            id="adult"
            name="adult"
            bind:value={adultNb}
            />
        </div>
        <div class="participants">
            <label for="child">Enfants</label>
            <input type="tel"
            id="child"
            name="child"
            bind:value={childNb}
            />
        </div>
    </section>
    <section class="saturday">
        <p>
            A partir de quand ?
        </p>
        <div class="saturday-answer">
            <div class="answer">
            <label for="saturday12">Le samedi midi</label>
                <input type="checkbox"
                id="saturday12"
                name="saturday12"
                bind:checked={saturday12}
                on:change={handleSaturday12Change}
            />
            </div>
            <div class="answer">
                <label for="saturday19">Le samedi soir</label>
                <input type="checkbox"
                id="saturday19"
                name="saturday19"
                bind:checked={saturday19}
                on:change={handleSaturday19Change}
            />
            </div>
        </div>

    </section>
    <section class="sunday">
        <p>
            Et le dimanche ?
        </p>
        <div class="sunday-answer">
            <div class="answer">
            <label for="sundayYes">Oui</label>
                <input type="checkbox"
                id="sundayYes"
                name="sundayYes"
                bind:checked={sundayYes}
                on:change={handleSundayYesChange}
            />
            </div>
            <div class="answer">
                <label for="sundayNo">Non</label>
                <input type="checkbox"
                id="sundayNo"
                name="sundayNo"
                bind:checked={sundayNo}
                on:change={handleSundayNoChange}
            />
            </div>
        </div>

    </section>    
    <section class="accommodation">
        <p>
            Hébergement en dortoir
        </p>
        <div class="accommodation-answer">
            <div class="answer">
            <label for="accommodation">Oui</label>
                <input type="checkbox"
                id="accommodation"
                name="accommodation"
                bind:checked={accommodation}
                on:change={handleAccommodationChange}
            />
            </div>
            <div class="answer">
                <label for="noAccommodation">Non</label>
                <input type="checkbox"
                id="noAccommodation"
                name="noAccommodation"
                bind:checked={noAccommodation}
                on:change={handleNoAccommodationChange}
            />
            </div>
        </div>
    </section>
        {/if}
        <div class="message">
            <textarea
            id="lastname"
            name="lastname"
            bind:value={message}
            rows="10"
            cols="31"
            placeholder="Une remarque ? Une question ? Dis nous tout (ex : Je ne viendrai pas parce que Kevin sera là. Est-ce qu'il fera beau ? Qu'est-ce qu'on mangera ? Est-ce que je peux venir avec mon chat ?...)"
            >
            </textarea>
        </div>
        <div class="form-validation">    
        <button class="submit" type="submit">Envoyer ma réponse</button>
        <button class="reset" type="reset">Annuler</button>
        </div>            
    </form>
</main>
<style>
    main {
        width: 300px;
        margin: 25px auto;
    }
    .form-header {
        margin-top: 20px;
    }
    h2 {
        margin: 0;
        background-color: white;
        color: var(--gold);
        font-style: italic;
    }
    form {
        margin-top: 25px;
        border: 2px solid white;
        padding: 10px 0px;
    }
    label {
        font-weight: 700;
    }
    input, textarea {
        border: transparent
    }
    input {
        height: 25px;
        text-align: center;
    }
    p {
        margin: 0;
        font-weight: 700;
    }    
    textarea {
        padding: 10px;
    }    
    .lastname, .participants {
        display: flex;
        flex-direction: column;
    }
    .lastname {
        margin: 0 auto;
        width: 246px;
    }
    .presence {
        margin-top: 15px;
    }
    .saturday {
        margin-top: 20px;
    }
    .saturday-answer, .sunday-answer  {
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .sunday {
        margin-top: 10px;
    }
    .saturday label, .sunday label {
        font-weight: 400;
    }
    .participants-container {
        display: flex;
        flex-direction: row;
        justify-content: center;
        margin-top: 10px;
    }
    .participants {
        margin: 0 10px;
        width: 100px;
    }
    .accommodation {
        margin-top: 15px;
    }
    .accommodation-answer, .presence {
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .answer {
        display: flex;
        flex-direction: row;
        justify-content: center;
        line-height: 1.75;
        margin: 0 10px;
    }
    .message{
        margin-top: 10px;
        margin-bottom: 15px;
    }
    .form-validation {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 15px;
        margin-bottom: 6px;
    }
    button {
        width: 200px;
        font-weight: 700;
        color: var(--gold);
    }
</style>