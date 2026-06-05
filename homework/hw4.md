作業內容
4.Example: https://github.com/rkuo2025/app-visual_assistant
1) Fork 
複製出 https://github.com/your_id/app-visual_assistant
2) Settings> Pages > Branch --> select Main [Save]
產生可執行網頁 https://your_id.github.io/app-visual_assistant
Homework: 簡化使用介面適合盲人使用
3) 使用ChatGPT / Gemini 修改 index.html 
4) 將index.html 上傳至 https://github.com/your_id/app-visual_assistant/
上傳github.com 網頁, 及手機截圖

成果
![成果1](../images/hw4-1.png)
![成果1](../images/hw4-2.png)
![成果2](../images/hw4-3.jpg)


程式碼:
``` 
<html lang="zh-TW">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>AI視覺輔助無障礙系統</title>

<style>

body{
    margin:0;
    padding:20px;
    background:black;
    color:white;
    font-family:Arial,sans-serif;
    text-align:center;
}

h1{
    font-size:38px;
    margin-bottom:20px;
}

video{
    width:95%;
    max-width:500px;
    border:5px solid yellow;
    border-radius:20px;
    margin-bottom:20px;
    background:#222;
}

button{
    width:90%;
    max-width:500px;
    font-size:30px;
    padding:25px;
    margin:15px 0;
    border:none;
    border-radius:20px;
    background:yellow;
    color:black;
    font-weight:bold;
    cursor:pointer;
}

button:active{
    background:orange;
}

input{
    width:90%;
    max-width:500px;
    font-size:22px;
    padding:15px;
    border-radius:12px;
    border:none;
    margin-bottom:20px;
}

#result{
    margin-top:20px;
    font-size:28px;
    color:#00ffcc;
    line-height:1.6;
    padding:15px;
    word-break:break-word;
}

</style>
</head>

<body>

AI視覺輔助系統


<input <br />type="password" 
id="apiKey" 
placeholder="請輸入 Gemini API Key">




<video id="video" autoplay playsinline>




<button onclick="startCamera()">📷 開啟相機

<button onclick="analyzeImage()">🧠 AI辨識環境

<button onclick="startRecognition()">🎤 語音輸入

<button onclick="readResult()">🔊 朗讀結果

<div id="result">系統尚未開始

<script>

// ===== DOM =====

const video = document.getElementById("video");
const resultDiv = document.getElementById("result");

// ===== 語音朗讀 =====

function speak(text){

    speechSynthesis.cancel();

    const utterance = new SpeechSynthesisUtterance(text);

    utterance.lang = "zh-TW";
    utterance.rate = 1;
    utterance.pitch = 1;

    speechSynthesis.speak(utterance);
}

// ===== 頁面啟動 =====

window.onload = function(){

    speak("歡迎使用AI視覺輔助系統");
}

// ===== 開啟相機 =====

async function startCamera(){

    try{

        const stream = await navigator.mediaDevices.getUserMedia({

            video:{
                facingMode:"environment"
            },

            audio:false
        });

        video.srcObject = stream;

        resultDiv.innerText = "相機已開啟";

        speak("相機已開啟");

    }catch(error){

        console.error(error);

        resultDiv.innerText = "無法開啟相機";

        speak("無法開啟相機");
    }
}

// ===== 擷取影像 =====

function captureImage(){

    const canvas = document.createElement("canvas");

    canvas.width = video.videoWidth;
    canvas.height = video.videoHeight;

    const ctx = canvas.getContext("2d");

    ctx.drawImage(video,0,0);

    return canvas.toDataURL("image/jpeg");
}

// ===== Gemini AI辨識 =====

async function analyzeImage(){

    const apiKey = document.getElementById("apiKey").value;

    if(!apiKey){

        resultDiv.innerText = "請先輸入API Key";

        speak("請先輸入API Key");

        return;
    }

    if(!video.srcObject){

        resultDiv.innerText = "請先開啟相機";

        speak("請先開啟相機");

        return;
    }

    resultDiv.innerText = "AI辨識中...";

    speak("正在分析環境");

    try{

        // ===== 擷取Base64圖片 =====

        const imageBase64 = captureImage().split(",")[1];

        // ===== 呼叫Gemini API =====

        const response = await fetch(

        `https://generativelanguage.googleapis.com/v1/models/gemini-1.5-flash:generateContent?key=${apiKey}`,

        {
            method:"POST",

            headers:{
                "Content-Type":"application/json"
            },

            body:JSON.stringify({

                contents:[

                    {

                        role:"user",

                        parts:[

                            {
                                text:"請用繁體中文簡短描述畫面中的人物、物件、障礙物與周遭環境，協助視障者理解周圍情況。"
                            },

                            {
                                inlineData:{
                                    mimeType:"image/jpeg",
                                    data:imageBase64
                                }
                            }
                        ]
                    }
                ]
            })
        });

        // ===== 取得回傳 =====

        const data = await response.json();

        console.log(data);

        // ===== API錯誤 =====

        if(data.error){

            console.error(data.error);

            resultDiv.innerText =
            "API錯誤：" + data.error.message;

            speak("API發生錯誤");

            return;
        }

        // ===== Gemini成功 =====

        if(
            data.candidates &&
            data.candidates.length > 0
        ){

            const text =
            data.candidates[0]
            .content.parts[0].text;

            resultDiv.innerText = text;

            speak(text);

        }else{

            console.log(data);

            resultDiv.innerText =
            "AI沒有成功產生內容";

            speak("AI沒有成功產生內容");
        }

    }catch(error){

        console.error(error);

        resultDiv.innerText =
        "AI辨識失敗：" + error.message;

        speak("AI辨識失敗");
    }
}

// ===== 朗讀結果 =====

function readResult(){

    speak(resultDiv.innerText);
}

// ===== 語音輸入 =====

function startRecognition(){

    if(!('webkitSpeechRecognition' in window)){

        resultDiv.innerText =
        "瀏覽器不支援語音輸入";

        speak("瀏覽器不支援語音輸入");

        return;
    }

    const recognition =
    new webkitSpeechRecognition();

    recognition.lang = "zh-TW";

    recognition.continuous = false;

    recognition.interimResults = false;

    speak("請說話");

    recognition.onresult = function(event){

        const text =
        event.results[0][0].transcript;

        resultDiv.innerText =
        "你說：" + text;

        speak("你說了" + text);
    }

    recognition.onerror = function(error){

        console.error(error);

        resultDiv.innerText =
        "語音辨識失敗";

        speak("語音辨識失敗");
    }

    recognition.start();
}

</script>
``` 

</body>
</html>
