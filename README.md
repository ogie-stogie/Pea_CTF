# Pea_CTF
[Write Up Link](https://github.com/tbart27/Pea_CTF/blob/main/README.md)

### Team: 466 Crew
Taylor Bart<br>
Haytham Mouti<br>
Matt Evans<br>
John Tiffany<br>
<br>
For this CTF, we were able to get 7 flags (I believe 2 for me, 2 for Matt, and 3 for John). For this writeup, I'll briefly explain the two flags I obtained and the third challenge that we spent the rest of our time on.<br>
<br>
### web: eff-twelve
This was a trivial challenge where they wanted beginners to inspect the html and see that flag was embedded in the html. This was a nice and easy way to quickly get on the board.<br>
<br>
### web: Bots
For this challenge, I immediately knew what to since they hinted about what web crawlers would see (i.e. robots.txt for the site)<br>
<br>
We were first greeted with this:<br>
![](https://github.com/tbart27/Pea_CTF/blob/main/web1.png)<br>
<br>
I then searched for http://45.32.128.108:28099/robots.txt which yields a page that ccontained:<br>
<br>
User-agent: *


Sitemap: /sitemap.xml<br>
<br>
A quick search for http://45.32.128.108:28099/sitemap.xml then returned a page with this output<br>
<br>
![](https://github.com/tbart27/Pea_CTF/blob/main/web2.png)<br>
<br>
We can now see that the flag was at http://45.32.128.108:28091/sAPttauzr4UrbuM5mEQnemRc1vT8iPHp.html where we obtained the flag:<br>
<br>
![](https://github.com/tbart27/Pea_CTF/blob/main/web3.png)<br>
<br>
### web: Secure Admin
John was able to solve this challenge in the meantime, so from his writeup you will see the basis for how we approached Secure Admin 2.<br>
<br>
### web: Secure Admin 2
Now for this one, our simple trick of using '-- for both the username and password is detected as a SQL injection. We spent the entirety of the rest of the CTF focused on this challenge between us all. This [link](https://owasp.org/www-community/attacks/SQL_Injection_Bypassing_WAF) provided useful information and we decided to return to Secure Admin to see if we could craft a new injection that could then be disguised to pass the WAF for secure admin 2. John eventually read and used this which worked:<br>
<br>
![](https://github.com/tbart27/Pea_CTF/blob/main/web4.png)<br>
<br>
We tried many attempts to disguise this such as:<br>
`
or 1-- -' or 1 or '1"or 1 or"<br>
%4Fr 1-- -' %4Fr 1 %4Fr '1"%4Fr 1 %4Fr"<br>
%4F%52 1-- -' %4F%52 1 %4F%52 '1"%4F%52 1 %4F%52"<br>
%4F%52/**/1--/**/-'/**/%4F%52/**/1/**/%4F%52/**/'1"%4F%52/**/1/**/%4F%52"<br>
%4F%52/* */1--/* */-'/* */%4F%52/* */1/* */%4F%52/* */'1"%4F%52/* */1/* */%4F%52"<br>
%4F%52%201--%20-'%20%4F%52%201%20%4F%52%20'1"%4F%52%201%20%4F%52"<br>
%4F%52%201%2D%2D%20%2D'%20%4F%52%201%20%4F%52%20'1"%4F%52%201%20%4F%52"<br>
%4F%72%201%2D%2D%20%2D'%20%4F%72%201%20%4F%72%20'1"%4F%72%201%20%4F%72"<br>
`<br>
<br>
We even tried mixing upper/lower case letters and even trying combinations like oORr to see if it was simply removing the blacklisted words. We tried comments and hex representations such o/**/r and o%0br<br>
<br>
Eventually time ran out and we concluded that the string must be checked after it is parsed on the server side. We believe the program then terminates and returns the SQLi detected page immediately before any of our attempts could perform their queries. If we were to continue, we were going to try and see what words were blacklisted and see if some mundane calculation that returned 1 could be used to trick the sql query into returning its records.<br>
<br>

### Conclusion
This was a fun  CTF but I wish we could've cracked the Secure Admin 2. This was the 2nd week in a row that I have encountered WAF but this time there had to actually be functionality implemented. If there was a way we could leak the WAF, this challenge would be trivial to crack however as of righting this, I still don't know of any valid methods that would accomplish this feat. In the end, it was nice to obtain 7 flags (up from the 0 last week) and I hope I enconter another sql injection / waf challenge next week!
