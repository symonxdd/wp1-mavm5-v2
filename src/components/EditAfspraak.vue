<template>
<div id="container">
    <table v-if="!isHidden" id="afsprakenTabel">
        <tr>
            <th>Naam patiënt</th>
            <th>Datum</th>
            <th>Tijdstip</th>
            <th>Contact</th>
            <th></th>
            <th></th>
        </tr>
        <tr v-for="(afspraak, index) in afspraken" v-bind:key="afspraak.id" :id="index">
            <td>{{afspraak.naam_patient}}</td>
            <td>
                <input type="date" class="datum" v-model="afspraak.datum" id="date"/>
            </td>
            <td>
                <input type="time" min="1" v-model="afspraak.tijdstip" id="tijdstip"/>
            </td>
            <td>
                <p v-if="afspraak.contact_met_coronavirus == 1">Ja</p>
                <p v-else>Nee</p>
            </td>
            <td>
                <button
                id="saveButton"
                v-on:click="updateAfspraak(
                    afspraak.id,
                    afspraak.naam_patient,
                    afspraak.datum,
                    afspraak.tijdstip,
                    afspraak.contact_met_coronavirus)">
                    Save</button>
            </td>
            <td>
                <button id="deleteButton" v-on:click="verwijderAfspraak(afspraak.id, $event)">Delete</button>
            </td>
        </tr>
    </table>
    <div id="snackbar">Succesvol veranderd</div>
    <div id="snackbar">Succesvol verwijderd</div>
    <div id="snackbar">Gelieve geldige waarden in te geven</div>
    <button class="toggleButton" v-on:click="isHidden = !isHidden">Show / Hide</button>

    <label for="filter">Zoek afspraken op ID</label>
    <input type="number" id="filter" v-on:input="zoekOpId($event)">

    <span>{{ error }}</span>
</div>
</template>

// Data Fetching in vue-router
// source: https://router.vuejs.org/guide/advanced/data-fetching.html#fetching-after-navigation

<script>
import $ from 'jquery'

export default {
    data() {
        return {
            loading: false,
            post: null,
            error: null,
            status: "",
            afspraken: [],
            isHidden: false
        };
    },
    mounted() {
        this.laadAfspraken();
    },
    watch: {
        $route: "laadAfspraken",
    },
    methods: {
        laadAfspraken() {
            const url = "http://localhost:3000/afspraken/";
            this.afspraken = [];
            fetch(url)
                .then((response) => {
                    if (response.status == 200) {
                        this.applyDefaults();
                        return response.json();
                    } else {
                        throw `error with status ${response.status}`;
                    }
                })
                .then((afspraken) => {
                    this.afspraken = afspraken;
                })
                .catch((error) => {
                    this.error = error;
                });
        },

        updateAfspraak: function (id, naam_patient, datum, tijdstip, contact_met_coronavirus) {
            this.status = "";
            this.error = "";
            let url = 'http://localhost:3000/afspraken/';
            let afspraak = {
                naam_patient: naam_patient,
                datum: datum,
                tijdstip: tijdstip,
                contact_met_coronavirus: contact_met_coronavirus};

            if(
                naam_patient.length >= 6 &&
                datum !== "" &&
                datum.substring(0, 4) <= (new Date().getFullYear() + 1) &&
                tijdstip >= "07:00:00" &&
                tijdstip <= "23:00:00" &&
                tijdstip !== "") {

                fetch(url + id,
                {
                    method: "PUT",
                    body: JSON.stringify(afspraak),
                    headers: {
                        'Accept': 'application/json',
                        'Content-Type': 'application/json'
                    }
                })
                .then((response) => {
                    if (response.status == 200 || response.status == 201) {

                        let snackbar = document.querySelectorAll("#snackbar")[0];
                        snackbar.className = "show";
                        setTimeout(function(){ snackbar.className = snackbar.className.replace("show", ""); }, 3000);

                        return response.json();
                    } else {
                        throw `error with status ${response.status}`;
                    }
                })
                .then((afspraak) => {
                    this.status = `updated ${afspraak.id} ${afspraak.naam_patient} ${afspraak.datum} ${afspraak.tijdstip} ${afspraak.contact_met_coronavirus}`;
                })
                .catch((error) => {
                    this.error = error;
                });
            }
            else {
                let snackbar = document.querySelectorAll("#snackbar")[2];
                snackbar.className = "show";
                setTimeout(function(){ snackbar.className = snackbar.className.replace("show", ""); }, 3000);
            }
        },

        verwijderAfspraak: function (id, event) {
            this.status = "";
            this.error = "";
            let url = "http://localhost:3000/afspraken/";
            fetch(url + id, {
                method: "DELETE",
            })
            .then((response) => {
                if (response.status == 200) {
                    // this.$router.go();

                    // ALL **** SOURCE: https://stackoverflow.com/a/44827987
                    // ngl zat uren te zoeken hoe een rij uit het DOM te verbergen,
                    // na een succesvolle DELETE...
                    // #letsgo tho
                    const rows = document.querySelectorAll('tr');
                    let trId = $(event.target).closest('tr').attr('id');
                    rows[parseInt(trId) + 1].style.display = "none";

                    let snackbar = document.querySelectorAll("#snackbar")[1];
                    snackbar.className = "show";
                    setTimeout(function(){ snackbar.className = snackbar.className.replace("show", ""); }, 3000);

                    // this.$router.go();
                    return response.json();
                } else {
                    throw `error with status ${response.status}`;
                }
            })
            .then((afspraak) => {
                this.status = `afspraak met ${afspraak.id} verwijderd`;
            })
            .catch((error) => {
                this.error = error;
            });
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

            const datums = document.querySelectorAll("#date");
            const tijdstippen = document.querySelectorAll("#tijdstip");
            
            datums.forEach(datum => {
                datum.setAttribute("min", date);
                datum.setAttribute("max", max);
            });

            tijdstippen.forEach(tijdstip => {
                tijdstip.setAttribute("min", "07:00:00");
                tijdstip.setAttribute("max", "23:00:00");
            });
        },

        zoekOpId(event) {
            if (event.target.value == "" || event.target.value == 0) {
                this.laadAfspraken();
            }
            else {
                const id = event.target.value;
                const url = `http://localhost:3000/afspraken?patient_id=${id}`;
                this.afspraken = [];
                fetch(url)
                    .then((response) => {
                        if (response.status == 200) {
                            this.applyDefaults();
                            return response.json();
                        } else {
                            throw `error with status ${response.status}`;
                        }
                    })
                    .then((afspraken) => {
                        this.afspraken = afspraken;
                    })
                    .catch((error) => {
                        this.error = error;
                    });
            }
           
        }
    },
    created: function() {
        window.addEventListener('focus', () => {
            // location.reload()
        });
    }
}
</script>

<style scoped>
/* wow: https://stackoverflow.com/a/62162805 */
::-webkit-calendar-picker-indicator {
    filter: invert(1);
}

* {
    font-family: 'Montserrat', sans-serif;
    outline: none;
}

#container {
    margin-top: 78px;
    padding-top: 78px;
}

.toggleButton {
    width: 150px;
    margin-top: 20px;
    padding: 10px;
    color: white;
    background-color: #282C34;
    border: none;
    transition: 0.1s;
    font-size: 12pt;
    box-shadow: 5px 5px 10px -3px rgba(0, 0, 0, 0.301) inset;
    transition: background-color, box-shadow 0.5s;
}

.toggleButton:hover {
    background-color: white;
    color: black;
    background-color: transparent;
    box-shadow: -5px -5px 10px -3px rgba(0, 0, 0, 0.301) inset; 
}

#afsprakenTabel {
    border-collapse: collapse;
    width: 100%;
}

#afsprakenTabel td {
    padding: 7px;
    color: white;
}

#afsprakenTabel td:first-child {
    padding-left: 20px;
}

#afsprakenTabel td {
    padding: 15px;
}

#afsprakenTabel tr:nth-child(even) {
    background-color: #282c34e7;
    transition: 0.2s;
}
#afsprakenTabel tr:nth-child(odd) {
    background-color: #282c34d3;
    transition: 0.2s;
}

#afsprakenTabel tr:nth-child(odd):hover {
    background-color: #282C34;
}

#afsprakenTabel tr:nth-child(even):hover {
    background-color: #282C34;
}

#afsprakenTabel th {
    padding: 12px 7px;
    background-color: #282C34;
    color: white;
    text-align: left;
}

#afsprakenTabel th:first-child {
    padding: 12px 20px;
}

input {
    width: 100%;
    border: 1px solid white;
    padding: 4px;
    background-color: inherit;
    color: white;
}

input#filter {
    width: 50px;
    border: 1px solid #282C34;
    padding: 4px;
    height: 37px;
    background-color: inherit;
    color: #282C34;
}

label {
    margin: 0 20px 0 50px;
}

#saveButton, #deleteButton {
    padding: 6px 10px;
    color: white;
    background-color: #488a6a;
    border: none;
    transition: 0.1s;
    font-size: 12pt;
    border-radius: 10px;
}

#deleteButton {
    background-color: #bd574e;
}

#saveButton:hover, #deleteButton:hover {
    background-color: #ffffff;
    color: #282C34;
}

#saveButton:focus, #deleteButton:focus {
    outline: none;
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