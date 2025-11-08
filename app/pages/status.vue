<template>
  <main class="confirm">
    <h1>Confirmation du don</h1>

    <section class="status" :class="statusClass">
      <div class="icon" aria-hidden="true">{{ icon }}</div>
      <div class="text">
        <h2>{{ title }}</h2>
        <p>{{ message }}</p>
        <p v-if="extra" class="extra">{{ extra }}</p>
      </div>
    </section>

    <section class="cta">
      <NuxtLink class="don-button" to="/">⬅️ Retour à l’accueil</NuxtLink>
      <NuxtLink class="don-button outline" to="/don">🤍 Revenir à la page de don</NuxtLink>
    </section>
  </main>
</template>

<script setup lang="ts">
// Head metadata (English comments only)
useHead({
  title: 'Confirmation du don – Ange Ebogo Emerent',
  meta: [
    { name: 'description', content: 'Page de confirmation du don avec affichage du statut.' }
  ]
})

// Read status from query and map to UI (English comments only)
const route = useRoute()

function normalizeStatus(input: string | null | undefined): string {
  if (!input) return ''
  const s = input.toString().trim().toLowerCase()
  // Basic diacritics handling for common French characters
  return s
    .replace(/é|è|ê/g, 'e')
    .replace(/à|â/g, 'a')
    .replace(/î/g, 'i')
    .replace(/ô/g, 'o')
    .replace(/ù/g, 'u')
}

const raw = route.query.status as string | undefined
const status = normalizeStatus(raw)

type StatusMap = {
  [key: string]: { title: string; message: string; extra?: string; icon: string; cls: string }
}

const statuses: StatusMap = {
  // complete: success
  complete: {
    title: 'Paiement réussi',
    message: 'Merci pour votre contribution. Votre don a été enregistré avec succès.',
    icon: '✅',
    cls: 'success'
  },
  success: {
    title: 'Paiement réussi',
    message: 'Merci pour votre contribution. Votre don a été enregistré avec succès.',
    icon: '✅',
    cls: 'success'
  },

  // failed: échoué
  failed: {
    title: 'Paiement échoué',
    message: 'Le paiement n’a pas pu aboutir. Vous pouvez réessayer ou choisir un autre moyen.',
    icon: '❌',
    cls: 'error'
  },
  echec: {
    title: 'Paiement échoué',
    message: 'Le paiement n’a pas pu aboutir. Vous pouvez réessayer ou choisir un autre moyen.',
    icon: '❌',
    cls: 'error'
  },

  // canceled: annulé
  canceled: {
    title: 'Paiement annulé',
    message: 'Vous avez annulé l’opération. Aucun débit n’a été effectué.',
    icon: '⚠️',
    cls: 'warn'
  },
  annule: {
    title: 'Paiement annulé',
    message: 'Vous avez annulé l’opération. Aucun débit n’a été effectué.',
    icon: '⚠️',
    cls: 'warn'
  },

  // abandonné: requires verification
  abandonne: {
    title: 'Paiement abandonné',
    message: 'L’opération a été interrompue. Des vérifications peuvent être nécessaires.',
    extra: 'Dans certains cas, le client a payé mais un incident chez l’opérateur a empêché la confirmation. Si vous avez été débité, contactez-nous.',
    icon: '⏳',
    cls: 'info'
  },
  abandon: {
    title: 'Paiement abandonné',
    message: 'L’opération a été interrompue. Des vérifications peuvent être nécessaires.',
    extra: 'Dans certains cas, le client a payé mais un incident chez l’opérateur a empêché la confirmation. Si vous avez été débité, contactez-nous.',
    icon: '⏳',
    cls: 'info'
  }
}

const fallback = {
  title: 'Statut inconnu',
  message: 'Nous n’avons pas reçu de statut valide. Merci de vérifier le lien ou de nous contacter.',
  icon: '❓',
  cls: 'neutral'
}

const current = statuses[status] ?? fallback

const title = current.title
const message = current.message
const extra = current.extra
const icon = current.icon
const statusClass = `box ${current.cls}`
</script>

<style>
.confirm {
  max-width: 900px;
  margin: 0 auto;
  padding: clamp(24px, 6vw, 40px) 16px;
  text-align: left;
}
.confirm h1 { color: #f6d465; text-align: center; }

.status.box {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 12px;
  align-items: start;
  padding: clamp(16px, 3vw, 24px);
  border-radius: 12px;
  border: 2px solid #f6d465;
  background: rgba(246, 212, 101, 0.1);
}
.status .icon { font-size: 1.8rem; }
.status .text h2 { margin: 0 0 6px; }
.status .extra { margin-top: 8px; opacity: 0.9; }

.box.success { border-color: #65f67f; background: rgba(101, 246, 127, 0.08); }
.box.error { border-color: #ff6b6b; background: rgba(255, 107, 107, 0.08); }
.box.warn { border-color: #ffd66b; background: rgba(255, 214, 107, 0.08); }
.box.info { border-color: #6bd0ff; background: rgba(107, 208, 255, 0.08); }
.box.neutral { border-color: #ccc; background: rgba(255, 255, 255, 0.06); }

.cta {
  display: flex;
  gap: 16px;
  margin-top: 24px;
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
.don-button:hover { background-color: #ffec9e; }
.don-button.outline { background: transparent; color: #f6d465; border: 2px solid #f6d465; }
.don-button.outline:hover { color: #0d1b4b; background-color: #ffec9e; }

@media (max-width: 560px) {
  .cta { flex-direction: column; }
  .don-button { width: 100%; text-align: center; }
}
</style>

