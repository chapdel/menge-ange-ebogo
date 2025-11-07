<template>
  <div>
    <div class="banner">
      <img src="/banner.svg" alt="Ange Ebogo Emerent" />
    </div>

    <header>
      <h1>Hommage à Ange Ebogo Emerent</h1>
      <p>
        Le Patriarche de la musique camerounaise nous a quittés. Unissons nos cœurs pour lui offrir des obsèques dignes et pleines de gratitude.
      </p>
      <div class="portrait">
        <img :src="portraitSrc" alt="Ange Ebogo Emerent" @error="onPortraitError" />
      </div>
      <NuxtLink class="don-button" to="/don">💛 Faire un don maintenant</NuxtLink>
    </header>

    <section class="section">
      <h2>🌿 À propos de cet hommage</h2>
      <p>
        Figure emblématique de la chanson camerounaise, Ange Ebogo Emerent a marqué des générations par sa voix, sa foi et son élégance. Ses mélodies ont bercé des foyers, ses paroles ont inspiré des vies. Aujourd’hui, nous lui rendons hommage à travers une collecte solidaire destinée à soutenir l’organisation de ses obsèques et d’une cérémonie artistique à sa mémoire.
      </p>
    </section>

    <section class="section">
      <h2>❤️ Pourquoi donner ?</h2>
      <p>
        Chaque contribution, petite ou grande, est une manière de dire <strong>merci</strong> pour l’héritage qu’il nous laisse. Les fonds récoltés serviront à :
      </p>
      <ul class="list">
        <li>Accompagner la famille dans l’organisation des obsèques,</li>
        <li>Préparer la cérémonie d’hommage musical,</li>
        <li>Préserver et transmettre son héritage artistique.</li>
      </ul>
      <NuxtLink class="don-button" to="/don">🤍 Contribuer à la collecte</NuxtLink>
      <p class="cit">« Un artiste ne meurt jamais : il vit à travers la musique qu’il a semée dans nos cœurs. »</p>
    </section>

    <section class="section">
      <h2>📅 Cérémonie d’hommage</h2>
      <p>
        Une grande soirée musicale et spirituelle sera organisée en son honneur, avec la participation de nombreux artistes et amis. Les détails seront communiqués très prochainement.
      </p>
    </section>

    <footer>
      <p>© 2025 Comité d’Hommage à Ange Ebogo Emerent – Tous droits réservés</p>
    </footer>
  </div>
</template>

<script setup>
// Set page head tags and title (English comments only)
useHead({
  title: 'Hommage à Ange Ebogo Emerent',
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1.0' }
  ]
})

// Runtime image path with graceful fallback (English comments only)
const portraitSrc = ref('/images/ange-ebogo.jpg')
function onPortraitError(e) {
  // Fallback to banner if real portrait is not yet present
  e.target.src = '/banner.svg'
}
</script>

<style>
/* Page-specific styles (English comments only) */
header {
  padding: clamp(32px, 6vw, 60px) 20px;
}
header h1 {
  font-size: clamp(1.8rem, 3.5vw + 1rem, 2.8rem);
  margin-bottom: 10px;
  color: #f6d465;
}
header p {
  font-size: clamp(1rem, 1.2vw + 0.2rem, 1.3rem);
  max-width: 700px;
  margin: 0 auto;
  line-height: 1.6em;
}
.portrait {
  margin-top: 40px;
  position: relative;
  display: inline-block;
}
.portrait img {
  width: min(90vw, 350px);
  border-radius: 10px;
  border: 3px solid #f6d465;
  box-shadow: 0 0 40px rgba(255, 215, 0, 0.8), 0 0 80px rgba(255, 215, 0, 0.6), 0 0 120px rgba(255, 215, 0, 0.4);
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}
.portrait img:hover {
  transform: scale(1.05);
  box-shadow: 0 0 60px rgba(255, 215, 0, 1), 0 0 100px rgba(255, 215, 0, 0.9), 0 0 150px rgba(255, 215, 0, 0.8);
}
.section {
  padding: clamp(32px, 6vw, 50px) 20px;
  max-width: 900px;
  margin: auto;
}
.don-button {
  background-color: #f6d465;
  color: #0d1b4b;
  padding: clamp(12px, 2.8vw, 18px) clamp(20px, 5.2vw, 40px);
  font-size: clamp(1rem, 1.2vw, 1.2em);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: 0.3s;
  margin-top: 30px;
}
.don-button:hover {
  background-color: #ffec9e;
}
.cit {
  font-style: italic;
  margin-top: 40px;
  opacity: 0.9;
}
footer {
  background: rgba(0,0,0,0.3);
  padding: 20px;
  font-size: 0.9em;
}
.banner img {
  width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
  border-bottom: 4px solid #f6d465;
  box-shadow: 0 0 60px rgba(255, 215, 0, 0.7);
}
.banner {
  /* Fallback background if image is missing (English comment) */
  background: linear-gradient(135deg, #0d1b4b, #1b3b8c);
  min-height: 200px;
}
.list {
  text-align: left;
  max-width: min(90vw, 600px);
  margin: 0 auto;
  line-height: 1.8em;
}

/* Responsive tweaks */
@media (max-width: 768px) {
  header { padding: 40px 16px; }
  .section { padding: 28px 16px; }
}

@media (max-width: 480px) {
  .don-button {
    width: 100%;
    display: inline-block;
    text-align: center;
  }
}
</style>
