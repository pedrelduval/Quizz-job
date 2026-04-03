<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Quiz - Qui seras-tu demain ? | Découvre ton futur métier</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', 'Poppins', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem 1rem;
    }
    #app {
      background: #fff;
      border-radius: 32px;
      padding: 2rem;
      max-width: 680px;
      width: 100%;
      box-shadow: 0 20px 40px rgba(0,0,0,0.15);
      transition: all 0.3s ease;
    }
    h1 { font-size: 28px; font-weight: 700; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      margin-bottom: 8px; }
    .sub { font-size: 15px; color: #6b7280; margin-bottom: 1.8rem; }
    .progress-bar {
      height: 8px;
      background: #e5e7eb;
      border-radius: 99px;
      margin-bottom: 1.5rem;
      overflow: hidden;
    }
    .progress-fill {
      height: 8px;
      background: linear-gradient(90deg, #667eea, #764ba2);
      border-radius: 99px;
      transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .question-label { 
      font-size: 12px;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 1px;
      color: #9ca3af; 
      margin-bottom: 8px; 
    }
    .question-text {
      font-size: 20px;
      font-weight: 700;
      color: #1a1a2e;
      margin-bottom: 1rem;
      line-height: 1.4;
    }
    .emoji-big { 
      font-size: 48px; 
      display: block; 
      margin-bottom: 12px;
      background: #f0eefc;
      width: fit-content;
      padding: 10px;
      border-radius: 60px;
    }
    .options { display: flex; flex-direction: column; gap: 12px; margin: 1.5rem 0; }
    .opt-btn {
      background: #fff;
      border: 2px solid #e5e7eb;
      border-radius: 20px;
      padding: 14px 18px;
      text-align: left;
      cursor: pointer;
      font-size: 15px;
      color: #1a1a2e;
      display: flex;
      align-items: center;
      gap: 14px;
      transition: all 0.2s ease;
      font-family: inherit;
      font-weight: 500;
    }
    .opt-btn:hover { 
      background: #f5f4ff; 
      border-color: #a5b4fc;
      transform: translateX(4px);
    }
    .opt-btn.selected {
      background: linear-gradient(135deg, #f5f3ff 0%, #ede9fe 100%);
      border-color: #7F77DD;
      color: #4c1d95;
      font-weight: 600;
      box-shadow: 0 4px 12px rgba(103, 110, 234, 0.2);
    }
    .opt-icon { font-size: 26px; min-width: 36px; }
    .nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 1rem;
      gap: 12px;
    }
    .btn-next {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border: none;
      border-radius: 40px;
      padding: 12px 28px;
      font-size: 16px;
      cursor: pointer;
      font-weight: 600;
      font-family: inherit;
      transition: all 0.2s;
      box-shadow: 0 4px 10px rgba(102, 126, 234, 0.3);
    }
    .btn-next:disabled { opacity: 0.5; cursor: not-allowed; transform: none; box-shadow: none; }
    .btn-next:not(:disabled):hover { transform: translateY(-2px); box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4); }
    .btn-back {
      background: transparent;
      border: 2px solid #e5e7eb;
      border-radius: 40px;
      padding: 10px 20px;
      font-size: 15px;
      cursor: pointer;
      color: #6b7280;
      font-weight: 500;
      font-family: inherit;
      transition: all 0.2s;
    }
    .btn-back:hover { background: #f9fafb; border-color: #cbd5e1; transform: translateX(-2px); }
    .q-counter { 
      font-size: 14px; 
      font-weight: 600;
      background: #f3f4f6;
      padding: 6px 14px;
      border-radius: 40px;
      color: #4b5563;
    }
    #result-box { display: none; animation: fadeIn 0.5s ease; }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .result-header { font-size: 28px; font-weight: 800; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent; 
      margin-bottom: 12px; }
    .result-sub { font-size: 15px; color: #6b7280; margin-bottom: 1.5rem; }
    .loader {
      display: flex;
      gap: 10px;
      align-items: center;
      justify-content: center;
      margin: 2rem 0;
      color: #667eea;
      font-size: 16px;
      font-weight: 500;
    }
    .dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      animation: bounce 1s infinite;
    }
    .dot:nth-child(2) { animation-delay: 0.2s; }
    .dot:nth-child(3) { animation-delay: 0.4s; }
    @keyframes bounce {
      0%, 80%, 100% { transform: scale(0.6); opacity: 0.5; }
      40% { transform: scale(1.2); opacity: 1; }
    }
    #ai-result {
      font-size: 15px;
      color: #1a1a2e;
      line-height: 1.7;
      background: linear-gradient(135deg, #faf9ff 0%, #f3f0ff 100%);
      border-radius: 24px;
      padding: 1.5rem;
      border: 1px solid #e0d9ff;
      margin: 1rem 0;
    }
    .career-card {
      background: white;
      border-radius: 20px;
      padding: 1rem;
      margin-bottom: 1rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      border-left: 5px solid #667eea;
    }
    .career-emoji { font-size: 32px; margin-right: 12px; }
    .career-title { font-size: 20px; font-weight: 700; color: #4c1d95; margin: 8px 0; }
    .career-desc { color: #4b5563; margin: 6px 0; }
    .career-match { font-size: 13px; color: #8b5cf6; font-weight: 600; margin-top: 8px; }
    .btn-retry {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border: none;
      border-radius: 40px;
      padding: 12px 24px;
      font-size: 15px;
      cursor: pointer;
      color: white;
      font-weight: 600;
      font-family: inherit;
      transition: all 0.2s;
      width: 100%;
      margin-top: 1rem;
    }
    .btn-retry:hover { transform: translateY(-2px); box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4); }
    footer {
      text-align: center;
      margin-top: 1.5rem;
      font-size: 12px;
      color: #cbd5e1;
    }
  </style>
</head>
<body>
<div id="app">
  <div id="quiz-box">
    <h1>✨ Qui seras-tu demain ?</h1>
    <p class="sub">Réponds aux questions et découvre les métiers faits pour toi !</p>
    <div class="progress-bar">
      <div class="progress-fill" id="prog" style="width:0%"></div>
    </div>
    <div class="question-label" id="q-label">Question 1 sur 10</div>
    <span class="emoji-big" id="q-emoji">🌟</span>
    <div class="question-text" id="q-text"></div>
    <div class="options" id="options-list"></div>
    <div class="nav">
      <button class="btn-back" id="btn-back">← Retour</button>
      <span class="q-counter" id="q-count">1 / 10</span>
      <button class="btn-next" id="btn-next" disabled>Suivant →</button>
    </div>
  </div>

  <div id="result-box">
    <div class="result-header">🎉 Bravo ! Voici ton profil unique</div>
    <div class="result-sub">Nous analysons tes talents pour te révéler des métiers inspirants...</div>
    <div class="loader" id="loader">
      <div class="dot"></div>
      <div class="dot"></div>
      <div class="dot"></div>
      <span style="margin-left: 8px;">Décodage de tes super-pouvoirs...</span>
    </div>
    <div id="ai-result"></div>
    <button class="btn-retry" id="restart-btn">🔄 Recommencer l'aventure</button>
  </div>
  <footer>💡 Chaque réponse te rapproche de ton futur talent !</footer>
</div>

<script>
  // ==================== QUESTIONS ====================
  const questions = [
    {
      emoji: "🎨",
      text: "Qu'est-ce que tu préfères faire le week-end ?",
      options: [
        { icon: "🎨", label: "Dessiner, créer, inventer des choses", category: "creatif" },
        { icon: "🏃", label: "Faire du sport ou bouger dehors", category: "sportif" },
        { icon: "📚", label: "Lire, apprendre, faire des expériences", category: "intellectuel" },
        { icon: "🤝", label: "Jouer avec des amis et aider les autres", category: "social" }
      ]
    },
    {
      emoji: "🦸",
      text: "Si tu pouvais avoir un super-pouvoir, lequel choisirais-tu ?",
      options: [
        { icon: "🔮", label: "Voir l'avenir et résoudre des mystères", category: "analytique" },
        { icon: "💪", label: "Être le plus fort et protéger les gens", category: "protecteur" },
        { icon: "💬", label: "Parler toutes les langues du monde", category: "communicatif" },
        { icon: "🌱", label: "Faire pousser n'importe quoi, guérir les plantes et animaux", category: "nature" }
      ]
    },
    {
      emoji: "🏫",
      text: "Quelle matière à l'école tu aimes le plus ?",
      options: [
        { icon: "🔢", label: "Les maths et les sciences", category: "scientifique" },
        { icon: "🎭", label: "L'art, la musique ou le théâtre", category: "artistique" },
        { icon: "📖", label: "Le français, la lecture ou l'histoire", category: "litteraire" },
        { icon: "⚽", label: "Le sport ou les activités manuelles", category: "manuel" }
      ]
    },
    {
      emoji: "🤔",
      text: "Quand tu as un problème, qu'est-ce que tu fais ?",
      options: [
        { icon: "🧩", label: "Je réfléchis longtemps et cherche la solution logique", category: "logique" },
        { icon: "💡", label: "J'invente une nouvelle façon de faire", category: "innovant" },
        { icon: "👥", label: "Je demande de l'aide à mes amis ou ma famille", category: "collaboratif" },
        { icon: "📋", label: "Je fais une liste et j'organise tout", category: "organise" }
      ]
    },
    {
      emoji: "🌍",
      text: "Si tu partais en voyage, tu choisirais quoi ?",
      options: [
        { icon: "🏙️", label: "Une grande ville pleine de musées et de culture", category: "culturel" },
        { icon: "🌴", label: "La nature sauvage et les animaux", category: "nature" },
        { icon: "🏗️", label: "Un chantier pour voir comment on construit les bâtiments", category: "technique" },
        { icon: "🎡", label: "Un parc d'attractions ou un festival", category: "festif" }
      ]
    },
    {
      emoji: "👫",
      text: "Comment tu préfères travailler ?",
      options: [
        { icon: "🙋", label: "Seul, je me concentre mieux", category: "independant" },
        { icon: "👨‍👩‍👧", label: "En équipe, c'est plus rigolo", category: "equipe" },
        { icon: "🎤", label: "Devant plein de gens, j'adore être au centre", category: "orateur" },
        { icon: "🔧", label: "En faisant des choses avec mes mains", category: "pratique" }
      ]
    },
    {
      emoji: "🐾",
      text: "Tu trouves un animal blessé dans la rue. Tu fais quoi ?",
      options: [
        { icon: "🏥", label: "Je l'amène chez le vétérinaire tout de suite", category: "soignant" },
        { icon: "📸", label: "Je le prends en photo et je partage pour aider", category: "communicatif" },
        { icon: "🛠️", label: "Je fabrique quelque chose pour le soigner", category: "creatif" },
        { icon: "📞", label: "J'appelle quelqu'un qui peut vraiment aider", category: "organise" }
      ]
    },
    {
      emoji: "🎯",
      text: "Qu'est-ce qui te rend vraiment fier(e) ?",
      options: [
        { icon: "🏆", label: "Gagner une compétition ou être le meilleur", category: "competitif" },
        { icon: "😊", label: "Voir que j'ai rendu quelqu'un heureux", category: "altruiste" },
        { icon: "🚀", label: "Créer quelque chose que personne n'avait fait avant", category: "innovant" },
        { icon: "📈", label: "Comprendre quelque chose de très difficile", category: "intellectuel" }
      ]
    },
    {
      emoji: "💭",
      text: "Dans ta tête, tu penses souvent à quoi ?",
      options: [
        { icon: "🌌", label: "L'espace, les inventions, le futur", category: "visionnaire" },
        { icon: "🎵", label: "Les histoires, la musique, les films", category: "artistique" },
        { icon: "🌿", label: "La nature, les animaux, l'écologie", category: "nature" },
        { icon: "⚙️", label: "Comment les choses fonctionnent et comment les améliorer", category: "analytique" }
      ]
    },
    {
      emoji: "🌈",
      text: "Dans 20 ans, comment tu t'imagines ?",
      options: [
        { icon: "🏥", label: "J'aide des gens à guérir ou à aller mieux", category: "medical" },
        { icon: "🎬", label: "Je crée des œuvres que tout le monde admire", category: "artiste" },
        { icon: "🔬", label: "Je fais des découvertes qui changent le monde", category: "scientifique" },
        { icon: "🏢", label: "Je dirige une équipe et je prends des grandes décisions", category: "leader" }
      ]
    }
  ];

  // ==================== BASE DE DONNÉES MÉTIERS (locale) ====================
  const careersDatabase = [
    { name: "🧑‍🎨 Artiste / Illustrateur", emoji: "🎨", description: "Tu donnes vie à l'imagination ! Tu pourrais créer des bandes dessinées, des animations ou des œuvres qui rendent le monde plus beau et coloré.", matchCategories: ["creatif", "artistique", "innovant", "visionnaire"], traits: ["créativité", "expression"] },
    { name: "🔬 Scientifique / Chercheur", emoji: "🧪", description: "Tu adores comprendre le pourquoi du comment ! Tu pourrais faire des découvertes incroyables pour guérir des maladies ou protéger la planète.", matchCategories: ["scientifique", "intellectuel", "logique", "analytique", "visionnaire"], traits: ["curiosité", "analyse"] },
    { name: "🩺 Médecin / Vétérinaire", emoji: "🐾", description: "Tu as un grand cœur et tu veux aider ceux qui souffrent. Comme médecin ou vétérinaire, tu pourrais soigner les gens ou les animaux chaque jour.", matchCategories: ["soignant", "medical", "nature", "altruiste"], traits: ["empathie", "soin"] },
    { name: "🏆 Sportif / Coach sportif", emoji: "⚽", description: "Tu as de l'énergie à revendre ! Tu pourrais devenir champion(ne) ou aider les autres à rester en forme et en bonne santé.", matchCategories: ["sportif", "competitif", "manuel", "pratique"], traits: ["dynamisme", "persévérance"] },
    { name: "💻 Développeur / Créateur de jeux vidéo", emoji: "🎮", description: "Tu aimes résoudre des problèmes et créer des mondes ! Tu pourrais concevoir des jeux, des applications ou des robots qui changent notre façon de vivre.", matchCategories: ["technique", "logique", "analytique", "innovant", "creatif"], traits: ["logique", "créativité tech"] },
    { name: "👩‍🏫 Enseignant / Formateur", emoji: "📚", description: "Tu aimes partager ce que tu sais ! En devenant prof, tu pourrais transmettre ta passion et aider des enfants ou des adultes à apprendre.", matchCategories: ["communicatif", "social", "litteraire", "orateur"], traits: ["pédagogie", "partage"] },
    { name: "🌱 Écologiste / Garde forestier", emoji: "🌳", description: "La nature t'appelle ! Tu pourrais protéger les forêts, les animaux ou trouver des solutions pour sauver la planète.", matchCategories: ["nature", "protecteur", "soignant", "visionnaire"], traits: ["amour de la nature", "protection"] },
    { name: "🏗️ Architecte / Ingénieur", emoji: "🏛️", description: "Tu aimes construire et organiser ! Tu pourrais dessiner des ponts, des maisons ou des machines géniales qui rendent la vie plus facile.", matchCategories: ["technique", "organise", "analytique", "pratique", "creatif"], traits: ["construction", "organisation"] },
    { name: "🎭 Comédien / Musicien", emoji: "🎭", description: "Tu es né(e) pour briller sur scène ! Avec ton talent, tu pourrais jouer dans des films, chanter ou faire rire les gens.", matchCategories: ["artistique", "orateur", "festif", "creatif"], traits: ["expression", "charisme"] },
    { name: "🤝 Psychologue / Médiateur", emoji: "💬", description: "Tu écoutes vraiment les autres ! Tu pourrais aider les gens à se sentir mieux, à résoudre leurs problèmes ou à mieux se comprendre.", matchCategories: ["social", "altruiste", "communicatif", "collaboratif", "medical"], traits: ["écoute", "empathie"] }
  ];

  let current = 0;
  let answers = []; // stocke l'index de la réponse pour chaque question

  function getCategoryFromAnswer(questionIdx, answerIdx) {
    if (!questions[questionIdx] || !questions[questionIdx].options[answerIdx]) return "general";
    return questions[questionIdx].options[answerIdx].category;
  }

  // Analyse locale : compte les catégories, génère 3 métiers personnalisés
  function generateLocalCareerResult() {
    const categoryCount = {};
    for (let i = 0; i < questions.length; i++) {
      const ansIdx = answers[i];
      if (ansIdx !== undefined) {
        const cat = getCategoryFromAnswer(i, ansIdx);
        categoryCount[cat] = (categoryCount[cat] || 0) + 1;
      }
    }
    
    // Trouver les top catégories
    const sortedCats = Object.entries(categoryCount).sort((a,b) => b[1] - a[1]);
    const topCategories = sortedCats.slice(0, 4).map(item => item[0]);
    
    // Score chaque métier selon combien de ses catégories correspondent aux top catégories
    const scoredCareers = careersDatabase.map(career => {
      let score = 0;
      career.matchCategories.forEach(cat => {
        if (topCategories.includes(cat)) score += 2;
        if (categoryCount[cat]) score += categoryCount[cat] * 1.5;
      });
      // bonus spécial si certaines personalités fortes
      if (career.traits.some(t => t === "créativité") && categoryCount["creatif"] > 1) score += 3;
      if (career.traits.some(t => t === "empathie") && categoryCount["altruiste"] > 1) score += 3;
      if (career.traits.some(t => t === "logique") && categoryCount["logique"] > 1) score += 3;
      return { ...career, score };
    });
    
    scoredCareers.sort((a,b) => b.score - a.score);
    const top3 = scoredCareers.slice(0, 3);
    
    // Construction du résultat HTML
    let html = `<div style="font-size: 1.1rem; margin-bottom: 1rem;">✨ D'après tes supers réponses, voici les métiers où tu pourrais t'épanouir :</div>`;
    top3.forEach((career, idx) => {
      let matchExplanation = "";
      if (idx === 0) matchExplanation = "🎯 Celui qui te correspond le plus !";
      else if (idx === 1) matchExplanation = "⭐ Un super choix aussi !";
      else matchExplanation = "💫 Tu as aussi un grand talent pour ça !";
      
      html += `
        <div class="career-card">
          <div style="display: flex; align-items: center;">
            <span class="career-emoji">${career.emoji}</span>
            <span class="career-title">${career.name}</span>
          </div>
          <div class="career-desc">${career.description}</div>
          <div class="career-match">${matchExplanation}</div>
        </div>
      `;
    });
    
    // Ajouter une phrase finale personnalisée
    let finalAdvice = "";
    if (topCategories.includes("creatif") && topCategories.includes("artistique")) {
      finalAdvice = "🎨 Tu as une âme d'artiste, n'arrête jamais de créer !";
    } else if (topCategories.includes("scientifique") && topCategories.includes("analytique")) {
      finalAdvice = "🔬 Tu as un esprit scientifique brillant, le monde a besoin de tes découvertes !";
    } else if (topCategories.includes("nature") && topCategories.includes("protecteur")) {
      finalAdvice = "🌿 Tu es un protecteur de la Terre, continue à prendre soin de notre planète !";
    } else if (topCategories.includes("social") && topCategories.includes("altruiste")) {
      finalAdvice = "💖 Ta gentillesse et ton empathie feront une grande différence autour de toi !";
    } else {
      finalAdvice = "🚀 Continue à explorer tes passions, tu as énormément de talents !";
    }
    html += `<div style="margin-top: 1rem; padding: 0.8rem; background: #ede9fe; border-radius: 18px; text-align: center; font-weight: 500;">✨ ${finalAdvice}</div>`;
    return html;
  }

  // Rendu d'une question
  function render() {
    const q = questions[current];
    document.getElementById('q-label').textContent = `Question ${current + 1} sur ${questions.length}`;
    document.getElementById('q-count').textContent = `${current + 1} / ${questions.length}`;
    document.getElementById('q-emoji').textContent = q.emoji;
    document.getElementById('q-text').textContent = q.text;
    document.getElementById('prog').style.width = ((current + 1) / questions.length * 100) + '%';
    
    const backBtn = document.getElementById('btn-back');
    backBtn.style.visibility = current === 0 ? 'hidden' : 'visible';
    
    const optionsDiv = document.getElementById('options-list');
    optionsDiv.innerHTML = '';
    
    q.options.forEach((opt, idx) => {
      const btn = document.createElement('button');
      btn.className = 'opt-btn';
      if (answers[current] === idx) btn.classList.add('selected');
      btn.innerHTML = `<span class="opt-icon">${opt.icon}</span><span>${opt.label}</span>`;
      btn.onclick = () => selectOption(idx);
      optionsDiv.appendChild(btn);
    });
    
    const nextBtn = document.getElementById('btn-next');
    nextBtn.disabled = answers[current] === undefined;
    nextBtn.textContent = current === questions.length - 1 ? '✨ Voir mon résultat ✨' : 'Suivant →';
  }
  
  function selectOption(selectedIdx) {
    answers[current] = selectedIdx;
    // Re-rendre la question pour mettre à jour les styles
    render();
  }
  
  function goNext() {
    if (answers[current] === undefined) return;
    if (current < questions.length - 1) {
      current++;
      render();
    } else {
      showLocalResult();
    }
  }
  
  function goBack() {
    if (current > 0) {
      current--;
      render();
    }
  }
  
  function showLocalResult() {
    // Cacher quiz, afficher résultat
    document.getElementById('quiz-box').style.display = 'none';
    document.getElementById('result-box').style.display = 'block';
    document.getElementById('loader').style.display = 'flex';
    document.getElementById('ai-result').innerHTML = '';
    
    // Simuler un petit délai d'analyse (plus réaliste et amusant)
    setTimeout(() => {
      const resultHtml = generateLocalCareerResult();
      document.getElementById('loader').style.display = 'none';
      document.getElementById('ai-result').innerHTML = resultHtml;
    }, 800);
  }
  
  function restartQuiz() {
    current = 0;
    answers = [];
    document.getElementById('quiz-box').style.display = 'block';
    document.getElementById('result-box').style.display = 'none';
    document.getElementById('ai-result').innerHTML = '';
    document.getElementById('loader').style.display = 'flex';
    render();
  }
  
  // Écouteurs d'événements
  document.getElementById('btn-next').addEventListener('click', goNext);
  document.getElementById('btn-back').addEventListener('click', goBack);
  document.getElementById('restart-btn').addEventListener('click', restartQuiz);
  
  // Initialisation
  render();
</script>
</body>
</html>
