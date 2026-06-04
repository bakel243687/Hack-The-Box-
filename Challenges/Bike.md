# Bike

Started up the machine and I was provided with an IP address

I performed an nmap scan on the provided Ip address and got two open ports, an ssh/22 and an http/80

With more details on the nmap scan, it was obvious that the system is running Node.js (Express Middlware) with a http-title called Bike.

upon pasting the IP address into the web browser, the page boots up with the title Bike.

There is only one input field with an information that the website is under maintenance. Well, this is good because with all the collected information, the vulnerability should be known already.

Key info:
- Port 22 and 80
- Node.js (Express Middleware) running on the website
- An input field that reflects what was inputted back on the screen.


With the above information, the vulnerability we are working with is a Server Side Template Injection of Node.js Express Middleware.

Identifying the vulnerability is the easy part, now comes the hard part which is researching about the vulnerability. It's not every vulnerability that is known as a security researcher, so over time, we would have to be researching proof of concepts on several vulnerabilities.

Now, I spent more than half a day researching this until I came across a website, HackTrick which provided the needed payload which I didn't expect to be that long.

