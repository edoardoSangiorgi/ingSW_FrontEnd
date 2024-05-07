<template>
  <div class="chat-container">
    <div class="chat-header">
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRvoX2HbQn78YpCfCeyV6oqkp1lQbjQOG2kNn2gKzHbPPTkamA2" alt="Icona Gruppo">
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
        <div class="additional-features" @click="toggleAdditionalOptions">
          <i class="fas fa-plus"></i>
        </div>
        <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Scrivi un messaggio...">
      </div>
      <button @click="sendMessage">Invia</button>
    </div>

    <div v-if="showAdditionalOptions" class="additional-options">
      <div @click="openCamera">Fotocamera</div>
      <div @click="sendLocation">Posizione</div>
      <div @click="sendFile">Immagine</div>
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
      console.log('Apri fotocamera');
    },
    sendFile() {
      console.log('Invia file');
    }
  },
};
</script>

<style scoped>

.chat-container {
  width: 200%;
  max-width: 1200px;
  height: 90vh;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 0px;
  background-image: url('https://i.pinimg.com/originals/c4/23/12/c4231254ad6f3a92d902a8356212809c.jpg'); 
  background-size: cover; 
  background-position: center; 
}

.text-titolo{
  font-style: italic;
}

.chat-header {
  text-align: center;
  margin-bottom: 60px;
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
  font-weight: bold;
}

.input-wrapper {
  display: flex;
  align-items: center;
}

.chat-input {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
  position: relative;
}

.chat-input input {
  flex: 1;
  min-width: 0;
  padding: 10px;
  border: 10px solid #0da0ef;
  border-radius: 0 10px 10px 0;
}

.chat-input button {
  padding: 20px 30px;
  background-color: #00e4fd;
  color: #1b18e2;
  border: none;
  border-radius: 5px;
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
  width: 40px;
  height: 40px;
  padding: 10px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.additional-options {
  position: fixed;
  bottom: 80px;
  right: 20px;
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
