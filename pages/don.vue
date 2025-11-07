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

    <section class="cta">
      <NuxtLink class="don-button" to="/">⬅️ Retour à l’accueil</NuxtLink>
      <a class="don-button outline" href="mailto:contact@example.com?subject=Don%20Ange%20Ebogo%20Emerent">
        📧 Nous écrire pour contribuer
      </a>
    </section>
  </main>
</template>

<script setup>
// Page head metadata (English comments only)
useHead({
  title: 'Contribuer à l’hommage – Ange Ebogo Emerent',
  meta: [
    { name: 'description', content: 'Page de contribution pour l’hommage à Ange Ebogo Emerent.' }
  ]
})
</script>

<style>
.don {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 20px;
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
.cta {
  display: flex;
  gap: 16px;
  margin-top: 30px;
}
.don-button {
  background-color: #f6d465;
  color: #0d1b4b;
  padding: 14px 24px;
  font-size: 1em;
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
</style>
