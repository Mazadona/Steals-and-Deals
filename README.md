# Steals & Deals

A personal marketplace built for selling second-hand items to Canva colleagues. Browse products, enquire or buy via Slack DM, and manage inventory through a built-in admin panel.

**Live site:** https://mazadona.github.io/Steals-and-Deals/

---

## How it works

The site is a single `index.html` file with no backend or build step. Product statuses (Available, Reserved, Sold) are stored in Firebase Realtime Database and loaded on every page visit, so all visitors see live inventory state.

---

## Buyer flow

1. Buyer browses the shop and finds an item they like
2. They click **Enquire** (for questions) or **Buy** (to purchase)
3. A modal appears with a pre-written message — they copy it and are taken directly to a Slack DM with the seller
4. The seller and buyer agree on the details in Slack
5. The seller marks the item as **Reserved** in the admin panel
6. Once the item is handed over, the seller marks it as **Sold**

---

## Seller / Admin panel

Access the admin panel by appending `?admin=true` to the site URL:

```
https://mazadona.github.io/Steals-and-Deals/?admin=true
```

From the admin panel you can:

- Toggle any item between **Available**, **Reserved**, and **Sold**
- Reserve an item for 2 hours (auto-expires back to Available)
- Copy a pre-written Reserved message to send the buyer via Slack
- Copy a Sold notification message
- Copy a buyer confirmation link
- View recent enquiries

---

## Adding a new product

1. Copy the product images into `/shop/` (the same folder as `index.html`)
2. Open `index.html` and find the `products` array (around line 600)
3. Add a new entry following this pattern:

```javascript
{
  id: 25,                          // next sequential ID
  name: "Product Name",
  desc: "Short description here.",
  price: 50.00,
  origPrice: 100.00,               // set to null if no original price
  images: ["photo1.jpg", "photo2.jpg"],  // files must be in the /shop/ folder
  category: "home",                // electronics | fashion | home | sports | beauty | baby
  condition: "Like New",           // New | Like New | Good
  badge: "Sale",                   // optional — remove this line if not needed
},
```

4. Save `index.html`
5. Push to GitHub:

```bash
cd /Users/mazenelaasser/work/shop
git add index.html photo1.jpg photo2.jpg
git commit -m "Add new product: Product Name"
git push
```

The site updates automatically within 1–2 minutes.

---

## Updating an existing product

Open `index.html`, find the product by name in the `products` array, and edit the relevant fields (`price`, `desc`, `condition`, etc.). Then push:

```bash
git add index.html
git commit -m "Update product: Product Name"
git push
```

---

## Changing a product status manually (without admin panel)

You can change status directly in Firebase if needed:

- Go to [Firebase Console](https://console.firebase.google.com/) → **steals-and-deals-f7545** → Realtime Database
- Under `statuses`, set the product's ID key to `"available"`, `"reserved"`, or `"sold"`

---

## Config reference

All key settings are at the top of the `<script>` section in `index.html`:

| Constant | What it does |
|---|---|
| `FIREBASE_URL` | Firebase Realtime Database URL |
| `SLACK_DM_URL` | Deep link that opens Slack DM with the seller |
| `SELLER_NAME` | Seller's name shown in pre-written messages |
| `RESERVATION_HOURS` | How long a reservation lasts before auto-expiring |

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (single file) |
| Hosting | GitHub Pages |
| Database | Firebase Realtime Database (REST API) |
| Messaging | Slack deep links |

---

## Firebase rules

The database is configured to only allow reads and writes on the three paths the site uses:

```json
{
  "rules": {
    "statuses":     { ".read": true, ".write": true },
    "reservations": { ".read": true, ".write": true },
    "enquiries":    { ".read": true, ".write": true }
  }
}
```

---

## Pickup & delivery

- **Pickup:** Waterloo NSW 2017 — available for all items
- **Delivery:** Surry Hills office — available for select items, upon agreement
