# 🧠 Brain Cash — Trivia & Earn on World App

> Answer trivia questions. Earn real WLD tokens. Play with millions of verified humans.

---

## 💡 What Is This?

Brain Cash is a **play-to-earn trivia mini app** built for World App (Worldcoin).  
Players answer multiple-choice questions across 8 categories and earn **WLD tokens** based on:
- Correct answers
- Streak bonuses (consecutive correct)
- Speed bonuses (answer fast)

**Revenue for you (the developer):**
1. **Rewarded Ads** — Players watch ads to earn WLD (you get the ad revenue)
2. **Power-Up Shop** — Players spend WLD on in-game upgrades
3. **Daily Prize Pool** — You collect a small platform fee from the prize pool
4. **Sponsorships** — Question categories can be sponsored by brands

---

## 🚀 Quick Start (Local Dev)

```bash
# Just open in browser — no build step needed
open index.html

# Or serve locally
npx serve .
# → http://localhost:3000
```

---

## 📱 Deploy to World App (Step by Step)

### Step 1 — Get Your App ID

1. Go to **https://developer.worldcoin.org**
2. Create a new app: `New App → Mini App`
3. Set **App URL** to your deployed URL (see Step 3)
4. Copy your **App ID** (looks like `app_abc123...`)

### Step 2 — Add Your App ID

Open `index.html` and find this line:

```javascript
// TODO: Replace with your actual App ID from World Developer Portal
// await MiniKit.install('app_YOUR_APP_ID_HERE');
```

Replace with:

```javascript
await MiniKit.install('app_YOUR_REAL_APP_ID');
```

### Step 3 — Deploy (Choose One)

**Option A — Vercel (Recommended, Free)**
```bash
npm install -g vercel
vercel
# Follow prompts → copy your URL
```

**Option B — GitHub Pages**
```bash
git init
git add .
git commit -m "brain cash v1"
git remote add origin https://github.com/YOUR_USERNAME/brain-cash.git
git push -u origin main
# Enable Pages in repo Settings → Pages → main branch / root
```

**Option C — Netlify**
```bash
npx netlify-cli deploy --dir . --prod
```

### Step 4 — Add Your Wallet Address

In `index.html`, find the `confirmClaim` function and update the `to` field:

```javascript
to: 'YOUR_WORLDCOIN_WALLET_ADDRESS_HERE',
```

This is where the player rewards will be routed FROM (your treasury wallet).

### Step 5 — Submit to World App

1. Go back to **developer.worldcoin.org**
2. In your app settings, paste your deployed URL
3. Set the Worldcoin action name: `brain-cash-play`
4. Submit for review

---

## 💰 Monetization Strategy

### Revenue Streams

| Stream | How | Est. Revenue |
|--------|-----|-------------|
| Rewarded Ads | 3 ads/day per user × DAU | $0.01–0.05/user/day |
| Power-Up Shop | WLD micro-purchases | 5–15% of earnings recycled |
| Daily Challenge Pool | 10% platform fee | Scales with prize pool |
| Sponsored Categories | Brand pays per question | $50–500/category/month |

### Prize Pool Model
- Daily Challenge: collect entry signal → top 10% share 70% of ad revenue that day
- You keep 30% as platform margin

### Scaling
- **10K DAU** → ~$100–500/day from ads alone
- **100K DAU** → ~$1,000–5,000/day
- Power-ups create a WLD flywheel: players earn → spend → earn more → addicted

---

## 🏗️ Architecture

```
brain-cash/
├── index.html          # Complete single-file app (HTML + CSS + JS)
├── README.md           # This file
└── (future)
    ├── api/
    │   ├── verify.js   # World ID verification backend
    │   ├── claim.js    # Payment processing
    │   └── scores.js   # Leaderboard API
    └── questions.json  # External question bank (expand to 1000+)
```

### Tech Stack
- **Frontend**: Vanilla HTML/CSS/JS (no framework needed — fast load in World App)
- **Wallet**: Worldcoin MiniKit SDK
- **Verification**: World ID (Orb level — sybil-proof)
- **Payments**: MiniKit `pay` command (WLD token transfers)
- **Fonts**: Bebas Neue + DM Sans (Google Fonts)

---

## 🔧 Customization Guide

### Adding More Questions

Find the `QUESTION_BANK` array in `index.html` and add objects:

```javascript
{ 
  cat: 'Your Category', 
  q: 'Your question text?', 
  opts: ['Option A', 'Option B', 'Option C', 'Option D'], 
  ans: 0,  // index of correct answer (0-3)
  fact: 'Interesting fact shown after answer.' 
},
```

### Adjusting Reward Rates

Find `modeRates` in the `showResult` function:

```javascript
const modeRates = { quick: 0.01, daily: 0.04, streak: 0.02, ranked: 0.035 };
```

- Increase to attract more players
- Decrease to improve margins
- Keep daily ≥ 4x quick to incentivize daily play

### Adding Real Ads

Replace the `watchAd()` function stub with a real ad SDK:

```javascript
// Unity Ads
UnityAds.showAd('rewardedVideo', {
  onComplete: () => { STATE.pendingBalance += 0.02; updateWalletUI(); }
});

// Or Google AdMob (if WebView)
// Or ironSource, AppLovin MAX, etc.
```

---

## 🌍 World App Integration Notes

### MiniKit Commands Used
- `MiniKit.install(appId)` — Initialize the SDK
- `MiniKit.commandsAsync.verify()` — World ID verification
- `MiniKit.commandsAsync.pay()` — WLD token payments

### Verification Levels
- `device` — Faster, less secure (good for first run)
- `orb` — Full biometric verification (required for real payouts)

### Testing in World App
1. Install World App on your phone
2. Go to **Explore → Developer Tools**  
3. Enter your localhost URL (use ngrok for local testing: `npx ngrok http 3000`)
4. Test all flows

---

## 📊 Analytics Events to Track

Add these to understand player behavior:

```javascript
// When player starts a game
trackEvent('game_start', { mode: STATE.gameMode });

// When player answers
trackEvent('answer', { correct: isCorrect, streak: STATE.streak });

// When player claims rewards
trackEvent('claim', { amount: STATE.pendingBalance });

// When player watches ad
trackEvent('ad_watched', { count: STATE.adsWatched });
```

Use **Mixpanel**, **PostHog**, or **Amplitude** (all free tiers available).

---

## 🔐 Security Notes

- Never store private keys in the frontend
- All WLD transfers should be validated server-side
- Use World ID verification to prevent bot farming
- Implement rate limiting on claim API (max 1 claim/hour)
- Questions should eventually come from a server (prevent cheating)

---

## 📈 Growth Hacks Built In

1. **Streak System** — Creates daily habit, FOMO if streak broken
2. **Leaderboard** — Social competition drives viral sharing
3. **Share Button** — Players share scores to social media
4. **Daily Challenge** — Only 1 attempt/day = must come back tomorrow
5. **Power-Up Economy** — Spending WLD on powerups = more addicted to earning

---

## 📝 License

MIT — Use freely, modify, deploy, profit. Attribution appreciated.

---

## 💬 Support

Issues? Feature requests? Open a GitHub issue or reach out.

**Built with ❤️ for the Worldcoin ecosystem.**
