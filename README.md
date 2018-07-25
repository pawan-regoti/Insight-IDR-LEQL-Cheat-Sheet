# Insight-IDR-LEQL-Cheat-Sheet
Comprehensive Cheat Sheet for Rapid7's Insight-IDR LEQL Search Language.

We have found it difficult to locate specific cheat sheets for Insight-IDR's Log Entry Query Language (LEQL). 

The basic syntax is as follows:
---
Where(search) GroupBy(field) Calculate(function:field)
---

Here are some custom searches designed to locate enemies. This will help you locate your attacker faster, with more precision.

---

Queries (Select) Mode (Advanced):

Query:

`where(connection_status=DENY)groupby(source_address)` //This search locates firewall logs that have denied entry to specific hosts, then groups them by the most denied.

`where(source_ip=<IP> AND result!=FAILED_BAD_LOGIN)` //This search locates Office365 login attempts and checks to see if there is any successful logins from what could possibly be a brute force attack.

`where(source_ip=/192.168.1./)` //This search checks the entire /24 subnet of 192.168.1.1 for logs. Helpful when looking for adversaries.

`where(source_ip/<IP>/ AND result!=SUCCESS)calculate(count)` //This search locates brute force attempts in order to create reporting for clients.

`where(result=FAILED_BAD_LOGIN)calculate(UNIQUE:source_ip)` //This search calculates the number of Unique IP Addresses in your logs for failed login attempts.

`where(source_ip=<IP>)groupby(result)calculate(count)` //This search groups up results 

`where(service=0365 AND result=FAILED_BAD_LOGIN)groupby(source_ip)` //This search groups by IP Address, all failed login attempts.

`where(/facebook/ AND user!="unknown")group_by(user)` //This search groups up Facebook users on your network. Good for looking up time wasters.

`where(geoip_city="<CITY NAME>"groupby(user))` //This search groups up users authenticating from specific IP Addresses, or any other log information that has geolocation.

`where(geoip_country_code!=US)groupby(geoip_country_name)sort(desc)` //This search groups up Countries by name and sorts them in descending order.

`(select "VPN" only under "Ingress Authentication")(account="<USER ID>")` //This search checks logins to VPN connections.

---

Overall, it is best to fine-tune your searches to what specific data you are feeding your Insight-IDR instance. If you learn your environment, learn your specific search queries, you can create rules to detect and thwart the most sophisticated hackers.
