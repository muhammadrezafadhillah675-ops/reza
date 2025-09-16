<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatFreand</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #f0f2f5;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 1000px;
            height: 90vh;
            background-color: #ffffff;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
            border-radius: 8px;
            display: flex;
            overflow: hidden;
        }

        .screen {
            width: 100%;
            display: none;
            flex-direction: column;
        }

        .screen.active {
            display: flex;
        }

        /* --- Otentikasi --- */
        #auth-screen {
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .logo {
            color: #075e54;
            font-size: 2.5em;
            margin-bottom: 20px;
        }

        .auth-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            width: 300px;
        }

        .auth-form input {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 1em;
        }

        .auth-form input.hidden {
            display: none;
        }

        #auth-button {
            padding: 12px;
            background-color: #075e54;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.1em;
        }

        .toggle-auth {
            margin-top: 15px;
            color: #555;
        }

        .toggle-auth a {
            color: #075e54;
            text-decoration: none;
            font-weight: bold;
        }

        .sender-name {
            font-weight: bold;
            font-size: 0.8em;
            color: #555;
            margin-bottom: 5px;
            display: block;
        }

        /* --- Halaman Chat --- */
        #chat-screen {
            display: grid;
            grid-template-rows: auto 1fr;
            height: 100%;
        }

        #chat-screen .header {
            background-color: #075e54;
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        #chat-screen .header h2 {
            margin: 0;
        }

        .main-content {
            display: grid;
            grid-template-columns: 2fr 5fr;
            height: 100%;
        }

        .sidebar {
            background-color: #f0f2f5;
            border-right: 1px solid #ddd;
            overflow-y: auto;
            padding: 10px;
            display: flex;
            flex-direction: column;
        }
        
        .sidebar-header {
            display: flex;
            flex-direction: column;
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
        
        .sidebar h3 {
            margin: 0;
            color: #555;
        }

        .search-container {
            margin-top: 10px;
        }
        
        #search-input {
            width: calc(100% - 20px);
            padding: 8px 10px;
            border: 1px solid #ccc;
            border-radius: 20px;
            font-size: 1em;
        }

        #contact-list {
            list-style: none;
            padding: 0;
            margin: 0;
            flex-grow: 1;
            overflow-y: auto;
        }

        #contact-list li {
            padding: 15px;
            cursor: pointer;
            border-bottom: 1px solid #eee;
        }

        #contact-list li:hover,
        #contact-list li.active {
            background-color: #e9e9e9;
        }

        .add-contact-btn {
            background-color: #25d366;
            color: white;
            border: none;
            padding: 10px;
            text-align: center;
            font-size: 1em;
            cursor: pointer;
            margin: 10px;
            border-radius: 5px;
        }
        
        .add-contact-btn:hover {
            background-color: #128c7e;
        }

        .chat-window {
            display: grid;
            grid-template-rows: auto 1fr auto;
        }

        .chat-header {
            background-color: #eee;
            padding: 15px;
            border-bottom: 1px solid #ddd;
        }

        .message-container {
            padding: 20px;
            overflow-y: auto;
            background-color: #e5ddd5;
        }

        .message {
            padding: 8px 12px;
            border-radius: 10px;
            margin-bottom: 10px;
            max-width: 70%;
            word-wrap: break-word;
        }

        .message.sent {
            background-color: #dcf8c6;
            margin-left: auto;
        }

        .message.received {
            background-color: #ffffff;
            margin-right: auto;
        }

        .message-form {
            display: flex;
            padding: 10px;
            background-color: #f0f2f5;
            border-top: 1px solid #ddd;
        }

        #message-input {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 20px;
            margin-right: 10px;
        }

        .message-form button {
            padding: 10px 20px;
            background-color: #075e54;
            color: white;
            border: none;
            border-radius: 20px;
            cursor: pointer;
        }

        /* --- Modal untuk Tambah Kontak --- */
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            padding: 20px;
            border-radius: 5px;
            width: 300px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .close-button {
            color: #aaa;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <div id="auth-screen" class="screen active">
            <h1 class="logo">ChatFreand</h1>
            <div class="auth-form">
                <input type="text" id="phone-number" placeholder="Nomor WhatsApp">
                <input type="password" id="password" placeholder="Password">
                <input type="text" id="username" placeholder="Nama Pengguna" class="hidden">
                <button id="auth-button">Login</button>
            </div>
            <p class="toggle-auth">Belum punya akun? <a href="#" id="toggle-register">Daftar sekarang</a></p>
        </div>

        <div id="chat-screen" class="screen">
            <div class="header">
                <h2 class="chat-title">Obrolan</h2>
                <button id="logout-button">Keluar</button>
            </div>
            <div class="main-content">
                <div class="sidebar">
                    <div class="sidebar-header">
                        <h3>Kontak</h3>
                        <div class="search-container">
                            <input type="text" id="search-input" placeholder="Cari kontak...">
                        </div>
                    </div>
                    <ul id="contact-list">
                        <li id="public-chat-button">Obrolan Publik</li>
                    </ul>
                    <button class="add-contact-btn" id="add-contact-button">Tambah Kontak</button>
                </div>
                <div class="chat-window">
                    <div class="chat-header">
                        <span id="chat-recipient">Pilih Kontak</span>
                    </div>
                    <div id="message-container" class="message-container">
                        </div>
                    <form id="message-form" class="message-form">
                        <input type="text" id="message-input" placeholder="Ketik pesan...">
                        <button type="submit">Kirim</button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <div id="add-contact-modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Tambah Kontak Baru</h3>
                <span class="close-button">&times;</span>
            </div>
            <input type="text" id="new-contact-number" placeholder="Nomor WhatsApp teman">
            <button id="save-contact-button">Tambah</button>
        </div>
    </div>

    <script>
        // Mengambil elemen-elemen dari HTML
        const authScreen = document.getElementById('auth-screen');
        const chatScreen = document.getElementById('chat-screen');
        const authButton = document.getElementById('auth-button');
        const toggleRegisterLink = document.getElementById('toggle-register');
        const phoneNumberInput = document.getElementById('phone-number');
        const passwordInput = document.getElementById('password');
        const usernameInput = document.getElementById('username');
        const logoutButton = document.getElementById('logout-button');
        const messageForm = document.getElementById('message-form');
        const messageInput = document.getElementById('message-input');
        const messageContainer = document.getElementById('message-container');
        const chatRecipient = document.getElementById('chat-recipient');
        const contactList = document.getElementById('contact-list');
        const publicChatButton = document.getElementById('public-chat-button');
        const searchInput = document.getElementById('search-input');
        const addContactButton = document.getElementById('add-contact-button');
        const addContactModal = document.getElementById('add-contact-modal');
        const closeButton = document.querySelector('.close-button');
        const newContactNumberInput = document.getElementById('new-contact-number');
        const saveContactButton = document.getElementById('save-contact-button');

        let isRegisterMode = false;
        let loggedInUser = null;
        let currentChatRecipient = null;

        // Simulasi data pengguna terdaftar (akan diganti dengan data dari backend)
        const registeredUsers = [
            { id: 'user1', name: 'Alisa', phone: '628123456789' },
            { id: 'user2', name: 'Budi', phone: '628129876543' },
            { id: 'user3', name: 'Cindy', phone: '6281355554444' },
            { id: 'user4', name: 'Dani', phone: '6281122334455' },
            { id: 'user5', name: 'Eka', phone: '6281988887777' },
        ];
        
        let contacts = [];

        const messages = {
            'public': [
                { sender: 'user1', text: 'Halo, semuanya!' },
                { sender: 'user2', text: 'Hai juga!' },
            ],
            'user1': [
                { sender: 'user1', text: 'Ini pesan pribadi.' },
                { sender: 'me', text: 'Oke, sudah diterima.' },
            ],
        };

        // Fungsi untuk menampilkan pesan di layar
        function displayMessage(senderName, message, type) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('message', type);
            
            if (type === 'received') {
                const senderSpan = document.createElement('span');
                senderSpan.classList.add('sender-name');
                senderSpan.innerText = senderName;
                messageElement.appendChild(senderSpan);
            }
            
            const messageText = document.createElement('p');
            messageText.innerText = message;
            messageElement.appendChild(messageText);
            
            messageContainer.appendChild(messageElement);
            messageContainer.scrollTop = messageContainer.scrollHeight;
        }

        // Fungsi untuk beralih mode login/daftar
        toggleRegisterLink.addEventListener('click', (e) => {
            e.preventDefault();
            isRegisterMode = !isRegisterMode;
            if (isRegisterMode) {
                authButton.innerText = 'Daftar';
                toggleRegisterLink.innerText = 'Login sekarang';
                usernameInput.classList.remove('hidden');
            } else {
                authButton.innerText = 'Login';
                toggleRegisterLink.innerText = 'Daftar sekarang';
                usernameInput.classList.add('hidden');
            }
        });

        // Fungsi untuk otentikasi (simulasi)
        authButton.addEventListener('click', () => {
            const phoneNumber = phoneNumberInput.value.trim();
            const password = passwordInput.value.trim();
            const username = usernameInput.value.trim();

            if (!phoneNumber || !password) {
                alert('Nomor WhatsApp dan password harus diisi.');
                return;
            }

            if (isRegisterMode) {
                if (!username) {
                    alert('Nama pengguna harus diisi.');
                    return;
                }
                alert(`Pendaftaran berhasil untuk ${username}.`);
                loggedInUser = { id: 'me', name: username, phone: phoneNumber };
                contacts = registeredUsers.slice();
            } else {
                if (phoneNumber === '123' && password === '123') {
                    alert('Login berhasil!');
                    loggedInUser = { id: 'me', name: 'Saya', phone: '123' };
                    contacts = registeredUsers.slice();
                } else {
                    alert('Nomor WhatsApp atau password salah.');
                    return;
                }
            }

            authScreen.classList.remove('active');
            chatScreen.classList.add('active');

            renderContactList();
            selectChat('public', 'Obrolan Publik');
        });

        // Fungsi untuk render daftar kontak
        function renderContactList(searchTerm = '') {
            const oldContacts = contactList.querySelectorAll('li:not(#public-chat-button)');
            oldContacts.forEach(contact => contact.remove());

            const filteredContacts = contacts.filter(contact =>
                contact.name.toLowerCase().includes(searchTerm.toLowerCase())
            );

            filteredContacts.forEach(contact => {
                if (contact.id !== loggedInUser.id) {
                    const li = document.createElement('li');
                    li.innerText = contact.name;
                    li.dataset.id = contact.id;
                    li.addEventListener('click', () => selectChat(contact.id, contact.name));
                    contactList.appendChild(li);
                }
            });
        }

        // Fungsi untuk memilih obrolan dengan kontak atau publik
        function selectChat(recipientId, recipientName) {
            currentChatRecipient = recipientId;
            chatRecipient.innerText = recipientName;
            messageContainer.innerHTML = '';
            
            if (messages[recipientId]) {
                messages[recipientId].forEach(msg => {
                    const senderName = (msg.sender === loggedInUser.id || msg.sender === 'me') ? 'Anda' : (contacts.find(c => c.id === msg.sender)?.name || 'Anonim');
                    const type = (msg.sender === loggedInUser.id || msg.sender === 'me') ? 'sent' : 'received';
                    displayMessage(senderName, msg.text, type);
                });
            }

            const contactItems = contactList.querySelectorAll('li');
            contactItems.forEach(item => item.classList.remove('active'));
            document.querySelector(`li[data-id="${recipientId}"]`)?.classList.add('active');
            
            if (recipientId === 'public') {
                publicChatButton.classList.add('active');
            }
        }

        // Menangani pengiriman pesan
        messageForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const messageText = messageInput.value.trim();

            if (messageText && currentChatRecipient) {
                const senderId = loggedInUser.id;
                
                displayMessage('Anda', messageText, 'sent');
                
                if (!messages[currentChatRecipient]) {
                    messages[currentChatRecipient] = [];
                }
                messages[currentChatRecipient].push({ sender: senderId, text: messageText });
                
                messageInput.value = '';
            }
        });

        // Log out
        logoutButton.addEventListener('click', () => {
            alert('Anda telah keluar.');
            loggedInUser = null;
            currentChatRecipient = null;
            authScreen.classList.add('active');
            chatScreen.classList.remove('active');
            
            phoneNumberInput.value = '';
            passwordInput.value = '';
            usernameInput.value = '';
        });

        // Tambahkan event listener untuk tombol obrolan publik
        publicChatButton.addEventListener('click', () => {
            selectChat('public', 'Obrolan Publik');
        });

        // Tambahkan event listener untuk kolom pencarian
        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value;
            renderContactList(searchTerm);
        });

        // Event listener untuk modal Tambah Kontak
        addContactButton.addEventListener('click', () => {
            addContactModal.style.display = 'flex';
        });

        closeButton.addEventListener('click', () => {
            addContactModal.style.display = 'none';
        });

        window.addEventListener('click', (e) => {
            if (e.target == addContactModal) {
                addContactModal.style.display = 'none';
            }
        });

        saveContactButton.addEventListener('click', () => {
            const newNumber = newContactNumberInput.value.trim();
            if (newNumber) {
                const userExists = registeredUsers.find(user => user.phone === newNumber);
                const contactExists = contacts.find(contact => contact.phone === newNumber);

                if (userExists && !contactExists) {
                    contacts.push(userExists);
                    renderContactList();
                    alert(`${userExists.name} berhasil ditambahkan ke kontak Anda.`);
                    addContactModal.style.display = 'none';
                    newContactNumberInput.value = '';
                } else if (contactExists) {
                    alert('Kontak ini sudah ada di daftar Anda.');
                } else {
                    alert('Nomor tidak terdaftar atau tidak valid.');
                }
            } else {
                alert('Nomor tidak boleh kosong.');
            }
        });
    </script>
</body>
</html>
