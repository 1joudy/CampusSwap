# CampusSwap

**Course:** Electronic Business Development (BINF 503) · Winter 2026
**Instructor:** Dr. Nourhan Hamdi
**Teaching Assistants:** Mr. Nour Gaser, Mr. Omar Alaa, Ms. Mariam Khalid, Mr. Younes Zied, Mr. Mahmoud Hani

## 1. Team

| Name | Student ID | Tutorial group | GitHub username |
|---|---|---|---|
| Joudy Ahmed  | 16001356 | T-6 | 1joudy |
| Norin Ashraf | 16007765 | T-6 | norinashraf |
| Jana Galal   | 16001815 | T-9 | janagalal12 |


## 2. The idea

CampusSwap is a web marketplace where university students buy and sell used textbooks, electronics, furniture and other items with each other. Today students do this in crowded WhatsApp and Facebook groups, where posts get lost within hours, prices are negotiated in scattered private messages, and there is no way to know whether a seller can be trusted. CampusSwap gives every item its own listing, keeps all offers for an item in one place, and lets buyers rate sellers after a sale, so students can find what they need cheaply and sell what they no longer use.

## 3. Features

### 3.1 The full product

- Register and log in with a name, email and password
- Post a listing with title, description, price, category, condition and a photo
- Edit, delete and manage my own listings
- Browse all available listings on the home page
- Search listings by keyword and filter by category, condition and price range
- Make an offer on a listing, with a price and a short message
- Accept or reject offers on my listings, which marks the item as sold
- Rate and review a seller after buying from them
- Seller profile page with their listings, reviews and average rating
- Save listings to a wishlist
- In-app chat between buyer and seller
- Email and in-app notifications for new offers and accepted offers
- Sign-up restricted to verified university email addresses
- Online payment through an Egyptian payment gateway, held until the buyer confirms receipt
- Suggested safe meeting points on campus for handing over items
- Report a listing or user, with an admin panel to remove them
- Mobile app for Android and iOS

### 3.2 The use cases we will build

| # | Use case | What the user does | What they see | What is stored |
|---|---|---|---|---|
| 1 | Registration and login | Registers with name, email and password; logs in with email and password | Their home page, with their name in the header | A User, with the password hashed |
| 2 | Post a listing | Fills in a form with title, description, price, category, condition and an optional image link, then submits it | The new listing on its own page and on the home page | A Listing linked to the seller, with status "available" |
| 3 | Manage my listings | Opens "My listings", edits a listing's details or price, or deletes it | The updated listing, or the list without the deleted one | The Listing updated or deleted; only its seller can do this |
| 4 | Make an offer | Opens a listing and offers a price with a short message; can withdraw the offer while it is still pending | Their offer under "My offers", with status "pending" | An Offer linked to the listing and the buyer; deleted if withdrawn |
| 5 | Respond to offers | As a seller, opens the offers on one of their listings and accepts or rejects each one | The offer's new status; after accepting, the listing shows "Sold" | Offer.status set to "accepted" or "rejected"; on accept, Listing.status set to "sold" and Listing.buyer set |
| 6 | Review a seller | As a buyer whose offer was accepted, rates the seller from 1 to 5 with a comment | The review on the seller's profile, with their average rating | A Review linked to the listing, the seller and the reviewer; one review per buyer per listing |

## 4. Who owns what

| Member | Use case |
|---|---|
| Jana Galal | 1. Registration and login |
| Jana Galal  | 2. Post a listing |
| Joudy Ahmed  | 3. Manage my listings |
| Joudy Ahmed  | 4. Make an offer |
| Norin Ashraf | 5. Respond to offers |
| Norin Ashraf | 6. Review a seller |

## 5. Data model

### User

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }, // stored hashed, never as plain text
}, { timestamps: true });
```

### Listing

```js
const listingSchema = new mongoose.Schema({
  title: { type: String, required: true },
  description: { type: String },
  price: { type: Number, required: true, min: 0 },
  category: {
    type: String,
    enum: ['Books', 'Electronics', 'Furniture', 'Clothing', 'Other'],
    required: true,
  },
  condition: {
    type: String,
    enum: ['New', 'Like new', 'Good', 'Fair'],
    required: true,
  },
  imageUrl: { type: String },
  seller: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  status: { type: String, enum: ['available', 'sold'], default: 'available' },
  buyer: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
}, { timestamps: true });
```

### Offer

```js
const offerSchema = new mongoose.Schema({
  listing: { type: mongoose.Schema.Types.ObjectId, ref: 'Listing', required: true },
  buyer: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  amount: { type: Number, required: true, min: 0 },
  message: { type: String },
  status: {
    type: String,
    enum: ['pending', 'accepted', 'rejected'],
    default: 'pending',
  },
}, { timestamps: true });
```

### Review

```js
const reviewSchema = new mongoose.Schema({
  listing: { type: mongoose.Schema.Types.ObjectId, ref: 'Listing', required: true },
  seller: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  reviewer: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  rating: { type: Number, required: true, min: 1, max: 5 },
  comment: { type: String },
}, { timestamps: true });
```

## 6. Run it locally

we follow these steps to run your project.

### Back end

```bash
cd backend
npm install
# create backend/.env from backend/.env.example (see the table below)
npm start
```

The API runs on http://localhost:3000.

| Variable in `.env` | What it is |
|---|---|
| `MONGODB_URI` | The connection string of your MongoDB Atlas database |
| `JWT_SECRET` | Any long random text, used to sign login tokens |


### Front end

```bash
cd frontend
npm install
npm run dev
```

The app runs on [the address `npm run dev` prints, e.g. http://localhost:5173].
