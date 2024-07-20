const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

module.exports = async (req, res) => {
  const { priceId } = req.body;

  const session = await stripe.checkout.sessions.create({
    payment_method_types: [
      'card', 
      'klarna', 
      'afterpay_clearpay', 
      'affirm',
      'alipay',
      'apple_pay',
      'google_pay'
    ],
    line_items: [{
      price: priceId,
      quantity: 1,
    }],
    mode: 'payment',
    success_url: `${req.headers.origin}/success.html`,
    cancel_url: `${req.headers.origin}/cancel.html`,
  });

  res.json({ id: session.id });
};
