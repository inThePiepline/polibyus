---
layout: home
title: "Polibyus"
description: "Polibyus is a cipher table created based on the Polybius square."
---

**4135322412544543 2443 11 132441231542 4411123215 13421511441514 1211431514 3534 442315 4135325412244543 43945114215.**

## Contents
 1. [Polibyus Table](#1-polibyus-table)
 2. [Examples](#2-examples)
 3. [Cipher / Decipher Generator](#3-cipher-decipher-generator)

<br>

## 1. Polibyus Table

| | 1 | 2 | 3 | 4 | 5 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | a | b | c | d | e |
| **2** | f | g | h | i | j |
| **3** | k | l | m | n | o |
| **4** | p | r | s | t | u |
| **5** | v | w | x | y | z |

> **Special Rule:** q = 9

<br>

## 2. Examples
To encrypt a message, find each character in the table and write its row number followed by its column number.

**442315 7 945241331 1242355234 213553 2545334143 35511542 442315 32115554 143522.**

> Plaintext: "The 7 quick brown fox jumps over the lazy dog."

<br>

## 3. Cipher / Decipher Generator

<div id="cipher-app">
  <label for="input-text" style="display: block; margin-bottom: 6px;"><strong>Enter text or cipher</strong></label>
  <textarea id="input-text" rows="6" style="width: 100%; box-sizing: border-box; border: 2px solid #ccc; border-radius: 4px; padding: 8px;" placeholder="Type English text to encrypt, or cipher numbers to decrypt..."></textarea>

  <div style="margin: 8px 0px;">
    <button type="button" onclick="encrypt()" style="margin-right: 6px; padding: 10px 20px; background-color: #184480; color: white; border: none; border-radius: 4px; cursor: pointer;">Encrypt</button>
    <button type="button" onclick="decrypt()" style="margin-right: 6px; padding: 10px 20px; background-color: #5c2b85; color: white; border: none; border-radius: 4px; cursor: pointer;">Decrypt</button>
    <button type="button" onclick="clearAll()" style="padding: 10px 16px; background-color: #666666; color: white; border: none; border-radius: 4px; cursor: pointer;">Clear</button>
  </div>

  <strong style="display: block; margin-top: 12px; margin-bottom: 6px;">Result</strong>
  <div id="output-text" style="padding: 18px; background: #f4f4f4; border: 2px solid #20b0e8; min-height: 50px; word-break: break-all;"></div>

  <div style="margin: 8px 0px;">
    <button type="button" onclick="copyToClipboard()" style="margin-right: 6px; padding: 10px 20px; background-color: #108050; color: white; border: none; border-radius: 4px; cursor: pointer;">Copy</button>
    <button type="button" onclick="useResultAsInput()" style="padding: 10px 16px; background-color: #4b6584; color: white; border: none; border-radius: 4px; cursor: pointer;">Use as Input</button>
    <span id="copy-message" style="margin-left: 10px; color: #40c840; display: none;">Copied!</span>
  </div>
</div>

<script>
// 暗号化用マッピング（アルファベット -> 2桁数字 / qは9）
const ENCRYPT_TABLE = {
  'a': '11', 'b': '12', 'c': '13', 'd': '14', 'e': '15',
  'f': '21', 'g': '22', 'h': '23', 'i': '24', 'j': '25',
  'k': '31', 'l': '32', 'm': '33', 'n': '34', 'o': '35',
  'p': '41', 'r': '42', 's': '43', 't': '44', 'u': '45',
  'v': '51', 'w': '52', 'x': '53', 'y': '54', 'z': '55',
  'q': '9'
};

// 復号用マッピング（2桁数字 / 9 -> アルファベット）
const DECRYPT_TABLE = {
  '11': 'a', '12': 'b', '13': 'c', '14': 'd', '15': 'e',
  '21': 'f', '22': 'g', '23': 'h', '24': 'i', '25': 'j',
  '31': 'k', '32': 'l', '33': 'm', '34': 'n', '35': 'o',
  '41': 'p', '42': 'r', '43': 's', '44': 't', '45': 'u',
  '51': 'v', '52': 'w', '53': 'x', '54': 'y', '55': 'z',
  '9': 'q'
};

const SPECIAL_Q_CIPHER = '9';

/**
 * 英語テキストを暗号化する
 * What: 入力された英文をポリュビオス暗号の数字列に変換する
 * Why: メッセージの暗号化を生成するため
 */
function encrypt() {
  const inputElement = document.getElementById('input-text');
  if (!inputElement) {
    return;
  }

  const rawInput = inputElement.value;
  const normalizedText = rawInput.normalize('NFKC').toLowerCase();

  let encryptedResult = "";
  for (let index = 0; index < normalizedText.length; index += 1) {
    const char = normalizedText[index];
    if (ENCRYPT_TABLE[char] !== undefined) {
      encryptedResult += ENCRYPT_TABLE[char];
    } else {
      encryptedResult += char;
    }
  }

  // 数字の塊の前後にスペースを挿入する
  const spacedText = encryptedResult.replace(/([0-9]+)/g, " $1 ");
  // 連続する余分なスペースを1つに統合する
  const trimmedText = spacedText.replace(/\s+/g, " ").trim();
  // 句読点（ピリオド・カンマ等）の直前に入った余計なスペースを詰める
  const finalText = trimmedText.replace(/\s+([.,!?;:])/g, "$1");

  const outputElement = document.getElementById('output-text');
  if (outputElement) {
    outputElement.innerText = finalText;
  }
}

/**
 * 暗号文を元の英語テキストに復号する
 * What: 数字文字列をポリュビオス暗号表に基づいてアルファベットに復元する
 * Why: 暗号化されたメッセージを誰でも簡単に読み解けるようにするため
 */
function decrypt() {
  const inputElement = document.getElementById('input-text');
  if (!inputElement) {
    return;
  }

  const rawInput = inputElement.value;
  // 全角英数などを半角に正規化する
  const normalizedText = rawInput.normalize('NFKC');

  let decryptedResult = "";
  let index = 0;
  const textLength = normalizedText.length;

  while (index < textLength) {
    const currentChar = normalizedText[index];

    // 特殊ルール 'q' (9) の判定
    if (currentChar === SPECIAL_Q_CIPHER) {
      decryptedResult += DECRYPT_TABLE[SPECIAL_Q_CIPHER];
      index += 1;
      continue;
    }

    // 2桁の暗号数字 (11〜55) の判定
    if (currentChar >= '1' && currentChar <= '5') {
      if (index + 1 < textLength) {
        const nextChar = normalizedText[index + 1];
        if (nextChar >= '1' && nextChar <= '5') {
          const cipherPair = currentChar + nextChar;
          if (DECRYPT_TABLE[cipherPair] !== undefined) {
            decryptedResult += DECRYPT_TABLE[cipherPair];
            index += 2;
            continue;
          }
        }
      }
    }

    // 暗号以外の文字（記号・スペース・改行・通常の数字など）はそのまま保持する
    decryptedResult += currentChar;
    index += 1;
  }

  const outputElement = document.getElementById('output-text');
  if (outputElement) {
    outputElement.innerText = decryptedResult;
  }
}

/**
 * 入力欄と出力結果をクリアする
 * What: テキストエリアと結果表示領域を空にする
 * Why: ユーザーが新しいテキストを素早く入力できるようにするため
 */
function clearAll() {
  const inputElement = document.getElementById('input-text');
  if (inputElement) {
    inputElement.value = "";
  }

  const outputElement = document.getElementById('output-text');
  if (outputElement) {
    outputElement.innerText = "";
  }
}

/**
 * 変換結果を入力欄に反映する
 * What: 現在の結果文字列を入力テキストエリアにセットする
 * Why: 暗号化した内容をすぐに復号テストしたい場合や再編集の手間を省くため
 */
function useResultAsInput() {
  const outputElement = document.getElementById('output-text');
  if (!outputElement) {
    return;
  }

  const currentResult = outputElement.innerText;
  if (!currentResult) {
    return;
  }

  const inputElement = document.getElementById('input-text');
  if (inputElement) {
    inputElement.value = currentResult;
  }
}

/**
 * 結果テキストをクリップボードにコピーする
 * What: 生成された結果をクリップボードにコピーし、成功メッセージを表示する
 * Why: ユーザーが手動で選択・コピーする手間を省くため
 */
function copyToClipboard() {
  const outputElement = document.getElementById('output-text');
  if (!outputElement) {
    return;
  }

  const outputText = outputElement.innerText;
  if (!outputText) {
    return;
  }

  navigator.clipboard.writeText(outputText).then(() => {
    const messageElement = document.getElementById('copy-message');
    if (messageElement) {
      messageElement.style.display = 'inline';
      setTimeout(() => {
        messageElement.style.display = 'none';
      }, 2000);
    }
  }).catch((error) => {
    console.error('Failed to copy to clipboard: ', error);
  });
}
</script>

---
