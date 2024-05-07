
<template>

  <div class="chat-container">
    
    <div class="chat-header">
      <img src="https://media.istockphoto.com/id/512895568/it/vettoriale/andare-fuori-con-gli-amici.jpg?s=170667a&w=0&k=20&c=LFvXYJ_OUmp6EnkKF_N1rhj2uOMOTwXD_YT7ux6vO00" alt="Icona Gruppo">
      <h2 class="text-titolo"> Organizzazione Evento</h2>
      <p>Apertura pub 2024</p>
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
      
      <button @click="sendLocation">
      <!-- Includi l'icona SVG inline come pulsante per la posizione -->
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="location-icon">
        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zM9.17 15H7l4-8h2.17l-4 8zM12 4c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm6.83 11h-2.17l-4-8h2.17l4 8z"/>
        <path d="M0 0h24v24H0z" fill="none"/>
      </svg>
    </button>
    <button @click="sendMessage">Invia</button>
    </div>
  </div>
</template>

  
  <script>


  export default {
    data() {
      return {
        messages: [
        { sender: 'Giulia', text: 'Ciao a tutti!', timestamp: this.getCurrentTime(), type: 'text' },
        { sender: 'Tu', text: 'Ciao Giulia!', timestamp: this.getCurrentTime(), type: 'text' },
        
        ], // Array di messaggi
        newMessage: '', // Nuovo messaggio in input
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
      // Implementa la logica per inviare la posizione
      console.log('Invia posizione');
      }
    },
  };
  </script>
  
  <style scoped> 

  .chat-container {
  
  width: auto;
  margin: 0 auto;
  padding:0px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-image: url('https://i.pinimg.com/originals/c4/23/12/c4231254ad6f3a92d902a8356212809c.jpg'); 
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
    background-color: #00e4fd; 
    color: #1b18e2; /* colore invia */
    border: none;
    border-radius: 0 10px 10px 0;
    
    cursor: pointer;
  }

  .location-icon {
  width: 25px;
  height: 25px;
  fill: #0da0ef; /* Colore dell'icona */
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
  background-color: #007bff;
  color: white;
}

.text {
  font-size: 16px;
}

.timestamp {
  font-size: 12px;
  color: #888;
}

.user-icon {
  font-size: 50px; /* Imposta la dimensione dell'icona */
  color: #007bff; /* Cambia il colore dell'icona */
  background-color: #24517e; /* Cambia il colore dello sfondo del cerchio */
  border: 2px solid #007bff; /* Aggiunge un bordo al cerchio */
  border-radius: 50%; /* Rende il contenuto del cerchio */
  padding: 10px; /* Aggiunge spazio intorno all'icona */
}
 


</style>  
