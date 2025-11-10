<template>
  <div>
    <header>
      <h1>Hommage à Ange Ebogo Emerent</h1>
      <p>
        Le Patriarche de la musique camerounaise nous a quittés. Unissons nos cœurs pour lui offrir des obsèques dignes et pleines de gratitude.
      </p>
      <div class="portrait">
        <img :src="portraitSrc" alt="Ange Ebogo Emerent" @error="onPortraitError" />
        <button class="don-button below" type="button" @click="openModal">💛 Faire un don maintenant</button>
      </div>
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
      <button class="don-button" type="button" @click="openModal">🤍 Contribuer à la collecte</button>
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
    
    <!-- Donation modal (English comments only) -->
    <div v-if="showModal" class="modal-overlay" @click="closeModal" aria-hidden="true"></div>
    <section
      v-if="showModal"
      class="modal"
      role="dialog"
      aria-modal="true"
      aria-labelledby="donation-title"
      tabindex="-1"
      @keydown.esc="closeModal"
      @keyup.esc.window="closeModal"
    >
      <div class="modal-content" @click.stop>
        <header class="modal-header">
          <h2 id="donation-title">Faire un don</h2>
          <button class="close" type="button" aria-label="Fermer" @click="closeModal">✖</button>
        </header>

        <p class="modal-help">Indiquez un montant ou choisissez une suggestion ci-dessous.</p>
        <div class="amount-row">
          <label for="home-don-amount">Montant (FCFA)</label>
          <input
            id="home-don-amount"
            type="number"
            inputmode="numeric"
            min="100"
            step="100"
            placeholder="Ex. 5 000"
            v-model.number="amount"
            aria-describedby="home-don-amount-desc"
          />
        </div>
        <p id="home-don-amount-desc" class="hint">Montant minimal: 100 FCFA. Vous pouvez ajuster librement.</p>

        <div class="suggestions" role="group" aria-label="Suggestions de montants">
          <button
            v-for="s in suggestions"
            :key="s"
            type="button"
            class="chip"
            :class="{ active: amount === s }"
            @click="selectAmount(s)"
          >{{ formatCurrency(s) }}</button>
        </div>

        <div class="pay-row">
          <button class="don-button pay mobile" :disabled="!isValidAmount" @click="handlePayMobile">
            📱 Mobile Money
          </button>
          <button class="don-button pay card" :disabled="!isValidAmount" @click="handlePayCard">
            💳 Carte / International
          </button>
          <p class="tiny" v-if="!isValidAmount">Veuillez saisir un montant valide (≥ 100 FCFA).</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
// Set page head tags and title (English comments only)
useHead({
  title: 'Hommage à Ange Ebogo Emerent',
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1.0' }
  ]
})

// Runtime image path with graceful fallback (English comments only)
const portraitSrc = ref('/ebogo.png')
function onPortraitError(e) {
  // Fallback to banner if real portrait is not yet present
  e.target.src = '/banner.svg'
}

// Donation modal state and helpers (English comments only)
const showModal = ref(false)
const amount = ref<number | null>(null)
const suggestions = [1000, 5000, 10000, 20000, 50000]

const isValidAmount = computed(() => {
  return typeof amount.value === 'number' && amount.value >= 100
})

function openModal() {
  showModal.value = true
  // Move focus to the amount input after the modal opens
  requestAnimationFrame(() => {
    const el = document.getElementById('home-don-amount') as HTMLInputElement | null
    el?.focus()
  })
}
function closeModal() {
  showModal.value = false
}
function selectAmount(v: number) {
  amount.value = v
}
function formatCurrency(v: number) {
  try {
    return new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'XAF', maximumFractionDigits: 0 }).format(v)
  } catch {
    return `${v.toLocaleString('fr-FR')} FCFA`
  }
}
async function handlePayMobile() {
  if (!isValidAmount.value) return
  const amt = amount.value as number
  try {
    const res = await $fetch('https://1api.notchpay.me/api/v1/quicks/justenous/standlone', {
      method: 'POST',
      body: {
        amount: amt,
        currency: 'XAF',
        email: 'guest@mendofiannces.com'
      }
    })
    if (res.action === 'redirect' && res.authorization_url) {
      window.location.href = res.authorization_url
    } else {
      alert('Réponse inattendue du serveur.')
    }
  } catch (err: any) {
    alert('Erreur lors de l’initialisation du paiement Mobile Money : ' + (err.data?.message || err.message))
  }
}
function handlePayCard() {
  if (!isValidAmount.value) return
  window.location.href = 'https://nbkfinance.com/ipercash/'
}

// Prevent background scroll when modal is open (English comments only)
watch(showModal, (val) => {
  try {
    document.body.style.overflow = val ? 'hidden' : ''
  } catch {}
})
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
  display: flex;
  flex-direction: column;
  align-items: center;
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
.portrait .don-button.below {
  margin-top: 12px;
  width: min(90vw, 350px);
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

/* Modal styles (English comments only) */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
  backdrop-filter: blur(2px);
  z-index: 999;
}
.modal {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  z-index: 1000;
}
.modal-content {
  width: min(92vw, 560px);
  background: #0d1b4b;
  color: #fff;
  border: 2px solid #f6d465;
  border-radius: 16px;
  padding: clamp(20px, 4vw, 28px);
  box-shadow: 0 0 30px rgba(246, 212, 101, 0.4);
}
.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}
.modal-help {
  opacity: 0.9;
  margin-bottom: 12px;
}
.close {
  background: transparent;
  color: #f6d465;
  border: 1px solid rgba(246, 212, 101, 0.5);
  border-radius: 8px;
  padding: 6px 10px;
  cursor: pointer;
}
.amount-row {
  display: grid;
  grid-template-columns: 1fr;
  gap: 8px;
}
.amount-row label {
  font-weight: 600;
}
.amount-row input {
  background: rgba(255, 255, 255, 0.08);
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.24);
  border-radius: 10px;
  padding: clamp(12px, 2.5vw, 14px) 12px;
  font-size: clamp(1rem, 1.2vw, 1.05rem);
}
.hint {
  font-size: 0.9rem;
  opacity: 0.8;
  margin-top: 6px;
}
.suggestions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 14px;
}
.chip {
  background: rgba(246, 212, 101, 0.15);
  color: #f6d465;
  border: 1px solid rgba(246, 212, 101, 0.35);
  border-radius: 24px;
  padding: 10px 14px;
  cursor: pointer;
  transition: 0.2s;
}
.chip:hover { background: rgba(246, 212, 101, 0.25); }
.chip.active { background: #f6d465; color: #0d1b4b; }
.pay-row { margin-top: 18px; }
.pay-row .tiny { font-size: 0.9rem; opacity: 0.85; margin-top: 8px; }
.don-button.pay { min-width: 160px; }
.pay-row { display: flex; flex-wrap: wrap; gap: 12px; justify-content: center; }
@media (max-width: 480px) {
  .pay-row { flex-direction: column; }
  .don-button.pay { width: 100%; }
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
