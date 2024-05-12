
<template>
  <input ref="fileInput" type="file" accept="image/*" capture="camera" style="display: none;" @change="handleFileInputChange">

  <div class="chat-container">

    <div class="group-container">
    <div class="chat-header">
      <img class="group-icon" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRvoX2HbQn78YpCfCeyV6oqkp1lQbjQOG2kNn2gKzHbPPTkamA2" alt="group-icon">
      <h2 class="text-titolo">Organizzazione Evento</h2>
     
      </div>
    </div>

   
    <div v-for="(message, index) in messages" :key="index" :class="{'sent-message': message.sender === 'Tu', 'received-message': message.sender !== 'Tu'}">
      
      <div>
        <i class="fas fa-user-circle user icon"></i>
      </div>
      <div class="sender">{{ message.sender }}</div>
      <div class="message-content">
        <!-- Se il messaggio è di tipo immagine, mostra l'immagine -->
    <img v-if="message.type === 'image' && !message.isExpanded" :src="message.text" class="message-image" @click="handleMessageClick(message)" />
    <!-- Se il messaggio è di tipo immagine ed è espanso, mostra l'immagine ingrandita -->
    <img v-if="message.type === 'image' && message.isExpanded" :src="message.text" class="expanded-message-image" @click="handleMessageClick(message)" />
    
    <div v-if="message.type === 'text'" class="message-content">
    <div class="text">{{ message.text }}</div>
    <div class="timestamp">{{ message.timestamp }}</div>
    </div>
      </div>

      <!-- Area di overlay per visualizzare l'immagine ingrandita -->
  <div v-if="message.type === 'image' && message.isExpanded" class="overlay" @click="handleMessageClick(message)">
    <img :src="message.text" class="expanded-image" />
  </div>


</div>

    <div class="chat-input">
      
      <div v-if="showAdditionalOptions" class="additional-options">
        <div @click="openCamera">Fotocamera</div>
        <div @click="sendLocation">Posizione</div>
        <div @click="openImageGallery">
          <i class="fas fa-images"></i> Galleria
        </div>
        
    </div>
    
      <div class="additional-features" @click="toggleAdditionalOptions">
        
          <i class="fas fa-plus"></i>
  
  </div>
      <div class="input-wrapper">
      </div>
      
        <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Scrivi un messaggio...">
        
        
      <button @click="sendMessage">Invia</button>
    </div> 

  </div>
</template>

<script>

export default {



  data() {
    return {
      messages: [
      { sender: 'Giulia', text: 'Ciao a tutti!', timestamp: this.getCurrentTime(), type: 'text', isExpanded: false },
      { sender: 'Tu', text: 'Ciao Giulia!', timestamp: this.getCurrentTime(), type: 'text', isExpanded: false },
      ],
      newMessage: '',
      isRecording: false,
      showAdditionalOptions: false
    };
  },
  methods: {

  
    getCurrentTime() {
      return new Date().toLocaleTimeString();
    },

    sendMessage() {
      if (this.newMessage.trim() !== '') {
        this.messages.push({
          sender: 'Tu',
          text: this.newMessage.trim(),
          timestamp: this.getCurrentTime(),
          type: 'text'
        });
        this.newMessage = '';
      }
    },

    handleFileInputChange(event) {
      const file = event.target.files[0];
      if (file) {
        console.log('Immagine selezionata:', file);

        // Leggi il file come URL dati
        const reader = new FileReader();
        reader.onload = () => {
          // Aggiungi un nuovo messaggio con l'immagine
          this.messages.push({
            sender: 'Tu',
            text: reader.result, // Usa il risultato del caricamento del file come testo
            timestamp: this.getCurrentTime(),
            type: 'image'
          });
        };
        reader.readAsDataURL(file);
      }
    },

    handleMessageClick(message) {
  if (message.type === 'image') {
    // Se il messaggio è un'immagine e non è già espansa, espandilo
    if (!message.isExpanded) {
      message.isExpanded = true;
    } else {
      // Se il messaggio è già espanso, riducilo
      message.isExpanded = false;
    }
  }
},

   

    toggleAdditionalOptions() {
      this.showAdditionalOptions = !this.showAdditionalOptions;
    },

    sendLocation() {
      console.log('Invia posizione');
    },
    
    openImageGallery() {
    // Simula il clic sull'input file per aprire la galleria delle immagini
    this.$refs.fileInput.click();
  },
    
    startRecording() {
      navigator.mediaDevices.getUserMedia({ audio: true })
        .then(stream => {
          // Se il permesso è stato concesso, puoi iniziare la registrazione audio utilizzando lo stream
          console.log('Accesso al microfono consentito');
          // Qui puoi chiamare una funzione per iniziare la registrazione audio
        })
        .catch(error => {
          // Se il permesso è stato negato o se si è verificato un altro errore, verrà eseguita questa funzione di gestione dell'errore
          console.error('Errore durante l\'accesso al microfono:', error);
          // Qui puoi gestire l'errore, ad esempio mostrando un messaggio all'utente
        });
    },
  },
};
</script>

<style scoped>

.chat-container {
  
  position: fixed;
  margin-top: 80px; /* Altezza della barra "Scrivi un messaggio" */
  left: 0;
  right: 0;
  bottom: 0;
  height: 100vh; /* Altezza massima della finestra */
  background-image: url('https://i.pinimg.com/originals/c4/23/12/c4231254ad6f3a92d902a8356212809c.jpg'); 
  background-size: cover; 
  background-position: center;
  overflow-y: auto; /* Per consentire lo scorrimento quando la chat diventa più lunga */
  
}


.group-icon {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin-right: 10px;
}

.text-titolo{
  font-style: italic;
  flex-grow: 1;
  max-width: 100%;
}

.group-container {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #007bff;
  justify-content: flex-start;
  color: white;
}

.group-name {
  margin: 0;
  font-size: 24px;
  font-weight: bold;
}

.group-container img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-right: 10px;
}

.chat-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #007bff;
  margin: 20px auto;
  padding: 10px;
  max-width: 300px;
}


.chat-header img {
  width: 80px; /* Rimpicciolisci l'immagine dell'icona */
  height: 80px; /* Rimpicciolisci l'immagine dell'icona */
  border-radius: 50%; /* Arrotonda i bordi dell'immagine */
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
  font-weight: bold;
}

.input-wrapper {
  display: flex;
  align-items: center;
  
}

.input-wrapper input {
  flex: 1; /* Occupa lo spazio rimanente */
}

.input-wrapper i {
  margin-left: 10 px;
}

.chat-input {
  display: flex;
  width: 100%;
  justify-content: space-between;
  margin-top: 20px;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;

}


.chat-input input {
  
  width: calc(100% - 100px); /* Larghezza dell'input meno quella del pulsante Invia */
  padding: 10px;
  border: 10px solid #007bff;
  border-radius: 5px;
   
}

.chat-input button {
  width: 100px;
  padding: 10px;
  margin-left: 0;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

.location-icon {
  width: 25px;
  height: 25px;
  fill: #0da0ef;
}

.sent-message {
  display: flex;
  justify-content: flex-end;
}

.received-message {
  display: flex;
}

.message-content {
  max-width: 80%;
  margin: 10px;
  padding: 0px;
  border-radius: 10px;
  background-color: #f0f0f0;
  margin-bottom: 10px;
  position: relative;
}

.message-content img {
  display: block; /* Imposta l'immagine come elemento block */
  width: 10%; /* Imposta la larghezza dell'immagine al 100% del contenitore */
  max-width: 100%; /* Assicura che l'immagine non superi mai la larghezza massima consentita */
  border-radius: 5px; /* Arrotonda i bordi dell'immagine */
  cursor: pointer; /* Cambia il cursore quando si passa sopra l'immagine */
  
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
  font-size: 50px;
  color: #007bff;
  background-color: #24517e;
  border: 2px solid #007bff;
  border-radius: 50%;
  padding: 10px;
}

.additional-features {
  background-color: #007bff;
  color: #fff;
  width: 30px;
  height: 58px;
  padding: 20px;
  border-radius: 0px;;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  
  
}

.additional-features i{
  font-size: 20px; /* Imposta la dimensione dell'icona */
}

.additional-options {
  position: absolute;
  top: calc(100% + 10px);
  left: 0;
  right: 0;
  bottom: 80px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  padding: 10px;
  display: flex;
  flex-direction: column;
}

.additional-options > div {
  padding: 10px;
  cursor: pointer;
}

.additional-options > div:hover {
  background-color: #f0f0f0;
}


.expanded-message-image {
  max-width: 100%;
  max-height: 100%;
  cursor: pointer;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7); /* Aggiungi un'opacità per l'overlay */
  display: flex;
  justify-content: center;
  align-items: center;
}

.overlay img {
  max-width: 90%; /* Limita la larghezza dell'immagine all'interno dell'overlay */
  max-height: 90vh; /* Limita l'altezza dell'immagine all'interno dell'overlay */
}

</style>

