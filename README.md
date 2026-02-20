<!DOCTYPE html>
<html lang="ka">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Advanced Caesar Cipher</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(-45deg,#0f2027,#203a43,#2c5364,#1c1c1c);
    background-size: 400% 400%;
    animation: gradientBG 12s ease infinite;
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
    color:white;
}

@keyframes gradientBG{
    0%{background-position:0% 50%;}
    50%{background-position:100% 50%;}
    100%{background-position:0% 50%;}
}

.card{
    background: rgba(255,255,255,0.08);
    backdrop-filter: blur(15px);
    padding:40px;
    border-radius:20px;
    width:600px;
    box-shadow:0 15px 35px rgba(0,0,0,0.4);
    animation: fadeIn 1.2s ease;
}

@keyframes fadeIn{
    from{opacity:0; transform:translateY(20px);}
    to{opacity:1; transform:translateY(0);}
}

h1{
    text-align:center;
    margin-bottom:25px;
}

textarea{
    width:100%;
    min-height:120px;
    padding:15px;
    border-radius:12px;
    border:none;
    margin-bottom:20px;
    font-size:16px;
    resize:vertical;
    outline:none;
    transition:0.3s;
}

textarea:focus{
    transform:scale(1.02);
}

.controls{
    display:flex;
    gap:10px;
    margin-bottom:20px;
}

input[type="number"]{
    width:100px;
    padding:10px;
    border-radius:10px;
    border:none;
    text-align:center;
    font-size:16px;
}

button{
    flex:1;
    padding:12px;
    font-size:16px;
    border-radius:12px;
    border:none;
    cursor:pointer;
    font-weight:600;
    transition:0.3s;
}

.encrypt{
    background:#00c6ff;
}

.copy{
    background:#00ff99;
}

button:hover{
    transform:scale(1.05);
    opacity:0.9;
}

.copy-success{
    animation: pulse 0.5s ease;
}

@keyframes pulse{
    0%{transform:scale(1);}
    50%{transform:scale(1.1);}
    100%{transform:scale(1);}
}

@media(max-width:700px){
    .card{width:90%;}
}
</style>
</head>

<body>

<div class="card">
    <h1>🔐 Caesar Cipher PRO</h1>

    <textarea id="inputText" placeholder="შეიყვანე ტექსტი..."></textarea>

    <div class="controls">
        <input type="number" id="shiftValue" value="3" min="1" max="32">
        <button class="encrypt" onclick="encrypt()">დაშიფვრა</button>
        <button class="copy" onclick="copyText()">Copy</button>
    </div>

    <textarea id="outputText" placeholder="დაშიფრული ტექსტი..." readonly></textarea>
</div>

<script>

// ქართული ანბანი
const georgianLower = "აბგდევზთიკლმნოპჟრსტუფქღყშჩცძწჭხჯჰ";
const georgianUpper = georgianLower.toUpperCase();

function caesarCipher(text, shift){
    let result = "";

    for(let char of text){

        // ინგლისური
        if(char.match(/[a-z]/)){
            let code = char.charCodeAt(0);
            result += String.fromCharCode((code - 97 + shift) % 26 + 97);
        }
        else if(char.match(/[A-Z]/)){
            let code = char.charCodeAt(0);
            result += String.fromCharCode((code - 65 + shift) % 26 + 65);
        }

        // ქართული პატარა
        else if(georgianLower.includes(char)){
            let index = georgianLower.indexOf(char);
            result += georgianLower[(index + shift) % georgianLower.length];
        }

        // ქართული დიდი
        else if(georgianUpper.includes(char)){
            let index = georgianUpper.indexOf(char);
            result += georgianUpper[(index + shift) % georgianUpper.length];
        }

        else{
            result += char;
        }
    }

    return result;
}

function encrypt(){
    let text = document.getElementById("inputText").value;
    let shift = parseInt(document.getElementById("shiftValue").value);
    document.getElementById("outputText").value = caesarCipher(text, shift);
}

function copyText(){
    let output = document.getElementById("outputText");
    output.select();
    document.execCommand("copy");

    let btn = document.querySelector(".copy");
    btn.classList.add("copy-success");
    btn.innerText = "Copied ✓";

    setTimeout(()=>{
        btn.classList.remove("copy-success");
        btn.innerText = "Copy";
    },1000);
}
</script>

</body>
</html>
