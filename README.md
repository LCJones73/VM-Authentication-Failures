# VM Authentication Failures
Creating a Threat Map that shows VM Failed Logons, including the ability for us to see if these could be brute force attacks.

> [!note]
> To better understand this section, consider reviewing the [Threat Maps Creation (Deep Dive)](https://github.com/LCJones73/Threat-Maps-Creating-Deep-Dive) first.<BR>

In this project we will create a Threat Map that is looking for Failed Logons, but we want to dig a bit deeper and see if there are a large number of failed logons per IPAddress.

The following Code is what we will use to create this particular Threat Map:

![image](https://github.com/user-attachments/assets/912faf99-245d-4e48-9c36-740d9583ce51)

This is the Threat Map it creates.

![image](https://github.com/user-attachments/assets/e20837a3-2a8a-41c7-9d82-89a6401786da)

You will notice this map looks busier than previous maps. There are more areas with the colored circles, and in more places than the previous Threat Maps. 

If you have not figured it out yet, the different colors actually are distinguished for a reason:

In Microsoft Sentinel's threat map, the colored circles represent geolocated security events or entities involved in incidents. The color of each circle typically corresponds to the severity level of the associated incident or alert. While Microsoft Sentinel doesn't provide a universal legend for these colors, a common convention is:
Microsoft Learn

- Red: High severity incidents
- Orange: Medium severity incidents
- Yellow: Low severity incidents
- Blue or Green: Informational alerts or entities

These visual cues help analysts quickly assess the criticality of threats and prioritize their investigations accordingly.

Now that we know this we will want to get the KQL code and analyze it and see a breakdown of what these failed logons look like.

![image](https://github.com/user-attachments/assets/c6e6cdaf-0578-4fe7-acb5-b8b5970916e6)

Due to the Red circles alone, we can logically expect to find some malicious logon attempts.

These are the results as we use this code through Microsoft Sentinel | Logs:

![image](https://github.com/user-attachments/assets/8954712b-aa2c-4aca-9dbd-6174f228a242)

> [!IMPORTANT]
> Let's Break This KQL Code Down!
>
> Notice that in the code line 3, is looking for the ActionType == "LogonFailed".
> You will also notice the IPV4 here as well which as we learned in the previous project, is how we are getting our Geo Location.
> Then the code further breaks down by looking for specific information on line 6.
>
> If you look at the code and then look below you can see how the KQL provides the information: RemoteIP, City, Country, friendly_location, Latitude, Longitude and LoginAttempts
>
> As you can see someone located in the Netherlands, specifically Tilburg, attempted to login 252 times. Also, someone in Cairo, Egypt attempted 107 logins.
>
> I want to explain some of this information so you understand what it means, I'll assume you understand their location.
>

## RemoteIP
What it is: The IP address of the external system connecting to your environment.<BR><BR>
Why it matters: Crucial for identifying attackers, command-and-control (C2) servers, or botnets.
Enables threat intel lookups (e.g., VirusTotal, abuseIPDB).
Used for IP reputation scoring or detecting uncommon geographies.

## friendly_location
What it is: A readable combination of city, region, and country.<BR><BR>
Why it matters: More intuitive than parsing City + Country separately.
Easier to read in dashboards and incident reports.
Helps you quickly understand where a login or access attempt came from.

## LoginAttempts
What it is: Number of login attempts recorded from the same source or account.<BR><BR>
Why it matters: Vital for detecting brute-force attacks, password sprays, and credential stuffing.
High login attempts from one IP can indicate a compromised or malicious source.
Enables threshold-based detections in custom KQL queries.

> [!IMPORTANT]
> Why All This Together Matters:<BR><BR>
> When combined, these fields allow you to: Triangulate threat sources geographically<BR><BR>
> Identify patterns of abnormal behavior (e.g., 15 login attempts from an IP in North Korea)<BR><BR>
> Build smart detections and visualizations<BR><BR>
> Respond faster and with more confidence<BR><BR>

### This ends this project. I hope it helps you understand a little better what the KQL code is doing in creating Threat Maps.
