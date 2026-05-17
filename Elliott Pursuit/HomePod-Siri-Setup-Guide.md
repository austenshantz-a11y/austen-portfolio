# Elliott's Learning Game — HomePod & Siri Setup Guide

This guide sets up an Apple Shortcut so Elliott can ask the HomePod to play her learning game and hear a fact out loud.

---

## How It Works

1. Elliott says **"Hey Siri, play Elliott's Learning Game"** to the HomePod
2. Siri picks a random category and reads a fact from it
3. Siri asks if she wants it repeated
4. The fact is selected from the same pool the app uses — cycling through all facts over time

> **Note:** Because the HomePod and the app run separately, the Shortcut maintains its own record of which facts have been read. After each session, check the app and manually mark any facts she learned via the HomePod as learned in the app.

---

## Step 1: Create the Shortcut on iPhone

1. Open the **Shortcuts** app on the iPhone that is signed into the same Apple ID as the HomePod
2. Tap **+** to create a new shortcut
3. Tap the shortcut name at the top and rename it: **Elliott's Learning Game**

---

## Step 2: Add These Actions (in order)

### Action 1 — Get the fact database from iCloud
- Add action: **Get File**
- Set path: `iCloud Drive/Shortcuts/elliottfacts.json`
- Turn ON: "Error if Not Found" OFF (so first run creates it)

### Action 2 — Check if file exists
- Add action: **If** (condition: `Shortcut Input` has any value)
  - In the "Otherwise" branch: Add **Text** action with this JSON and name it "Default Data":
    ```json
    {"usedIds":[],"lastCat":""}
    ```
  - Add **Set Variable** → name: `GameData`, value: `Default Data`
- In the "If" branch:
  - Add **Get Dictionary from Input** (input: the file from Step 1)
  - Add **Set Variable** → name: `GameData`, value: the dictionary

### Action 3 — Pick a random category
- Add action: **List** with these 6 items:
  - `animals`
  - `art`
  - `numbers`
  - `stories`
  - `world`
  - `music`
- Add action: **Get Random Item from List**
- Add **Set Variable** → name: `ChosenCat`

### Action 4 — Category name and color for Siri to say
- Add action: **If** (variable: `ChosenCat` contains `animals`) → **Text**: `The Animal Kingdom 🦁`
- Repeat similar If/Text blocks for each category:
  - `art` → `Colors and Art 🎨`
  - `numbers` → `Numbers and Shapes 🔢`
  - `stories` → `Stories and Books 📚`
  - `world` → `The World 🌍`
  - `music` → `Music and Movement 🎵`
- Add **Set Variable** → name: `CatLabel`

### Action 5 — Say the category name
- Add action: **Speak Text**
- Text: `Elliott! Get ready! Today's category is... [CatLabel]!`
- Voice: Siri voice (default), Rate: Slow (for 5-year-old)

### Action 6 — Speak the fact
*(Add one Text block per category with 3 facts each, using an If/Else chain on `ChosenCat`. Pick one fact randomly from each group using "Get Random Item from List" on a list of 3 facts.)*

Example for Animals:
- **Text** (name: `AnimalsFacts`):
  - Item 1: `Butterflies taste with their feet! Their tiny feet can tell if a leaf is yummy enough to eat.`
  - Item 2: `Sea otters hold hands when they sleep so they don't float away from each other.`
  - Item 3: `Elephants are the biggest animals on land. Baby elephants can walk just hours after being born!`
- **Get Random Item from List** (input: `AnimalsFacts`)
- **Set Variable** → name: `TodaysFact`
- Add **Speak Text**: `[TodaysFact]`

### Action 7 — Ask if she wants it repeated
- Add action: **Ask for Input**
  - Prompt: `Did you want me to say that again? Say yes or no.`
  - Input type: Text
- Add **If** (input contains `yes`)
  - Add **Speak Text**: `[CatLabel]! [TodaysFact]`

### Action 8 — Closing
- Add action: **Speak Text**: `Great listening, Elliott! Don't forget to check the app with Mom or Dad to mark what you learned! Bye for now! ⭐`

---

## Step 3: Enable on HomePod

1. On iPhone, go to **Settings → Siri & Search**
2. Make sure **"Use with HomePod"** is turned on
3. In the Shortcuts app, open **Elliott's Learning Game**
4. Tap the settings icon (ⓘ) → turn on **"Use with Siri"**
5. Confirm the phrase: **"Play Elliott's Learning Game"**

---

## Step 4: Test It

Say to your HomePod: **"Hey Siri, play Elliott's Learning Game"**

Siri should announce a category, read the fact, then ask if she wants it repeated.

---

## Syncing with the App

After Elliott hears a fact on the HomePod:
- Open **Elliott's Learning Game** on iPhone
- Tap **⭐ Learn Today's Facts**
- Tap the matching category, read the same fact, and tap **"I learned it!"**
- This marks it in the app so she can answer the quiz question and earn her pie piece

---

## Tips

- **Best HomePod placement:** In a common area at Elliott's height so Siri can hear her clearly
- **Slow the speech rate** in the Speak Text action — young kids benefit from slower pacing
- **Run it at the same time each day** to build a routine (e.g., after breakfast)
- The Shortcut can be updated over time with more facts from the app's database

