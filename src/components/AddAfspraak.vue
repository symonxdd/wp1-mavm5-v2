<template>
    <div> <!-- this div is vital, code breaks otherwise -->
        <form>
            <h2>Ingelogd als Alicia Del Rosario</h2>
            <h3>Maak een afspraak</h3>

            <label for="naam_patient">Naam patiënt:</label>
            <input type="text" id="naam_patient" v-model="naam_patient" minlength="6" required>

            <label for="date">Datum:</label>
            <input type="date" id="date" v-model="datum"/>

            <label for="date">Tijdstip:</label>
            <input type="time" id="tijdstip" v-model="tijdstip"/>

            <span id="tip">Heeft de patiënt ooit contact gehad met het coronavirus?</span>
            <span id="radio">
                <input type="radio" id="nee" name="contact" value="0" checked>
                <label for="nee">Nee</label>
                <input type="radio" id="ja" name="contact" value="1">
                <label for="ja">Ja</label>
            </span>

            <button id="addButton" v-on:click="addAfspraak()">Toevoegen</button>
        </form>
        <div id="snackbar">Succesvol toegevoegd</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            naam_patient: "",
            datum: "",
            tijdstip: ""
        };
    },
    mounted() {
        this.applyDefaults();
    },
    methods: {
        addAfspraak: function () {
            const geenContact = document.getElementById("nee");
            let contact = 0;

            if(geenContact.checked !== true) {
                contact = 1
            }

            let naam_patient = document.getElementById("naam_patient").value;
            let datum = document.getElementById("date").value;
            let tijdstip = document.getElementById("tijdstip").value;

            this.status = "";
            this.error = "";
            let url = "http://localhost:3000/afspraken/";

            let afspraak = {
                naam_patient: naam_patient,
                datum: datum,
                tijdstip: tijdstip,
                contact_met_coronavirus: contact
            };

            if(
                naam_patient.length >= 6 &&
                datum !== "" &&
                datum.substring(0, 4) <= (new Date().getFullYear() + 1) &&
                tijdstip >= "07:00:00" &&
                tijdstip <= "23:00:00" &&
                tijdstip !== "") {

                    fetch(url, {
                        method: "POST",
                        body: JSON.stringify(afspraak),
                        headers: {
                            Accept: "application/json",
                            "Content-Type": "application/json",
                        }
                    })
                    .then((response) => {
                        if (response.status == 201) {
                            
                            let snackbar = document.getElementById("snackbar");
                            snackbar.className = "show";
                            setTimeout(function(){ snackbar.className = snackbar.className.replace("show", ""); }, 3000);

                            return response.json();

                        } else {
                            throw `error with status ${response.status}`;
                        }
                    })
                    .then((afspraak) => {
                        this.status = `added ${afspraak.id} ${afspraak.naam_patient} ${afspraak.datum} ${afspraak.tijdstip} ${afspraak.contact_met_coronavirus}`;
                    })
                    .catch((error) => {
                        this.error = error;
                    });
            }
            else {
                alert("Gelieve geldige waarden in te geven");
            }
        },
        applyDefaults() {
            let date = new Date();
            let dd = date.getDate();
            let mm = date.getMonth() + 1;
            let yyyy = date.getFullYear();

            if(dd < 10) {
                dd = '0' + dd;
            }
            if(mm < 10) {
                mm = '0' + mm;
            }

            date = yyyy + '-' + mm + '-' + dd;
            let max = (yyyy + 1) + '-' + mm + '-' + dd;

            const datum = document.querySelector("input[type='date']");

            datum.setAttribute("value", date);
            datum.setAttribute("min", date);
            datum.setAttribute("max", max);

            const datetime = document.querySelector("input[type='time']");

            const time = new Date();
            const hour = time.getHours();
            const minute = time.getMinutes();
            if(hour <= "18" && hour >= "7") {
                datetime.setAttribute("value", `${hour}:${minute}`);
            }
            else {
                datetime.setAttribute("value", "18:00");
            }
            datetime.setAttribute("min", "07:00:00");
            datetime.setAttribute("max", "18:00:00");
        }
    }
}

</script>
<style scoped>
* {
    color: black;
    outline: none;
    font-family: 'Montserrat', sans-serif;
}

form {
    display: inline-block;
    width: 450px;
    /* margin: 0 auto; */
    margin: 78px 0 0 0;
}

h3 {
    color: white;
    background-color: #393E46;
    padding: 10px;
    margin: 60px 0 15px 0;
}

h2 {
    border-left: 3px solid black;
    padding: 15px 0 15px 15px;
}

label {
    font-size: 14px;
}

input {
    border: 3px solid #3F546D;
    padding: 5px;
    margin: 3px 0 15px 0;
    width: 100%;
}

#tip {
    margin: 15px 0 2px 0;
    display: block;
}

input#nee, input#ja {
    width: initial;
}

button {
    display: block;
    background-color: #393E46;
    color: white;
    padding: 14px 25px;
    border: none;
    margin-top: 20px;
}

label[for=nee], label[for=ja] {
    padding: 5px;
}



/* The snackbar - position it at the bottom and in the middle of the screen */
#snackbar {
  visibility: hidden; /* Hidden by default. Visible on click */
  min-width: 250px; /* Set a default minimum width */
  margin-left: -125px; /* Divide value of min-width by 2 */
  background-color: #19A15F; /* Black background color */
  color: #fff; /* White text color */
  text-align: center; /* Centered text */
  border-radius: 2px; /* Rounded borders */
  padding: 16px; /* Padding */
  position: fixed; /* Sit on top of the screen */
  z-index: 1; /* Add a z-index if needed */
  left: 50%; /* Center the snackbar */
  bottom: 60px; /* 30px from the bottom */
}

/* Show the snackbar when clicking on a button (class added with JavaScript) */
#snackbar.show {
  visibility: visible; /* Show the snackbar */
  /* Add animation: Take 0.5 seconds to fade in and out the snackbar.
  However, delay the fade out process for 2.5 seconds */
  -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
  animation: fadein 0.5s, fadeout 0.5s 2.5s;
}

/* Animations to fade the snackbar in and out */
@-webkit-keyframes fadein {
  from {bottom: 0; opacity: 0;}
  to {bottom: 60px; opacity: 1;}
}

@keyframes fadein {
  from {bottom: 0; opacity: 0;}
  to {bottom: 60px; opacity: 1;}
}

@-webkit-keyframes fadeout {
  from {bottom: 60px; opacity: 1;}
  to {bottom: 0; opacity: 0;}
}

@keyframes fadeout {
  from {bottom: 60px; opacity: 1;}
  to {bottom: 0; opacity: 0;}
}
</style>
