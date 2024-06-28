# iaragames
laraGames - Integração com Stripe
Este projeto demonstra como integrar a API de pagamento do Stripe em um site de vendas de jogos. Ele inclui um formulário de pagamento seguro e um backend para processar os tokens de pagamento.

Requisitos
Node.js
NPM (Node Package Manager)
Conta no Stripe (com as chaves de API configuradas)
Instalação
1. Clone o repositório
bash
Copiar código
git clone https://github.com/seu-usuario/laraGames.git
cd laraGames
2. Instale as dependências
bash
Copiar código
npm install express body-parser stripe
3. Configure as chaves da API do Stripe
No arquivo server.js, substitua 'your-secret-key-here' pela sua chave secreta do Stripe:

javascript
Copiar código
const stripe = require('stripe')('your-secret-key-here');
No arquivo index.html, substitua 'your-publishable-key-here' pela sua chave publicável do Stripe:

javascript
Copiar código
const stripe = Stripe('your-publishable-key-here');
Estrutura do Projeto
graphql
Copiar código
laraGames/
│
├── index.html      # Frontend HTML com o formulário de pagamento
├── server.js       # Backend Node.js para processar pagamentos
└── package.json    # Dependências do projeto
Configuração do Frontend
1. Incluir o Stripe.js
No arquivo index.html, incluímos a biblioteca Stripe.js:

html
Copiar código
<head>
  ...
  <script src="https://js.stripe.com/v3/"></script>
  ...
</head>
2. Adicionar o Formulário de Pagamento
Adicionamos um formulário de pagamento no HTML:

html
Copiar código
<div class="container mt-4">
  ...
  <h2 class="mt-5">Pagamento</h2>
  <form id="payment-form">
    <div id="card-element"></div>
    <button id="submit" class="btn btn-primary mt-3">Pagar</button>
    <div id="card-errors" role="alert" class="mt-2 text-danger"></div>
  </form>
</div>
3. Adicionar JavaScript para Processar o Pagamento
Incluímos o script para processar o pagamento:

html
Copiar código
<script>
  const stripe = Stripe('your-publishable-key-here');
  const elements = stripe.elements();

  const style = {
    base: {
      color: '#32325d',
      fontFamily: 'Arial, sans-serif',
      fontSmoothing: 'antialiased',
      fontSize: '16px',
      '::placeholder': {
        color: '#aab7c4'
      }
    },
    invalid: {
      color: '#fa755a',
      iconColor: '#fa755a'
    }
  };

  const card = elements.create('card', {style: style});
  card.mount('#card-element');

  card.on('change', function(event) {
    const displayError = document.getElementById('card-errors');
    if (event.error) {
      displayError.textContent = event.error.message;
    } else {
      displayError.textContent = '';
    }
  });

  const form = document.getElementById('payment-form');
  form.addEventListener('submit', function(event) {
    event.preventDefault();

    stripe.createToken(card).then(function(result) {
      if (result.error) {
        const errorElement = document.getElementById('card-errors');
        errorElement.textContent = result.error.message;
      } else {
        stripeTokenHandler(result.token);
      }
    });
  });

  function stripeTokenHandler(token) {
    const form = document.getElementById('payment-form');
    const hiddenInput = document.createElement('input');
    hiddenInput.setAttribute('type', 'hidden');
   



