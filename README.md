# Burping Practice Da Exam


 ![img](https://i.imgur.com/PW3aGOx.png)
Once we start the practice exam, we need to solve 3 tasks within the given time. You need to solve the tasks in order else, it wont work.

 ![img](https://i.imgur.com/9GtNq9F.png)
First task is about finding and exploiting XSS. If you're lazy, just spam intruder with xss payloads and find which one works for you.

 ![img](https://i.imgur.com/IQyEXnA.png)
If we observe the source code, we can see a  file located at resource/js/searchResults.js

 ![img](https://i.imgur.com/haveNKM.png)
Once we observe the script, it actually taking our input directly. We only need to escape our payload certain place. "-alert()-" works fine here.

 ![img](https://i.imgur.com/bkFwQVm.png)
To exploit this XSS, we must obtain the cookies. If we use payload "-alert(document.cookie)-" our payload instantly get blocked by the WAF. So, once again we need to find which symbol/characters are blocked.

![img](https://i.imgur.com/A8EVg2p.png)
As a certified skid, I just paste my good ole XSS testcase. Start removing each symbol/character till we identify which one gets blocked by WAF. Turns out, its the  " . " So, without " . " how can we call the document.cookie thing? Well, if we can't use it, just find new payload that works.

 ![img](https://i.imgur.com/EzkZeun.png)
"-alert(window["document"]["cookie"])-"  and boom, we get our cookies. Now we are going to craft payload and send to victim

 ![img](https://i.imgur.com/qJvAiK1.png)
Remember to replace the link with your own exploit URL. And make sure to url encode your payload. In case there's WAF, just find your way to bypass it.

 ![img](https://i.imgur.com/1MQEMAc.png)
First we're going to test the payload on ourself and see if its actually works. Great, we got our own cookie.

 ![img](https://i.imgur.com/yZEcufI.png)
Once we deliver the exploit to victim, we'll instantly get carlos's cookie.


 ![img](https://i.imgur.com/B7A4NiQ.png)
Copy and use Carlo's cookie.

 ![img](https://i.imgur.com/U9pCaRk.png)
So whats next? We solved one of the task.  Now for second task, its related to SQL Injection. Find the

 ![img](https://i.imgur.com/6SM7yP2.png)
The SQLi located at advanced search. TO confirm whether its vulnerable or not, slap our magic payload aka single quote ( ' ).

 ![img](https://i.imgur.com/fw43iOT.png)
use SQLmap to save our time and we get the user & pass. We're interested at the admin account. Use that credentials to login.

 ![img](https://i.imgur.com/8NY2Xa3.png)
Now, we solved 2/3 tasks. Next task is related to deserialization. I recommend using Java Deserialization plugin (you can find at Burp Store)

 ![img](https://i.imgur.com/CrhhAP8.png)
Now once, we logged as admin, go to admin panel and observe in Burp Suite ( Well, of course u need to constantly use Burp for this practice exam).

If we look at the req:
```
GET /admin-panel HTTP/1.1
Host: acda1fb11f0e592bc0bc347c001700bf.web-security-academy.net
Cookie: admin-prefs=H4sIAAAAAAAA%2fzWPPU7DQBCFF0RSQcMJpkOi2PTQEH4iCkcKClJEOV6Pk8HrHbO7dmKQOA4VJ%2bAI3IU7sBahm%2fn09PS9zx81Cl6dW8w1msjigjZS1%2bJ0IM9o%2bRVzS3pa1OwWnsrw9vUxDqvv7FAdZeq4xE48R5qJFFGdZs%2fY4cSiW0%2bW0bNbX2bq5D%2fz0EqkF%2fWuDvaw
```

The vulnerability is at admin-prefs. Find how the data being encoded using tools like CyberChef. It'll auto detect for you.

 ![img](https://i.imgur.com/hIaZyWw.png)
First we need to know how the app encode the strings. At CyberChef use the magic wand to auto solve it. Now, we know the order of the decoding process. Gzip->Base64->UrlEncode.

 ![img](https://i.imgur.com/eTZDRZf.png)
Send the request to Java Deserialization Plugin and find the right gadget. Oh, make sure to reverse the order of encoding from what we got at CyberChef.Run the scan and we can see potentials gadgets. But bear in mind, this tools not accurate at all. So, you have to test it manually.
 ![img](https://i.imgur.com/eK64VfK.png)
Once we find the right gadget, use the exploit tab and make sure, to set your YSOSERIAL jar path. In my case, i have no idea why it doesnt work. So i had to use manual way to generate payload.

 ![img](https://i.imgur.com/UNfaCnq.png)
I had to generate payload inside Kali but u can do it in any Linux distro. The reason is, MacOS doesnt have option for return result in single line. In Kali we can use -w0.

 ![img](https://i.imgur.com/S1UatHV.png)
For OOB data exfiltration Reference 

 ![img](https://i.imgur.com/SPJP5pZ.png)
Recreate the req and modify the admin-prefs value with our payload. make sure to url encode first.

 ![img](https://i.imgur.com/tMyEm7e.png)
Chech our BurpCollaborator and boom, we got /home/carlos/secret data here. Submit and we solved all tasks. 


