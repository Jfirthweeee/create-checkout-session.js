const stripe = require('stripe')('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');

module.exports = async (req, res) => {
    if (req.method === 'POST') {
        const { priceId } = req.body;

        try {
            const session = await stripe.checkout.sessions.create({
                payment_method_types: ['card', 'klarna', 'afterpay_clearpay', 'google_pay', 'apple_pay'],
                line_items: [
                    {
                        price: priceId,
                        quantity: 1,
                    },
                ],
                mode: 'payment',
                success_url: 'https://www.1000beaute.com/success',
                cancel_url: 'https://www.1000beaute.com/cancel',
            });

            res.status(200).json({ id: session.id });
        } catch (error) {
            res.status(500).json({ error: error.message });
        }
    } else {
        res.setHeader('Allow', 'POST');
        res.status(405).end('Method Not Allowed');
    }
};
# create-checkout-session.js
