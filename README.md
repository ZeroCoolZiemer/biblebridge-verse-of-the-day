# Node.js Verse of the Day API Example (KJV JSON)

This repository demonstrates how to build a **Verse of the Day feature in Node.js** using a **KJV Bible JSON API**.

It shows how to programmatically fetch a single Scripture verse and embed it into:

- Church websites  
- Devotional applications  
- Discord or Telegram bots  
- Scheduled cron jobs  
- Email automation systems  

If you are looking for a **free KJV Bible API with JSON output**, this example provides a minimal, production-safe reference implementation.

**Canonical documentation:**  
https://holybible.dev/use-cases/verse-of-the-day

---

## Overview

A “Verse of the Day” feature is commonly implemented using one of two approaches:

1. Fully random verse selection  
2. Curated verse selection  

This example demonstrates a predictable, curated approach using a predefined verse set while retaining full canonical API access for advanced use cases.

The focus here is on:

- Clean API usage  
- Stable JSON response handling  
- Production-safe implementation  
- No framework dependencies  

---

## Why Not Fully Random Verses?

Fully random verse selection can surface:

- Partial sentences  
- Genealogies  
- Legal material  
- Context-dependent passages  

Most production devotional systems instead select from a curated or categorized verse set while still allowing full canonical Scripture access elsewhere.

---

## Example: Verse of the Day (Node.js)

```js
const API_BASE = "https://holybible.dev/api/scripture";
const API_KEY = process.env.BIBLEBRIDGE_API_KEY;

// Canonical verse whitelist
const VERSES = [
  { bookID: 19, chapter: 23, verse: 1 },  // Psalm 23:1
  { bookID: 43, chapter: 3, verse: 16 },  // John 3:16
  { bookID: 20, chapter: 3, verse: 5 }    // Proverbs 3:5
];

function pickRandomVerse() {
  return VERSES[Math.floor(Math.random() * VERSES.length)];
}

async function fetchVerse(ref) {
  const params = new URLSearchParams({
    key: API_KEY,
    bookID: ref.bookID,
    chapter: ref.chapter,
    verse: ref.verse,
    version: "KJV"
  });

  const response = await fetch(`${API_BASE}?${params.toString()}`);

  if (!response.ok) {
    throw new Error("Failed to fetch verse");
  }

  return response.json();
}

async function run() {
  const ref = pickRandomVerse();
  const verse = await fetchVerse(ref);

  const reference =
    `${verse.book.name} ${verse.chapter}:${verse.data.verse}`;

  console.log(reference);
  console.log(verse.data.text);
}

run();
```
## Example API Response
```json
{
  "status": "success",
  "type": "single_verse",
  "version": "KJV",
  "book": {
    "id": 43,
    "name": "John"
  },
  "chapter": 3,
  "range": null,
  "results_count": 1,
  "data": {
    "verse": 16,
    "text": "For God so loved the world, that he gave his only begotten Son, that whosoever believeth in him should not perish, but have everlasting life."
  }
}
