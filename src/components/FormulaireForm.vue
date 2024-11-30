<template>
  <div class="container">
    <section class="payment-validation">
      <h1>Validation du Paiement</h1>
      <form class="payment-form" @submit="submitPayment">
        <label for="">Montant</label>
        <input type="number" v-model="amount" placeholder="Montant" />
        <label for="">Mode de paiment</label>
        <select v-model="paymentMethod">
          <option value="mobile_money">Mobile Money</option>
          <option value="card">Card</option>
        </select>
        <label for="">Devise</label>
        <select v-model="currency">
          <option value="XAF">Fcfa</option>
          <option value="EUR">euro</option>
        </select>
        <label for="">Nom</label>
        <input type="text" v-model="name" placeholder="Nom" />
        <label for="">Adresse email</label>
        <input type="email" v-model="email" placeholder="Adresse mail" />
        <label for="">Téléphone</label>
        <input type="text" v-model="phone" placeholder="Téléphone" />
        <button type="submit">Valider le paiement</button>
      </form>
      <div v-if="paymentStatus" :class="paymentStatusClass">
        <p>{{ paymentStatusMessage }}</p>
      </div>
    </section>

    <section class="payment-details">
      <p><strong>Montant :</strong> {{ amount }} USD</p>
      <p><strong>Produit :</strong> {{ productName }}</p>
    </section>
  </div>
</template>

<script>

export default {
  data() {
    return {
      amount: 50, 
      productName: "Produit Exemple",
      paymentMethod: '',
      currency: '',
      name: '',
      email: '',
      phone: '',
      paymentStatus: null,
      paymentStatusMessage: '',
      paymentStatusClass: ''
    };
  },
  methods: {
    async submitPayment(e) {
      e.preventDefault();
      
      try {
        const response = await this.processPayment({
          amount: this.amount,
          currency: this.currency,
          paymentMethod: this.paymentMethod,
          name: this.name,
          email: this.email,
          phone: this.phone
        });

        if (response.success == 'success') {
          this.paymentStatus = 'success';
          this.paymentStatusMessage = "Paiement réussi ! Merci pour votre achat.";
          this.paymentStatusClass = 'success';
        } else {
          this.paymentStatus = 'failure';
          this.paymentStatusMessage = "Erreur lors du paiement. Veuillez réessayer.";
          this.paymentStatusClass = 'failure';
        }
      } catch (error) {
        this.paymentStatus = 'failure';
        this.paymentStatusMessage = "Erreur serveur. Veuillez réessayer.";
        this.paymentStatusClass = 'failure';
      }
    },
    processPayment(data) {
      console.log(data);
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          // 80% chance de succès
          if (Math.random() < 0.8) {
            resolve({ success: 'success', transactionId: 'TR' + Date.now() });
          } else {
            reject({ success: 'failed', transactionId: 'Transaction failed' });
          }
        }, 2000);
      });
    }
  }
};
</script>

<style scoped>
body {
  width: 100%;
  margin: 0 auto;
  padding: 20px;
  background-color: #dcfce7 !important;
  font-family: Arial, sans-serif;
  /* box-sizing: border-box; */
}
.container {
  display: flex;
  flex-direction: row;
  padding: 40px;
  align-items: center;
  justify-content: space-between;
}
.payment-validation {
  /* text-align: center; */
}
section {
  width: 50%;
  height: 100%;
}
.payment-details {
  margin: 20px 0;
  display: flex;
  flex-direction: column;
  align-items: start;
  padding-left: 40px;
  height: 500px;
  color: white;
  background-color: #17624d;
}
.payment-form {
  margin: 20px 0;
  display: flex;
  flex-direction: column;
}
.payment-form input, .payment-form select {
  margin: 5px;
  padding: 10px;
  width: 80%;
  border: 1px solid #ccc;
}
.payment-form button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}
.payment-form button:hover {
  background-color: #45a049;
}
.success {
  color: green;
}
.failure {
  color: red;
}
</style>