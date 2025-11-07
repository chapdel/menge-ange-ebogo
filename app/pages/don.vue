<template>
  <main class="don">
    <h1>Contribuer à l’hommage</h1>
    <p class="intro">
      Merci pour votre soutien. Cette page centralise les informations pour contribuer
      à l’organisation des obsèques et de la cérémonie artistique en mémoire d’Ange Ebogo Emerent.
    </p>

    <section class="options">
      <h2>Moyens de contribution</h2>
      <ul>
        <li>
          Virement bancaire — contactez-nous pour les coordonnées.
        </li>
        <li>
          Dons en ligne — nous pouvons intégrer un prestataire (Stripe/PayPal/MoMo, etc.).
        </li>
        <li>
          Participation logistique — bénévolat pour la cérémonie.
        </li>
      </ul>
    </section>
    <!-- Contribution form: amount input with suggestions and pay button (English comments only) -->
    <section class="contrib" aria-label="Formulaire de contribution">
      <h2>Votre contribution</h2>
      <p class="help">Indiquez un montant ou choisissez une suggestion ci-dessous.</p>

      <div class="amount-row">
        <label for="don-amount">Montant (FCFA)</label>
        <input
          id="don-amount"
          type="number"
          inputmode="numeric"
          min="100"
          step="100"
          placeholder="Ex. 5 000"
          v-model.number="amount"
          aria-describedby="don-amount-desc"
        />
      </div>
      <p id="don-amount-desc" class="hint">Montant minimal: 100 FCFA. Vous pouvez ajuster librement.</p>

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
        <button class="don-button pay" :disabled="!isValidAmount" @click="handlePay">
          💳 Payer
        </button>
        <p class="tiny" v-if="!isValidAmount">Veuillez saisir un montant valide (≥ 100 FCFA).</p>
      </div>
    </section>

    <section class="cta">
      <NuxtLink class="don-button" to="/">⬅️ Retour à l’accueil</NuxtLink>
      <a class="don-button outline" href="mailto:contact@example.com?subject=Don%20Ange%20Ebogo%20Emerent">
        📧 Nous écrire pour contribuer
      </a>
    </section>
  </main>
</template>

<script setup lang="ts">
// Page head metadata (English comments only)
useHead({
  title: 'Contribuer à l’hommage – Ange Ebogo Emerent',
  meta: [
    { name: 'description', content: 'Page de contribution pour l’hommage à Ange Ebogo Emerent.' }
  ]
})

// Contribution form state and helpers (English comments only)
const amount = ref<number | null>(null)
const suggestions = [1000, 5000, 10000, 20000, 50000]

const isValidAmount = computed(() => {
  return typeof amount.value === 'number' && amount.value >= 100
})

function selectAmount(v: number) {
  amount.value = v
}

function formatCurrency(v: number) {
  try {
    return new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'XAF', maximumFractionDigits: 0 }).format(v)
  } catch {
    // Fallback when Intl for 'XAF' is not available
    return `${v.toLocaleString('fr-FR')} FCFA`
  }
}

function handlePay() {
  if (!isValidAmount.value) return
  // Placeholder payment handler: opens email with prefilled content
  // Replace with real payment provider integration (Stripe/PayPal/MoMo/etc.)
  const amt = amount.value as number
  const subject = encodeURIComponent('Contribution – Ange Ebogo Emerent')
  const body = encodeURIComponent(`Bonjour,\n\nJe souhaite contribuer au montant de ${formatCurrency(amt)}.\nMerci de me communiquer les instructions de paiement.\n\nCordialement.`)
  window.location.href = `mailto:contact@example.com?subject=${subject}&body=${body}`
}
</script>

<style>
.don {
  max-width: 900px;
  margin: 0 auto;
  padding: clamp(24px, 6vw, 40px) 16px;
  text-align: left;
}
.don h1 {
  color: #f6d465;
  text-align: center;
}
.intro {
  line-height: 1.7em;
  opacity: 0.95;
}
.options h2 {
  margin-top: 30px;
}
.options ul {
  line-height: 1.8em;
}

/* Contribution form styles (English comments only) */
.contrib {
  margin-top: clamp(16px, 3vw, 24px);
  padding-top: clamp(8px, 2vw, 12px);
  border-top: 1px solid rgba(255, 255, 255, 0.16);
}
.contrib h2 {
  margin-bottom: 8px;
}
.help {
  opacity: 0.9;
  margin-bottom: 12px;
}
.amount-row {
  display: grid;
  grid-template-columns: 1fr;
  gap: 8px;
  max-width: 420px;
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

.pay-row {
  margin-top: 18px;
}
.pay-row .tiny {
  font-size: 0.9rem;
  opacity: 0.85;
  margin-top: 8px;
}
.don-button.pay {
  min-width: 160px;
}
.cta {
  display: flex;
  gap: 16px;
  margin-top: 30px;
  flex-wrap: wrap;
}
.don-button {
  background-color: #f6d465;
  color: #0d1b4b;
  padding: clamp(12px, 2.5vw, 14px) clamp(20px, 4vw, 24px);
  font-size: clamp(1rem, 1.2vw, 1.05rem);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: 0.3s;
  text-decoration: none;
}
.don-button:hover {
  background-color: #ffec9e;
}
.don-button.outline {
  background: transparent;
  color: #f6d465;
  border: 2px solid #f6d465;
}
.don-button.outline:hover {
  color: #0d1b4b;
  background-color: #ffec9e;
}

@media (max-width: 560px) {
  .cta { flex-direction: column; }
  .don-button { width: 100%; text-align: center; }
}
</style>
