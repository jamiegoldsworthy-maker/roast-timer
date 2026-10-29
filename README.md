# roast-timer
🍗 RoastTimer — Multi-Dish Dinner Coordinator PWA

RoastTimer is a real-time, multi-device kitchen coordinator and backwards-scheduling Progressive Web App (PWA) designed to take the stress out of cooking complex roast dinners, festive feasts, and multi-component meals.

Enter your Target Serve Time (e.g., 14:30), select your dishes, and RoastTimer works backward from the plate—calculating every prep task, preheat requirement, concurrent sub-step, and resting window so that every dish lands on the table hot and on time.

🔗 Live App: https://jamiegoldsworthy-maker.github.io/roast-timer/

✨ Key Features

1. Backwards Scheduling & Timeline Engine

Target Serve Time Anchoring: Set your desired serve time (or tap the orange Serve HH:MM badge in the header) and all dish steps automatically calculate backward.

Automatic Final 'Serve & Plating' Step: Appends a final 2-minute plating/serve step to the end of each component's timeline so every item has a clear finish line.

Solo Oven Slot Support (soloOvenSlot): Dishes that require exclusive, high-heat oven access at the end of the cook (such as Giant Yorkshire Puddings at $230^\circ\text{C}$) automatically shift all other oven dishes earlier so they finish and rest while the solo item bakes undisturbed.

Concurrent Sub-Steps (1a, 1b): Break complex steps into parallel sub-tasks (e.g., simmering potatoes while preheating the roasting tray with goose fat) that automatically align to finish together at the end of the parent step.

Intelligent Simultaneous Step Merging: When multiple dishes have steps scheduled for the exact same time (such as turning potatoes and glazing parsnips), RoastTimer merges them into a unified Simultaneous card with both a master group checkbox and individual dish checkboxes. Filtering by a single dish automatically unmerges them.

2. Multi-Oven Management & Auto-Preheat

Oven 1 (Main) & Optional Oven 2 (Second Oven): Configure up to two ovens with custom names, default temperatures, and Fan Assisted vs. Conventional modes.

Automatic Preheat Steps: Generates a dedicated Preheat Oven step (30 mins before the earliest oven step by default, adjustable from 5 to 90 minutes) displaying the exact required temperature (including automatic $+20^\circ\text{C}$ conversion notes for conventional ovens).

Live Oven Switching: Assign dishes or individual steps to Oven 1, Oven 2, or Hob / No Oven in the component builder, or swap a step between ovens on the fly from the ••• step menu on the Live Steps screen.

3. Live Steps Execution & Focus Tools

Calm, Minimalist Slate & Orange UI: Designed for high-contrast readability in a busy kitchen. Toggle between Minimal and Detailed card views.

Isolate / Focus Mode: Tap Isolate on any step card to pop it out over a blurred backdrop with a large live countdown timer, sub-step checkboxes, end instructions, and quick -5m / +5m controls.

Delay & Cascade Engine: Running behind or ahead on a step? Open the Cascade Delay modal and choose between:

Dish-Only Shift: Shift subsequent steps for that specific dish only.

Catch-Up Override: Keep subsequent steps locked to their original times.

Global Meal Shift: Push the entire meal's Target Serve Time back and realign every dish.

Audio & Haptic Feedback: Synthesizes a clean Web Audio completion "whoosh" sound when ticking off steps and a harmonic three-note kitchen bell chime for step alarms (zero external audio files required).

Screen Wake Lock: Utilizes the browser navigator.wakeLock API to prevent your phone, tablet, or Chromebook screen from dimming or locking while cooking.

4. Customizable Alerts & Lead Times

Visual Notifications: Choose between a Full-Screen Takeover or a compact Popup Banner, with configurable lead times (At step start (0s), 15s, 30s, 1m, 2m, 5m, or 10m before a step starts).

Audio Chime Alarms: Independently toggle the audio chime and set its lead time (defaults to 30 sec before).

Per-Step Overrides: Customize or mute notifications and alarms for any individual step via its ••• action drawer.

5. Items Library & Custom Categories (Screen 2)

Reusable Dish Components: Build your personal library of dishes with badge colors, default oven assignments, soloOvenSlot toggles, multi-line ingredients, technique notes, and ordered steps (h & m duration inputs).

Customizable Categories: Organize dishes into categories (Meat, Poultry, Potatoes, Veg, Sides & Yorkshires, Gravy & Sauces, or create your own).

Multi-Select Active Dishes Dropdown: Quickly tick or untick active dishes from the Burger Menu (☰) using a searchable, category-grouped multi-select dropdown.

One-Click Sample Cleanup: Permanently remove the 5 built-in sample dishes and sample presets whenever you're ready to use exclusively your own recipes.

6. Meal Planner, Presets & Shopping List (Screen 3)

Save & Load Meal Presets: Save complete roast lineups and target serve times (e.g., Sunday Beef Roast, Christmas Dinner) for one-click loading.

Consolidated Shopping & Prep Drawer: Automatically aggregates all ingredients across every active dish in your current session.

🏗️ Tech Stack & Architecture

Single-File Client Architecture: Built inside index.html using React 18, Tailwind CSS, Babel Standalone, and Lucide Icons—requiring zero local build steps or Node.js bundlers.

Web Audio API: Pure procedural audio synthesis (AudioContext) for completion whooshes and harmonic kitchen chimes.

Dual-Layer Persistence & Real-Time Sync:

Local-First (localStorage + Persistent Storage API): Every edit saves synchronously to localStorage with navigator.storage.persist() protection and millisecond timestamps (roasttimer_updated_at).

Google Firebase Cloud Firestore: Real-time two-way synchronization (onSnapshot) across all devices sharing the same Kitchen PIN (e.g., ROAST1).

Portable JSON Backups: One-click Export Backup (.json) and Import Backup in the Burger Menu back up all custom categories, library items, presets, and active session states.

🚀 Deployment Guide (GitHub Pages + Firebase)

1. Repository File Structure

Ensure the root of your GitHub repository contains these three files:

roast-timer/
├── index.html       # Complete RoastTimer application & Firebase client
├── manifest.json    # Web App Manifest for standalone PWA installation
├── icon-512.png     # 512x512 PNG Roast Chicken app & favicon icon
└── README.md        # Project documentation



2. Firebase Cloud Firestore Setup (Free Spark Plan)

Go to the Firebase Console and create a project (e.g., roast-timer-sync).

Enable Anonymous Auth: Navigate to Build $\rightarrow$ Authentication $\rightarrow$ Sign-in method, enable Anonymous, and click Save.

Create Firestore Database: Navigate to Build $\rightarrow$ Firestore Database, click Create database, and select Start in test mode (or apply the security rules below).

Firestore Security Rules: In the Rules tab, allow read/write access to your kitchen sync path:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/public/data/kitchens/{pin} {
      allow read, write: if request.auth != null;
    }
  }
}



Add Web App Config: In Project Settings $\rightarrow$ Your apps, register a Web App (</>), copy your firebaseConfig object, and paste it into CUSTOM_FIREBASE_CONFIG inside index.html.

Authorized Domains: Under Authentication $\rightarrow$ Settings $\rightarrow$ Authorized domains, add your GitHub Pages domain (jamiegoldsworthy-maker.github.io).

3. Hosting on GitHub Pages

Push index.html, manifest.json, and icon-512.png to the main branch of your GitHub repository.

In your repository on GitHub, go to Settings $\rightarrow$ Pages.

Under Build and deployment, set Source to Deploy from a branch, select main and / (root), and click Save.

Your app will be live within 60 seconds at https://<username>.github.io/<repo-name>/.

📲 Installing as a Standalone PWA

📱 Android (Chrome)

Open https://jamiegoldsworthy-maker.github.io/roast-timer/ in Google Chrome on your Android phone or tablet.

Tap the Three Dots (⋮) menu in the top-right corner.

Tap Install app (or Add to Home screen $\rightarrow$ Install).

RoastTimer will install to your app drawer with the 🍗 icon and launch full-screen without browser address bars.

💻 Chromebook & Desktop (Chrome / Edge)

Open the live URL in Chrome or Edge.

Click the Install icon ([↓] monitor icon) on the right side of the address bar.

Click Install to pin RoastTimer to your shelf/taskbar as a dedicated window.

🍏 iOS (iPhone & iPad)

Open the live URL in Safari.

Tap the Share button at the bottom of the screen.

Scroll down and tap Add to Home Screen.

🔄 Syncing Across Devices in Your Kitchen

Open RoastTimer on both your phone and your desktop/Chromebook.

Click the Kitchen PIN badge in the top-right corner of the header (defaults to ROAST1).

Enter the same alphanumeric PIN on all devices and click Connect Kitchen PIN.

Any step checked off, delayed, or edited on one device will update across all other devices in under half a second.
