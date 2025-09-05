<script>
import toast, { Toaster } from 'svelte-french-toast'

let lastname = '';
let presence = ''; // "present" ou "absent"
let adultNb = '0';
let childNb = '0';
let saturday = ''; // "12" ou "19"
let sunday = '';   // "yes" ou "no"
let accommodation = ''; // "yes" ou "no"
let message = '';

async function handleSubmit(event) {
  event.preventDefault();
  try {
    const response = await fetch(import.meta.env.VITE_API_BASE_URL + "items/wedding", {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        lastname,
        isPresent: presence === "present",
        isAbsent: presence === "absent",
        adultNb,
        childNb,
        saturday12: saturday === "12",
        saturday19: saturday === "19",
        sundayYes: sunday === "yes",
        sundayNo: sunday === "no",
        accommodation: accommodation === "yes",
        noAccommodation: accommodation === "no",
        message
      }),
    });

    if (response.ok) {
      toast('Réponse envoyée. A bientôt.\n\nSteph et Vincent', {
        style: 'background-color: white; text-align: center; font-weight: 700; color: #ab9861',
        position: "top-center",
        duration: 10000,
      });

      lastname = '';
      presence = '';
      adultNb = '0';
      childNb = '0';
      saturday = '';
      sunday = '';
      accommodation = '';
      message = '';

      window.scrollTo(0, 0);
    } else {
      toast.error("Erreur !!! Contacte-nous directement. Merci.", {
        position: "top-center",
        duration: 10000,
      });
    }
  } catch (error) {
    toast.error("Une erreur est survenue", {
      position: "top-center",
      duration: 10000,
    });
  }
}
</script>

<Toaster />
<main>
  <section class="form-header">   
    <h2>Alors ? Tu seras là ?</h2>
  </section>

  <form on:submit={handleSubmit}>
    <div class="lastname">
      <label for="lastname">Nom</label>
      <input type="text" id="lastname" bind:value={lastname} required />
    </div>

    <!-- Présence -->
    <fieldset>
      <label class="radio">Présent(e.s)
        <input type="radio" name="presence" value="present" bind:group={presence} />
        <span class="custom-radio"></span>
      </label>
      <label class="radio">Absent(e.s)
        <input type="radio" name="presence" value="absent" bind:group={presence} />
        <span class="custom-radio"></span>
      </label>
    </fieldset>

    {#if presence === "present"}
      <!-- Nombre de participants -->
      <section class="participants-container">
        <div class="participants">
          <label for="adult">Adultes</label>
          <input type="tel" id="adult" bind:value={adultNb} />
        </div>
        <div class="participants">
          <label for="child">Enfants</label>
          <input type="tel" id="child" bind:value={childNb} />
        </div>
      </section>

      <!-- Samedi -->
      <section class="saturday">
        <p>A partir de quand ?</p>
        <div class="saturday-answer">
          <label class="radio">Samedi midi
            <input type="radio" name="saturday" value="12" bind:group={saturday} />
            <span class="custom-radio"></span>
          </label>
          <label class="radio">Samedi soir
            <input type="radio" name="saturday" value="19" bind:group={saturday} />
            <span class="custom-radio"></span>
          </label>
        </div>
      </section>

      <!-- Dimanche -->
      <section class="sunday">
        <p>Et le dimanche midi ?</p>
        <div class="sunday-answer">
          <label class="radio">Oui
            <input type="radio" name="sunday" value="yes" bind:group={sunday} />
            <span class="custom-radio"></span>
          </label>
          <label class="radio">Non
            <input type="radio" name="sunday" value="no" bind:group={sunday} />
            <span class="custom-radio"></span>
          </label>
        </div>
      </section>

      <!-- Hébergement -->
      <section class="accommodation">
        <p>Hébergement en dortoir ?</p>
        <div class="accommodation-answer">
          <label class="radio">Oui
            <input type="radio" name="accommodation" value="yes" bind:group={accommodation} />
            <span class="custom-radio"></span>
          </label>
          <label class="radio">Non
            <input type="radio" name="accommodation" value="no" bind:group={accommodation} />
            <span class="custom-radio"></span>
          </label>
        </div>
      </section>
    {/if}

    <!-- Message -->
    <div class="message">
      <textarea
        id="message"
        bind:value={message}
        rows="10"
        cols="31"
        placeholder="Une remarque ? Une question ? Dis nous tout (ex : Je ne viendrai pas parce que Kevin sera là. Est-ce qu'il fera beau ? Qu'est-ce qu'on mangera ? Est-ce que je peux venir avec mon chat ?...)"
      ></textarea>
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
fieldset {
  display: flex;
  justify-content: space-around;
  margin-top: 12px;
  border: none;
  padding: 0;
}
fieldset label, .participants label {
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
.saturday p, .sunday p, .accommodation p {
  background-color: black;
  color: var(--gold);
  margin: 0 35px;
  text-align: center;
  font-weight: 700;
  /* line-height: 1.5; */
}
.saturday, .sunday, .accommodation {
  margin-top: 15px;
}
.saturday-answer, .sunday-answer, .accommodation-answer {
  display: flex;
  justify-content: center;
  margin-top: 5px;
  gap: 15px;
}
.participants-container {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}
.participants {
  margin: 0 10px;
  width: 100px;
}
.message {
  margin: 10px 0 15px;
}
.form-validation {
  display: flex;
  flex-direction: column;
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

/* === Radios custom avec rond à droite === */
label.radio {
  position: relative;
  padding-right: 25px; /* espace pour le cercle */
  cursor: pointer;
  /* font-weight: 700; */
}

label.radio input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

label.radio .custom-radio {
  position: absolute;
  right: 0;
  top: 4px;
  width: 16px;
  height: 16px;
  border: 1px solid black;
  border-radius: 50%;
  transition: background 0.2s, border 0.2s;
}

label.radio input:checked + .custom-radio {
  background: black;
  border-color: black;
}

label.radio input:checked + .custom-radio::after {
  content: "";
  position: absolute;
  left: 5px;
  top: 5px;
  width: 6px;
  height: 6px;
  background: var(--gold);
  border-radius: 50%;
}
</style>
