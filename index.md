---
layout: home
title: "Polibyus"
description: "Polibyus is a cipher table created based on the Polybius square."
---

## Contents
 1. [Polibyus Table](#1-polibyus-table)
 2. [Examples](#2-examples)
 3. [Cipher Generator](#2-cipher-generator)

<br>

## 1. Polibyus Table

| | 1 | 2 | 3 | 4 | 5 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | a | b | c | d | e |
| 2 | f | g | h | i | j |
| 3 | k | l | m | n | o |
| 4 | p | r | s | t | u |
| 5 | v | w | x | y | z |

> **Special Rule:** q = 9

<br>

## 2. Examples
To encrypt a message, find each character in the table and write its row number followed by its column number.

**442315 7 945241331 1242355234 223553 2545334143 35511542 442315 32115554 143522.**

> Plaintext: "The 7 quick brown fox jumps over the lazy dog."

<br>

## 3. Cipher Generator

<div id="cipher-app" style="max-width: 600px; margin: 20px auto; font-family: sans-serif;">
  <label for="input-text">Enter English text:</label>
  <textarea id="input-text" rows="4" style="width: 100%; padding: 10px; margin-top: 10px; border: 1px solid #ccc; border-radius: 4px;" placeholder="Type your message here..."></textarea>
  <button onclick="encrypt()" style="margin-top: 10px; padding: 10px 20px; background-color: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer;">Encrypt</button>
  <h3 style="margin-top: 30px;">Result (Polibyus Cipher):</h3>
  <div id="output-text" style="padding: 15px; background: #f8f9fa; border: 1px dashed #007bff; min-height: 50px; word-break: break-all; font-family: monospace; font-size: 1.2em;"></div>
</div>

<script>
function encrypt() {
  const table = {
    'a': '11', 'b': '12', 'c': '13', 'd': '14', 'e': '15',
    'f': '21', 'g': '22', 'h': '23', 'i': '24', 'j': '25',
    'k': '31', 'l': '32', 'm': '33', 'n': '34', 'o': '35',
    'p': '41', 'r': '42', 's': '43', 't': '44', 'u': '45',
    'v': '51', 'w': '52', 'x': '53', 'y': '54', 'z': '55',
    'q': '9'
  };

  const rawInput = document.getElementById('input-text').value;
  let text = rawInput.normalize('NFKC').toLowerCase();
  
  // アルファベットを一つずつ数字に変換する
  let result = "";
  for (let char of text) {
    if (table[char]) {
      result += table[char];
    } else {
      result += char;
    }
  }

  // 数字の塊を見つけて前後にスペースを入れる
  let step1 = result.replace(/([0-9]+)/g, " $1 ");
  // 連続してしまったスペース（"  "）を一つ（" "）にまとめる
  let step2 = step1.replace(/\s+/g, " ");
  // 最初と最後にある余計なスペースを削って完成！
  let finalOutput = step2.trim();

  document.getElementById('output-text').innerText = finalOutput;
}
</script>

---
