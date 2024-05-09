<template>
  <input ref="fileInput" type="file" accept="image/*" capture="camera" style="display: none;" @change="handleFileInputChange">

  <div class="chat-container">
    <div class="chat-header">
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRvoX2HbQn78YpCfCeyV6oqkp1lQbjQOG2kNn2gKzHbPPTkamA2" alt="group-icon">
      <h2 class="text-titolo">Organizzazione Evento</h2>
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
      <div class="input-wrapper">
      </div>
        <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Scrivi un messaggio...">
    
        <div class="additional-features" @click="toggleAdditionalOptions">
          <i class="fas fa-plus"></i>
          <div v-if="showAdditionalOptions" class="additional-options">
      <div @click="openCamera">Fotocamera</div>
      <div @click="sendLocation">Posizione</div>
      <div @click="sendFile">Immagine</div>
    </div>
  </div>
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
      ],
      newMessage: '',
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
    toggleAdditionalOptions() {
      this.showAdditionalOptions = !this.showAdditionalOptions;
    },
    sendLocation() {
      console.log('Invia posizione');
    },
    openCamera() {
     
      this.$refs.fileInput.click();
    },
    handleFileInputChange(event) {
  const file = event.target.files[0];
  if (file) {
    // Esegui qui la logica per gestire il file (ad esempio, mostrare l'immagine nell'interfaccia utente)
    console.log('Immagine selezionata:', file);
  }
},

    sendFile() {
      console.log('Invia file');
    }
    
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

.text-titolo{
  font-style: italic;
  flex-grow: 1;
  max-width: 100%;
}

.chat-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
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

.chat-input {
  display: flex;
  width: 100%;
  justify-content: space-between;
  margin-top: 20px;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 20px;
}


.chat-input input {
  
  width: calc(100% - 100px); /* Larghezza dell'input meno quella del pulsante Invia */
  padding: 10px;
  border: 10px solid #007bff;
  border-radius: 5px;
   
}

.chat-input button {
  width: 80px;
  padding: 10px;
  margin-left: 10px;
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
  padding: 10px;
  border-radius: 10px;
  background-color: #f0f0f0;
  margin-bottom: 35px;
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
  height: 55px;
  padding: 10px;
  border-radius: 5px;;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  margin-right: 10px;
  
}

.additional-features i{
  font-size: 20px; /* Imposta la dimensione dell'icona */
}

.additional-options {
  position: absolute;
  top:100%;
  left: 0;
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
</style>
