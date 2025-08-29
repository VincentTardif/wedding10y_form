<script>
import toast, {Toaster} from 'svelte-french-toast'

let lastname= '';
let isPresent = false; // État de la case à cocher
let isAbsent = false; // État de la case à cocher
let adultNb= '0';
let childNb='0';
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
        isPresent: isPresent ? "oui" : "non",
        isAbsent: isAbsent ? "oui" : "non",
        adultNb,
        childNb,
        saturday12 : saturday12 ? "oui" : "non",
        saturday19 : saturday19 ? "oui" : "non",
        sundayYes : sundayYes ? "oui" : "non",
        sundayNo : sundayNo ? "oui" : "non",
        accommodation : accommodation ? "oui" : "non",
        noAccommodation : noAccommodation ? "oui" : "non",
        message
      }),
    });
    if (response.ok) {
        toast('Réponse envoyée. A bientôt.\n\nSteph et Vincent',
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
        saturday12 = false;
        saturday19 = false;
        sundayYes = false;
        sundayNo = false;        
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
                <label for="present" class="checkbox">
                    Présent(es)
                    <input type="checkbox"
                    id="present"
                    name="present"
                    class="checkbox"
                    bind:checked={isPresent}
                    on:change={handleIsPresentChange}
                    />
                    <span class="checkmark"></span>
                </label>
            </div>
            <div class="answer">
                <label for="absent" class="checkbox">
                    Absent(es)
                    <input type="checkbox"
                    id="absent"
                    name="absent"
                    class="checkbox"
                    bind:checked={isAbsent}
                    on:change={handleIsAbsentChange}
                    />
                    <span class="checkmark"></span>
                </label>
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
                <label for="saturday12" class="checkbox">
                    Samedi midi
                    <input type="checkbox"
                    id="saturday12"
                    name="saturday12"
                    bind:checked={saturday12}
                    on:change={handleSaturday12Change}
                    />
                    <span class="checkmark"></span>
                </label>
            </div>
            <div class="answer">
                <label for="saturday19" class="checkbox">Samedi soir
                    <input type="checkbox"
                    id="saturday19"
                    name="saturday19"
                    bind:checked={saturday19}
                    on:change={handleSaturday19Change}
                    />
                    <span class="checkmark"></span>
                </label>
            </div>
        </div>

            </section>
            <section class="sunday">
        <p>
            Et le dimanche midi ?
        </p>
        <div class="sunday-answer">
            <div class="answer">
            <label for="sundayYes" class="checkbox">
                Oui
                <input type="checkbox"
                id="sundayYes"
                name="sundayYes"
                bind:checked={sundayYes}
                on:change={handleSundayYesChange}
                />
                <span class="checkmark"></span>

            </label>
            </div>
            <div class="answer">
                <label for="sundayNo" class="checkbox">
                    Non
                    <input type="checkbox"
                    id="sundayNo"
                    name="sundayNo"
                    bind:checked={sundayNo}
                    on:change={handleSundayNoChange}
                    />
                    <span class="checkmark"></span>
                </label>
            </div>
        </div>

            </section>    
            <section class="accommodation">
        <p>
            Hébergement en dortoir ?
        </p>
        <div class="accommodation-answer">
            <div class="answer">
            <label for="accommodation" class="checkbox">
                Oui
                <input type="checkbox"
                id="accommodation"
                name="accommodation"
                bind:checked={accommodation}
                on:change={handleAccommodationChange}
                />
                <span class="checkmark"></span>
            </label>
            </div>
            <div class="answer">
                <label for="noAccommodation" class="checkbox">
                    Non
                    <input type="checkbox"
                    id="noAccommodation"
                    name="noAccommodation"
                    bind:checked={noAccommodation}
                    on:change={handleNoAccommodationChange}
                    />
                    <span class="checkmark"></span>
                </label>
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
        margin: 12px auto;
    }
    .form-header {
        margin-top: 12px;
    }
    h2 {
        margin: 0;
        background-color: black;
        color: var(--gold);
        font-style: italic;
    }
    form {
        margin-top: 20px;
        border: 2px solid black;
        padding: 10px 0px;
    }
    label {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        font-weight: 700;
    }
    input, textarea {
        border: transparent;
        background-color: #c2b891;
    }
    input {
        height: 25px;
        text-align: center;
    }
    /* Cache la case à cocher native */
    .checkbox input{
        position: absolute;
        opacity: 0;
        cursor: pointer;
    }    
    /* Aspect de la case personnalisée */
    .checkmark {
        width: 15px;
        height: 15px;
        border: 2px solid black;
        border-radius: 4px; /* carré arrondi */
        margin-right: 8px;
        display: inline-block;
        position: relative;
        transition: background 0.2s, border 0.2s;
        margin-left: 5px;
    }
    /* Effet quand la case est cochée */
    .checkbox input:checked + .checkmark {
        background: black;
        border-color: black;
    }
    /* Ajout d’une "coche" */
    .checkbox input:checked + .checkmark::after {
        content: "";
        position: absolute;
        left: 3px;
        top: -2px;
        width: 6px;
        height: 12px;
        border: solid var(--gold);
        border-width: 0 3px 3px 0;
        transform: rotate(45deg);
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
    .saturday p, .sunday p, .accommodation p {
        background-color: black;
        color: var(--gold);
        margin: 0 35px;
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
    .saturday label, .sunday label, .accommodation label {
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
        margin-top: 5px;
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
        margin-top: 7px;
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
        background-color: black;
    }
</style>