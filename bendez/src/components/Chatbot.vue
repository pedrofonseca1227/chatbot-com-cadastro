<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const chatbox = ref(null);
const chatInput = ref(null);
const sendChatBtn = ref(null);
const closeBtn = ref(null);
const chatbotToggler = ref(null);

const userMessage = ref('');
const API_KEY = 'sk-proj-CByS2T4lqx6Dkre7wAyqT3BlbkFJj2xkcrh24fPWdsemsE5a';
const inputInitHeight = ref(0);

const createChatLi = (message, className) => {
  const chatLi = document.createElement('li');
  chatLi.classList.add('chat', `${className}`);
  const chatContent = className === 'outgoing' ? '<p></p>' : '<span class="material-symbols-outlined">smart_toy</span><p></p>';
  chatLi.innerHTML = chatContent;
  chatLi.querySelector('p').textContent = message;
  return chatLi;
};

const generateResponse = async (chatElement) => {
  const API_URL = 'https://api.openai.com/v1/chat/completions';
  const messageElement = chatElement.querySelector('p');

  try {
    const response = await axios.post(API_URL, {
      model: 'gpt-3.5-turbo',
      messages: [{ role: 'user', content: userMessage.value }],
    }, {
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${API_KEY}`,
      }
    });

    messageElement.textContent = response.data.choices[0].message.content.trim();
  } catch (error) {
    messageElement.classList.add('error');
    messageElement.textContent = 'Oops! Alguma coisa deu errado. Por favor tente novamente mais tarde.';
  } finally {
    chatbox.value.scrollTo(0, chatbox.value.scrollHeight);
  }
};

const handleChat = () => {
  if (!userMessage.value.trim()) return;

  chatInput.value.value = '';
  chatInput.value.style.height = `${inputInitHeight.value}px`;

  chatbox.value.appendChild(createChatLi(userMessage.value, 'outgoing'));
  chatbox.value.scrollTo(0, chatbox.value.scrollHeight);

  setTimeout(() => {
    const incomingChatLi = createChatLi('Digitando...', 'incoming');
    chatbox.value.appendChild(incomingChatLi);
    chatbox.value.scrollTo(0, chatbox.value.scrollHeight);
    generateResponse(incomingChatLi);
  }, 600);
};

onMounted(() => {
  inputInitHeight.value = chatInput.value.scrollHeight;

  chatInput.value.addEventListener('input', () => {
    chatInput.value.style.height = `${inputInitHeight.value}px`;
    chatInput.value.style.height = `${chatInput.value.scrollHeight}px`;
    userMessage.value = chatInput.value.value;
  });

  chatInput.value.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !e.shiftKey && window.innerWidth > 800) {
      e.preventDefault();
      handleChat();
    }
  });

  sendChatBtn.value.addEventListener('click', handleChat);
  closeBtn.value.addEventListener('click', () => document.body.classList.remove('show-chatbot'));
  chatbotToggler.value.addEventListener('click', () => document.body.classList.toggle('show-chatbot'));
});

onUnmounted(() => {
  chatInput.value.removeEventListener('input', () => {});
  chatInput.value.removeEventListener('keydown', () => {});
  sendChatBtn.value.removeEventListener('click', handleChat);
  closeBtn.value.removeEventListener('click', () => {});
  chatbotToggler.value.removeEventListener('click', () => {});
});
</script>



<template>
  <button ref="chatbotToggler" class="chatbot-toggler">
    <span class="material-symbols-rounded">mode_comment</span>
    <span class="material-symbols-outlined">close</span>
  </button>
  <div class="chatbot">
    <header>
      <div class="header-icon"></div>
      <div class="header-text">
        <h2>Chatbot</h2>
        <h3>Online</h3>
        <a>Atendimento Manual</a>
        <a href="https://fulltime.com.br/contato-Fulltime/" class="material-symbols-outlined">support_agent</a>
      </div>
      <span ref="closeBtn" class="close-btn material-symbols-outlined">close</span>
    </header>
    <ul ref="chatbox" class="chatbox">
      <li class="chat incoming">
        <span class="material-symbols-outlined">smart_toy</span>
        <p>Olá 👋<br>Como posso te ajudar hoje?</p>
      </li>
    </ul>
    <div class="chat-input">
      <textarea ref="chatInput" placeholder="Digite uma mensagem..." spellcheck="false" required></textarea>
      <span ref="sendChatBtn" id="send-btn" class="material-symbols-rounded">send</span>
    </div>
  </div>
</template>


<style scoped>
/* Import Google font - Poppins */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap');

.chatbot-toggler {
  position: fixed;
  bottom: 30px;
  right: 35px;
  outline: none;
  border: none;
  height: 50px;
  width: 50px;
  display: flex;
  cursor: pointer;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  background: #b71414fc;
  transition: all 0.2s ease;
}
header a {
  color: rgb(255, 255, 255);
  padding: 0 2px;
}
header h3 {
  color: #0f0;
}
/* ChatBOt */
body.show-chatbot .chatbot-toggler {
  transform: rotate(90deg);
}
.chatbot-toggler span {
  color: #fff;
  position: absolute;
}
.chatbot-toggler span:last-child,
body.show-chatbot .chatbot-toggler span:first-child {
  opacity: 0;
}
body.show-chatbot .chatbot-toggler span:last-child {
  opacity: 1;
}
.chatbot {
  font-family: "Roboto", sans-serif;
  position: fixed;
  right: 35px;
  bottom: 90px;
  width: 350px;
  height: 550px;
  background: #fff;
  border-radius: 15px;
  overflow: hidden;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.5);
  transform-origin: bottom right;
  box-shadow: 0 0 128px 0 rgba(0, 0, 0, 0.1),
    0 32px 64px -48px rgba(0, 0, 0, 0.5);
  transition: all 0.1s ease;
}
body.show-chatbot .chatbot {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1);
}
.chatbot header {
  padding: 1px 0;
  height: 140px;
  position: relative;
  text-align: center;
  color: #fff;
  background: #b71414fc;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
header h2 {
  font-size: 1.4rem;
}
.chatbot .chatbox {
  overflow-y: auto;
  height: 510px;
  padding: 30px 20px 100px;
}
.chatbot :where(.chatbox, textarea)::-webkit-scrollbar {
  width: 6px;
}
.chatbot :where(.chatbox, textarea)::-webkit-scrollbar-track {
  background: #fff;
  border-radius: 25px;
}
.chatbot :where(.chatbox, textarea)::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 25px;
}
.chatbox .chat {
  display: flex;
  list-style: none;
}
.chatbox .outgoing {
  margin: 20px 0;
  justify-content: flex-end;
}
.chatbox .incoming span {
  width: 32px;
  height: 32px;
  color: #fff;
  cursor: default;
  text-align: center;
  line-height: 32px;
  align-self: flex-end;
  background: #b71414fc;
  border-radius: 4px;
  margin: 0 10px 7px 0;
}
.chatbox .chat p {
  white-space: pre-wrap;
  padding: 12px 16px;
  border-radius: 10px 10px 0 10px;
  max-width: 75%;
  color: #fff;
  font-size: 0.95rem;
  background: #b71414fc;
}
.chatbox .incoming p {
  border-radius: 10px 10px 10px 0;
}
.chatbox .chat p.error {
  color: #721c24;
  background: #f8d7da;
}
.chatbox .incoming p {
  color: #000;
  background: #f2f2f2;
}
.chatbot .chat-input {
  display: flex;
  gap: 5px;
  position: absolute;
  bottom: 0;
  width: 100%;
  background: #fff;
  padding: 3px 20px;
  border-top: 1px solid #ddd;
}
.chat-input textarea {
  height: 25px;
  width: 80%;
  border: none;
  outline: none;
  resize: none;
  max-height: 180px;
  padding: 15px 15px 15px 0;
  font-size: 0.95rem;
}
.chat-input span {
  align-self: flex-end;
  color: #b71414fc;
  cursor: pointer;
  height: 55px;
  display: flex;
  align-items: center;
  font-size: 1.50rem;
}
.close-btn {
  color: #ffff;
  cursor: pointer;
  visibility: hidden;
}

@media (max-width: 490px) {
  .chatbot-toggler {
    right: 20px;
    bottom: 20px;
  }
  .chatbot {
    right: 0;
    bottom: 0;
    height: 100%;
    border-radius: 0;
    width: 100%;
  }
  .chatbot .chatbox {
    height: 90%;
    padding: 25px 15px 100px;
  }
  .chatbot .chat-input {
    padding: 5px 15px;
  }
  .chatbot header span {
    display: block;
  }
}
</style>
