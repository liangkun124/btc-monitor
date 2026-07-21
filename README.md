<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>BTC 实时价格</title>
<style>
body{
    margin:0;
    background:#000;
    color:#fff;
    font-family:-apple-system;
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
}
#box{
    text-align:center;
}
#price{
    font-size:72px;
    color:#00ff66;
    font-weight:bold;
}
#sum{
    margin-top:20px;
    font-size:48px;
    color:#ffd60a;
}
#status{
    margin-top:15px;
    color:#999;
    font-size:20px;
}
</style>
</head>

<body>

<div id="box">

<div id="price">连接中...</div>

<div id="sum"></div>

<div id="status"></div>

</div>

<script>

function calc(str){

let s=0;

for(let c of str){

if(c>='0'&&c<='9')
s+=parseInt(c);

}

return s;

}

function connect(){

const ws=new WebSocket("wss://stream.binance.com:9443/ws/btcusdt@trade");

ws.onopen=()=>{

document.getElementById("status").innerHTML="Binance 已连接";

}

ws.onmessage=(e)=>{

const data=JSON.parse(e.data);

const price=data.p;

document.getElementById("price").innerHTML=price;

document.getElementById("sum").innerHTML=
"数字和："+calc(price);

}

ws.onclose=()=>{

document.getElementById("status").innerHTML="重新连接...";

setTimeout(connect,2000);

}

ws.onerror=()=>{

ws.close();

}

}

connect();

</script>

</body>

</html>
