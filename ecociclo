<!DOCTYPE html>
<html lang="pt-BR" class="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ecociclo - Dashboard de Sustentabilidade</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
    <!-- LeafletJS for Interactive Map -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--color-background);
            transition: background-color 0.3s, color 0.3s;
        }
        .tab-content, .community-sub-tab-content, .friends-sub-tab-content { display: none; }
        .tab-content.active, .community-sub-tab-content.active, .friends-sub-tab-content.active { display: block; }
        
        #chatbot-tab.active, #map-tab.active, #community-tab.active { display: flex; flex-direction: column; }

        :root {
            --color-background: #F7F8FA; /* Lighter, neutral gray */
            --color-foreground: #ffffff;
            --color-text-primary: #111827; /* Darker for better contrast */
            --color-text-secondary: #6b7280; /* Standard gray */
            --color-primary: #16a34a;   /* A vibrant, friendly green (like Tailwind's green-600) for primary actions */
            --color-secondary: #84cc16; /* A bright lime green (like Tailwind's lime-500) as secondary/accent */
            --color-accent: #f97316; /* A brighter orange for accents */
            --color-border: #e5e7eb; /* Standard light gray */
            --color-shadow: rgba(0, 0, 0, 0.05);
            --color-gradient-end: #F0F2F5;
        }

        html.dark {
            --color-background: #111827; /* Darker gray */
            --color-foreground: #1f2937; /* Lighter gray for cards */
            --color-text-primary: #f9fafb; /* Off-white text */
            --color-text-secondary: #9ca3af; /* Softer secondary text */
            --color-primary: #4ADE80; /* Brighter green for dark mode primary */
            --color-secondary: #A3E635; /* Vibrant lime for accent */
            --color-accent: #fbbd23;
            --color-border: #374151; /* Softer border */
            --color-shadow: rgba(0, 0, 0, 0.25);
            --color-gradient-end: #1a243a;
        }

        .bg-background { background-color: var(--color-background); }
        .bg-foreground { background-color: var(--color-foreground); }
        .text-primary { color: var(--color-text-primary); }
        .text-secondary { color: var(--color-text-secondary); }
        .text-theme-primary { color: var(--color-primary); }
        .bg-theme-primary { background-color: var(--color-primary); }
        .bg-theme-secondary { background-color: var(--color-secondary); }
        .border-theme { border-color: var(--color-border); }
        .shadow-custom { box-shadow: 0 4px 6px -1px var(--color-shadow), 0 2px 4px -2px var(--color-shadow); }
        .shadow-custom-lg { box-shadow: 0 10px 15px -3px var(--color-shadow), 0 4px 6px -4px var(--color-shadow); }
        .bg-home-gradient { background-color: var(--color-gradient-end); }


        /* Animation Styles */
        .fade-in { animation: fadeIn 0.5s ease-in-out forwards; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .fade-out { animation: fadeOut 0.3s ease-in-out forwards; }
        @keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }

        .animate-on-scroll {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.6s ease-out, transform 0.6s ease-out;
        }

        .animate-on-scroll.is-visible {
            opacity: 1;
            transform: translateY(0);
        }

        .fade-in-up { animation: fadeInUp 0.5s ease-in-out forwards; opacity: 0; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        .delay-100 { animation-delay: 0.1s; }
        .delay-200 { animation-delay: 0.2s; }
        .delay-300 { animation-delay: 0.3s; }
        .delay-400 { animation-delay: 0.4s; }
        .delay-500 { animation-delay: 0.5s; }
        .delay-600 { animation-delay: 0.6s; }
        
        @keyframes splash-title-fade-in { 0%, 60% { opacity: 0; transform: translateY(10px); } 100% { opacity: 1; transform: translateY(0); } }
        .splash-title {
            animation: splash-title-fade-in 1.5s ease-out forwards;
            animation-delay: 0.3s;
        }

        .progress-bar-fill { transition: width 0.8s ease-in-out; }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #8b949e; border-radius: 8px; }
        ::-webkit-scrollbar-thumb:hover { background: #6b7280; }

        .leaderboard-item:nth-child(1) .rank { background-color: #ffd700; color: #fff; }
        .leaderboard-item:nth-child(2) .rank { background-color: #c0c0c0; color: #fff; }
        .leaderboard-item:nth-child(3) .rank { background-color: #cd7f32; color: #fff; }

        .circular-progress, .goal-progress {
            background: conic-gradient(var(--color-secondary) var(--progress-angle, 0deg), var(--color-border) 0deg);
            transition: background 0.8s ease-in-out;
        }

        #activity-history-container .content {
            max-height: 160px;
            overflow: hidden;
            transition: max-height 0.5s ease-in-out;
        }
        #activity-history-container.expanded .content {
            max-height: 1000px;
        }
        #activity-history-container #expand-history-btn i {
            transition: transform 0.3s;
        }
        #activity-history-container.expanded #expand-history-btn i {
            transform: rotate(180deg);
        }
        
        /* Leaflet styles override */
        .leaflet-popup-content-wrapper, .leaflet-popup-tip {
            background-color: var(--color-foreground);
            color: var(--color-text-primary);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .reward-filter-btn {
            background-color: var(--color-background);
            color: var(--color-text-primary);
            font-size: 14px;
            font-weight: 600;
            padding: 8px 16px;
            border-radius: 9999px;
            transition: all 0.2s ease-in-out;
            border: 1px solid var(--color-border);
        }
        .reward-filter-btn:hover {
            background-color: var(--color-primary);
            color: white;
            transform: translateY(-2px);
        }
        .reward-filter-btn.active {
            background-color: var(--color-primary);
            color: white;
            box-shadow: 0 4px 10px -2px rgba(22, 163, 74, 0.4);
        }

        .parallax-container {
            position: absolute;
            inset: 0;
            overflow: hidden;
            pointer-events: none;
        }
        .parallax-leaf {
            position: absolute;
            transition: transform 0.2s ease-out;
            will-change: transform;
        }
        @keyframes pulse-light {
          70% {
            box-shadow: 0 0 0 20px rgba(163, 230, 53, 0);
          }
        }
        .animate-pulse-light {
            box-shadow: 0 0 0 0 rgba(163, 230, 53, 0.7);
            animation: pulse-light 2s infinite;
        }

        /* Tree Animation Styles Removed */
        
        #tree-confirmation-card {
            opacity: 0;
            animation: fade-in-card 0.8s ease-in-out forwards;
        }

        @keyframes fade-in-card {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
    
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, runTransaction, setDoc, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        setLogLevel('Debug');

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-ecociclo-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' && __firebase_config ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        let db, auth;
        let isAuthReady = false;
        let map, userMarker, markersGroup;
        let simulatedUsers = {};
        let userLatLng = null;
        let locationMarkers = {};

        const appState = {
            userId: null,
            userName: "Visitante",
            points: 0,
            vouchers: [],
            stats: { recycledCount: 0, pointsEarned: 0, plasticCount: 0, acceptedChallenges: [] },
            pointsHistory: []
        };
        
        const API_KEY = "AIzaSyCh398TASAJjPfg06l56FqS-YBmyHnNBj4"; // INSIRA SUA CHAVE DE API DO GEMINI AQUI
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${API_KEY}`;
        let chatHistory = [];

        const FIRESTORE_COLLECTION = 'ecociclo_data_v3';
        const PROFILE_DOC_ID = 'user_profile';
        
        let elements = {};
        
        const levels = [
            { name: 'Iniciante Eco', minPoints: 0, icon: 'fas fa-seedling', color: 'text-green-500' },
            { name: 'Guardião Verde', minPoints: 500, icon: 'fas fa-leaf', color: 'text-blue-500' },
            { name: 'Mestre Reciclador', minPoints: 1500, icon: 'fas fa-crown', color: 'text-yellow-500' },
        ];

        function getUserLevel(points) { return levels.slice().reverse().find(level => points >= level.minPoints) || levels[0]; }
        function getNextLevel(points) { return levels.find(level => points < level.minPoints) || { name: 'Máximo', minPoints: levels[levels.length - 1].minPoints }; }

        async function initializeFirebase(isGuest, userData = {}) {
            if (isGuest || !firebaseConfig || Object.keys(firebaseConfig).length === 0) {
                console.warn(isGuest ? "Modo visitante ativado." : "Configuração do Firebase não encontrada.");
                isAuthReady = true; 
                appState.userName = isGuest ? "Visitante" : userData.name;
                updateUserUI();
                renderApp();
                return;
            }
            try {
                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                onAuthStateChanged(auth, user => {
                    if (user) {
                        appState.userId = user.uid;
                        const savedName = localStorage.getItem(`ecociclo-username-${user.uid}`);
                        appState.userName = savedName || userData.name;
                    } else {
                        appState.userId = 'anonimo-' + crypto.randomUUID().substring(0, 8);
                        appState.userName = userData.name;
                    }
                    isAuthReady = true;
                    setupDataListener(isGuest);
                    updateUserUI();
                });

                if (initialAuthToken) { await signInWithCustomToken(auth, initialAuthToken); } 
                else { await signInAnonymously(auth); }

            } catch (error) { console.error("Erro na inicialização do Firebase:", error); }
        }

        function getUserProfileDocRef() {
            if (!db || !appState.userId || appState.userId.startsWith('anonimo-')) return null;
            return doc(db, 'artifacts', appId, 'users', appState.userId, FIRESTORE_COLLECTION, PROFILE_DOC_ID);
        }

        function setupDataListener(isGuest) {
            if (isGuest) {
                renderApp();
                return;
            }
            const docRef = getUserProfileDocRef();
            if (!docRef) { renderApp(); return; }

            onSnapshot(docRef, (docSnap) => {
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    appState.points = data.points || 0;
                    appState.vouchers = data.vouchers || [];
                    appState.stats = data.stats || { recycledCount: 0, pointsEarned: 0, plasticCount: 0, acceptedChallenges: [] };
                    appState.pointsHistory = data.pointsHistory || [];
                } else {
                    const defaultProfile = { points: 0, vouchers: [], stats: { recycledCount: 0, pointsEarned: 0, plasticCount: 0, acceptedChallenges: [] }, pointsHistory: [], createdAt: new Date().toISOString() };
                    setDoc(docRef, defaultProfile).catch(e => console.error("Erro ao criar perfil:", e));
                }
                renderApp();
            }, (error) => { console.error("Erro no Firestore:", error); });
        }

        async function addPoints(amount, reason) {
            const docRef = getUserProfileDocRef();
            const processPoints = (isGuest = false) => {
               if(isGuest) {
                    appState.points += amount;
                    appState.stats.recycledCount += 1;
                    appState.stats.pointsEarned += amount;
                    if (reason === 'Plásticos') {
                        appState.stats.plasticCount = (appState.stats.plasticCount || 0) + 1;
                    }
                    const newHistoryEntry = { date: new Date().toISOString(), amount: amount, reason: reason, id: crypto.randomUUID() };
                    appState.pointsHistory = [newHistoryEntry, ...appState.pointsHistory].slice(0, 20);
               }
                renderApp();

                const customButton = {
                    text: `✨ Ver Ideias para Reutilizar ${reason}`,
                    className: 'mt-4 w-full bg-yellow-400 text-yellow-900 font-bold py-2 px-4 rounded-lg transition-transform hover:scale-105',
                    onClick: () => {
                        closeModal(elements.customModal);
                        generateReuseIdeas(reason);
                    }
                };
                alertModal("EcoPoints Ganhos!", `<p class="text-green-600 font-bold">+${amount} EcoPoints!</p><p>Item registrado: ${reason}.</p>`, "success", customButton);
            };

            if (!docRef) {
                processPoints(true);
                return;
            }

            try {
                await runTransaction(db, async (transaction) => {
                    const docSnap = await transaction.get(docRef);
                    const data = docSnap.data() || {};
                    
                    const currentStats = data.stats || { recycledCount: 0, pointsEarned: 0, plasticCount: 0, acceptedChallenges: [] };
                    const newStats = { 
                        ...currentStats,
                        recycledCount: (currentStats.recycledCount || 0) + 1, 
                        pointsEarned: (currentStats.pointsEarned || 0) + amount,
                        plasticCount: reason === 'Plásticos' ? (currentStats.plasticCount || 0) + 1 : (currentStats.plasticCount || 0)
                    };

                    const newPoints = (data.points || 0) + amount;
                    const newHistoryEntry = { date: new Date().toISOString(), amount: amount, reason: reason, id: crypto.randomUUID() };
                    const updatedHistory = [newHistoryEntry, ...(data.pointsHistory || [])].slice(0, 20);
                    transaction.set(docRef, { points: newPoints, stats: newStats, pointsHistory: updatedHistory, lastUpdate: new Date().toISOString() }, { merge: true });
                });
                 processPoints(false);
            } catch (error) { console.error("Erro ao adicionar pontos:", error); }
        }
        
        async function redeemVoucher(reward) {
            const docRef = getUserProfileDocRef();

            if (appState.points < reward.cost) {
                alertModal("Pontos Insuficientes", `Você precisa de ${reward.cost} EcoPoints, mas só tem ${appState.points}.`, "warning");
                return;
            }
            
            const isTreeDonation = reward.id === 3;

            // Handle visitor mode
            if (!docRef) {
                appState.points -= reward.cost;
                const newVoucher = { id: crypto.randomUUID(), title: reward.title, code: `ECO-${Math.random().toString(36).substring(2, 8).toUpperCase()}`, redeemedAt: new Date().toISOString(), status: 'active' };
                appState.vouchers.push(newVoucher);
                if (reward.id === 1) { appState.stats.organicMission = true; }
                
                if (isTreeDonation) {
                    triggerTreeAnimation();
                } else {
                    alertModal("Resgate Concluído!", `<p class="font-bold text-green-600">Parabéns!</p><p>Você resgatou: <strong>${reward.title}</strong>.</p><p class="text-sm mt-1">Seu código pode ser visto na aba 'Vouchers'.</p>`, "success");
                }
                renderApp(); // Render after state change
                return;
            }

            // Handle authenticated user
            try {
                await runTransaction(db, async (transaction) => {
                    const docSnap = await transaction.get(docRef);
                    if (!docSnap.exists()) throw new Error("Perfil de usuário não encontrado.");
                    const data = docSnap.data();
                    const currentPoints = data.points || 0;
                    if (currentPoints < reward.cost) throw new Error("Pontos insuficientes.");
                    
                    const newVoucher = { id: crypto.randomUUID(), title: reward.title, code: `ECO-${Math.random().toString(36).substring(2, 8).toUpperCase()}`, redeemedAt: new Date().toISOString(), status: 'active' };
                    const updatedVouchers = [...(data.vouchers || []), newVoucher];
                    
                    const newStats = { ...(data.stats || {}) }; // Use existing stats from snapshot
                    if (reward.id === 1) {
                        newStats.organicMission = true;
                    }
                    
                    transaction.update(docRef, { 
                        points: currentPoints - reward.cost, 
                        vouchers: updatedVouchers, 
                        stats: newStats,
                        lastUpdate: new Date().toISOString() 
                    });
                });

                // Show confirmation AFTER successful transaction
                if (isTreeDonation) {
                    triggerTreeAnimation();
                } else {
                    alertModal("Resgate Concluído!", `<p class="font-bold text-green-600">Parabéns!</p><p>Você resgatou: <strong>${reward.title}</strong>.</p><p class="text-sm mt-1">Seu código pode ser visto na aba 'Vouchers'.</p>`, "success");
                }
            } catch (error) {
                console.error("Erro ao resgatar voucher:", error);
                alertModal("Erro ao Resgatar", `Não foi possível completar o resgate. ${error.message}`, "error");
            }
        }

        function triggerTreeAnimation() {
            // Card is now animated by CSS as soon as the modal opens
            openModal(elements.treeAnimationModal);
        }

        async function updateVoucherStatus(voucherId, newStatus) {
            const docRef = getUserProfileDocRef();
            if (!docRef) { // Modo visitante
                const voucher = appState.vouchers.find(v => v.id === voucherId);
                if (voucher) {
                    voucher.status = newStatus;
                    renderApp(); 
                }
                return;
            }

            try {
                await runTransaction(db, async (transaction) => {
                    const docSnap = await transaction.get(docRef);
                    if (!docSnap.exists()) throw new Error("Perfil de usuário não encontrado.");
                    
                    const data = docSnap.data();
                    const currentVouchers = data.vouchers || [];
                    
                    const updatedVouchers = currentVouchers.map(v => {
                        if (v.id === voucherId) {
                            return { ...v, status: newStatus };
                        }
                        return v;
                    });
                    
                    transaction.update(docRef, { vouchers: updatedVouchers });
                });
            } catch (error) {
                console.error("Erro ao atualizar status do voucher:", error);
                alertModal("Erro", "Não foi possível atualizar o voucher. Tente novamente.", "error");
            }
        }

        const availableRewards = [
            { id: 8, title: 'Ingresso para o Parque do Sabiá', cost: 400, icon: 'fas fa-tree text-green-700', category: 'lifestyle'},
            { id: 1, title: 'Cupom de R$5 em Lojas de Orgânicos', cost: 500, icon: 'fas fa-carrot text-orange-500', category: 'food' },
            { id: 5, title: '15% OFF em cosméticos veganos', cost: 800, icon: 'fas fa-spa text-pink-500', category: 'lifestyle' },
            { id: 6, title: 'Cupom de R$10 em restaurantes vegetarianos', cost: 1000, icon: 'fas fa-utensils text-orange-600', category: 'food' },
            { id: 2, title: '1 Mês de App de Meditação', cost: 1200, icon: 'fas fa-brain text-purple-500', category: 'lifestyle' },
            { id: 7, title: 'Ecobag Exclusiva Ecociclo', cost: 1800, icon: 'fas fa-shopping-bag text-teal-500', category: 'lifestyle' },
            { id: 3, title: 'Doação de 1 Árvore na Amazônia', cost: 2000, icon: 'fas fa-hand-holding-heart text-red-500', category: 'donations' },
            { id: 4, title: 'Garrafa de Água Reutilizável', cost: 3500, icon: 'fas fa-bottle-water text-blue-500', category: 'lifestyle'}
        ];

        function switchTab(targetTabId) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            const targetTab = document.getElementById(targetTabId);
            if (targetTab) {
                targetTab.classList.add('active');
                
                if(targetTabId === 'home-tab') {
                    // Re-trigger scroll animations on home tab
                    document.querySelectorAll('.animate-on-scroll').forEach(el => {
                        el.classList.remove('is-visible');
                    });
                    setupScrollAnimations();
                }

                if (targetTabId === 'community-tab') renderCommunityTab();
                if (targetTabId === 'map-tab' && !map) initMap();

                // Show/hide tip FAB based on the current tab
                if (elements.tipFab) {
                    if (targetTabId === 'home-tab') {
                        elements.tipFab.classList.remove('hidden', 'opacity-0');
                    } else {
                        elements.tipFab.classList.add('hidden', 'opacity-0');
                    }
                }
            }
            elements.navButtons.forEach(button => {
                const isActive = button.getAttribute('data-tab') === targetTabId;
                button.classList.toggle('text-theme-primary', isActive);
                button.classList.toggle('text-secondary', !isActive);
                button.classList.toggle('bg-theme-primary/10', isActive);
            });
        }

        function updateUserUI() {
            document.querySelectorAll('.user-display-name').forEach(el => el.textContent = appState.userName);
            document.querySelectorAll('.user-avatar-img').forEach(el => el.src = `https://placehold.co/80x80/10b981/FFFFFF?text=${appState.userName.charAt(0)}`);
        }
        
        function renderHomeTab() {
            const { points } = appState;
            const currentLevel = getUserLevel(points);
            const nextLevel = getNextLevel(points);
            const progress = Math.max(0, (points - currentLevel.minPoints) / (nextLevel.minPoints - currentLevel.minPoints)) * 100;
            
            if (elements.homeUserLevel) {
                 elements.homeUserLevel.innerHTML = `<i class="${currentLevel.icon} text-lime-300 mr-2"></i>${currentLevel.name}`;
            }

            if (elements.homeLevelProgress) {
                elements.homeLevelProgress.style.width = `${progress}%`;
                elements.homeLevelProgressText.textContent = `${points.toLocaleString('pt-BR')} / ${nextLevel.minPoints.toLocaleString('pt-BR')}`;
            }
            
            renderActivityFeed(elements.homeActivityFeed, 3);
        }

        function renderApp() {
            const { points, vouchers, stats, pointsHistory } = appState;
            const currentLevel = getUserLevel(points);
            const nextLevel = getNextLevel(points);
            const progress = Math.max(0, (points - currentLevel.minPoints) / (nextLevel.minPoints - currentLevel.minPoints)) * 100;

            elements.pointsDisplays.forEach(el => el.textContent = points.toLocaleString('pt-BR'));
            elements.userLevelDisplays.forEach(el => { el.innerHTML = `<i class="${currentLevel.icon} ${currentLevel.color} mr-2"></i>${currentLevel.name}`; });
            
            // Home Tab Render
            renderHomeTab();

            // Account Tab Specific Renders
            if (elements.levelProgress) {
                elements.levelProgress.style.width = `${progress}%`;
                elements.levelProgressText.textContent = `${points.toLocaleString('pt-BR')} / ${nextLevel.minPoints.toLocaleString('pt-BR')} pts`;
            }
             if (elements.accountStatItems) elements.accountStatItems.textContent = stats.recycledCount || 0;
            if (elements.accountStatPoints) elements.accountStatPoints.textContent = stats.pointsEarned || 0;
            if (elements.accountStatVouchers) elements.accountStatVouchers.textContent = vouchers.length || 0;

            renderRewards();
            renderFeaturedReward();
            renderAchievements();
            
            if(elements.voucherList) {
                elements.voucherList.innerHTML = vouchers.length ? vouchers.map(v => {
                    const reward = availableRewards.find(r => r.title === v.title);
                    const iconClass = reward ? reward.icon : 'fas fa-gift';
                    const isUsed = v.status === 'used';
                    
                    return `
                    <li class="voucher-card bg-foreground rounded-xl shadow-custom border-l-4 ${isUsed ? 'border-gray-400 opacity-60' : 'border-theme-primary'} overflow-hidden transition-all duration-300" data-voucher-id="${v.id}" data-status="${v.status}">
                        <div class="p-4 flex flex-col sm:flex-row items-start sm:items-center gap-4">
                            <div class="bg-theme-primary/10 text-theme-primary h-12 w-12 flex-shrink-0 flex items-center justify-center rounded-full">
                                <i class="${iconClass} text-xl"></i>
                            </div>
                            <div class="flex-grow">
                                <h4 class="font-bold text-md text-primary ${isUsed ? 'line-through' : ''}">${v.title}</h4>
                                <p class="text-xs text-gray-400 mt-1">Resgatado em: ${new Date(v.redeemedAt).toLocaleDateString('pt-BR')}</p>
                            </div>
                            <div class="w-full sm:w-auto flex flex-row sm:flex-col items-center sm:items-end justify-between gap-2 mt-2 sm:mt-0">
                                <div class="text-left sm:text-right">
                                    <p class="text-sm text-secondary">Código:</p>
                                    <p class="font-mono bg-background p-2 rounded-md text-primary font-bold tracking-wider">${v.code}</p>
                                </div>
                                <button class="copy-code-btn text-theme-primary font-semibold text-sm py-2 px-3 rounded-lg hover:bg-theme-primary/10 transition-colors" data-code="${v.code}" title="Copiar Código">
                                    <i class="fas fa-copy"></i>
                                </button>
                            </div>
                        </div>
                        <div class="bg-background px-4 py-2 text-right">
                            <button class="mark-status-btn text-xs font-semibold rounded-md px-3 py-1 transition-colors ${isUsed ? 'bg-green-100 dark:bg-green-800/50 text-green-700 dark:text-green-300 hover:bg-green-200' : 'bg-gray-200 dark:bg-gray-700/50 text-gray-700 dark:text-gray-300 hover:bg-gray-300'}" data-voucher-id="${v.id}" data-current-status="${v.status}">
                                ${isUsed ? '<i class="fas fa-undo-alt mr-2"></i>Reativar' : '<i class="fas fa-check mr-2"></i>Marcar como Usado'}
                            </button>
                        </div>
                    </li>`;
                }).join('') : `<li class="text-center text-secondary py-10 bg-foreground rounded-xl shadow-sm"><div class="text-center"><i class="fas fa-ticket-alt text-4xl text-gray-300 dark:text-gray-600 mb-3"></i><h4 class="font-bold text-primary">Nenhum Voucher</h4><p class="text-sm mt-1">Troque seus EcoPoints por recompensas incríveis na loja!</p></div></li>`;

                // Manter o estado do toggle após a re-renderização
                if(elements.toggleUsedVouchers) {
                    const isChecked = elements.toggleUsedVouchers.checked;
                     document.querySelectorAll('.voucher-card').forEach(card => {
                        if (card.dataset.status === 'used') {
                            card.style.display = isChecked ? 'list-item' : 'none';
                        }
                    });
                }
            }
        
            renderActivityFeed(elements.activityFeed, 20);
        }

        function renderFeaturedReward() {
            const featuredReward = availableRewards.find(r => r.id === 3); // Doação de 1 Árvore
            if (!featuredReward || !elements.featuredRewardContainer) return;

            const isRedeemable = appState.points >= featuredReward.cost;

            elements.featuredRewardContainer.innerHTML = `
                <h2 class="text-xl font-bold text-primary mb-3">Recompensa em Destaque</h2>
                <div class="relative bg-gradient-to-tr from-green-600 to-lime-500 text-white p-6 rounded-2xl shadow-lg overflow-hidden flex flex-col md:flex-row items-center gap-6 transition-transform duration-300 hover:scale-[1.02]">
                    <span class="absolute -top-2 -right-2 bg-yellow-400 text-yellow-900 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider">Destaque</span>
                    <div class="flex-shrink-0 text-center">
                        <i class="${featuredReward.icon} text-6xl text-white/80" style="text-shadow: 0 2px 5px rgba(0,0,0,0.2);"></i>
                    </div>
                    <div class="flex-grow text-center md:text-left">
                        <h3 class="text-2xl font-bold">${featuredReward.title}</h3>
                        <p class="mt-1 text-white/90">Faça a diferença! Cada doação ajuda a reflorestar a Amazônia e combater as mudanças climáticas.</p>
                        <p class="mt-2 text-lg font-bold">Custo: <span class="text-yellow-300">${featuredReward.cost.toLocaleString('pt-BR')} EcoPoints</span></p>
                    </div>
                    <div class="flex-shrink-0">
                         <button data-reward-id="${featuredReward.id}" class="redeem-btn w-full md:w-auto bg-white text-green-700 font-bold py-3 px-6 rounded-lg shadow-md transition-all duration-200 hover:bg-yellow-300 active:scale-95 ${!isRedeemable ? 'cursor-not-allowed bg-gray-300 text-gray-500' : ''}" ${!isRedeemable ? 'disabled' : ''}>
                            ${isRedeemable ? 'Resgatar Agora' : 'Pontos Insuficientes'}
                        </button>
                    </div>
                </div>
            `;
        }

        function renderRewards(filter = 'all') {
            const { points } = appState;
            const filteredRewards = filter === 'all' ? availableRewards : availableRewards.filter(r => r.category === filter);
            
            if (!elements.rewardsContainer) return;

            elements.rewardsContainer.innerHTML = filteredRewards.map(reward => {
                const isRedeemable = points >= reward.cost;
                const progress = isRedeemable ? 100 : Math.floor((points / reward.cost) * 100);
                
                return `
                <div class="bg-foreground rounded-xl shadow-custom border-theme border flex flex-col transition-all duration-300 ${isRedeemable ? 'hover:shadow-custom-lg hover:-translate-y-1' : 'opacity-70'}">
                    <div class="p-5 flex-grow">
                        <div class="flex items-center gap-4 mb-3">
                            <i class="${reward.icon} text-3xl w-8 text-center"></i>
                            <h4 class="font-bold text-md text-primary leading-tight flex-1">${reward.title}</h4>
                        </div>
                        <p class="text-sm text-secondary">Custo: <span class="font-bold text-theme-primary">${reward.cost.toLocaleString('pt-BR')}</span> pontos</p>
                    </div>
                    <div class="bg-background px-5 py-3 rounded-b-xl">
                        ${!isRedeemable ? `
                        <div class="mb-2">
                            <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-2">
                                <div class="bg-theme-primary h-2 rounded-full" style="width: ${progress}%"></div>
                            </div>
                            <p class="text-xs text-secondary text-right mt-1">${progress}% completo</p>
                        </div>` : ''}
                        <button data-reward-id="${reward.id}" class="redeem-btn w-full bg-theme-primary text-white font-bold py-2 px-4 text-sm rounded-lg transition-all duration-200 hover:opacity-80 active:scale-95 ${!isRedeemable ? 'cursor-not-allowed bg-gray-400 dark:bg-gray-600' : ''}" ${!isRedeemable ? 'disabled' : ''}>
                            ${isRedeemable ? 'Resgatar Agora' : 'Pontos Insuficientes'}
                        </button>
                    </div>
                </div>`;
            }).join('');
        }

        const achievements = [
            { id: 'first_recycle', title: 'Primeiro Passo', description: 'Registrou seu primeiro item reciclável.', icon: 'fa-shoe-prints', check: (stats) => stats.recycledCount >= 1 },
            { id: 'plastic_hero', title: 'Herói do Plástico', description: 'Reciclou 10 itens de plástico.', icon: 'fa-bottle-water', check: (stats) => (stats.plasticCount || 0) >= 10 },
            { id: 'point_collector', title: 'Junta-Pontos', description: 'Acumulou 1.000 EcoPoints.', icon: 'fa-star', check: (stats) => stats.pointsEarned >= 1000 },
            { id: 'first_voucher', title: 'Recompensa Eco', description: 'Resgatou seu primeiro voucher.', icon: 'fa-ticket-alt', check: (vouchers) => vouchers.length >= 1 },
            { id: 'tree_friend', title: 'Amigo da Floresta', description: 'Doou uma árvore na Amazônia.', icon: 'fa-hand-holding-heart', check: (vouchers) => vouchers.some(v => v.title.includes('Árvore')) },
            { id: 'eco_novice', title: 'Novato Eco', description: 'Alcançou o nível Guardião Verde.', icon: 'fa-leaf', check: (points) => points >= 500 },
        ];

        function renderAchievements() {
            if (!elements.achievementsContainer) return;
            elements.achievementsContainer.innerHTML = achievements.map(ach => {
                let unlocked = false;
                if (ach.id === 'first_voucher' || ach.id === 'tree_friend') {
                    unlocked = ach.check(appState.vouchers);
                } else if(ach.id === 'eco_novice') {
                    unlocked = ach.check(appState.points);
                } else {
                    unlocked = ach.check(appState.stats);
                }
                
                return `
                <div class="text-center transition-opacity ${unlocked ? 'opacity-100' : 'opacity-40'}" title="${ach.description}${!unlocked ? ' (Bloqueado)' : ''}">
                    <div class="w-16 h-16 mx-auto rounded-full flex items-center justify-center ${unlocked ? 'bg-yellow-400 text-white' : 'bg-background'}">
                        <i class="fas ${ach.icon} text-3xl ${!unlocked ? 'text-secondary' : ''}"></i>
                    </div>
                    <p class="text-xs font-semibold text-primary mt-2">${ach.title}</p>
                </div>
                `;
            }).join('');
        }


        function renderActivityFeed(container, limit) {
            const { pointsHistory } = appState;
            const activityFeed = container;
            if (!activityFeed) return;
            activityFeed.innerHTML = '';

            if (pointsHistory.length > 0) {
                const iconMap = {
                    "Papel e Papelão": { icon: "fas fa-box", color: "text-green-500", bg: "bg-green-100 dark:bg-green-900/50" },
                    "Plásticos": { icon: "fas fa-bottle-water", color: "text-blue-500", bg: "bg-blue-100 dark:bg-blue-900/50" },
                    "Metais": { icon: "fas fa-cog", color: "text-gray-500", bg: "bg-gray-100 dark:bg-gray-800/50" },
                    "Vidros": { icon: "fas fa-wine-bottle", color: "text-red-500", bg: "bg-red-100 dark:bg-red-900/50" },
                    "Móveis Pequenos": { icon: "fas fa-chair", color: "text-purple-500", bg: "bg-purple-100 dark:bg-purple-900/50" },
                    "Móveis Grandes": { icon: "fas fa-couch", color: "text-indigo-500", bg: "bg-indigo-100 dark:bg-indigo-900/50" },
                    "Entulho": { icon: "fas fa-trowel-bricks", color: "text-orange-500", bg: "bg-orange-100 dark:bg-orange-900/50" },
                    "Lixo Eletrônico": { icon: "fas fa-mobile-alt", color: "text-yellow-600", bg: "bg-yellow-100 dark:bg-yellow-900/50" },
                    "default": { icon: "fas fa-recycle", color: "text-gray-500", bg: "bg-gray-100 dark:bg-gray-800/50" }
                };

                pointsHistory.slice(0, limit).forEach(entry => {
                    const itemData = iconMap[entry.reason] || iconMap.default;
                    const date = new Date(entry.date);
                    const formattedDate = date.toLocaleDateString('pt-BR', { day: '2-digit', month: 'short' });

                    const itemHTML = `
                        <div class="flex items-center p-3">
                            <div class="${itemData.bg} ${itemData.color} w-10 h-10 flex items-center justify-center rounded-full mr-4">
                                <i class="${itemData.icon}"></i>
                            </div>
                            <div class="flex-grow">
                                <p class="text-sm text-primary">Você ganhou <strong class="text-green-500">+${entry.amount} pontos</strong></p>
                                <p class="text-xs text-secondary">por reciclar ${entry.reason}</p>
                            </div>
                            <div class="text-xs text-secondary">${formattedDate}</div>
                        </div>
                    `;
                    activityFeed.innerHTML += itemHTML;
                });
            } else {
                activityFeed.innerHTML = '<p class="text-center text-sm text-secondary py-10">Nenhuma atividade recente para mostrar. Comece a reciclar!</p>';
            }
        }


        function renderCommunityTab() {
            renderLeaderboard('weekly');
            renderCommunityGoal();
            renderCommunityChallenges();
            renderCommunityEvents();
            renderCommunityTopics();
            renderFriendsFeed();
            renderFriendsGoals();
            renderFriendsChallenges();
        }

        const communityData = {
            leaderboard: [
                { name: 'Anna R.', points: 2840, avatar: 'https://placehold.co/40x40/FFC107/FFFFFF?text=A' },
                { name: 'João P.', points: 1950, avatar: 'https://placehold.co/40x40/03A9F4/FFFFFF?text=J' },
                { name: 'Carlos F.', points: 1780, avatar: 'https://placehold.co/40x40/9C27B0/FFFFFF?text=C' },
                { name: 'Beatriz L.', points: 1210, avatar: 'https://placehold.co/40x40/F44336/FFFFFF?text=B' },
                { name: 'Ana C.', points: 850, avatar: 'https://placehold.co/40x40/E91E63/FFFFFF?text=A' },
                { name: 'Lucas M.', points: 430, avatar: 'https://placehold.co/40x40/2196F3/FFFFFF?text=L' },
                { name: 'Fernanda G.', points: 210, avatar: 'https://placehold.co/40x40/009688/FFFFFF?text=F' },
            ],
            communityGoal: {
                title: 'Reciclar 10.000 garrafas PET',
                current: 7852,
                target: 10000,
                reward: 'Sorteio de 5 Ecobags'
            },
            challenges: [
                { id: 'plastic_king', title: 'Rei do Plástico', description: 'Descarte 15 itens de plástico esta semana.', reward: 50, target: 15, current: () => appState.stats.plasticCount || 0, icon: 'fa-bottle-water' },
                { id: 'organic_mission', title: 'Missão Orgânica', description: 'Troque um voucher em lojas de orgânicos parceiras.', reward: 100, target: 1, current: () => appState.stats.organicMission ? 1 : 0, icon: 'fa-carrot' },
            ],
            events: [
                { title: 'Mutirão de Limpeza no Parque do Sabiá', date: '25/10/2025', description: 'Vamos juntos limpar uma das áreas mais bonitas da nossa cidade.'},
                { title: 'Workshop de Compostagem (Online)', date: '05/11/2025', description: 'Aprenda a transformar seu lixo orgânico em adubo para suas plantas.'},
            ]
        };

        let communityTopicsData = [
            { id: 1, title: 'Qual a melhor forma de limpar embalagens de iogurte?', content: 'Estou com dificuldade de tirar todo o resíduo das embalagens de iogurte antes de descartar. Alguém tem alguma dica para fazer isso sem gastar muita água?', author: 'Beatriz L.', avatar: 'https://placehold.co/40x40/F44336/FFFFFF?text=B', time: '3h atrás', likes: 15, isLiked: false, comments: [ {author: 'João P.', text: 'Eu costumo usar um pouco de água de reuso da louça!'} ] },
            { id: 2, title: 'Dica: borra de café é um ótimo adubo para as plantas!', content: 'Pessoal, descobri que a borra de café é fantástica para as plantas aqui de casa. Elas ficaram muito mais vistosas! É só deixar secar e misturar na terra. #FicaADica', author: 'Carlos F.', avatar: 'https://placehold.co/40x40/9C27B0/FFFFFF?text=C', time: '1d atrás', likes: 42, isLiked: true, comments: [] },
            { id: 3, title: 'Alguém sabe onde descartar isopor aqui em Uberlândia?', content: 'Recebi uma encomenda que veio com muito isopor e não sei qual o Ecoponto certo para levar. Alguma sugestão?', author: 'Fernanda G.', avatar: 'https://placehold.co/40x40/009688/FFFFFF?text=F', time: '2d atrás', likes: 8, isLiked: false, comments: [] },
        ];

        let friendsData = {
            feed: [
                { id: 1, type: 'achievement', friend: 'Anna R.', avatar: 'https://placehold.co/40x40/FFC107/FFFFFF?text=A', action: 'alcançou o nível', details: 'Mestre Reciclador!', time: '2h atrás', likes: 12, comments: [{author: 'João P.', text: 'Parabéns!'}], isLiked: false },
                { id: 2, type: 'reward', friend: 'João P.', avatar: 'https://placehold.co/40x40/03A9F4/FFFFFF?text=J', action: 'resgatou a recompensa', details: '1 Mês de App de Meditação', time: '5h atrás', likes: 8, comments: [], isLiked: false },
                { id: 3, type: 'challenge', friend: 'Carlos F.', avatar: 'https://placehold.co/40x40/9C27B0/FFFFFF?text=C', action: 'completou o desafio', details: 'Rei do Plástico', time: '1d atrás', likes: 25, comments: [], isLiked: false },
            ],
            goals: [
                { id: 1, title: 'Reciclar 50 garrafas PET este mês', current: 35, target: 50, personal: true },
                { id: 2, title: 'Juntar 1000 EcoPoints para doação', current: 850, target: 1000, personal: true },
            ],
            challenges: [
                { id: 1, title: 'Desafio Semanal de Pontos', participants: ['Você', 'Anna R.'], description: 'Quem ganha mais EcoPoints esta semana?', yourScore: 120, friendScore: 150 },
                { id: 2, title: 'Batalha do Papelão', participants: ['Você', 'João P.'], description: 'Quem recicla mais papel/papelão até sexta?', yourScore: 5, friendScore: 8 },
            ]
        };

        function renderFriendsFeed() {
            elements.friendsFeedContainer.innerHTML = friendsData.feed.map(post => `
                <div class="bg-background p-4 rounded-xl shadow-sm border border-theme" data-post-id="${post.id}">
                    <div class="flex items-start gap-3">
                        <img src="${post.avatar}" class="w-10 h-10 rounded-full object-cover">
                        <div class="flex-grow">
                             ${post.type === 'custom' ? `<p class="text-sm"><strong class="text-primary">${post.friend}</strong></p><p class="text-primary mt-1">${post.details}</p>` : `<p class="text-sm"><strong class="text-primary">${post.friend}</strong> ${post.action} <strong class="text-theme-primary">${post.details}</strong></p>`}
                            <p class="text-xs text-secondary mt-2">${post.time}</p>
                        </div>
                    </div>
                    <div class="mt-3 flex items-center gap-4 text-secondary text-sm pt-3 border-t border-theme">
                        <button class="like-btn flex items-center gap-1 hover:text-red-500 transition-colors ${post.isLiked ? 'text-red-500' : ''}"><i class="fas fa-heart"></i> ${post.likes}</button>
                        <button class="comment-btn flex items-center gap-1 hover:text-theme-primary transition-colors"><i class="fas fa-comment"></i> ${post.comments.length}</button>
                    </div>
                </div>
            `).join('');
        }

        function renderFriendsGoals() {
            elements.friendsGoalsContainer.innerHTML = friendsData.goals.map(goal => {
                 const progress = Math.min((goal.current / goal.target) * 100, 100);
                 const progressAngle = (progress / 100) * 360;
                 return `
                      <div class="bg-background p-4 rounded-xl shadow-sm border border-theme flex items-center gap-4">
                           <div class="relative w-20 h-20">
                                <div class="goal-progress w-full h-full rounded-full" style="--progress-angle: ${progressAngle}deg;"></div>
                                <div class="absolute inset-0 flex items-center justify-center font-bold text-lg text-theme-secondary">${Math.round(progress)}%</div>
                           </div>
                           <div class="flex-grow">
                                <p class="font-bold text-primary">${goal.title}</p>
                                   <p class="text-sm text-secondary mt-1">${goal.current} / ${goal.target}</p>
                           </div>
                      </div>
                 `;
            }).join('') + `
                <button id="create-goal-btn" class="w-full text-center bg-theme-primary/10 text-theme-primary font-semibold p-3 rounded-lg hover:bg-theme-primary/20 transition-colors">
                    <i class="fas fa-plus mr-2"></i>Criar Nova Meta
                </button>
            `;
        }
        
        function renderFriendsChallenges() {
             elements.friendsChallengesContainer.innerHTML = friendsData.challenges.map(challenge => {
                 const isWinner = challenge.yourScore > challenge.friendScore;
                 return `
                 <div class="bg-background p-4 rounded-xl shadow-sm border border-theme">
                      <p class="font-bold text-primary text-center">${challenge.title}</p>
                      <p class="text-sm text-secondary mt-1 text-center">${challenge.description}</p>
                      <div class="mt-4 grid grid-cols-2 gap-4 items-center">
                           <!-- You -->
                           <div class="text-center">
                                <div class="relative inline-block">
                                     <img src="https://placehold.co/64x64/10b981/FFFFFF?text=${appState.userName.charAt(0)}" class="w-16 h-16 rounded-full object-cover mx-auto border-4 ${isWinner ? 'border-yellow-400' : 'border-theme'}">
                                     ${isWinner ? '<i class="fas fa-crown text-yellow-400 absolute -top-2 right-0 text-lg"></i>' : ''}
                                </div>
                                <p class="font-bold text-primary mt-2">${challenge.participants[0]}</p>
                                <p class="font-bold text-2xl text-theme-primary">${challenge.yourScore}</p>
                           </div>
                           <!-- Friend -->
                           <div class="text-center">
                                 <div class="relative inline-block">
                                     <img src="https://placehold.co/64x64/03A9F4/FFFFFF?text=${challenge.participants[1].charAt(0)}" class="w-16 h-16 rounded-full object-cover mx-auto border-4 ${!isWinner ? 'border-yellow-400' : 'border-theme'}">
                                     ${!isWinner ? '<i class="fas fa-crown text-yellow-400 absolute -top-2 right-0 text-lg"></i>' : ''}
                                 </div>
                                <p class="font-bold text-primary mt-2">${challenge.participants[1]}</p>
                                <p class="font-bold text-2xl text-theme-primary">${challenge.friendScore}</p>
                           </div>
                      </div>
                 </div>
            `}).join('') + `
                <button id="create-challenge-btn" class="w-full text-center bg-theme-primary/10 text-theme-primary font-semibold p-3 rounded-lg hover:bg-theme-primary/20 transition-colors">
                    <i class="fas fa-plus mr-2"></i>Criar Novo Desafio
                </button>
            `;
        }


        function renderLeaderboard(filter) {
            // In a real app, you'd fetch data for each filter. Here, we'll simulate it.
            const multiplier = { weekly: 0.3, monthly: 0.7, alltime: 1 };
            
            const userData = { name: 'Você', points: appState.points, isCurrentUser: true, avatar: `https://placehold.co/40x40/10b981/FFFFFF?text=${appState.userName.charAt(0)}` };

            const fullLeaderboard = [
                ...communityData.leaderboard.map(u => ({...u, points: Math.floor(u.points * multiplier[filter])})),
                userData
            ].sort((a, b) => b.points - a.points);
            
            const userRank = fullLeaderboard.findIndex(u => u.isCurrentUser) + 1;

            elements.leaderboardList.innerHTML = fullLeaderboard.slice(0, 5).map((user, index) => {
                const level = getUserLevel(user.points);
                return `
                <div class="leaderboard-item flex items-center p-3 rounded-lg transition-colors ${user.isCurrentUser ? 'bg-theme-primary/10' : 'hover:bg-gray-50 dark:hover:bg-slate-800/50'}">
                    <div class="rank w-8 h-8 flex items-center justify-center font-bold text-sm rounded-full mr-3 bg-slate-200 dark:bg-slate-600">${index + 1}</div>
                    <img src="${user.avatar}" class="w-10 h-10 rounded-full mr-3 object-cover">
                    <div class="flex-grow">
                        <p class="font-bold text-primary">${user.name}</p>
                        <p class="text-xs text-secondary">${level.name}</p>
                    </div>
                    <div class="font-extrabold text-theme-primary">${user.points.toLocaleString('pt-BR')} pts</div>
                </div>`
            }).join('');

            // Highlight user's rank if not in top 5
            if (userRank > 5) {
                const level = getUserLevel(userData.points);
                elements.userRankHighlight.innerHTML = `
                <div class="leaderboard-item flex items-center p-3 rounded-lg bg-theme-primary/10 border-t-2 border-dashed border-theme">
                    <div class="rank w-8 h-8 flex items-center justify-center font-bold text-sm rounded-full mr-3 bg-slate-200 dark:bg-slate-600">${userRank}</div>
                    <img src="${userData.avatar}" class="w-10 h-10 rounded-full mr-3 object-cover">
                    <div class="flex-grow">
                        <p class="font-bold text-primary">${userData.name}</p>
                        <p class="text-xs text-secondary">${level.name}</p>
                    </div>
                    <div class="font-extrabold text-theme-primary">${userData.points.toLocaleString('pt-BR')} pts</div>
                </div>`;
            } else {
                elements.userRankHighlight.innerHTML = '';
            }
        }

        function renderCommunityGoal() {
            const goal = communityData.communityGoal;
            const progress = (goal.current / goal.target) * 100;
            elements.communityGoalContainer.innerHTML = `
                <p class="font-bold text-md text-primary">${goal.title}</p>
                <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-4 mt-2 overflow-hidden">
                    <div class="bg-theme-secondary h-4 rounded-full text-white text-xs flex items-center justify-center transition-all duration-500" style="width: ${progress}%">${Math.round(progress)}%</div>
                </div>
                <p class="text-xs text-secondary mt-2 text-center">${goal.current.toLocaleString('pt-BR')} / ${goal.target.toLocaleString('pt-BR')}</p>
                <p class="text-sm text-secondary mt-3"><i class="fas fa-gift text-theme-accent mr-2"></i><strong>Recompensa:</strong> ${goal.reward}</p>
            `;
        }

        async function acceptChallenge(challengeId) {
            const acceptedChallenges = appState.stats.acceptedChallenges || [];
            if (acceptedChallenges.includes(challengeId)) return;

            const docRef = getUserProfileDocRef();
            if (!docRef) { // Visitor mode
                appState.stats.acceptedChallenges = [...acceptedChallenges, challengeId];
                renderCommunityChallenges();
                alertModal("Desafio Aceito!", "Acompanhe seu progresso aqui na aba Comunidade.", "success");
                return;
            }

            try {
                await runTransaction(db, async (transaction) => {
                    const docSnap = await transaction.get(docRef);
                    if (!docSnap.exists()) throw "Document does not exist!";
                    
                    const data = docSnap.data();
                    const currentStats = data.stats || {};
                    const currentChallenges = currentStats.acceptedChallenges || [];
                    
                    if (!currentChallenges.includes(challengeId)) {
                        const newChallenges = [...currentChallenges, challengeId];
                        transaction.set(docRef, { stats: { ...currentStats, acceptedChallenges: newChallenges } }, { merge: true });
                    }
                });
                alertModal("Desafio Aceito!", "Acompanhe seu progresso aqui na aba Comunidade.", "success");
            } catch (error) {
                console.error("Erro ao aceitar desafio: ", error);
                alertModal("Erro", "Não foi possível aceitar o desafio. Tente novamente.", "error");
            }
        }

        function renderCommunityChallenges() {
            const acceptedChallenges = appState.stats.acceptedChallenges || [];
            elements.communityChallengesContainer.innerHTML = communityData.challenges.map(challenge => {
                const current = challenge.current();
                const progress = Math.min((current / challenge.target) * 100, 100);
                const isAccepted = acceptedChallenges.includes(challenge.id);
                const isCompleted = isAccepted && current >= challenge.target;

                let buttonHTML = '';
                if (isCompleted) {
                    buttonHTML = `<button disabled class="text-xs font-bold text-white bg-theme-secondary py-1 px-3 rounded-full cursor-not-allowed flex items-center gap-1"><i class="fas fa-check"></i>Concluído</button>`;
                } else if (isAccepted) {
                    buttonHTML = `<button disabled class="text-xs font-bold text-white bg-blue-400 dark:bg-blue-600 py-1 px-3 rounded-full cursor-not-allowed">Em Progresso</button>`;
                } else {
                    buttonHTML = `<button data-challenge-id="${challenge.id}" class="accept-challenge-btn text-xs font-bold text-white bg-theme-primary py-1 px-3 rounded-full hover:opacity-80 active:scale-95 transition-all">Aceitar Desafio</button>`;
                }

                return `
                <div class="bg-background p-4 rounded-lg border-l-4 ${isCompleted ? 'border-theme-secondary' : 'border-theme-primary'} ${isCompleted ? 'opacity-70' : ''}">
                    <div class="flex justify-between items-start">
                        <div>
                            <p class="font-bold text-primary">${challenge.title}</p>
                            <p class="text-sm text-secondary mt-1">${challenge.description}</p>
                        </div>
                        <i class="fas ${challenge.icon} text-xl ${isCompleted ? 'text-theme-secondary' : 'text-theme-primary/70'}"></i>
                    </div>
                    <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-2 mt-3">
                        <div class="h-2 rounded-full transition-all duration-500 ${isCompleted ? 'bg-theme-secondary' : 'bg-theme-primary'}" style="width: ${progress}%"></div>
                    </div>
                    <div class="flex justify-between items-center mt-2">
                        <p class="text-xs text-secondary">${current} / ${challenge.target}</p>
                        ${buttonHTML}
                    </div>
                </div>
                `;
            }).join('');
        }

        function renderCommunityEvents() {
            elements.communityEventsContainer.innerHTML = communityData.events.map(event => `
                <div class="flex items-start gap-4">
                    <div class="bg-red-100 dark:bg-red-900/50 text-red-600 dark:text-red-300 font-bold p-2 rounded-lg text-center leading-tight">
                        <span class="text-xs">${new Date(event.date.split('/').reverse().join('-')).toLocaleString('pt-BR', { month: 'short' }).toUpperCase()}</span>
                        <span class="text-lg">${event.date.split('/')[0]}</span>
                    </div>
                    <div>
                         <p class="font-bold text-sm text-primary">${event.title}</p>
                         <p class="text-xs text-secondary mt-1">${event.description}</p>
                    </div>
                </div>
            `).join('<hr class="border-theme my-3">');
        }

        function renderCommunityTopics() {
            if(!elements.communityTopicsContainer) return;

            elements.communityTopicsContainer.innerHTML = communityTopicsData.map(topic => `
                <div data-topic-id="${topic.id}" class="community-topic-item bg-background hover:bg-theme-primary/5 p-4 rounded-lg border border-theme flex items-start gap-4 cursor-pointer transition-colors">
                    <img src="${topic.avatar}" class="w-10 h-10 rounded-full object-cover flex-shrink-0">
                    <div class="flex-grow">
                        <p class="font-bold text-primary">${topic.title}</p>
                        <p class="text-xs text-secondary mt-1">por ${topic.author} • ${topic.time}</p>
                    </div>
                    <div class="flex-shrink-0 flex items-center gap-4 text-secondary text-sm">
                        <span class="flex items-center gap-1.5"><i class="fas fa-heart ${topic.isLiked ? 'text-red-500' : ''}"></i> ${topic.likes}</span>
                        <span class="flex items-center gap-1.5"><i class="fas fa-comment"></i> ${topic.comments.length}</span>
                    </div>
                </div>
            `).join('');
        }
        
        function renderTopicDetail(topicId) {
            const topic = communityTopicsData.find(t => t.id == topicId);
            if (!topic) return;

            elements.topicDetailTitle.textContent = topic.title;
            elements.topicDetailModal.dataset.topicId = topicId;
            
            elements.topicDetailContent.innerHTML = `
                <div class="flex items-center gap-3 pb-4 border-b border-theme">
                    <img src="${topic.avatar}" class="w-10 h-10 rounded-full object-cover">
                    <div>
                        <p class="font-bold text-primary">${topic.author}</p>
                        <p class="text-xs text-secondary">${topic.time}</p>
                    </div>
                </div>
                <p class="text-primary my-4">${topic.content.replace(/\n/g, '<br>')}</p>
                <div class="flex items-center gap-4 text-secondary text-sm pt-3 border-t border-theme">
                     <button class="topic-like-btn flex items-center gap-1.5 hover:text-red-500 transition-colors ${topic.isLiked ? 'text-red-500' : ''}" data-topic-id="${topic.id}">
                        <i class="fas fa-heart"></i> <span id="topic-like-count">${topic.likes}</span>
                    </button>
                    <span class="flex items-center gap-1.5"><i class="fas fa-comment"></i> ${topic.comments.length}</span>
                </div>
            `;
            
            elements.topicCommentsList.innerHTML = topic.comments.map(comment => `
                <li class="p-3 bg-background rounded-lg">
                    <div class="flex items-center gap-2">
                        <p class="font-bold text-primary text-sm">${comment.author}</p>
                    </div>
                    <p class="text-secondary text-sm mt-1">${comment.text}</p>
                </li>
            `).join('') || '<li class="text-center text-sm text-secondary p-4">Nenhum comentário ainda. Seja o primeiro!</li>';
            
            openModal(elements.topicDetailModal);
        }

        function openModal(modal) {
            if (modal) {
                modal.classList.remove('hidden', 'opacity-0');
                modal.querySelector('div').classList.remove('scale-95');
            }
        }

        function alertModal(title, message, type = "info", customButton = null) {
            if(elements.modalTitle) elements.modalTitle.innerHTML = `<i class="fas ${type === 'success' ? 'fa-check-circle text-green-500' : 'fa-info-circle text-blue-500'} mr-2"></i> ${title}`;
            if(elements.modalMessage) elements.modalMessage.innerHTML = message;

            if(elements.modalCustomButtonContainer) {
                elements.modalCustomButtonContainer.innerHTML = '';
                if (customButton) {
                    const btn = document.createElement('button');
                    btn.innerHTML = customButton.text;
                    btn.className = customButton.className;
                    btn.onclick = customButton.onClick;
                    elements.modalCustomButtonContainer.appendChild(btn);
                }
            }

            if(elements.customModal) openModal(elements.customModal);
        }

        function closeModal(modal) {
            if (modal) {
                modal.classList.add('opacity-0');
                modal.querySelector('div:first-child').classList.add('scale-95');
                setTimeout(() => modal.classList.add('hidden'), 200);
            }
        }
        
        function toggleLoadingIndicator(show) {
            let loadingIndicator = document.getElementById('loading-indicator');
            if (show) {
                if (!loadingIndicator) {
                    loadingIndicator = document.createElement('div');
                    loadingIndicator.id = 'loading-indicator';
                    loadingIndicator.className = 'flex items-end gap-3 justify-start mb-4 fade-in-up';
                    loadingIndicator.innerHTML = `
                        <img src="https://placehold.co/32x32/34d399/FFFFFF?text=W" class="w-8 h-8 rounded-full shadow-md flex-shrink-0" alt="Avatar do Wall">
                        <div class="bg-foreground p-3 shadow-lg rounded-2xl rounded-bl-none">
                            <div class="flex items-center space-x-1">
                                <div class="w-2 h-2 bg-gray-400 rounded-full animate-bounce [animation-delay:-0.3s]"></div>
                                <div class="w-2 h-2 bg-gray-400 rounded-full animate-bounce [animation-delay:-0.15s]"></div>
                                <div class="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
                            </div>
                        </div>`;
                    elements.chatWindow.appendChild(loadingIndicator);
                    elements.chatWindow.scrollTop = elements.chatWindow.scrollHeight;
                }
            } else if (loadingIndicator) {
                loadingIndicator.remove();
            }
        }

        function displayMessage(text, sender, sources = []) {
            const messageWrapper = document.createElement('div');
            messageWrapper.className = `flex mb-4 items-end gap-3 fade-in-up`;
            
            if (sender === 'user') {
                messageWrapper.classList.add('justify-end');
            } else {
                messageWrapper.classList.add('justify-start');
                const avatar = document.createElement('img');
                avatar.src = 'https://placehold.co/32x32/34d399/FFFFFF?text=W';
                avatar.className = 'w-8 h-8 rounded-full shadow-md flex-shrink-0';
                avatar.alt = 'Avatar do Wall';
                messageWrapper.appendChild(avatar);
            }

            const messageBubble = document.createElement('div');
            messageBubble.className = `p-3 max-w-[85%] rounded-2xl shadow-lg ${sender === 'user' ? 'bg-theme-primary text-white rounded-br-none' : 'bg-foreground text-primary rounded-bl-none'}`;
            let cleanText = text.replace(/`/g, '').replace(/\n/g, '<br>').replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
            messageBubble.innerHTML = `<p>${cleanText}</p>`;

            if (sender === 'bot' && sources.length > 0) {
                const sourcesDiv = document.createElement('div');
                sourcesDiv.className = 'mt-2 pt-2 border-t border-theme text-xs text-gray-500';
                sourcesDiv.innerHTML = `<strong>Fontes:</strong> ${sources.slice(0, 3).map((src, i) => `<a href="${src.uri}" target="_blank" class="text-blue-500 hover:underline block truncate" title="${src.title}">${i + 1}. ${src.title || src.uri}</a>`).join('')}`;
                messageBubble.appendChild(sourcesDiv);
            }

            messageWrapper.appendChild(messageBubble);
            elements.chatWindow.appendChild(messageWrapper);
            elements.chatWindow.scrollTop = elements.chatWindow.scrollHeight;
        }

        async function callGeminiAPI(userText, systemPrompt, useHistory = true) {
            if (!API_KEY || API_KEY.includes("SUA CHAVE")) {
                return { text: "Desculpe, a função de chat não está configurada. Uma chave de API é necessária." };
            }
            
            const contents = useHistory ? [...chatHistory, { role: "user", parts: [{ text: userText }] }] : [{ role: "user", parts: [{ text: userText }] }];
            if (useHistory) {
                chatHistory.push({ role: "user", parts: [{ text: userText }] });
            }

            const payload = { 
                contents: contents, 
                systemInstruction: { parts: [{ text: systemPrompt }] },
                tools: [{ "google_search": {} }], 
            };

            try {
                const fetchResponse = await fetch(API_URL, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
                if (!fetchResponse.ok) throw new Error(`API error: ${fetchResponse.status}`);
                const response = await fetchResponse.json();

                if (response && response.candidates?.length > 0) {
                    const candidate = response.candidates[0];
                    const botText = candidate.content?.parts?.[0]?.text || 'Desculpe, não consegui processar sua pergunta.';
                    const sources = candidate.groundingMetadata?.groundingAttributions?.map(attr => attr.web).filter(Boolean) || [];
                    
                    if (useHistory) {
                        chatHistory.push({ role: "model", parts: [{ text: botText }] });
                    }
                    return { text: botText, sources: sources };
                } else {
                     return { text: 'Houve um problema ao processar sua solicitação.' };
                }
            } catch (error) {
                console.error("Erro na chamada da API Gemini:", error);
                return { text: `Desculpe, ocorreu um erro ao me conectar. (${error.message})` };
            }
        }

        async function handleChat(userText) {
             const systemPrompt = `Você é o 'Wall', um assistente de IA amigável e especialista em sustentabilidade e reciclagem no Brasil, focado na cidade de Uberlândia, como parte da plataforma Ecociclo. Apresente-se como 'Wall, seu EcoBot' apenas na primeira mensagem da conversa.
            Sua base de conhecimento principal é a seguinte:
            - O usuário atual tem ${appState.points.toLocaleString('pt-BR')} EcoPoints.
            - O app tem uma aba "Mapas" que lista Ecopontos e pontos de coleta de lixo eletrônico em Uberlândia. Sempre que perguntado sobre onde descartar algo, mencione que a lista completa e interativa está na aba "Mapas".
            - Informações específicas de reciclagem:
                - Isopor (EPS): É reciclável, mas deve estar limpo e seco. Nem todas as coletas aceitam. Isopor com restos de comida não é reciclável.
                - Pilhas e Baterias: São lixo tóxico e NUNCA devem ir para o lixo comum ou reciclável. Descarte em pontos de coleta especiais.
                - Óleo de Cozinha: Jamais jogar na pia. Armazene em garrafas PET e leve a um ponto de coleta.
            Para todas as perguntas, use a base de conhecimento acima PRIMEIRO. Se a resposta não estiver lá, use a Pesquisa Google. Seja sempre positivo e conciso.`;
            
            toggleLoadingIndicator(true);
            const response = await callGeminiAPI(userText, systemPrompt, true);
            toggleLoadingIndicator(false);
            displayMessage(response.text, 'bot', response.sources);
        }

        async function generateReuseIdeas(itemName) {
            openModal(elements.reuseIdeasModal);
            elements.reuseModalTitle.innerHTML = `<i class="fas fa-lightbulb text-yellow-400"></i> Ideias para Reutilizar ${itemName}`;
            
            // Skeleton loading state
            elements.reuseModalContent.innerHTML = `
                <div class="animate-pulse space-y-6">
                    ${[...Array(3)].map(() => `
                        <div class="bg-foreground p-5 rounded-xl border border-theme">
                            <div class="h-6 bg-slate-200 dark:bg-slate-700 rounded w-3/4 mb-4"></div>
                            <div class="space-y-2">
                                <div class="h-4 bg-slate-200 dark:bg-slate-700 rounded w-full"></div>
                                <div class="h-4 bg-slate-200 dark:bg-slate-700 rounded w-5/6"></div>
                            </div>
                            <div class="h-5 bg-slate-200 dark:bg-slate-700 rounded w-1/4 my-4"></div>
                            <div class="space-y-2">
                                <div class="h-3 bg-slate-200 dark:bg-slate-700 rounded w-1/2 ml-4"></div>
                                <div class="h-3 bg-slate-200 dark:bg-slate-700 rounded w-1/3 ml-4"></div>
                                <div class="h-3 bg-slate-200 dark:bg-slate-700 rounded w-2/5 ml-4"></div>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;

            const systemPrompt = `Você é um especialista em sustentabilidade e projetos "faça você mesmo" (DIY). Sua missão é inspirar as pessoas a reutilizar materiais. Seja criativo, direto e use uma linguagem amigável.`;
            const userQuery = `Forneça 3 ideias criativas e simples para reutilizar ou reciclar o seguinte item: ${itemName}. 
            Formate a resposta em HTML. Para cada ideia, use EXATAMENTE a seguinte estrutura:
            <div class="bg-foreground p-5 rounded-xl border border-theme">
                <h4 class="text-lg font-bold text-primary flex items-center gap-2">[Emoji] [Título da Ideia]</h4>
                <p class="text-secondary mt-2 mb-4">[Descrição de 2-3 frases]</p>
                <h5 class="font-semibold text-primary mb-2">Você vai precisar de:</h5>
                <ul class="list-disc list-inside text-secondary space-y-1">
                    <li>[Material 1]</li>
                    <li>[Material 2]</li>
                </ul>
            </div>`;
            
            const response = await callGeminiAPI(userQuery, systemPrompt, false);
            
            if (response.text.includes("Desculpe") || response.text.includes("Erro")) {
                 elements.reuseModalContent.innerHTML = `
                    <div class="text-center p-8">
                        <i class="fas fa-exclamation-triangle text-4xl text-red-500"></i>
                        <p class="mt-4 font-semibold text-primary">Oops! Ocorreu um erro.</p>
                        <p class="mt-2 text-secondary">${response.text}</p>
                    </div>
                 `;
            } else {
                elements.reuseModalContent.innerHTML = `<div class="space-y-6">${response.text}</div>`;
            }
        }

        async function generatePostIdea() {
            const btn = elements.generatePostIdeaBtn;
            btn.disabled = true;
            btn.innerHTML = '<i class="fas fa-spinner animate-spin"></i> Gerando...';

            const systemPrompt = "Você é um influenciador digital focado em sustentabilidade. Seu tom é positivo, inspirador e casual.";
            const userQuery = `Crie uma legenda curta e inspiradora para um post em rede social sobre a importância de pequenas ações de reciclagem no dia a dia. Inclua de 2 a 3 hashtags relevantes como #Ecociclo, #Sustentabilidade, #Reciclagem. Não use aspas na resposta.`;

            const response = await callGeminiAPI(userQuery, systemPrompt, false);
            elements.newPostInput.value = response.text;

            btn.disabled = false;
            btn.innerHTML = '✨ Gerar outra ideia';
        }

        const dailyTips = [
            "Sabia que uma torneira pingando pode desperdiçar até 40 litros de água por dia? Verifique sempre se estão bem fechadas!",
            "Desligue aparelhos eletrônicos da tomada quando não estiverem em uso. Mesmo em standby, eles consomem energia.",
            "Use sacolas reutilizáveis para suas compras. Elas ajudam a reduzir drasticamente o lixo plástico.",
            "Separe o lixo orgânico para compostagem. Suas plantas agradecerão e você reduzirá o lixo em aterros.",
            "Prefira produtos com menos embalagens ou com embalagens recicláveis. Pequenas escolhas fazem grande diferença.",
            "Reutilize potes de vidro para guardar alimentos. São ótimos, duráveis e evitam o uso de plástico.",
            "Economize papel! Imprima apenas o necessário e use os dois lados da folha."
        ];
        
        const welcomeHTML = `
            <div id="chat-welcome" class="text-center m-auto p-4">
                <img src="https://placehold.co/80x80/34d399/FFFFFF?text=W" class="w-20 h-20 rounded-full shadow-lg mx-auto" alt="Avatar do Wall">
                <h2 class="text-2xl font-bold text-primary mt-4">Olá! Eu sou o Wall.</h2>
                <p class="text-secondary mt-1">Seu assistente para um mundo mais verde. Como posso ajudar hoje?</p>
                <div class="flex flex-wrap gap-2 justify-center mt-6 text-sm">
                    <button class="prompt-btn bg-background hover:bg-theme-primary/10 text-primary font-medium py-2 px-4 rounded-full transition-all active:scale-95 border border-theme"><i class="fas fa-wallet mr-2 opacity-60"></i>Qual meu saldo?</button>
                    <button class="prompt-btn bg-background hover:bg-theme-primary/10 text-primary font-medium py-2 px-4 rounded-full transition-all active:scale-95 border border-theme"><i class="fas fa-map-marker-alt mr-2 opacity-60"></i>Onde tem ecoponto?</button>
                    <button class="prompt-btn bg-background hover:bg-theme-primary/10 text-primary font-medium py-2 px-4 rounded-full transition-all active:scale-95 border border-theme"><i class="fas fa-question-circle mr-2 opacity-60"></i>Isopor é reciclável?</button>
                </div>
            </div>`;

        function setupScrollAnimations() {
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('is-visible');
                    }
                });
            }, {
                threshold: 0.1
            });

            const elementsToAnimate = document.querySelectorAll('.animate-on-scroll');
            elementsToAnimate.forEach(el => observer.observe(el));
        }

        function setupParallaxEffect() {
            const banner = document.getElementById('welcome-banner');
            if (!banner) return;
            const leaves = banner.querySelectorAll('.parallax-leaf');

            banner.addEventListener('mousemove', (e) => {
                const { clientX, clientY } = e;
                const { left, top, width, height } = banner.getBoundingClientRect();
                
                const centerX = left + width / 2;
                const centerY = top + height / 2;

                const moveX = (clientX - centerX) / (width / 2);
                const moveY = (clientY - centerY) / (height / 2);

                leaves.forEach(leaf => {
                    const speed = leaf.dataset.speed || 1;
                    const offsetX = -moveX * 10 * speed;
                    const offsetY = -moveY * 10 * speed;
                    leaf.style.transform = `translateX(${offsetX}px) translateY(${offsetY}px)`;
                });
            });
        }

        function setupEventListeners() {
            elements.navButtons.forEach(button => button.addEventListener('click', () => switchTab(button.getAttribute('data-tab'))));
            
            if (elements.chatForm) {
                elements.chatForm.addEventListener('submit', async (e) => {
                    e.preventDefault();
                    const welcomeEl = document.getElementById('chat-welcome');
                    if (welcomeEl) {
                        welcomeEl.remove();
                    }
                    const userText = elements.userInput.value.trim();
                    if (!userText) return;
                    elements.userInput.disabled = true;
                    elements.sendButton.disabled = true;
                    displayMessage(userText, 'user');
                    elements.userInput.value = '';
                    await handleChat(userText);
                    elements.userInput.disabled = false;
                    elements.sendButton.disabled = false;
                    elements.userInput.focus();
                });
            }
            
            const handleWelcomePromptClick = (e) => {
                const promptButton = e.target.closest('.prompt-btn');
                if (promptButton) {
                    const promptText = promptButton.textContent.trim();
                    elements.userInput.value = promptText;
                    elements.chatForm.dispatchEvent(new Event('submit', { bubbles: true, cancelable: true }));
                }
            };
            if (elements.chatWindow) {
                elements.chatWindow.addEventListener('click', handleWelcomePromptClick);
            }

            if (elements.clearChatBtn) {
                elements.clearChatBtn.addEventListener('click', () => {
                    if (elements.chatWindow) {
                        elements.chatWindow.innerHTML = welcomeHTML;
                    }
                    chatHistory = [];
                });
            }

            if (elements.rewardsContainer) {
                elements.rewardsContainer.addEventListener('click', (e) => {
                    const redeemButton = e.target.closest('.redeem-btn');
                    if (redeemButton && !redeemButton.disabled) {
                        const rewardId = parseInt(redeemButton.dataset.rewardId);
                        const reward = availableRewards.find(r => r.id === rewardId);
                        if (reward) {
                            redeemVoucher(reward);
                        }
                    }
                });
            }

            if (elements.featuredRewardContainer) {
                elements.featuredRewardContainer.addEventListener('click', (e) => {
                    const redeemButton = e.target.closest('.redeem-btn');
                    if (redeemButton && !redeemButton.disabled) {
                        const rewardId = parseInt(redeemButton.dataset.rewardId);
                        const reward = availableRewards.find(r => r.id === rewardId);
                        if (reward) {
                            redeemVoucher(reward);
                        }
                    }
                });
            }

            if (elements.voucherList) {
                elements.voucherList.addEventListener('click', e => {
                    const copyButton = e.target.closest('.copy-code-btn');
                    const statusButton = e.target.closest('.mark-status-btn');
                    
                    if (copyButton) {
                        const code = copyButton.dataset.code;
                        const textarea = document.createElement('textarea');
                        textarea.value = code;
                        document.body.appendChild(textarea);
                        textarea.select();
                        try {
                           document.execCommand('copy');
                           alertModal("Copiado!", `O código <strong>${code}</strong> foi copiado para a sua área de transferência.`, "success");
                        } catch (err) {
                           console.error('Copy failed', err);
                           alertModal("Erro", "Não foi possível copiar o código.", "error");
                        }
                        document.body.removeChild(textarea);
                    }

                     if (statusButton) {
                        const voucherId = statusButton.dataset.voucherId;
                        const currentStatus = statusButton.dataset.currentStatus;
                        const newStatus = currentStatus === 'active' ? 'used' : 'active';
                        updateVoucherStatus(voucherId, newStatus);
                    }
                });
            }
            
            if (elements.toggleUsedVouchers) {
                elements.toggleUsedVouchers.addEventListener('change', (e) => {
                    const isChecked = e.target.checked;
                    document.querySelectorAll('.voucher-card').forEach(card => {
                        if (card.dataset.status === 'used') {
                            card.style.display = isChecked ? 'list-item' : 'none';
                        }
                    });
                });
            }

            if (elements.simulateRecycleButton) {
                elements.simulateRecycleButton.addEventListener('click', () => { 
                    openModal(elements.recycleModal);
                });
            }
            if (elements.closeRecycleModal) {
                elements.closeRecycleModal.addEventListener('click', () => closeModal(elements.recycleModal));
            }
            if (elements.recycleOptions) {
                elements.recycleOptions.addEventListener('click', (e) => {
                    const button = e.target.closest('.recycle-option-btn');
                    if (button) { addPoints(parseInt(button.dataset.points), button.dataset.reason); closeModal(elements.recycleModal); }
                });
            }

            if (elements.openPointsTableBtn) {
                elements.openPointsTableBtn.addEventListener('click', () => { 
                    openModal(elements.pointsTableModal);
                });
            }
            if (elements.closePointsTableBtn) {
                elements.closePointsTableBtn.addEventListener('click', () => closeModal(elements.pointsTableModal));
            }
            
            if (elements.openRewardsQuickBtn) {
                elements.openRewardsQuickBtn.addEventListener('click', () => {
                    switchTab('rewards-tab');
                });
            }

            if (elements.expandHistoryBtn) {
                elements.expandHistoryBtn.addEventListener('click', () => {
                    elements.activityHistoryContainer.classList.toggle('expanded');
                    const icon = elements.expandHistoryBtn.querySelector('i');
                    const text = elements.expandHistoryBtn.querySelector('span');
                    if (elements.activityHistoryContainer.classList.contains('expanded')) {
                        text.textContent = 'Recolher';
                    } else {
                        text.textContent = 'Ver Tudo';
                    }
                });
            }

            if (elements.closeModalButton) {
                elements.closeModalButton.onclick = () => closeModal(elements.customModal);
            }
            
            if (elements.editNameButton) {
                elements.editNameButton.addEventListener('click', () => {
                    elements.newNameInput.value = appState.userName;
                    openModal(elements.editNameModal);
                });
            }
            if (elements.closeNameModal) {
                elements.closeNameModal.addEventListener('click', () => closeModal(elements.editNameModal));
            }
            if (elements.saveNameButton) {
                elements.saveNameButton.addEventListener('click', () => {
                    const newName = elements.newNameInput.value.trim();
                    if (newName && newName !== appState.userName) {
                        appState.userName = newName;
                        localStorage.setItem(`ecociclo-username-${appState.userId}`, newName);
                        updateUserUI();
                    }
                    closeModal(elements.editNameModal);
                });
            }

            if (elements.themeToggle) {
                elements.themeToggle.addEventListener('click', () => {
                    const isDarkMode = document.documentElement.classList.contains('dark');
                    setTheme(isDarkMode ? 'light' : 'dark');
                });
            }
            
            elements.themeButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const theme = button.dataset.theme;
                    setTheme(theme);
                });
            });

            if (elements.logoutButton) {
                elements.logoutButton.addEventListener('click', () => {
                    openModal(elements.logoutConfirmModal);
                });
            }

            if (elements.cancelLogoutBtn) {
                elements.cancelLogoutBtn.addEventListener('click', () => {
                    closeModal(elements.logoutConfirmModal);
                });
            }

            if (elements.confirmLogoutBtn) {
                elements.confirmLogoutBtn.addEventListener('click', () => {
                    window.location.reload(); 
                });
            }

            // Map event listeners
            document.querySelectorAll('.map-filter-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    document.querySelectorAll('.map-filter-btn').forEach(b => b.classList.remove('bg-theme-primary', 'text-white', 'dark:bg-blue-500'));
                    e.currentTarget.classList.add('bg-theme-primary', 'text-white', 'dark:bg-blue-500');
                    updateMapAndList(e.currentTarget.dataset.filter);
                });
            });

            const mapSearchInput = document.getElementById('map-search-input');
            if (mapSearchInput) {
                mapSearchInput.addEventListener('input', () => {
                     const currentFilter = document.querySelector('.map-filter-btn.bg-theme-primary').dataset.filter;
                     updateMapAndList(currentFilter);
                });
            }
            
            const findUserLocationBtn = document.getElementById('find-user-location-btn');
            if (findUserLocationBtn) {
                findUserLocationBtn.addEventListener('click', findUserLocation);
            }

            const rewardsFilters = document.getElementById('rewards-filters');
            if(rewardsFilters) {
                rewardsFilters.addEventListener('click', e => {
                    const button = e.target.closest('.reward-filter-btn');
                    if (button) {
                        document.querySelectorAll('.reward-filter-btn').forEach(btn => btn.classList.remove('active'));
                        button.classList.add('active');
                        renderRewards(button.dataset.filter);
                    }
                });
            }

            // Community Tab Listeners
            elements.communityMainTabs.forEach(button => {
                button.addEventListener('click', (e) => {
                    elements.communityMainTabs.forEach(btn => {
                        btn.classList.remove('border-theme-primary', 'text-theme-primary');
                        btn.classList.add('border-transparent', 'text-secondary');
                    });
                    e.currentTarget.classList.add('border-theme-primary', 'text-theme-primary');
                    e.currentTarget.classList.remove('border-transparent', 'text-secondary');
                    
                    document.querySelectorAll('.community-sub-tab-content').forEach(content => content.classList.remove('active'));
                    document.getElementById(e.currentTarget.dataset.tab).classList.add('active');
                });
            });

            elements.friendsSubTabs.forEach(button => {
                button.addEventListener('click', (e) => {
                    elements.friendsSubTabs.forEach(btn => {
                        btn.classList.remove('bg-theme-primary', 'text-white');
                         btn.classList.add('text-primary');
                    });
                    e.currentTarget.classList.add('bg-theme-primary', 'text-white');
                    e.currentTarget.classList.remove('text-primary');
                    
                    document.querySelectorAll('.friends-sub-tab-content').forEach(content => content.classList.remove('active'));
                    document.getElementById(e.currentTarget.dataset.tab).classList.add('active');
                });
            });
            
            if (elements.friendsFeedContainer) {
                elements.friendsFeedContainer.addEventListener('click', e => {
                    const likeBtn = e.target.closest('.like-btn');
                    const commentBtn = e.target.closest('.comment-btn');
                    
                    if (likeBtn) {
                        const postId = likeBtn.closest('[data-post-id]').dataset.postId;
                        const post = friendsData.feed.find(p => p.id == postId);
                        if (post) {
                            post.isLiked = !post.isLiked;
                            post.likes += post.isLiked ? 1 : -1;
                            renderFriendsFeed();
                        }
                    }

                    if(commentBtn) {
                        const postId = commentBtn.closest('[data-post-id]').dataset.postId;
                        const post = friendsData.feed.find(p => p.id == postId);
                        if (post) {
                            elements.commentsList.innerHTML = post.comments.map(c => `<li class="text-sm text-secondary border-b border-theme last:border-b-0 py-2">${c.text} <span class="text-xs text-gray-400">- ${c.author}</span></li>`).join('') || '<li class="text-sm text-secondary text-center py-4">Nenhum comentário ainda.</li>';
                            elements.commentForm.dataset.postId = postId;
                            openModal(elements.commentsModal);
                        }
                    }
                });
            }

            if (elements.addPostBtn) {
                elements.addPostBtn.addEventListener('click', () => {
                    const postText = elements.newPostInput.value.trim();
                    if (postText) {
                        friendsData.feed.unshift({
                            id: Date.now(),
                            type: 'custom',
                            friend: appState.userName,
                            avatar: `https://placehold.co/40x40/10b981/FFFFFF?text=${appState.userName.charAt(0)}`,
                            details: postText,
                            time: 'Agora',
                            likes: 0,
                            comments: [],
                            isLiked: false
                        });
                        renderFriendsFeed();
                        elements.newPostInput.value = '';
                    }
                });
            }
            if(elements.generatePostIdeaBtn) {
                elements.generatePostIdeaBtn.addEventListener('click', generatePostIdea);
            }

            if (elements.commentForm) {
                elements.commentForm.addEventListener('submit', e => {
                    e.preventDefault();
                    const postId = e.target.dataset.postId;
                    const commentText = elements.commentInput.value.trim();
                    if (commentText && postId) {
                        const post = friendsData.feed.find(p => p.id == postId);
                        if (post) {
                            post.comments.push({ author: appState.userName, text: commentText });
                            renderFriendsFeed();
                            closeModal(elements.commentsModal);
                            elements.commentInput.value = '';
                        }
                    }
                });
            }
            if (elements.closeCommentsModal) {
                elements.closeCommentsModal.onclick = () => closeModal(elements.commentsModal);
            }
            
            const communityFriendsContent = document.getElementById('community-friends-content');
            if (communityFriendsContent) {
                communityFriendsContent.addEventListener('click', e => {
                    if(e.target.closest('#create-goal-btn')) {
                        openModal(elements.createGoalModal);
                    }
                    if(e.target.closest('#create-challenge-btn')) {
                        openModal(elements.createChallengeModal);
                    }
                });
            }
            if (elements.closeGoalModal) {
                elements.closeGoalModal.onclick = () => closeModal(elements.createGoalModal);
            }
            if (elements.closeChallengeModal) {
                elements.closeChallengeModal.onclick = () => closeModal(elements.createChallengeModal);
            }
            if (elements.closeReuseModal) {
                 elements.closeReuseModal.onclick = () => closeModal(elements.reuseIdeasModal);
            }

            if (elements.closeTreeAnimationModal) {
                elements.closeTreeAnimationModal.addEventListener('click', () => {
                    closeModal(elements.treeAnimationModal);
                });
            }

            if (elements.createGoalForm) {
                elements.createGoalForm.addEventListener('submit', e => {
                    e.preventDefault();
                    const title = elements.goalTitleInput.value.trim();
                    const target = parseInt(elements.goalTargetInput.value);
                    if (title && target > 0) {
                        friendsData.goals.push({ id: Date.now(), title, current: 0, target, personal: true });
                        renderFriendsGoals();
                        closeModal(elements.createGoalModal);
                        e.target.reset();
                    }
                });
            }

            if (elements.createChallengeForm) {
                elements.createChallengeForm.addEventListener('submit', e => {
                    e.preventDefault();
                    const title = elements.challengeTitleInput.value.trim();
                    const description = elements.challengeDescriptionInput.value.trim();
                    const friend = elements.challengeFriendSelect.value;
                    if(title && description && friend) {
                        friendsData.challenges.push({ id: Date.now(), title, description, participants: ['Você', friend], yourScore: 0, friendScore: 0 });
                        renderFriendsChallenges();
                        closeModal(elements.createChallengeModal);
                        e.target.reset();
                    }
                });
            }
            
            const leaderboardFilters = document.getElementById('leaderboard-filters');
            if (leaderboardFilters) {
                leaderboardFilters.addEventListener('click', e => {
                    const button = e.target.closest('.leaderboard-filter-btn');
                    if (button) {
                        document.querySelectorAll('.leaderboard-filter-btn').forEach(btn => {
                            btn.classList.remove('bg-theme-primary', 'text-white');
                            btn.classList.add('bg-background', 'text-primary');
                        });
                        button.classList.add('bg-theme-primary', 'text-white');
                        button.classList.remove('bg-background', 'text-primary');
                        renderLeaderboard(button.dataset.filter);
                    }
                });
            }

            if (elements.communityChallengesContainer) {
                elements.communityChallengesContainer.addEventListener('click', e => {
                    const button = e.target.closest('.accept-challenge-btn');
                    if (button) {
                        const challengeId = button.dataset.challengeId;
                        acceptChallenge(challengeId);
                    }
                });
            }

            // --- NEW: Topics Event Listeners ---
            if (elements.communityTopicsContainer) {
                elements.communityTopicsContainer.addEventListener('click', e => {
                    const topicItem = e.target.closest('.community-topic-item');
                    if (topicItem) {
                        renderTopicDetail(topicItem.dataset.topicId);
                    }
                });
            }

            if (elements.createNewTopicBtn) {
                elements.createNewTopicBtn.addEventListener('click', () => {
                    openModal(elements.createTopicModal);
                });
            }

            if (elements.closeCreateTopicModal) {
                elements.closeCreateTopicModal.onclick = () => closeModal(elements.createTopicModal);
            }
            
            if (elements.closeTopicDetailModal) {
                elements.closeTopicDetailModal.onclick = () => closeModal(elements.topicDetailModal);
            }

            if (elements.createTopicForm) {
                elements.createTopicForm.addEventListener('submit', e => {
                    e.preventDefault();
                    const title = elements.newTopicTitleInput.value.trim();
                    const content = elements.newTopicContentInput.value.trim();
                    if (title && content) {
                        const newTopic = {
                            id: Date.now(),
                            title: title,
                            content: content,
                            author: appState.userName,
                            avatar: `https://placehold.co/40x40/10b981/FFFFFF?text=${appState.userName.charAt(0)}`,
                            time: 'Agora',
                            likes: 0,
                            isLiked: false,
                            comments: []
                        };
                        communityTopicsData.unshift(newTopic);
                        renderCommunityTopics();
                        closeModal(elements.createTopicModal);
                        elements.createTopicForm.reset();
                    }
                });
            }

            if (elements.addCommentForm) {
                elements.addCommentForm.addEventListener('submit', e => {
                    e.preventDefault();
                    const topicId = elements.topicDetailModal.dataset.topicId;
                    const commentText = elements.addCommentInput.value.trim();
                    if (commentText && topicId) {
                        const topic = communityTopicsData.find(t => t.id == topicId);
                        if (topic) {
                            topic.comments.push({ author: appState.userName, text: commentText });
                            renderCommunityTopics(); // To update the comment count on the main list
                            renderTopicDetail(topicId); // To refresh the modal view
                            elements.addCommentInput.value = '';
                        }
                    }
                });
            }
            
             if (elements.topicDetailModal) {
                elements.topicDetailModal.addEventListener('click', e => {
                    const likeBtn = e.target.closest('.topic-like-btn');
                    if (likeBtn) {
                        const topicId = likeBtn.dataset.topicId;
                        const topic = communityTopicsData.find(t => t.id == topicId);
                        if(topic) {
                            topic.isLiked = !topic.isLiked;
                            topic.likes += topic.isLiked ? 1 : -1;
                            renderCommunityTopics(); // Update main list
                            renderTopicDetail(topicId); // Update modal
                        }
                    }
                });
            }

            if (elements.tipFab) {
                elements.tipFab.addEventListener('click', () => {
                    const today = new Date().getDay();
                    elements.tipModalContent.textContent = dailyTips[today];
                    openModal(elements.tipModal);
                });
            }

            if (elements.closeTipModal) {
                elements.closeTipModal.addEventListener('click', () => {
                    closeModal(elements.tipModal);
                });
            }
            
            if (elements.openDisposalGuideBtn) {
                elements.openDisposalGuideBtn.addEventListener('click', () => {
                    openModal(elements.disposalGuideModal);
                });
            }

            if (elements.closeDisposalGuideBtn) {
                elements.closeDisposalGuideBtn.addEventListener('click', () => {
                    closeModal(elements.disposalGuideModal);
                });
            }
            
            setupScrollAnimations();
            setupParallaxEffect();
        }
        
        const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
        function handleSystemThemeChange(e) { if (localStorage.getItem('ecociclo-theme') === 'system') { applyTheme(e.matches ? 'dark' : 'light'); } }
        function applyTheme(theme) {
            document.documentElement.classList.remove('light', 'dark');
            document.documentElement.classList.add(theme);
            if(elements.themeToggle) {
                updateThemeIcon(theme === 'dark');
            }
        }
        function setTheme(theme) {
            localStorage.setItem('ecociclo-theme', theme);
            mediaQuery.removeEventListener('change', handleSystemThemeChange);
            if (theme === 'system') {
                applyTheme(mediaQuery.matches ? 'dark' : 'light');
                mediaQuery.addEventListener('change', handleSystemThemeChange);
            } else { applyTheme(theme); }
            updateThemeButtons(theme);
        }
        function updateThemeButtons(activeTheme) {
            elements.themeButtons.forEach(btn => {
                btn.classList.toggle('bg-theme-primary', btn.dataset.theme === activeTheme);
                btn.classList.toggle('text-white', btn.dataset.theme === activeTheme);
                btn.classList.toggle('bg-background', btn.dataset.theme !== activeTheme);
                btn.classList.toggle('text-primary', btn.dataset.theme !== activeTheme);
            });
        }
        function updateThemeIcon(isDarkMode) { if(elements.themeToggle) elements.themeToggle.querySelector('i').className = isDarkMode ? 'fas fa-sun' : 'fas fa-moon'; }
        
        // --- MAP FUNCTIONS ---
        
        function getMapsLink(lat, lng, label) {
            // Universal link that works on web and prompts to open in the native maps app on mobile.
            // This is more reliable than trying to detect OS and use specific URI schemes.
            return `https://www.google.com/maps/dir/?api=1&destination=${lat},${lng}`;
        }
        
        const locations = [
            // ECOPONTOS
            { id: 1, name: 'Ecoponto Daniel Fonseca', address: 'Rua Itabira, 1720', type: 'ecoponto', coords: [-18.9135, -48.2678] },
            { id: 2, name: 'Ecoponto Guarani', address: 'Rua do Repentista, 350', type: 'ecoponto', coords: [-18.8893, -48.2435] },
            { id: 3, name: 'Ecoponto Jd. Canaã', address: 'Av. Palestina – esquina com a Rua Menfins e Biblios', type: 'ecoponto', coords: [-18.9485, -48.2407] },
            { id: 4, name: 'Ecoponto Lagoinha', address: 'Alameda Arnolde de Almeida Castro, 172', type: 'ecoponto', coords: [-18.8950, -48.2936] },
            { id: 5, name: 'Ecoponto Luizote de Freitas', address: 'Rua Wilson Gonçalves de Souza, 10, esquina com Rua Paulo Margonari', type: 'ecoponto', coords: [-18.9248, -48.3305] },
            { id: 6, name: 'Ecoponto Mansour', address: 'Rua Rio Corumbá, 20 – esquina com a Avenida Rio Nilo', type: 'ecoponto', coords: [-18.9388, -48.3188] },
            { id: 7, name: 'Ecoponto Monte Hebron', address: 'Av. Benedita Fagundes da Costa, 12 – esquina com a avenida Guimara Alves Oliveira', type: 'ecoponto', coords: [-18.8573, -48.3241] },
            { id: 8, name: 'Ecoponto Morumbi', address: 'Rua campeio, 247 esquina com a avenida José Maria Ribeiro', type: 'ecoponto', coords: [-18.9208, -48.2045] },
            { id: 9, name: 'Ecoponto Pequis', address: 'Rua do Tapiti, 250', type: 'ecoponto', coords: [-18.8778, -48.3496] },
            { id: 10, name: 'Ecoponto Roosevelt', address: 'Rua Olívia de Freitas Guimarães, 950', type: 'ecoponto', coords: [-18.8919, -48.2721] },
            { id: 11, name: 'Ecoponto Santa Rosa', address: 'Rua Ângela Alckmin, 211 – esquina com Rua Elis Regina', type: 'ecoponto', coords: [-18.8724, -48.2505] },
            { id: 12, name: 'Ecoponto São Jorge', address: 'Avenida Serra do Mar, 411 – esquina com Avenida Serra do Espinhaço', type: 'ecoponto', coords: [-18.9567, -48.2163] },
            { id: 17, name: 'Ecoponto São Lucas', address: 'Rua do Cientista – esquina com Rua do Gari', type: 'ecoponto', coords: [-18.9427, -48.2275] },
            { id: 18, name: 'Ecoponto Segismundo Pereira', address: 'Sebastião Alves Nunes, 49 – esquina com a rua Dr. Laerte V. Gonçalves', type: 'ecoponto', coords: [-18.9037, -48.2325] },
            { id: 19, name: 'Ecoponto Shopping Park', address: 'Av. Sul Americana, 627', type: 'ecoponto', coords: [-18.9545, -48.3039] },
            { id: 20, name: 'Ecoponto Tocantins', address: 'Rua Docelino de Freitas Costa – esquina com a rua Bernadete Silva Arantes, ao lado do Condomínio Morada do Sol', type: 'ecoponto', coords: [-18.9599, -48.2612] },
            
            // ELETRÔNICOS
            { id: 14, name: 'DMAE', address: 'Av. Rondon Pacheco, 6400 – Tibery', type: 'eletronico', coords: [-18.9088, -48.2423] },
            { id: 15, name: 'Casas Bahia', address: 'Av. Afonso Pena, 217 – Centro', type: 'eletronico', coords: [-18.9189, -48.2783] },
            { id: 16, name: 'Magazine Luiza', address: 'Center Shopping – Av. João Naves de Ávila, 1331 – Tibery', type: 'eletronico', coords: [-18.9090, -48.2590] },
            { id: 21, name: 'Americanas', address: 'Praça Tubal Vilela, 252 – Centro', type: 'eletronico', coords: [-18.9175, -48.2786] },
            { id: 22, name: 'Fast Shop', address: 'Center Shopping - Av. João Naves de Ávila, 1331 – Tibery', type: 'eletronico', coords: [-18.9088, -48.2592] },
            { id: 23, name: 'Casas Bahia', address: 'Av. Afonso Pena, 526 – Centro', type: 'eletronico', coords: [-18.9174, -48.2771] },
            { id: 24, name: 'Uberlândia Shopping', address: 'Av. Paulo Gracindo, 15 – Morada da Colina', type: 'eletronico', coords: [-18.9385, -48.2526] },
            { id: 25, name: 'Carrefour', address: 'Center Shopping – Av. João Naves de Ávila, 1441 – Santa Mônica', type: 'eletronico', coords: [-18.9086, -48.2584] },
            { id: 26, name: 'UFU - Campus Umuarama', address: 'Rua Ceará – Umuarama', type: 'eletronico', coords: [-18.8951, -48.2575] },
        ];
        
        function initMap() {
            if (map) return;
            map = L.map('map').setView([-18.9186, -48.2772], 12); // Center of Uberlândia
            L.tileLayer('https://{s}.google.com/vt/lyrs=m&x={x}&y={y}&z={z}', {
                maxZoom: 20,
                subdomains:['mt0','mt1','mt2','mt3'],
                attribution: '&copy; <a href="https://www.google.com/maps">Google Maps</a>'
            }).addTo(map);
            markersGroup = L.layerGroup().addTo(map);
            updateMapAndList('all');
        }

        function updateMapAndList(filter = 'all') {
            if (!map) return; // Add guard clause in case map is not initialized
            markersGroup.clearLayers();
            locationMarkers = {};
            const locationList = document.getElementById('location-list');
            locationList.innerHTML = '';
            const searchTerm = document.getElementById('map-search-input').value.toLowerCase();

            const createIcon = (color, size = 24) => L.divIcon({
                html: `<i class="fas fa-map-marker-alt" style="color: ${color}; font-size: ${size}px; text-shadow: 0 1px 3px rgba(0,0,0,0.4);"></i>`,
                className: 'bg-transparent border-none',
                iconSize: [size, size],
                iconAnchor: [size / 2, size],
                popupAnchor: [0, -size]
            });

            const filteredLocations = locations.filter(loc => {
                const typeMatch = filter === 'all' || loc.type === filter;
                const searchMatch = !searchTerm || loc.name.toLowerCase().includes(searchTerm) || loc.address.toLowerCase().includes(searchTerm);
                return typeMatch && searchMatch;
            });
            
            if (filteredLocations.length === 0) {
                locationList.innerHTML = `<p class="p-4 text-center text-secondary">Nenhum local encontrado.</p>`;
                return;
            }
            
            const highlightedIcon = createIcon('var(--color-secondary)', 40);

            filteredLocations.forEach(loc => {
                const iconColor = loc.type === 'ecoponto' ? 'var(--color-primary)' : 'var(--color-accent)';
                const markerIcon = createIcon(iconColor, 28);

                const marker = L.marker(loc.coords, {icon: markerIcon, locationId: loc.id}).addTo(markersGroup);
                locationMarkers[loc.id] = marker;

                const mapsLink = getMapsLink(loc.coords[0], loc.coords[1], loc.name);
                const popupContent = `
                    <div class="text-base">
                        <strong class="text-primary">${loc.name}</strong>
                        <p class="text-secondary text-sm">${loc.address}</p>
                        ${loc.type === 'ecoponto' ? '<p class="text-xs text-gray-500 mt-1">Horário: 7h às 19h</p>' : ''}
                        <a href="${mapsLink}" target="_blank" class="text-blue-500 font-bold mt-2 inline-block">Ver Rotas &rarr;</a>
                    </div>`;
                marker.bindPopup(popupContent);

                const listItem = document.createElement('div');
                listItem.className = 'bg-background p-4 rounded-lg border border-theme cursor-pointer hover:shadow-lg hover:border-theme-primary transition-all';
                listItem.dataset.locationId = loc.id;
                
                const distance = userLatLng ? (L.latLng(userLatLng).distanceTo(loc.coords) / 1000) : null;

                listItem.innerHTML = `
                    <p class="font-bold text-primary">${loc.name}</p>
                    <p class="text-xs text-secondary mt-1">${loc.address}</p>
                    ${distance ? `<div class="text-xs text-blue-500 font-semibold mt-2"><i class="fas fa-road mr-1"></i> ${distance.toFixed(1)} km de você</div>` : ''}
                `;
                
                listItem.addEventListener('click', (e) => {
                    const clickedId = e.currentTarget.dataset.locationId;
                    const selectedLocation = locations.find(l => l.id == clickedId);
                    const selectedMarker = locationMarkers[clickedId];
                    if (selectedLocation && selectedMarker) {
                        map.flyTo(selectedLocation.coords, 15);
                        selectedMarker.openPopup();
                    }
                });

                listItem.addEventListener('mouseenter', (e) => {
                    const hoveredId = e.currentTarget.dataset.locationId;
                    const hoveredMarker = locationMarkers[hoveredId];
                    if (hoveredMarker) {
                        hoveredMarker.setIcon(highlightedIcon);
                        hoveredMarker.setZIndexOffset(1000);
                    }
                });
                listItem.addEventListener('mouseleave', (e) => {
                     const hoveredId = e.currentTarget.dataset.locationId;
                    const hoveredMarker = locationMarkers[hoveredId];
                    const originalLocation = locations.find(l => l.id == hoveredId);
                    if (hoveredMarker && originalLocation) {
                        const originalIconColor = originalLocation.type === 'ecoponto' ? 'var(--color-primary)' : 'var(--color-accent)';
                        hoveredMarker.setIcon(createIcon(originalIconColor, 28));
                        hoveredMarker.setZIndexOffset(0);
                    }
                });

                locationList.appendChild(listItem);
            });
        }
        
        function findUserLocation() {
            const findBtn = document.getElementById('find-user-location-btn');
            const originalText = findBtn.innerHTML;
            findBtn.innerHTML = `<i class="fas fa-spinner animate-spin"></i> Procurando...`;
            findBtn.disabled = true;

            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(position => {
                    const lat = position.coords.latitude;
                    const lng = position.coords.longitude;
                    userLatLng = [lat, lng];
                    
                    if (userMarker) {
                        map.removeLayer(userMarker);
                    }
                    
                    const userIcon = L.divIcon({
                        html: '<i class="fas fa-circle-user text-blue-500 text-3xl"></i>',
                        className: 'bg-transparent border-none',
                        iconSize: [30, 30],
                        iconAnchor: [15, 15]
                    });
                    
                    userMarker = L.marker(userLatLng, {icon: userIcon}).addTo(map);
                    userMarker.bindPopup("Você está aqui!").openPopup();
                    map.flyTo(userLatLng, 14);

                    updateMapAndList(document.querySelector('.map-filter-btn.bg-theme-primary').dataset.filter);
                    
                    findBtn.innerHTML = originalText;
                    findBtn.disabled = false;

                }, () => {
                    alertModal("Erro de Localização", "Não foi possível obter sua localização. Verifique as permissões do seu navegador.", "error");
                    findBtn.innerHTML = originalText;
                    findBtn.disabled = false;
                });
            } else {
                 alertModal("Incompatível", "Seu navegador não suporta geolocalização.", "warning");
                 findBtn.innerHTML = originalText;
                 findBtn.disabled = false;
            }
        }

        async function enterApp(isGuest, userData = {}) {
            const authScreen = elements.loginScreen.classList.contains('hidden') ? elements.registerScreen : elements.loginScreen;

            // Inicia a preparação do app em segundo plano
            const appSetupPromise = (async () => {
                await initializeFirebase(isGuest, userData);
                setupEventListeners();
                switchTab('home-tab');
            })();

            // Inicia a animação de fade-out da tela de autenticação
            authScreen.classList.add('opacity-0');

            // Após a animação, esconde a tela de autenticação e mostra o app
            setTimeout(async () => {
                authScreen.classList.add('hidden');

                // Garante que a preparação do app terminou antes de exibi-lo
                // Isso evita uma tela em branco caso o setup demore mais que a animação.
                await appSetupPromise;

                elements.appContainer.classList.remove('hidden');
                elements.appContainer.classList.add('fade-in');
            }, 300); // Corresponde à duração da transição (alterada para 300ms)
        }

        window.onload = () => {
            Object.assign(elements, {
                splashScreen: document.getElementById('splash-screen'),
                appContainer: document.getElementById('app-container'),
                loginScreen: document.getElementById('login-screen'),
                registerScreen: document.getElementById('register-screen'),
                loginForm: document.getElementById('login-form'),
                guestLoginButton: document.getElementById('guest-login-button'),
                goToRegisterButton: document.getElementById('go-to-register-button'),
                registerForm: document.getElementById('register-form'),
                goToLoginButton: document.getElementById('go-to-login-button'),
                loginError: document.getElementById('login-error'),
                themeToggle: document.getElementById('theme-toggle'),
                themeButtons: document.querySelectorAll('.theme-btn'),
                userAvatar: document.querySelector('.user-avatar-img'), // More generic selector
                editNameButton: document.getElementById('edit-name-button'),
                editNameModal: document.getElementById('edit-name-modal'),
                closeNameModal: document.getElementById('close-name-modal'),
                saveNameButton: document.getElementById('save-name-button'),
                newNameInput: document.getElementById('new-name-input'),
                logoutButton: document.getElementById('logout-button'),
                logoutConfirmModal: document.getElementById('logout-confirm-modal'),
                confirmLogoutBtn: document.getElementById('confirm-logout-btn'),
                cancelLogoutBtn: document.getElementById('cancel-logout-btn'),
                chatWindow: document.getElementById('chat-window'),
                chatForm: document.getElementById('chat-form'),
                userInput: document.getElementById('user-input'),
                sendButton: document.getElementById('send-button'),
                errorMessage: document.getElementById('error-message'),
                navButtons: document.querySelectorAll('.nav-button'),
                pointsDisplays: document.querySelectorAll('[data-bind-points]'),
                voucherList: document.getElementById('voucher-list'),
                rewardsContainer: document.getElementById('rewards-container'),
                customModal: document.getElementById('custom-modal'),
                modalTitle: document.getElementById('modal-title'),
                modalMessage: document.getElementById('modal-message'),
                modalCustomButtonContainer: document.getElementById('modal-custom-button-container'),
                closeModalButton: document.getElementById('close-modal'),
                userLevelDisplays: document.querySelectorAll('[data-bind-level]'),
                levelProgress: document.getElementById('level-progress'),
                levelProgressText: document.getElementById('level-progress-text'),
                homeLevelProgress: document.getElementById('home-level-progress'),
                homeLevelProgressText: document.getElementById('home-level-progress-text'),
                accountStatItems: document.getElementById('account-stat-items'),
                accountStatPoints: document.getElementById('account-stat-points'),
                accountStatVouchers: document.getElementById('account-stat-vouchers'),
                achievementsContainer: document.getElementById('achievements-container'),
                simulateRecycleButton: document.getElementById('simulate-recycle-button'),
                recycleModal: document.getElementById('recycle-modal'),
                closeRecycleModal: document.getElementById('close-recycle-modal'),
                recycleOptions: document.getElementById('recycle-options'),
                pointsTableModal: document.getElementById('points-table-modal'),
                openPointsTableBtn: document.getElementById('open-points-table-btn'),
                openRewardsQuickBtn: document.getElementById('open-rewards-quick-btn'),
                closePointsTableBtn: document.getElementById('close-points-table-btn'),
                rewardsFilters: document.getElementById('rewards-filters'),
                leaderboardList: document.getElementById('leaderboard-list'),
                userRankHighlight: document.getElementById('user-rank-highlight'),
                communityGoalContainer: document.getElementById('community-goal-container'),
                communityChallengesContainer: document.getElementById('community-challenges-container'),
                communityEventsContainer: document.getElementById('community-events-container'),
                clearChatBtn: document.getElementById('clear-chat-btn'),
                activityFeed: document.getElementById('activity-feed'),
                homeActivityFeed: document.getElementById('home-activity-feed'),
                activityHistoryContainer: document.getElementById('activity-history-container'),
                expandHistoryBtn: document.getElementById('expand-history-btn'),
                mapSearchInput: document.getElementById('map-search-input'),
                tipFab: document.getElementById('tip-fab'),
                tipModal: document.getElementById('tip-modal'),
                tipModalContent: document.getElementById('tip-modal-content'),
                closeTipModal: document.getElementById('close-tip-modal'),
                openDisposalGuideBtn: document.getElementById('open-disposal-guide-btn'),
                disposalGuideModal: document.getElementById('disposal-guide-modal'),
                closeDisposalGuideBtn: document.getElementById('close-disposal-guide-btn'),
                homeAvatarProgress: document.getElementById('home-avatar-progress'),
                homeUserLevel: document.getElementById('home-user-level'),
                communityMainTabs: document.querySelectorAll('.community-main-tab'),
                friendsSubTabs: document.querySelectorAll('.friends-sub-tab'),
                friendsFeedContainer: document.getElementById('friends-feed-container'),
                friendsGoalsContainer: document.getElementById('friends-goals-container'),
                friendsChallengesContainer: document.getElementById('friends-challenges-container'),
                createGoalModal: document.getElementById('create-goal-modal'),
                createChallengeModal: document.getElementById('create-challenge-modal'),
                commentsModal: document.getElementById('comments-modal'),
                closeGoalModal: document.getElementById('close-goal-modal'),
                closeChallengeModal: document.getElementById('close-challenge-modal'),
                closeCommentsModal: document.getElementById('close-comments-modal'),
                createGoalForm: document.getElementById('create-goal-form'),
                createChallengeForm: document.getElementById('create-challenge-form'),
                commentForm: document.getElementById('comment-form'),
                goalTitleInput: document.getElementById('goal-title-input'),
                goalTargetInput: document.getElementById('goal-target-input'),
                challengeTitleInput: document.getElementById('challenge-title-input'),
                challengeDescriptionInput: document.getElementById('challenge-description-input'),
                challengeFriendSelect: document.getElementById('challenge-friend-select'),
                commentsList: document.getElementById('comments-list'),
                commentInput: document.getElementById('comment-input'),
                addPostBtn: document.getElementById('add-post-btn'),
                newPostInput: document.getElementById('new-post-input'),
                generatePostIdeaBtn: document.getElementById('generate-post-idea-btn'),
                reuseIdeasModal: document.getElementById('reuse-ideas-modal'),
                reuseModalTitle: document.getElementById('reuse-modal-title'),
                reuseModalContent: document.getElementById('reuse-modal-content'),
                closeReuseModal: document.getElementById('close-reuse-modal'),
                toggleUsedVouchers: document.getElementById('toggle-used-vouchers'),
                featuredRewardContainer: document.getElementById('featured-reward-container'),
                communityTopicsContainer: document.getElementById('community-topics-container'),
                createNewTopicBtn: document.getElementById('create-new-topic-btn'),
                createTopicModal: document.getElementById('create-topic-modal'),
                closeCreateTopicModal: document.getElementById('close-create-topic-modal'),
                createTopicForm: document.getElementById('create-topic-form'),
                newTopicTitleInput: document.getElementById('new-topic-title-input'),
                newTopicContentInput: document.getElementById('new-topic-content-input'),
                topicDetailModal: document.getElementById('topic-detail-modal'),
                closeTopicDetailModal: document.getElementById('close-topic-detail-modal'),
                topicDetailTitle: document.getElementById('topic-detail-title'),
                topicDetailContent: document.getElementById('topic-detail-content'),
                topicCommentsList: document.getElementById('topic-comments-list'),
                addCommentForm: document.getElementById('add-comment-form'),
                addCommentInput: document.getElementById('add-comment-input'),
                treeAnimationModal: document.getElementById('tree-animation-modal'),
                treeTrunk: document.getElementById('tree-trunk'),
                treeLeaves: document.getElementById('tree-leaves'),
                treeConfirmationCard: document.getElementById('tree-confirmation-card'),
                closeTreeAnimationModal: document.getElementById('close-tree-animation-modal'),

            });

            const savedTheme = localStorage.getItem('ecociclo-theme') || 'system';
            setTheme(savedTheme);
            
            elements.chatWindow.innerHTML = welcomeHTML;

            setTimeout(() => {
                elements.splashScreen.classList.add('opacity-0');
                elements.splashScreen.addEventListener('transitionend', () => {
                    elements.splashScreen.classList.add('hidden');
                    elements.loginScreen.classList.remove('hidden');
                    elements.loginScreen.classList.add('fade-in');
                }, { once: true });
            }, 1200);

            elements.loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const email = e.target.email.value;
                const password = e.target.password.value;
                const user = simulatedUsers[email];

                if (user && user.password === password) {
                    elements.loginError.classList.add('hidden');
                    enterApp(false, { name: user.name });
                } else {
                    elements.loginError.textContent = "E-mail ou senha inválidos.";
                    elements.loginError.classList.remove('hidden');
                }
            });

            elements.registerForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const name = e.target.name.value;
                const email = e.target.email.value;
                const password = e.target.password.value;
                
                simulatedUsers[email] = { name, password };
                enterApp(false, { name });
            });

            elements.guestLoginButton.addEventListener('click', () => enterApp(true));
            
            elements.goToRegisterButton.addEventListener('click', () => {
                elements.loginScreen.classList.remove('fade-in');
                elements.loginScreen.classList.add('fade-out');
                setTimeout(() => {
                    elements.loginScreen.classList.add('hidden');
                    elements.registerScreen.classList.remove('hidden', 'fade-out');
                    elements.registerScreen.classList.add('fade-in');
                }, 300);
            });

            elements.goToLoginButton.addEventListener('click', () => {
                elements.registerScreen.classList.remove('fade-in');
                elements.registerScreen.classList.add('fade-out');
                setTimeout(() => {
                    elements.registerScreen.classList.add('hidden');
                    elements.loginScreen.classList.remove('hidden', 'fade-out');
                    elements.loginScreen.classList.add('fade-in');
                }, 300);
            });
        };
    </script>
</head>
<body class="bg-background text-primary">

    <div id="splash-screen" class="fixed inset-0 bg-foreground z-[999] flex flex-col items-center justify-center transition-opacity duration-700 ease-in-out">
        <div class="text-center">
            <i class="fas fa-leaf text-6xl text-theme-primary animate-bounce"></i>
            <h1 class="text-4xl font-bold text-primary mt-4 splash-title">Ecociclo</h1>
        </div>
    </div>

    <div id="login-screen" class="hidden fixed inset-0 z-[998] flex flex-col items-center justify-center bg-background p-4 transition-opacity duration-300">
        <div class="w-full max-w-sm text-center">
            <i class="fas fa-leaf text-5xl text-theme-primary"></i>
            <h1 class="text-3xl font-bold text-primary mt-3">Bem-vindo de Volta!</h1>
            <p class="text-secondary mt-2 mb-8">Faça login para continuar sua jornada sustentável.</p>

            <div class="bg-foreground p-8 rounded-2xl shadow-custom">
                <form id="login-form">
                    <div class="space-y-4">
                        <input type="email" name="email" placeholder="E-mail" class="w-full p-3 border border-theme rounded-lg bg-background text-primary focus:ring-2 focus:ring-theme-primary transition" required>
                        <input type="password" name="password" placeholder="Senha" class="w-full p-3 border border-theme rounded-lg bg-background text-primary focus:ring-2 focus:ring-theme-primary transition" required>
                    </div>
                    <p id="login-error" class="text-red-500 text-sm mt-4 text-left hidden"></p>
                    <button type="submit" class="w-full bg-theme-primary text-white font-bold py-3 mt-6 rounded-lg hover:opacity-90 active:scale-95 transition-all">Entrar</button>
                </form>
            </div>

            <div class="mt-6 space-y-4">
                 <button id="go-to-register-button" class="text-primary font-semibold hover:text-theme-primary transition">Não tem uma conta? <span class="text-theme-primary font-bold">Crie uma agora</span></button>
                <button id="guest-login-button" class="text-secondary font-semibold hover:text-theme-primary transition">ou entre como Visitante &rarr;</button>
            </div>
        </div>
    </div>

    <div id="register-screen" class="hidden fixed inset-0 z-[998] flex flex-col items-center justify-center bg-background p-4 transition-opacity duration-300">
        <div class="w-full max-w-sm text-center">
            <i class="fas fa-leaf text-5xl text-theme-primary"></i>
            <h1 class="text-3xl font-bold text-primary mt-3">Crie sua Conta</h1>
            <p class="text-secondary mt-2 mb-8">Junte-se a nós e comece a fazer a diferença hoje mesmo.</p>

            <div class="bg-foreground p-8 rounded-2xl shadow-custom">
                <form id="register-form">
                    <div class="space-y-4">
                        <input type="text" name="name" placeholder="Nome Completo" class="w-full p-3 border border-theme rounded-lg bg-background text-primary focus:ring-2 focus:ring-theme-primary transition" required>
                        <input type="email" name="email" placeholder="E-mail" class="w-full p-3 border border-theme rounded-lg bg-background text-primary focus:ring-2 focus:ring-theme-primary transition" required>
                        <input type="password" name="password" placeholder="Senha" class="w-full p-3 border border-theme rounded-lg bg-background text-primary focus:ring-2 focus:ring-theme-primary transition" required>
                    </div>
                    <button type="submit" class="w-full bg-theme-secondary text-black font-bold py-3 mt-6 rounded-lg hover:opacity-90 active:scale-95 transition-all">Cadastrar</button>
                </form>
            </div>

            <div class="mt-6">
                <button id="go-to-login-button" class="text-secondary font-semibold hover:text-theme-primary transition">Já tenho uma conta? <span class="text-theme-primary font-bold">Faça login</span></button>
            </div>
        </div>
    </div>


    <div id="app-container" class="hidden flex h-screen">
        <!-- SIDEBAR (Desktop) -->
        <aside id="sidebar" class="hidden md:flex flex-col w-64 bg-foreground border-r border-theme transition-all duration-300">
            <div class="flex items-center justify-center h-16 border-b border-theme">
                <i class="fas fa-leaf text-2xl text-theme-primary"></i>
                <h1 class="text-xl font-bold text-primary ml-2">Ecociclo</h1>
            </div>
            <nav class="flex-grow p-4 space-y-2">
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="home-tab"><i class="fas fa-home w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Início</span></button>
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="community-tab"><i class="fas fa-users w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Comunidade</span></button>
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="map-tab"><i class="fas fa-map-marked-alt w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Mapas</span></button>
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="rewards-tab"><i class="fas fa-ticket-alt w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Vouchers</span></button>
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="chatbot-tab"><i class="fas fa-robot w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Wall</span></button>
            </nav>
            <div class="p-4 border-t border-theme">
                <button class="nav-button w-full flex items-center p-3 rounded-lg transition-colors text-secondary" data-tab="account-tab"><i class="fas fa-user-circle w-6 text-center text-lg"></i><span class="ml-4 font-semibold">Minha Conta</span></button>
            </div>
        </aside>

        <!-- MAIN CONTENT -->
        <div id="main-content" class="flex-1 flex flex-col h-screen">
            <!-- HEADER (Mobile) -->
            <header class="md:hidden p-4 bg-foreground text-primary shadow-custom sticky top-0 z-10 flex items-center justify-between border-b border-theme">
                 <div class="flex items-center space-x-3">
                    <i class="fas fa-leaf text-2xl text-theme-primary"></i>
                    <h1 class="text-xl font-bold">Ecociclo</h1>
                </div>
                <button id="theme-toggle" class="text-xl focus:outline-none text-secondary"><i class="fas fa-moon"></i></button>
            </header>

            <main class="flex-1 overflow-y-auto">
                <div id="home-tab" class="tab-content p-4 md:p-6 lg:p-8 space-y-8 bg-home-gradient">
                    
                    <div class="bg-foreground p-6 rounded-2xl shadow-custom animate-on-scroll">
                        <div class="flex flex-col sm:flex-row justify-between items-center gap-4">
                            <div>
                                <p class="text-sm font-semibold text-secondary">Seu Saldo de EcoPoints</p>
                                <p class="text-4xl font-bold text-theme-primary"><span data-bind-points>0</span></p>
                            </div>
                            <div class="w-full sm:w-1/2 lg:w-1/3">
                                <div class="flex justify-between items-center mb-1">
                                    <p data-bind-level class="font-semibold text-md text-primary"></p>
                                    <p id="home-level-progress-text" class="text-sm text-secondary"></p>
                                </div>
                                <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-2.5">
                                    <div id="home-level-progress" class="bg-theme-secondary h-2.5 rounded-full transition-all duration-500" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div id="welcome-banner" class="relative bg-gradient-to-br from-gray-900 to-green-900 text-white p-8 rounded-2xl shadow-lg overflow-hidden text-center flex flex-col items-center justify-center min-h-[40vh] fade-in-up">
                        <div class="parallax-container">
                             <i class="parallax-leaf fas fa-leaf absolute -top-8 -left-10 text-white/10 text-9xl -rotate-45" data-speed="2"></i>
                             <i class="parallax-leaf fas fa-leaf absolute -bottom-12 -right-8 text-white/10 text-[160px] rotate-45" data-speed="-3"></i>
                             <i class="parallax-leaf fas fa-leaf absolute top-1/4 -right-5 text-white/5 text-8xl rotate-12" data-speed="1.5"></i>
                             <i class="parallax-leaf fas fa-leaf absolute bottom-1/4 -left-12 text-white/5 text-7xl -rotate-12" data-speed="2.5"></i>
                             <i class="parallax-leaf fas fa-leaf absolute top-10 right-1/3 text-white/5 text-5xl rotate-[25deg]" data-speed="-1"></i>
                             <i class="parallax-leaf fas fa-leaf absolute bottom-8 left-1/4 text-white/5 text-4xl rotate-[-35deg]" data-speed="1"></i>
                        </div>
                        <div class="relative z-10 flex flex-col items-center justify-center">
                            <div class="mb-6 border-4 border-lime-400 rounded-full p-2">
                                <div class="w-16 h-16 bg-lime-400 rounded-full flex items-center justify-center">
                                    <i class="fas fa-leaf text-4xl text-green-900"></i>
                                </div>
                            </div>
                            <h1 class="text-4xl sm:text-5xl lg:text-7xl font-extrabold tracking-tight" style="text-shadow: 0 3px 6px rgba(0,0,0,0.4);">O futuro é agora!</h1>
                            <button id="simulate-recycle-button" class="mt-8 bg-lime-400 text-green-900 font-bold py-3 px-6 sm:py-4 sm:px-8 rounded-full text-lg transition-transform duration-300 hover:scale-105 shadow-lg animate-pulse-light">
                                Registrar Descarte
                            </button>
                            <p class="mt-4 text-lime-100/80 font-semibold" style="text-shadow: 0 1px 3px rgba(0,0,0,0.3);">EcoCiclo para um futuro melhor!</p>
                        </div>
                    </div>
                
                     <!-- Quick Actions & Recent Activity -->
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <div class="bg-foreground p-6 rounded-2xl shadow-custom animate-on-scroll delay-300">
                            <h3 class="text-lg font-bold text-primary mb-4">Ações Rápidas</h3>
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                <button id="open-rewards-quick-btn" class="w-full text-left bg-background p-3 sm:p-4 rounded-lg transition-all duration-200 flex items-center hover:bg-theme-primary/10 active:scale-[0.98]">
                                    <div class="bg-theme-primary/10 text-theme-primary p-2 sm:p-3 rounded-full mr-3 sm:mr-4 flex-shrink-0"><i class="fas fa-gift text-xl"></i></div>
                                    <div class="flex-shrink min-w-0"><p class="font-bold text-primary">Ver Recompensas</p></div>
                                </button>
                                <button class="w-full text-left bg-background p-3 sm:p-4 rounded-lg transition-all duration-200 flex items-center hover:bg-theme-primary/10 active:scale-[0.98]" onclick="document.querySelector('[data-tab=map-tab]').click()">
                                    <div class="bg-theme-primary/10 text-theme-primary p-2 sm:p-3 rounded-full mr-3 sm:mr-4 flex-shrink-0"><i class="fas fa-map-marked-alt text-xl"></i></div>
                                    <div class="flex-shrink min-w-0"><p class="font-bold text-primary">Explorar Mapa</p></div>
                                </button>
                                <button id="open-disposal-guide-btn" class="w-full text-left bg-background p-3 sm:p-4 rounded-lg transition-all duration-200 flex items-center hover:bg-theme-primary/10 active:scale-[0.98] sm:col-span-2">
                                    <div class="bg-theme-primary/10 text-theme-primary p-2 sm:p-3 rounded-full mr-3 sm:mr-4 flex-shrink-0"><i class="fas fa-book-open text-xl"></i></div>
                                    <div class="flex-shrink min-w-0"><p class="font-bold text-primary">Guias e Dicas</p></div>
                                </button>
                            </div>
                        </div>

                        <div class="bg-foreground p-6 rounded-2xl shadow-custom animate-on-scroll delay-400">
                             <h3 class="text-lg font-bold text-primary mb-2">Atividade Recente</h3>
                             <div id="home-activity-feed" class="space-y-1"></div>
                        </div>
                    </div>
                </div>

                <div id="community-tab" class="tab-content p-4 md:p-8 space-y-6">
                    <div class="fade-in-up">
                        <h1 class="text-3xl font-bold text-primary">Comunidade Ecociclo</h1>
                        <p class="text-secondary mt-1">Junte-se às discussões, desafios e faça a diferença com seus amigos.</p>
                    </div>

                    <!-- Community Main Tabs -->
                    <div class="border-b border-theme flex flex-wrap gap-x-4 gap-y-2">
                        <button class="community-main-tab py-2 px-1 text-lg font-semibold border-b-2 border-theme-primary text-theme-primary" data-tab="community-general-content">Geral</button>
                        <button class="community-main-tab py-2 px-1 text-lg font-semibold border-b-2 border-transparent text-secondary hover:text-primary" data-tab="community-friends-content">Amigos</button>
                    </div>

                    <!-- General Community Content -->
                    <div id="community-general-content" class="community-sub-tab-content active space-y-8">
                        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                             <!-- LEFT COLUMN -->
                            <div class="lg:col-span-2 space-y-8">
                                <!-- NEW: Community Topics -->
                                <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-100">
                                    <div class="flex justify-between items-center mb-4">
                                        <h2 class="text-xl font-bold text-primary flex items-center"><i class="fas fa-comments text-cyan-500 mr-3"></i>Tópicos da Comunidade</h2>
                                        <button id="create-new-topic-btn" class="bg-theme-primary/10 text-theme-primary font-bold py-2 px-4 text-sm rounded-lg hover:bg-theme-primary/20 transition-colors active:scale-95">
                                            <i class="fas fa-plus mr-2"></i>Novo Tópico
                                        </button>
                                    </div>
                                    <div id="community-topics-container" class="space-y-3">
                                        <!-- Topics rendered by JS -->
                                    </div>
                                </div>
                                <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-400">
                                    <h2 class="text-xl font-bold text-primary mb-4 flex items-center"><i class="fas fa-tasks text-blue-500 mr-3"></i>Desafios Ativos</h2>
                                    <div id="community-challenges-container" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                                </div>
                            </div>

                             <!-- RIGHT COLUMN -->
                            <div class="lg:col-span-1 space-y-8">
                                <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-200 transition-transform duration-300 hover:-translate-y-1">
                                    <div class="flex flex-wrap justify-between items-center mb-4 gap-2">
                                        <h2 class="text-xl font-bold text-primary flex items-center"><i class="fas fa-trophy text-yellow-500 mr-3"></i>Leaderboard</h2>
                                        <div id="leaderboard-filters" class="flex items-center bg-background p-1 rounded-lg text-sm">
                                            <button data-filter="weekly" class="leaderboard-filter-btn py-1 px-3 rounded-md font-semibold bg-theme-primary text-white">Semanal</button>
                                            <button data-filter="monthly" class="leaderboard-filter-btn py-1 px-3 rounded-md font-semibold bg-background text-primary">Mensal</button>
                                            <button data-filter="alltime" class="leaderboard-filter-btn py-1 px-3 rounded-md font-semibold bg-background text-primary">Geral</button>
                                        </div>
                                    </div>
                                    <div id="leaderboard-list" class="space-y-2 max-h-96 overflow-y-auto pr-2"></div>
                                     <div id="user-rank-highlight" class="mt-4"></div>
                                </div>
                                <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-300 transition-transform duration-300 hover:-translate-y-1">
                                    <h2 class="text-xl font-bold text-primary mb-4 flex items-center"><i class="fas fa-users-rays text-teal-500 mr-3"></i>Meta Comunitária</h2>
                                    <div id="community-goal-container"></div>
                                </div>
                                <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-400 transition-transform duration-300 hover:-translate-y-1">
                                    <h2 class="text-xl font-bold text-primary mb-4 flex items-center"><i class="fas fa-calendar-alt text-red-500 mr-3"></i>Próximos Eventos</h2>
                                    <div id="community-events-container" class="space-y-3"></div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Friends Content -->
                    <div id="community-friends-content" class="community-sub-tab-content space-y-6">
                        <div class="flex items-center bg-background p-1 rounded-full text-sm max-w-md shadow-inner">
                            <button class="friends-sub-tab flex-1 py-2 px-3 rounded-full font-semibold bg-theme-primary text-white" data-tab="friends-feed-tab">Feed</button>
                            <button class="friends-sub-tab flex-1 py-2 px-3 rounded-full font-semibold text-primary" data-tab="friends-goals-tab">Metas</button>
                            <button class="friends-sub-tab flex-1 py-2 px-3 rounded-full font-semibold text-primary" data-tab="friends-challenges-tab">Desafios</button>
                        </div>
                        
                        <!-- Friends Feed -->
                        <div id="friends-feed-tab" class="friends-sub-tab-content active">
                            <div class="bg-foreground p-4 rounded-xl shadow-custom">
                                <div class="flex items-start gap-3">
                                    <img src="https://placehold.co/40x40/10b981/FFFFFF?text=V" class="user-avatar-img w-10 h-10 rounded-full object-cover">
                                    <textarea id="new-post-input" class="w-full p-2 border-none rounded-lg bg-background text-primary focus:ring-0" placeholder="No que você está pensando?"></textarea>
                                </div>
                                <div class="flex justify-between items-center mt-2">
                                     <button id="generate-post-idea-btn" class="bg-yellow-400/20 text-yellow-700 dark:text-yellow-300 font-semibold py-1 px-3 text-xs rounded-full hover:bg-yellow-400/40 transition-colors">✨ Gerar ideia</button>
                                    <button id="add-post-btn" class="bg-theme-secondary text-black font-bold py-1 px-4 text-sm rounded-full hover:opacity-90">Postar</button>
                                </div>
                            </div>
                            <div id="friends-feed-container" class="space-y-4 mt-4"></div>
                        </div>

                        <!-- Friends Goals -->
                        <div id="friends-goals-tab" class="friends-sub-tab-content">
                            <div id="friends-goals-container" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                        </div>
                        
                        <!-- Friends Challenges -->
                        <div id="friends-challenges-tab" class="friends-sub-tab-content">
                             <div id="friends-challenges-container" class="space-y-4"></div>
                        </div>
                    </div>
                </div>

                 <div id="map-tab" class="tab-content h-full">
                     <div class="flex-1 flex flex-col md:flex-row overflow-hidden">
                         <!-- Left Panel -->
                         <div class="h-2/3 md:h-full md:flex-none md:w-1/3 lg:w-1/4 flex flex-col bg-foreground border-r border-theme">
                             <div class="p-4 border-b border-theme">
                                 <h1 class="text-2xl font-bold text-primary">Pontos de Coleta</h1>
                                 <p class="text-secondary text-sm">Encontre o local mais próximo de você.</p>
                                 <div class="relative mt-4">
                                     <input type="text" id="map-search-input" placeholder="Buscar por nome ou endereço..." class="w-full p-2 pl-10 border border-theme rounded-lg bg-background focus:ring-2 focus:ring-theme-primary transition">
                                     <i class="fas fa-search absolute left-3 top-1/2 -translate-y-1/2 text-secondary"></i>
                                 </div>
                                 <div class="flex flex-wrap gap-2 mt-4 text-sm font-semibold">
                                      <button data-filter="all" class="map-filter-btn flex-1 bg-theme-primary text-white dark:bg-blue-500 py-2 px-3 rounded-lg transition-colors">Todos</button>
                                      <button data-filter="ecoponto" class="map-filter-btn flex-1 bg-background dark:bg-slate-700 py-2 px-3 rounded-lg transition-colors">Ecopontos</button>
                                      <button data-filter="eletronico" class="map-filter-btn flex-1 bg-background dark:bg-slate-700 py-2 px-3 rounded-lg transition-colors">Eletrônicos</button>
                                 </div>
                             </div>
                             <div id="location-list" class="flex-1 overflow-y-auto p-2 space-y-2">
                                 <!-- Location cards will be rendered here -->
                             </div>
                              <div class="p-4 border-t border-theme">
                                 <button id="find-user-location-btn" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-3 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 active:scale-95"><i class="fas fa-location-arrow"></i>Encontrar Minha Localização</button>
                             </div>
                         </div>
                         <!-- Map -->
                         <div id="map" class="h-1/3 md:h-full md:flex-1"></div>
                     </div>
                 </div>

                 <div id="rewards-tab" class="tab-content p-4 md:p-8 space-y-8">
                     <div class="fade-in-up">
                         <h1 class="text-3xl font-bold text-primary">Recompensas e Vouchers</h1>
                         <p class="text-secondary">Use seus EcoPoints para resgatar prêmios incríveis!</p>
                     </div>

                     <!-- Points Summary -->
                     <div class="bg-foreground p-5 rounded-xl shadow-custom flex justify-between items-center fade-in-up delay-100">
                         <div>
                             <p class="text-sm font-semibold text-secondary">Seu Saldo</p>
                             <p class="text-3xl font-bold text-theme-primary"><span data-bind-points>0</span> EcoPoints</p>
                         </div>
                         <i class="fas fa-wallet text-4xl text-theme-primary/20"></i>
                     </div>

                     <!-- Featured Reward -->
                     <div id="featured-reward-container" class="fade-in-up delay-200">
                         <!-- Content injected by JS -->
                     </div>

                     <!-- Filters -->
                     <div class="fade-in-up delay-300">
                         <h2 class="text-xl font-bold text-primary mb-3">Loja de Recompensas</h2>
                         <div id="rewards-filters" class="flex flex-wrap gap-2">
                              <button class="reward-filter-btn active" data-filter="all">Todos</button>
                              <button class="reward-filter-btn" data-filter="food">Alimentação</button>
                              <button class="reward-filter-btn" data-filter="lifestyle">Bem-Estar</button>
                              <button class="reward-filter-btn" data-filter="donations">Doações</button>
                         </div>
                     </div>

                    <div id="rewards-container" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-5 fade-in-up delay-400"></div>
                    
                    <div class="pt-4 fade-in-up delay-500">
                         <div class="flex justify-between items-center mb-4">
                            <h2 class="text-xl font-bold text-primary">Meus Vouchers Resgatados</h2>
                            <div class="flex items-center space-x-2 text-sm">
                                <label for="toggle-used-vouchers" class="text-secondary font-medium">Mostrar usados</label>
                                <label class="relative inline-flex items-center cursor-pointer">
                                  <input type="checkbox" id="toggle-used-vouchers" class="sr-only peer">
                                  <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-green-300 dark:peer-focus:ring-green-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-theme-primary"></div>
                                </label>
                            </div>
                        </div>
                        <ul id="voucher-list" class="space-y-4"></ul>
                    </div>
                 </div>

                 <div id="chatbot-tab" class="tab-content h-full bg-background">
                     <!-- New Header -->
                     <div class="p-4 md:p-6 border-b border-theme bg-foreground flex justify-between items-center flex-shrink-0">
                         <div class="flex items-center gap-4">
                             <div class="relative">
                                 <img src="https://placehold.co/40x40/34d399/FFFFFF?text=W" class="w-10 h-10 rounded-full shadow-md" alt="Avatar do Wall">
                                 <span class="absolute bottom-0 right-0 block h-2.5 w-2.5 rounded-full bg-green-500 ring-2 ring-white"></span>
                             </div>
                             <div>
                                 <h1 class="text-xl font-bold text-primary">Wall</h1>
                                 <p class="text-xs text-secondary font-semibold">Seu EcoBot Pessoal</p>
                             </div>
                         </div>
                         <button id="clear-chat-btn" title="Limpar conversa" class="text-secondary hover:text-theme-primary transition-colors"><i class="fas fa-trash-alt text-lg"></i></button>
                     </div>
                
                     <!-- Main Chat Window -->
                     <main id="chat-window" class="flex-1 p-4 md:p-8 overflow-y-auto flex flex-col">
                         <!-- Welcome message will be injected by JS -->
                     </main>
                
                     <!-- New Footer/Input -->
                     <footer class="p-4 bg-foreground border-t border-theme flex-shrink-0">
                         <form id="chat-form" class="flex items-center gap-3 bg-background rounded-xl p-2 border border-theme focus-within:ring-2 focus-within:ring-theme-primary transition-shadow">
                             <input type="text" id="user-input" placeholder="Digite sua dúvida aqui..." class="flex-grow p-2 bg-transparent border-none focus:ring-0 text-primary placeholder-secondary" autocomplete="off" required>
                             <button type="submit" id="send-button" class="bg-theme-secondary text-black font-bold w-10 h-10 rounded-lg transition-transform duration-150 flex items-center justify-center shadow-lg disabled:bg-gray-400 disabled:scale-100 active:scale-95" title="Enviar"><i class="fas fa-paper-plane"></i></button>
                         </form>
                     </footer>
                 </div>

                <div id="account-tab" class="tab-content p-4 md:p-8 space-y-8">
                    <div class="fade-in-up">
                        <h1 class="text-3xl font-bold text-primary">Meu Perfil</h1>
                    </div>
                    
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                        <!-- Coluna do Perfil -->
                        <div class="lg:col-span-1 space-y-8">
                            <div class="bg-foreground p-6 rounded-2xl shadow-custom text-center fade-in-up delay-100">
                                <div class="relative w-24 h-24 mx-auto">
                                    <img class="user-avatar-img w-24 h-24 rounded-full object-cover border-4 border-foreground shadow-lg" src="https://placehold.co/96x96/10b981/FFFFFF?text=V">
                                    <button class="absolute -bottom-1 -right-1 bg-theme-primary text-white w-8 h-8 rounded-full flex items-center justify-center text-xs border-2 border-foreground hover:scale-110 transition-transform"><i class="fas fa-camera"></i></button>
                                </div>
                                <div class="mt-4">
                                    <div class="flex items-center justify-center space-x-2">
                                        <h2 class="user-display-name text-2xl font-bold text-primary"></h2>
                                        <button id="edit-name-button" class="text-secondary hover:text-theme-primary"><i class="fas fa-pencil-alt text-sm"></i></button>
                                    </div>
                                    <p data-bind-level class="font-semibold text-md text-secondary flex items-center justify-center mt-1"></p>
                                    <p class="text-xs text-gray-400 mt-2">Membro desde: Out 2025</p>
                                </div>
                                <div class="mt-4">
                                    <p id="level-progress-text" class="text-sm text-secondary mb-1"></p>
                                    <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-2.5">
                                        <div id="level-progress" class="bg-theme-primary h-2.5 rounded-full transition-all duration-500" style="width: 0%"></div>
                                    </div>
                                </div>
                            </div>
                             <div class="bg-foreground p-5 rounded-xl shadow-custom fade-in-up delay-400">
                                <h3 class="font-bold text-lg text-primary mb-4">Preferências</h3>
                                <div>
                                    <label class="text-sm font-semibold text-secondary">Aparência</label>
                                    <div class="mt-2 grid grid-cols-3 gap-2 bg-background p-1 rounded-lg">
                                        <button data-theme="light" class="theme-btn text-sm font-semibold py-2 px-3 rounded-md transition-colors flex items-center justify-center"><i class="fas fa-sun mr-2"></i>Claro</button>
                                        <button data-theme="dark" class="theme-btn text-sm font-semibold py-2 px-3 rounded-md transition-colors flex items-center justify-center"><i class="fas fa-moon mr-2"></i>Escuro</button>
                                        <button data-theme="system" class="theme-btn text-sm font-semibold py-2 px-3 rounded-md transition-colors flex items-center justify-center"><i class="fas fa-desktop mr-2"></i>Sistema</button>
                                    </div>
                                </div>
                            </div>

                            <div class="fade-in-up delay-500">
                                <button id="logout-button" class="w-full text-red-500 font-bold py-3 px-4 rounded-lg bg-red-500/10 hover:bg-red-500/20 active:scale-95 transition-all flex items-center justify-center gap-2">
                                    <i class="fas fa-sign-out-alt"></i> Sair da Conta
                                </button>
                            </div>
                        </div>

                        <!-- Coluna de Informações -->
                        <div class="lg:col-span-2 space-y-8">
                            <div class="bg-foreground p-6 rounded-2xl shadow-custom fade-in-up delay-200">
                                <h2 class="text-lg font-bold text-primary mb-4">Minhas Estatísticas</h2>
                                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 text-center">
                                    <div class="bg-background p-4 rounded-lg">
                                        <i class="fas fa-recycle text-2xl text-theme-secondary mb-2"></i>
                                        <p id="account-stat-items" class="text-2xl font-bold text-primary">0</p>
                                        <p class="text-xs font-semibold text-secondary uppercase tracking-wider">Itens Reciclados</p>
                                    </div>
                                    <div class="bg-background p-4 rounded-lg">
                                        <i class="fas fa-star text-2xl text-theme-accent mb-2"></i>
                                        <p id="account-stat-points" class="text-2xl font-bold text-primary">0</p>
                                        <p class="text-xs font-semibold text-secondary uppercase tracking-wider">Pontos Ganhos</p>
                                    </div>
                                    <div class="bg-background p-4 rounded-lg">
                                        <i class="fas fa-ticket-alt text-2xl text-theme-primary mb-2"></i>
                                        <p id="account-stat-vouchers" class="text-2xl font-bold text-primary">0</p>
                                        <p class="text-xs font-semibold text-secondary uppercase tracking-wider">Vouchers Resgatados</p>
                                    </div>
                                </div>
                            </div>
                            
                            <div id="activity-history-container" class="bg-foreground p-6 rounded-2xl shadow-custom fade-in-up delay-300">
                                <div class="flex justify-between items-center mb-4">
                                    <h2 class="text-lg font-bold text-primary">Histórico de Pontos</h2>
                                    <button id="expand-history-btn" class="text-sm font-semibold text-theme-primary flex items-center"><span>Ver Tudo</span><i class="fas fa-chevron-down ml-2"></i></button>
                                </div>
                                <div id="activity-feed" class="space-y-2 content pr-2"></div>
                            </div>

                            <div class="bg-foreground p-6 rounded-2xl shadow-custom fade-in-up delay-400">
                                <h3 class="font-bold text-lg text-primary mb-4">Conquistas</h3>
                                <div id="achievements-container" class="grid grid-cols-3 sm:grid-cols-6 gap-4">
                                    <!-- Badges will be rendered here by JS -->
                                </div>
                            </div>

                        </div>
                    </div>
                 </div>

            </main>

            <!-- BOTTOM NAV (Mobile) -->
            <nav class="md:hidden flex justify-around items-center p-1 bg-foreground border-t border-theme shadow-[0_-2px_10px_rgba(0,0,0,0.05)] z-20">
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="home-tab"><i class="fas fa-home text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Início</span></button>
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="community-tab"><i class="fas fa-users text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Comunidade</span></button>
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="map-tab"><i class="fas fa-map-marked-alt text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Mapas</span></button>
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="rewards-tab"><i class="fas fa-ticket-alt text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Vouchers</span></button>
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="chatbot-tab"><i class="fas fa-robot text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Wall</span></button>
                <button class="nav-button flex flex-col items-center justify-center p-1 flex-1 h-14" data-tab="account-tab"><i class="fas fa-user-circle text-lg sidebar-icon"></i><span class="text-[10px] mt-1 font-medium">Conta</span></button>
            </nav>
        </div>
    </div>

    <!-- MODALS -->
    <div id="edit-name-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm transform transition-all duration-200 scale-95">
             <div class="p-5"><h3 class="text-lg font-bold text-primary">Alterar nome de exibição</h3><input type="text" id="new-name-input" class="w-full mt-4 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Digite seu novo nome"></div>
            <div class="bg-background px-6 py-3 flex justify-end space-x-3"><button id="close-name-modal" class="text-secondary font-semibold py-2 px-4 rounded-lg transition">Cancelar</button><button id="save-name-button" class="bg-theme-primary hover:opacity-90 text-white font-semibold py-2 px-4 rounded-lg transition active:scale-95">Salvar</button></div>
        </div>
    </div>
    
    <div id="points-table-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme"><div class="flex justify-between items-center"><h3 class="text-lg font-bold text-primary flex items-center"><i class="fas fa-star text-yellow-500 mr-2"></i> Tabela de Pontos</h3><button id="close-points-table-btn" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button></div><p class="text-sm text-secondary mt-1">Veja quantos EcoPoints vale cada item.</p></div>
            <div class="p-5 max-h-[70vh] overflow-y-auto">
                <div class="space-y-3">
                    <div class="grid grid-cols-3 gap-2 p-2 font-semibold bg-background rounded-t-lg text-sm text-primary"><div>Item</div><div class="text-center">Tipo</div><div class="text-right">Pontos</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Papelão, jornais, revistas, caixas</div><div class="text-center text-xs text-gray-500">Reciclável de Rotina</div><div class="text-right font-bold text-green-600">5</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Garrafas PET, potes, embalagens de limpeza</div><div class="text-center text-xs text-gray-500">Plástico Leve</div><div class="text-right font-bold text-green-600">6</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Latas de alumínio e aço, tampas, arames</div><div class="text-center text-xs text-gray-500">Metal</div><div class="text-right font-bold text-green-600">8</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Garrafas, potes de conserva, frascos</div><div class="text-center text-xs text-gray-500">Vidro</div><div class="text-right font-bold text-green-600">10</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Cadeiras, pias, vasos, armários pequenos</div><div class="text-center text-xs text-gray-500">Volumoso Pequeno</div><div class="text-right font-bold text-yellow-600">50</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Sofás, colchões, guarda-roupas</div><div class="text-center text-xs text-gray-500">Volumoso Grande</div><div class="text-right font-bold text-yellow-600">100</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Entulho com pequena mistura</div><div class="text-center text-xs text-gray-500">Volumoso Pequeno</div><div class="text-right font-bold text-orange-600">150</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 border-b border-theme text-sm items-center"><div>Restos de cerâmica, gesso, areia em sacos</div><div class="text-center text-xs text-gray-500">Pequeno Entulho</div><div class="text-right font-bold text-red-600">250</div></div>
                    <div class="grid grid-cols-3 gap-2 p-2 text-sm items-center"><div>Micro-ondas, ventiladores, monitores</div><div class="text-center text-xs text-gray-500">Eletrodoméstico</div><div class="text-right font-bold text-red-700">400</div></div>
                </div>
            </div>
        </div>
    </div>
    <div id="recycle-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
         <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme"><div class="flex justify-between items-center"><h3 class="text-lg font-bold text-primary">Simular Descarte Correto</h3><button id="close-recycle-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button></div><p class="text-sm text-secondary mt-1">Selecione o tipo de material para ganhar EcoPoints!</p></div>
            <div id="recycle-options" class="p-5 grid grid-cols-2 sm:grid-cols-4 gap-3 max-h-[70vh] overflow-y-auto">
                <button class="recycle-option-btn p-4 bg-green-50 dark:bg-green-900/20 hover:bg-green-100 dark:hover:bg-green-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="5" data-reason="Papel e Papelão"><i class="fas fa-box text-3xl text-green-600"></i><p class="mt-2 text-sm font-semibold text-green-800 dark:text-green-300">Papel</p></button>
                <button class="recycle-option-btn p-4 bg-blue-50 dark:bg-blue-900/20 hover:bg-blue-100 dark:hover:bg-blue-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="6" data-reason="Plásticos"><i class="fas fa-bottle-water text-3xl text-blue-500"></i><p class="mt-2 text-sm font-semibold text-blue-800 dark:text-blue-300">Plásticos</p></button>
                <button class="recycle-option-btn p-4 bg-gray-100 dark:bg-gray-800/20 hover:bg-gray-200 dark:hover:bg-gray-800/40 rounded-lg text-center transition-transform hover:scale-105" data-points="8" data-reason="Metais"><i class="fas fa-cog text-3xl text-gray-600 dark:text-gray-400"></i><p class="mt-2 text-sm font-semibold text-gray-800 dark:text-gray-300">Metais</p></button>
                <button class="recycle-option-btn p-4 bg-red-50 dark:bg-red-900/20 hover:bg-red-100 dark:hover:bg-red-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="10" data-reason="Vidros"><i class="fas fa-wine-bottle text-3xl text-red-500"></i><p class="mt-2 text-sm font-semibold text-red-800 dark:text-red-300">Vidros</p></button>
                <button class="recycle-option-btn p-4 bg-purple-50 dark:bg-purple-900/20 hover:bg-purple-100 dark:hover:bg-purple-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="50" data-reason="Móveis Pequenos"><i class="fas fa-chair text-3xl text-purple-500"></i><p class="mt-2 text-sm font-semibold text-purple-800 dark:text-purple-300">Móveis Peq.</p></button>
                <button class="recycle-option-btn p-4 bg-indigo-50 dark:bg-indigo-900/20 hover:bg-indigo-100 dark:hover:bg-indigo-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="100" data-reason="Móveis Grandes"><i class="fas fa-couch text-3xl text-indigo-500"></i><p class="mt-2 text-sm font-semibold text-indigo-800 dark:text-indigo-300">Móveis Gdes.</p></button>
                <button class="recycle-option-btn p-4 bg-orange-50 dark:bg-orange-900/20 hover:bg-orange-100 dark:hover:bg-orange-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="250" data-reason="Entulho"><i class="fas fa-trowel-bricks text-3xl text-orange-500"></i><p class="mt-2 text-sm font-semibold text-orange-800 dark:text-orange-300">Entulho</p></button>
                <button class="recycle-option-btn p-4 bg-yellow-50 dark:bg-yellow-900/20 hover:bg-yellow-100 dark:hover:bg-yellow-900/40 rounded-lg text-center transition-transform hover:scale-105" data-points="400" data-reason="Lixo Eletrônico"><i class="fas fa-mobile-alt text-3xl text-yellow-600"></i><p class="mt-2 text-sm font-semibold text-yellow-800 dark:text-yellow-300">Eletrônicos</p></button>
            </div>
        </div>
    </div>
    <div id="custom-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm overflow-hidden transform transition-all duration-200 scale-95">
            <div class="p-6 text-center">
                <h3 id="modal-title" class="text-xl font-bold text-primary mb-2 flex items-center justify-center"></h3>
                <div id="modal-message" class="text-secondary text-sm"></div>
                <div id="modal-custom-button-container" class="mt-4"></div>
            </div>
            <div class="bg-background px-6 py-3 flex justify-center">
                <button id="close-modal" class="bg-theme-primary hover:opacity-90 text-white font-semibold py-2 px-6 rounded-lg transition active:scale-95">Ok</button>
            </div>
        </div>
    </div>

    <!-- LOGOUT CONFIRMATION MODAL -->
    <div id="logout-confirm-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm transform transition-all duration-200 scale-95">
            <div class="p-6 text-center">
                <h3 class="text-xl font-bold text-primary mb-2 flex items-center justify-center"><i class="fas fa-exclamation-triangle text-yellow-500 mr-2"></i>Sair da Conta</h3>
                <div class="text-secondary text-sm">Tem certeza que deseja sair?</div>
            </div>
            <div class="bg-background px-6 py-3 flex justify-end space-x-3">
                <button id="cancel-logout-btn" class="text-secondary font-semibold py-2 px-4 rounded-lg transition hover:bg-gray-200 dark:hover:bg-gray-700">Cancelar</button>
                <button id="confirm-logout-btn" class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg transition active:scale-95">Sim, Sair</button>
            </div>
        </div>
    </div>

    <!-- Tree Donation Animation Modal -->
    <div id="tree-animation-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-[60] flex items-center justify-center p-4 transition-opacity duration-300">
        <!-- Confirmation Card -->
        <div id="tree-confirmation-card" class="bg-foreground p-8 rounded-2xl shadow-2xl text-center flex flex-col items-center justify-center w-full max-w-md">
             <i class="fas fa-seedling text-5xl text-theme-secondary mb-4"></i>
             <h3 class="text-2xl font-bold text-primary">Você Fez a Diferença!</h3>
             <p class="text-secondary mt-2">Graças a você, uma nova árvore será plantada na Amazônia. O planeta agradece sua contribuição.</p>
             <button id="close-tree-animation-modal" class="mt-6 bg-theme-primary text-white font-bold py-2 px-6 rounded-lg transition active:scale-95">Continuar</button>
        </div>
    </div>

    <!-- Floating Action Button for Tip of the Day -->
    <button id="tip-fab" class="fixed bottom-20 md:bottom-6 right-6 bg-yellow-400 hover:bg-yellow-500 text-white w-16 h-16 rounded-full shadow-lg flex items-center justify-center transition-all duration-200 hover:scale-110 active:scale-100 z-40 hidden opacity-0">
        <i class="fas fa-lightbulb text-2xl"></i>
    </button>

    <!-- TIP OF THE DAY MODAL -->
    <div id="tip-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm overflow-hidden transform transition-all duration-200 scale-95">
            <div class="p-6 text-center">
                <h3 class="text-xl font-bold text-primary mb-2 flex items-center justify-center"><i class="fas fa-lightbulb text-yellow-500 mr-2"></i> Dica do Dia</h3>
                <div id="tip-modal-content" class="text-secondary text-sm"></div>
            </div>
            <div class="bg-background px-6 py-3 flex justify-center">
                <button id="close-tip-modal" class="bg-theme-primary hover:opacity-90 text-white font-semibold py-2 px-6 rounded-lg transition active:scale-95">Entendido</button>
            </div>
        </div>
    </div>
    
    <!-- DISPOSAL GUIDE MODAL -->
    <div id="disposal-guide-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-lg transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme">
                <div class="flex justify-between items-center">
                    <h3 class="text-lg font-bold text-primary flex items-center"><i class="fas fa-info-circle text-blue-500 mr-2"></i> Guia de Descarte Correto</h3>
                    <button id="close-disposal-guide-btn" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
                </div>
            </div>
            <div class="p-5 max-h-[70vh] overflow-y-auto">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="bg-background p-4 rounded-lg border-l-4 border-blue-500">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-bottle-water text-blue-500 mr-2"></i>Plásticos</h4>
                        <p class="text-sm text-secondary mt-1">Lave as embalagens para remover restos de alimentos e líquidos. Se possível, amasse para reduzir o volume.</p>
                    </div>
                    <div class="bg-background p-4 rounded-lg border-l-4 border-green-500">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-box text-green-500 mr-2"></i>Papel e Papelão</h4>
                        <p class="text-sm text-secondary mt-1">Mantenha seco e limpo. Não amasse, apenas dobre. Caixas de pizza com gordura não devem ser recicladas.</p>
                    </div>
                    <div class="bg-background p-4 rounded-lg border-l-4 border-red-500">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-wine-bottle text-red-500 mr-2"></i>Vidros</h4>
                        <p class="text-sm text-secondary mt-1">Descarte com cuidado. Se estiver quebrado, enrole em jornal grosso para evitar acidentes com os coletores.</p>
                    </div>
                     <div class="bg-background p-4 rounded-lg border-l-4 border-gray-500">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-cog text-gray-500 mr-2"></i>Metais</h4>
                        <p class="text-sm text-secondary mt-1">Lave latas de alimentos para remover resíduos. Cuidado com objetos pontiagudos.</p>
                    </div>
                    <div class="bg-background p-4 rounded-lg border-l-4 border-yellow-600">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-mobile-alt text-yellow-600 mr-2"></i>Lixo Eletrônico</h4>
                        <p class="text-sm text-secondary mt-1">Nunca descarte no lixo comum. Procure os pontos de coleta específicos na aba "Mapas".</p>
                    </div>
                     <div class="bg-background p-4 rounded-lg border-l-4 border-orange-500">
                        <h4 class="font-bold text-primary flex items-center"><i class="fas fa-oil-can text-orange-500 mr-2"></i>Óleo de Cozinha</h4>
                        <p class="text-sm text-secondary mt-1">Jamais jogue na pia. Armazene em garrafas PET e leve a um ponto de coleta especializado.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Create Goal Modal -->
    <div id="create-goal-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme flex justify-between items-center">
                <h3 class="text-lg font-bold text-primary">Criar Nova Meta</h3>
                <button id="close-goal-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <form id="create-goal-form" class="p-5 space-y-4">
                <div>
                    <label for="goal-title-input" class="text-sm font-semibold text-secondary">Título da Meta</label>
                    <input type="text" id="goal-title-input" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Ex: Reciclar 100 latas" required>
                </div>
                <div>
                    <label for="goal-target-input" class="text-sm font-semibold text-secondary">Valor Alvo</label>
                    <input type="number" id="goal-target-input" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Ex: 100" required>
                </div>
                <button type="submit" class="w-full bg-theme-primary text-white font-bold py-2 rounded-lg hover:opacity-90 active:scale-95 transition-all">Salvar Meta</button>
            </form>
        </div>
    </div>

    <!-- Create Challenge Modal -->
    <div id="create-challenge-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme flex justify-between items-center">
                <h3 class="text-lg font-bold text-primary">Criar Novo Desafio</h3>
                <button id="close-challenge-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <form id="create-challenge-form" class="p-5 space-y-4">
                <div>
                    <label for="challenge-title-input" class="text-sm font-semibold text-secondary">Título do Desafio</label>
                    <input type="text" id="challenge-title-input" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Ex: Batalha da Reciclagem" required>
                </div>
                <div>
                    <label for="challenge-description-input" class="text-sm font-semibold text-secondary">Descrição</label>
                    <textarea id="challenge-description-input" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Ex: Quem recicla mais plástico essa semana?" required></textarea>
                </div>
                 <div>
                    <label for="challenge-friend-select" class="text-sm font-semibold text-secondary">Desafiar Amigo</label>
                    <select id="challenge-friend-select" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary">
                        <option>Anna R.</option>
                        <option>João P.</option>
                        <option>Carlos F.</option>
                    </select>
                </div>
                <button type="submit" class="w-full bg-theme-primary text-white font-bold py-2 rounded-lg hover:opacity-90 active:scale-95 transition-all">Enviar Desafio</button>
            </form>
        </div>
    </div>
    
    <!-- Comments Modal -->
    <div id="comments-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-sm transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme flex justify-between items-center">
                <h3 class="text-lg font-bold text-primary">Comentários</h3>
                <button id="close-comments-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <ul id="comments-list" class="p-5 space-y-2 max-h-60 overflow-y-auto">
                <!-- Comments will be rendered here -->
            </ul>
            <form id="comment-form" class="p-5 border-t border-theme flex gap-2">
                <input type="text" id="comment-input" class="flex-grow p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Escreva um comentário..." required>
                <button type="submit" class="bg-theme-secondary text-black font-bold px-4 rounded-lg hover:opacity-90 active:scale-95 transition-all">Enviar</button>
            </form>
        </div>
    </div>
    
    <!-- Reuse Ideas Modal -->
    <div id="reuse-ideas-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-2xl shadow-2xl w-full max-w-2xl transform transition-all duration-200 scale-95 flex flex-col max-h-[90vh]">
            <div class="p-5 border-b border-theme flex justify-between items-center flex-shrink-0">
                <h3 id="reuse-modal-title" class="text-xl font-bold text-primary flex items-center gap-3"></h3>
                <button id="close-reuse-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <div id="reuse-modal-content" class="p-6 bg-background overflow-y-auto">
                <!-- Gemini content or loading state goes here -->
            </div>
        </div>
    </div>
    
    <!-- NEW: Create Topic Modal -->
    <div id="create-topic-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-lg transform transition-all duration-200 scale-95">
            <div class="p-5 border-b border-theme flex justify-between items-center">
                <h3 class="text-lg font-bold text-primary">Criar Novo Tópico</h3>
                <button id="close-create-topic-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <form id="create-topic-form" class="p-5 space-y-4">
                <div>
                    <label for="new-topic-title-input" class="text-sm font-semibold text-secondary">Título</label>
                    <input type="text" id="new-topic-title-input" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Qual a sua dúvida ou dica?" required>
                </div>
                <div>
                    <label for="new-topic-content-input" class="text-sm font-semibold text-secondary">Conteúdo</label>
                    <textarea id="new-topic-content-input" rows="4" class="w-full mt-1 p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Descreva sua ideia com mais detalhes..." required></textarea>
                </div>
                <button type="submit" class="w-full bg-theme-primary text-white font-bold py-3 rounded-lg hover:opacity-90 active:scale-95 transition-all">Publicar Tópico</button>
            </form>
        </div>
    </div>
    
    <!-- NEW: Topic Detail Modal -->
    <div id="topic-detail-modal" class="hidden opacity-0 fixed inset-0 bg-gray-900 bg-opacity-75 z-50 flex items-center justify-center p-4 transition-opacity duration-200">
        <div class="bg-foreground rounded-xl shadow-2xl w-full max-w-2xl transform transition-all duration-200 scale-95 flex flex-col max-h-[90vh]">
            <div class="p-5 border-b border-theme flex justify-between items-center flex-shrink-0">
                <h3 id="topic-detail-title" class="text-lg font-bold text-primary"></h3>
                <button id="close-topic-detail-modal" class="text-gray-400 hover:text-gray-600 text-2xl">&times;</button>
            </div>
            <div class="flex-grow overflow-y-auto p-5">
                <div id="topic-detail-content"></div>
                <h4 class="font-bold text-primary mt-6 mb-3">Comentários</h4>
                <ul id="topic-comments-list" class="space-y-3"></ul>
            </div>
            <form id="add-comment-form" class="p-5 border-t border-theme flex gap-2 flex-shrink-0">
                <input type="text" id="add-comment-input" class="flex-grow p-2 border border-theme rounded-lg bg-background text-primary" placeholder="Adicionar um comentário..." required>
                <button type="submit" class="bg-theme-secondary text-black font-bold px-4 rounded-lg hover:opacity-90 active:scale-95 transition-all">Enviar</button>
            </form>
        </div>
    </div>


</body>
</html>




