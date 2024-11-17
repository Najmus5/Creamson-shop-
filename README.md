# Creamson-shop-
E-commerce 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Creamson Shop</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="root"></div>
  <script src="bundle.js"></script>
</body>
</html>
import React from 'react';
import Header from './Header';
import ProductList from './ProductList';
import Footer from './Footer';

function App() {
  return (
    <div className="App">
      <Header />
      <ProductList />
      <Footer />
    </div>
  );
}

export default App;
import React from 'react';

const Header = () => {
  return (
    <header className="header">
      <div className="logo">
        <a href="/">Creamson Shop</a>
      </div>
      <div className="search-bar">
        <input type="text" placeholder="Search..." />
      </div>
      <div className="user-nav">
        <a href="/login">Login</a>
        <a href="/cart">Cart</a>
      </div>
    </header>
  );
};

export default Header;
import React from 'react';
import Header from './Header';
import ProductList from './ProductList';
import Footer from './Footer';

function App() {
  return (
    <div className="App">
      <Header />
      <ProductList />
      <Footer />
    </div>
  );
}

export default App;
import React from 'react';

const Footer = () => {
  return (
    <footer className="footer">
      <p>&copy; 2024 Creamson Shop. All rights reserved.</p>
      <div className="social-links">
        <a href="https://facebook.com">Facebook</a>
        <a href="https://twitter.com">Twitter</a>
        <a href="https://instagram.com">Instagram</a>
      </div>
    </footer>
  );
};

export default Footer;
import React from 'react';

const ProductList = () => {
  const products = [
    { id: 1, name: 'Smartphone', price: '$499', image: '/images/smartphone.jpg' },
    { id: 2, name: 'Laptop', price: '$899', image: '/images/laptop.jpg' },
    // Add more products here
  ];

  return (
    <div className="product-list">
      {products.map(product => (
        <div className="product" key={product.id}>
          <img src={product.image} alt={product.name} />
          <h3>{product.name}</h3>
          <p>{product.price}</p>
          <button>Add to Cart</button>
        </div>
      ))}
    </div>
  );
};

export default ProductList;
const express = require('express');
const app = express();
const cors = require('cors');
const bodyParser = require('body-parser');

app.use(cors());
app.use(bodyParser.json());

const products = [
  { id: 1, name: 'Smartphone', price: 499 },
  { id: 2, name: 'Laptop', price: 899 },
  // Add more products here
];

app.get('/api/products', (req, res) => {
  res.json(products);
});

app.listen(5000, () => {
  console.log('Server is running on port 5000');
});
body {
  font-family: Arial, sans-serif;
}

.header, .footer {
  background-color: #232f3e;
  color: white;
  padding: 10px 0;
}

.header .logo a {
  font-size: 24px;
  text-decoration: none;
  color: white;
  margin-left: 20px;
}

.header .search-bar input {
  padding: 8px;
  width: 250px;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin-top: 20px;
}

.product {
  border: 1px solid #ddd;
  padding: 15px;
  width: 200px;
  margin-bottom: 20px;
}

.product img {
  width: 100%;
}

.footer {
  text-align: center;
  padding: 20px;
}
const stripe = require('stripe')('your-stripe-secret-key');
const YOUR_DOMAIN = 'http://localhost:3000';

app.post('/create-checkout-session', async (req, res) => {
  const session = await stripe.checkout.sessions.create({
    payment_method_types: ['card'],
    line_items: req.body.items.map(item => ({
      price_data: {
        currency: 'usd',
        product_data: {
          name: item.name,
        },
        unit_amount: item.price * 100, // amount in cents
      },
      quantity: item.quantity,
    })),
    mode: 'payment',
    success_url: `${YOUR_DOMAIN}/success`,
    cancel_url: `${YOUR_DOMAIN}/cancel`,
  });

  res.json({ id: session.id });
});
const handleCheckout = async () => {
  const response = await fetch('/create-checkout-session', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ items: cartItems }),
  });
npm install stripe
  const session = await response.json();
  const stripe = Stripe('your-stripe-publishable-key');
  await stripe.redirectToCheckout({ sessionId: session.id });
};
