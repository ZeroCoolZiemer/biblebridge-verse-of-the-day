# BibleBridge – Verse of the Day (JavaScript)

A minimal, production-safe JavaScript reference demonstrating how to fetch a daily Scripture verse using the BibleBridge JSON API.

This example is designed to be read, copied, and embedded directly into an existing Node.js application or scheduled job.

Canonical reference:  
https://holybible.dev/use-cases/verse-of-the-day

---

## Overview

A “Verse of the Day” feature is commonly used in church websites, devotionals, notification systems, and messaging applications.

This example demonstrates a predictable and canonical approach to daily verse selection without relying on fully random Scripture output.

The focus is on **Scripture access and API behavior**, not frameworks, bots, or delivery channels.

---

## Why Curated Verses?

Fully random verse selection can surface partial sentences, genealogies, or legal material that may be unsuitable for daily presentation.

Most production systems therefore select from a curated or categorized verse set while retaining full canonical access for advanced use cases.

---

## Example: Verse of the Day (Node.js)

```js
const API_BASE = "https://holybible.dev/api";
const API_KEY = process.env.BIBLEBRIDGE_API_KEY;

// Canonical verse whitelist (bookID 1–66)
const VERSES = [
  { bookID: 19, chapter: 23, verse: 1 },  // Psalm 23:1
  { bookID: 43, chapter: 3, verse: 16 },  // John 3:16
  { bookID: 20, chapter: 3, verse: 5 }   // Proverbs 3:5
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
  "data": {
    "verse": 16,
    "text": "For God so loved the world..."
  }
}
