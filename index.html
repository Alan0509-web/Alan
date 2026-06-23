// ==========================================================================
// 1. 本地簡易字典數據 (當 API 斷線或找不到時的備用 fallback)
// ==========================================================================
const localDictionary = {
    "hello": { "zh_TW": "你好", "zh_CN": "你好", "ja": "こんにちは", "ko": "안녕하세요", "es": "Hola", "fr": "Bonjour", "de": "Hallo", "it": "Ciao", "pt": "Olá", "en": "Hello" },
    "good morning": { "zh_TW": "早安", "zh_CN": "早安", "ja": "おはようございます", "ko": "좋은 아침입니다", "es": "Buenos días", "fr": "Bonjour", "de": "Guten Morgen", "it": "Buongiorno", "pt": "Bom dia", "en": "Good morning" },
    "thank you": { "zh_TW": "謝謝你", "zh_CN": "谢谢你", "ja": "ありがとう", "ko": "감사합니다", "es": "Gracias", "fr": "Merci", "de": "Danke", "it": "Grazie", "pt": "Obrigado", "en": "Thank you" },
    "sorry": { "zh_TW": "對不起", "zh_CN": "对不起", "ja": "ごめんなさい", "ko": "미안합니다", "es": "Lo siento", "fr": "Désolé", "de": "Es tut mir leid", "it": "Mi dispiace", "pt": "Desculpe", "en": "Sorry" },
    "how are you": { "zh_TW": "你好嗎？", "zh_CN": "你好吗？", "ja": "お元気ですか？", "ko": "잘 지냈어요?", "es": "全面 ¿Cómo estás?", "fr": "Comment ça va?", "de": "Wie geht es dir?", "it": "Come stai?", "pt": "Como você está?", "en": "How are you?" },
    "nice to meet you": { "zh_TW": "很高興認識你", "zh_CN": "很高兴认识你", "ja": "はじめまして", "ko": "만나서 반가워요", "es": "Gusto en conocerte", "fr": "Ravi de vous rencontrer", "de": "Freut mich, dich kennenzulernen", "it": "Piacere di conoscerti", "pt": "Prazer em conhecê-lo", "en": "Nice to meet you" },
    "welcome": { "zh_TW": "歡迎", "zh_CN": "欢迎", "ja": "ようこそ", "ko": "환영합니다", "es": "Bienvenido", "fr": "Bienvenue", "de": "Willkommen", "it": "Benvenuto", "pt": "Bem-vindo", "en": "Welcome" },
    "goodbye": { "zh_TW": "再見", "zh_CN": "再见", "ja": "さようなら", "ko": "안녕히 가세요", "es": "Adiós", "fr": "Au revoir", "de": "Auf Wiedersehen", "it": "Arrivederci", "pt": "Adeus", "en": "Goodbye" }
};

// 反向查詢字典 (本地備用方案使用)
const reverseDictionary = {};
for (const [enKey, translations] of Object.entries(localDictionary)) {
    for (const [lang, val] of Object.entries(translations)) {
        const normalizedVal = val.toLowerCase().replace(/[？\?]/g, '');
        if (!reverseDictionary[normalizedVal]) {
            reverseDictionary[normalizedVal] = enKey;
        }
    }
}

// ==========================================================================
// 2. 配置設定 (已成功替換為您的 Google GAS Web App 網址)
// ==========================================================================
const GAS_WEB_APP_URL = "https://script.google.com/macros/s/AKfycbzRFLT5hQunAgRUstrcIOZ0whuOI9L31VFYhQ8LBVnHPsJeN4fNEJn3DJmZshcCb5WMBQ/exec"; 

// ==========================================================================
// 3. DOM 元素獲取
// ==========================================================================
const sourceLangSelect = document.getElementById('source-lang');
const targetLangSelect = document.getElementById('target-lang');
const sourceTextarea = document.getElementById('source-text');
const targetTextarea = document.getElementById('target-text');
const sourceCount = document.getElementById('source-count');
const targetCount = document.getElementById('target-count');

const translateBtn = document.getElementById('translate-btn');
const swapBtn = document.getElementById('swap-btn');
const copyBtn = document.getElementById('copy-btn');
const clearBtn = document.getElementById('clear-btn');
const speakBtn = document.getElementById('speak-btn');

const presetButtons = document.querySelectorAll('.preset-btn');
const historyList = document.getElementById('history-list');
const clearHistoryBtn = document.getElementById('clear-history-btn');
const toast = document.getElementById('toast');

// ==========================================================================
// 4. 初始化與事件綁定
// ==========================================================================
document.addEventListener('DOMContentLoaded', () => {
    updateCharCount(sourceTextarea, sourceCount);
    renderHistory();

    // 輸入監聽
    sourceTextarea.addEventListener('input', () => updateCharCount(sourceTextarea, sourceCount));
    
    // 功能按鈕點擊事件
    translateBtn.addEventListener('click', performTranslation);
    swapBtn.addEventListener('click', swapLanguages);
    copyBtn.addEventListener('click', copyToClipboard);
    clearBtn.addEventListener('click', clearAll);
    speakBtn.addEventListener('click', speakResult);
    clearHistoryBtn.addEventListener('click', clearHistory);

    // 常用短語點擊事件
    presetButtons.forEach(btn => {
        btn.addEventListener('click', () => {
            sourceTextarea.value = btn.getAttribute('data-text');
            sourceLangSelect.value = 'en'; // 預設短語皆為英文
            updateCharCount(sourceTextarea, sourceCount);
            performTranslation(); // 自動觸發翻譯
        });
    });
});

// ==========================================================================
// 5. 核心邏輯函數 (整合線上 API、本地 Fallback 與 GAS 寫入)
// ==========================================================================

// 更新字數統計
function updateCharCount(textarea, countSpan) {
    const len = textarea.value.length;
    countSpan.textContent = len;
    if (len > 5000) {
        textarea.value = textarea.value.substring(0, 5000);
        countSpan.textContent = 5000;
    }
}

// 核心翻譯函數
async function performTranslation() {
    const text = sourceTextarea.value.trim();
    if (!text) {
        targetTextarea.value = '';
        updateCharCount(targetTextarea, targetCount);
        return;
    }

    const fromLang = sourceLangSelect.value;
    const toLang = targetLangSelect.value;

    // 如果源語言與目標語言相同，直接輸出
    if (fromLang === toLang) {
        targetTextarea.value = text;
        updateCharCount(targetTextarea, targetCount);
        return;
    }

    try {
        showToast('翻譯中...');
        
        // 1. 呼叫免費的 MyMemory 翻譯 API
        const apiUrl = `https://api.mymemory.translated.net/get?q=${encodeURIComponent(text)}&langpair=${fromLang}|${toLang}`;
        const response = await fetch(apiUrl);
        const data = await response.json();
        
        if (data.responseData && data.responseData.translatedText) {
            let result = data.responseData.translatedText;
            
            // 填入翻譯結果與統計字數
            targetTextarea.value = result;
            updateCharCount(targetTextarea, targetCount);
            showToast('翻譯完成！');

            // 同步將數據儲存到 Google Sheets 與本地歷史紀錄
            sendToGoogleSheet(text, result, fromLang, toLang);
            saveToHistory(text, result, fromLang, toLang);
        } else {
            throw new Error('API 回傳數據異常');
        }
    } catch (error) {
        console.warn('API 呼叫失敗，啟用備用本地字典：', error);
        runLocalFallback(text, fromLang, toLang);
    }
}

// 本地備用字典翻譯機制 (當沒有網路或 API 限制時)
function runLocalFallback(text, fromLang, toLang) {
    let result = '';
    const normalizedText = text.toLowerCase().replace(/[？\?]/g, '');

    if (fromLang === 'en' && localDictionary[normalizedText]) {
        result = localDictionary[normalizedText][toLang];
    } else if (reverseDictionary[normalizedText]) {
        const enKey = reverseDictionary[normalizedText];
        result = localDictionary[enKey][toLang];
    }

    if (!result) {
        result = `[離線模式] 無法翻譯 "${text}"。請連網或嘗試常用短語（如 Hello）。`;
    }

    targetTextarea.value = result;
    // 修正：這裡原本錯寫成 countSpan，已更正為對應的 DOM 元素 targetCount
    updateCharCount(targetTextarea, targetCount);
    showToast('已切換至本地字典');
    
    // 即使本地成功，也記錄在歷史紀錄中
    if (!result.includes('[離線模式]')) {
        sendToGoogleSheet(text, result, fromLang, toLang);
        saveToHistory(text, result, fromLang, toLang);
    }
}

// 發送資料至 Google Apps Script 存入試算表
function sendToGoogleSheet(source, target, fromLang, toLang) {
    if (!GAS_WEB_APP_URL || GAS_WEB_APP_URL.includes("XXXXX")) {
        console.log('未設定 GAS_WEB_APP_URL，跳過試算表同步。');
        return;
    }

    const payload = {
        sourceText: source,
        targetText: target,
        sourceLang: fromLang,
        targetLang: toLang
    };

    fetch(GAS_WEB_APP_URL, {
        method: 'POST',
        mode: 'no-cors', // 避開瀏覽器跨網域 CORS 限制
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(payload)
    })
    .then(() => console.log('資料已成功同步至 Google Sheet'))
    .catch(err => console.error('同步至 Google Sheet 失敗：', err));
}

// ==========================================================================
// 6. 介面辅助功能 (交換、複製、清空、朗讀、歷史紀錄)
// ==========================================================================

function swapLanguages() {
    const tempLang = sourceLangSelect.value;
    sourceLangSelect.value = targetLangSelect.value;
    targetLangSelect.value = tempLang;

    const tempText = sourceTextarea.value;
    sourceTextarea.value = targetTextarea.value;
    targetTextarea.value = tempText;

    updateCharCount(sourceTextarea, sourceCount);
    updateCharCount(targetTextarea, targetCount);
}

function copyToClipboard() {
    const text = targetTextarea.value;
    if (!text) {
        showToast('沒有可複製的文本！');
        return;
    }
    navigator.clipboard.writeText(text).then(() => {
        showToast('複製成功！');
    }).catch(() => {
        showToast('複製失敗，請手動複製。');
    });
}

function clearAll() {
    sourceTextarea.value = '';
    targetTextarea.value = '';
    updateCharCount(sourceTextarea, sourceCount);
    updateCharCount(targetTextarea, targetCount);
    showToast('已清空');
}

function speakResult() {
    const text = targetTextarea.value;
    if (!text || text.includes('[離線模式]')) {
        showToast('沒有有效的翻譯結果可供朗讀！');
        return;
    }

    window.speechSynthesis.cancel();
    const utterance = new SpeechSynthesisUtterance(text);
    
    const langMap = {
        'en': 'en-US', 'zh_TW': 'zh-TW', 'zh_CN': 'zh-CN', 'ja': 'ja-JP',
        'ko': 'ko-KR', 'es': 'es-ES', 'fr': 'fr-FR', 'de': 'de-DE',
        'it': 'it-IT', 'pt': 'pt-PT'
    };
    utterance.lang = langMap[targetLangSelect.value] || 'en-US';
    window.speechSynthesis.speak(utterance);
}

// 土司提示效果
function showToast(message) {
    toast.textContent = message;
    toast.classList.add('show');
    setTimeout(() => {
        toast.classList.remove('show');
    }, 2500);
}

function saveToHistory(source, target, fromLang, toLang) {
    let history = JSON.parse(localStorage.getItem('trans_history')) || [];
    const newItem = {
        id: Date.now(),
        source,
        target,
        langPair: `${fromLang.toUpperCase()} ➔ ${toLang.toUpperCase()}`,
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    };

    if (history.length > 0 && history[0].source === source && history[0].target === target) {
        return; 
    }

    history.unshift(newItem);
    if (history.length > 20) history.pop();

    localStorage.setItem('trans_history', JSON.stringify(history));
    renderHistory();
}

function renderHistory() {
    const history = JSON.parse(localStorage.getItem('trans_history')) || [];
    historyList.innerHTML = '';

    if (history.length === 0) {
        historyList.innerHTML = '<div class="history-item" style="color: #9ca3af; justify-content: center;">暫無歷史紀錄</div>';
        return;
    }

    history.forEach(item => {
        const div = document.createElement('div');
        div.className = 'history-item';
        div.innerHTML = `
            <div class="history-text" title="${item.source} -> ${item.target}">
                <strong>[${item.langPair}]</strong> ${item.source} ➔ ${item.target}
            </div>
            <div class="history-time">${item.time}</div>
        `;
        historyList.appendChild(div);
    });
}

function clearHistory() {
    localStorage.removeItem('trans_history');
    renderHistory();
    showToast('歷史紀錄已清除');
}
