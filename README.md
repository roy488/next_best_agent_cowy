# Summit Sales Router - Production Router Upgrade

This version moves routing assignment into Firebase Functions so multiple telemarketers can use the app at the same time without client-side race conditions.

## What changed
- Next-agent selection is now server-side via the `advanceRouter` callable.
- The function logs the event, updates stats, applies skip penalties, and reserves the next agent in one transaction.
- The client no longer writes transfer events or stats directly.
- Firestore rules are tightened so direct client writes to `events`, `stats`, and `sessions` are blocked.

## Seed agents
In Admin > Agents, click **Seed Defaults** if the list is empty.
Default weights:
- Brady 30
- Hayden 30
- Nate 25
- Cullen 15

## Deploy today
### 1. Install root app dependencies
```bash
npm install
```

### 2. Install Firebase Functions dependencies
```bash
cd functions
npm install
npm run build
cd ..
```

### 3. Log into Firebase
```bash
firebase login
firebase use <your-project-id>
```

### 4. Deploy Firestore rules and functions
```bash
firebase deploy --only firestore:rules,functions
```

### 5. Build and deploy hosting
```bash
npm run build
firebase deploy --only hosting
```

## Local development
### Front-end
```bash
npm run dev
```

### Functions emulator
In another terminal:
```bash
cd functions
npm install
npm run build
firebase emulators:start --only functions
```

## Notes
- Telemarketers still sign in with Google.
- Admin access is based on `roy@mysummitinsuranceagency.com` in the client and Firestore rules.
- For a cleaner long-term setup, move admin authorization to custom claims later.
