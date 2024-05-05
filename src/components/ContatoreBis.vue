
<template>

  <div class="chat-container">
   
    <div class="chat-header">
      <img src="/Users/giuliapanarello/Desktop/ingegneriadelsoftware/ingSW_FrontEnd/src/components/download.jpg" alt="Icona Gruppo">
      <h2 class="text-titolo"> Organizzazione Evento</h2>
      <p>...</p>
    </div>

    <div v-for="(message, index) in messages" :key="index" :class="{'sent-message': message.sender === 'Tu', 'received-message': message.sender !== 'Tu'}">
      <div>
        <i class="fas fa-user-circle user icon"></i>
      </div>
      <div class="sender">{{ message.sender }}</div>
      <div class="message-content">
        <div class="text">{{ message.text }}</div>
        <div class="timestamp">{{ message.timestamp }}</div>

      </div>
    </div>

    <div class="chat-input">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Scrivi un messaggio...">
      <emoji-mart @emoji="handleEmojiSelect" />
      <div id="map" class="map"></div>
    <button @click="sendLocation">Invia Posizione</button>
    <button @click="sendMessage">Invia</button>
    </div>
  </div>
</template>

  
  <script>
import L from 'leaflet'; // Importa Leaflet.js
import 'leaflet/dist/leaflet.css'; // Importa i file CSS di Leaflet.js


  export default {

 
    data() {
      return {
        messages: [
        { sender: 'Giulia', text: 'Ciao a tutti!', timestamp: this.getCurrentTime(), type: 'text' },
        { sender: 'Tu', text: 'Ciao Giulia!', timestamp: this.getCurrentTime(), type: 'text' },
        
        ], // Array di messaggi
        newMessage: '', // Nuovo messaggio in input
        map: null
      };
    },
    methods: {

      getCurrentTime() {
      return new Date().toLocaleTimeString();
    },
      sendMessage() {
        // Aggiungi il nuovo messaggio all'array dei messaggi
       

        if (this.newMessage.trim() !== '') {
          this.messages.push({
            sender: 'Tu', // Puoi personalizzare il nome dell'utente o recuperarlo dall'autenticazione
            text: this.newMessage.trim(),
            timestamp: this.getCurrentTime(),
            type: 'text'
          });
          // Pulisci l'input del messaggio
          this.newMessage = '';
        }
      },

      sendLocation() {
      // Controlla se il browser supporta la geolocalizzazione
      if (navigator.geolocation) {
        // Ottieni la posizione attuale
        navigator.geolocation.getCurrentPosition(position => {
          const { latitude, longitude } = position.coords;
          // Visualizza la mappa con la posizione attuale
          this.showMap(latitude, longitude);
        }, error => {
          console.error("Errore durante il recupero della posizione:", error);
        });
      } else {
        console.error("Geolocalizzazione non supportata dal browser.");
      }
    },
    showMap(latitude, longitude) {
      // Crea una mappa Leaflet.js e imposta la posizione attuale come centro
      const map = L.map('map').setView([latitude, longitude], 13);
      
      // Aggiungi un layer di mappa OpenStreetMap
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
      
      // Aggiungi un marker sulla mappa per indicare la posizione
      L.marker([latitude, longitude]).addTo(map)
        .bindPopup('La tua posizione attuale.')
        .openPopup();
    }
    }
  }


  </script>
  
  <style scoped> 

  .chat-container {
  
  width: auto;
  margin: 0 auto;
  padding:0px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-image:  url('/Users/giuliapanarello/Desktop/ingegneriadelsoftware/ingSW_FrontEnd/src/components/azzurro.jpg');
  background-size: cover; 
  background-position: center; 
   
}

.text-titolo{
  font-style: italic;
}
  .chat-header {
    text-align: center;
    margin-bottom: 20px;
    background-color: #0da0ef;


  }
  
  .chat-messages {
    max-height: 700px;
    overflow-y: auto;
  }
  
  .message {
    margin-bottom: 10px;
  }

  .sender {
  font-weight: bold;
}
  
  .message-sender {
    font-weight: bold; /* grassetto */
    
  }
  
  .chat-input {
    display: flex;
    margin-top: 250px;
   

  }
  
  .chat-input input {
    flex: 1;
    padding: 20px;
    border: 10px solid #0da0ef;
    border-radius: 10px 0 0 10px;
  }
  
  .chat-input button {  /* invia bottone */
    padding: 20px 40px;
    background-color: #0da0ef; 
    color: #3119e5; /* colore invia */
    border: none;
    border-radius: 0 10px 10px 0;
    
    cursor: pointer;
  }


.sent-message {
  display: flex;
  justify-content: flex-end;
}

.received-message {
  display: flex;
}

.message-content {
  max-width: 70%;
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
  background-color: #f0f0f0;
}

.sent-message .message-content {
  background-color: #017cff;
  color: rgb(246, 249, 249);
}

.text {
  font-size: 16px;
}

.timestamp {
  font-size: 12px;
  color: #080707;
}

.user-icon {
  font-size: 50px; /* Imposta la dimensione dell'icona */
  color: #007bff; /* Cambia il colore dell'icona */
  background-color: #24517e; /* Cambia il colore dello sfondo del cerchio */
  border: 2px solid #6e7e8f; /* Aggiunge un bordo al cerchio */
  border-radius: 50%; /* Rende il contenuto del cerchio */
  padding: 10px; /* Aggiunge spazio intorno all'icona */
}

#map {
  height: 100px; /* Altezza della mappa */
}


</style>
  
  
