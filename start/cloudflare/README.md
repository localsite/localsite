# Cloudflare Setup

Cloudflare provides free and easy DNS management, proxy service for fast page loading, https routing using one CertifytheWeb cert [in IIS](https://model.earth/setup) for multiple domains.  Using CNAME records, you can point one domain at a [site].github.io root containing multiple Github repos. ([See step 6](../../start/steps/)). Or you can pull multiple repos into one Cloudflare site using a Github [.submodules file](../submodules/).

You may want to leave off the proxy service for some domains. (With the proxy on, sometimes you'll need to  use the Cloudflare cache clearing button to view recent file changes.)

Important: Turn on the proxy service for domains receiving a lot of traffic, otherwise you may exceed GitHubs allowed traffic levels and encounter rate limiting.  

Here's a nice overview of the [advantages of combining GitHub with Cloudflare](https://www.toptal.com/github/unlimited-scale-web-hosting-github-pages-cloudflare).<br>Their sponsored project [jsdelivr](https://gomakethings.com/how-to-turn-any-github-repo-into-a-cdn/) is another great option for delivering any GitHub file via a CDN.

During setup, Cloudflare will provide nameservers to enter at your current registrar.  
You can transfer an existing domain to Cloudflare for cheaper hosting.  

## Configuration

These are in the walk-through when adding a new site:

- Automatic HTTPS Rewrites - On (the default)
- Always Use HTTPS - On  
- Brotli compression - On (the default)  

DNSSEC - Cloudflare recommends turning on. DNS > Settings

### Go to "Speed > Optimization"  

- Auto Minify - Javascript and CSS. Avoid HTML because comments will be removed.  
- Brotli compression - On (the default)  
- Early Hints  
- AVOID Rocket Loader™ - Currently not using since it changes javascript loading sequence, which breaks "View Dynmaic Version" link.  <!--Improve the paint time for pages which include JavaScript.  -->
- AMP Real URL - Display your site’s actual URL on your AMP pages, instead of the traditional Google AMP cache URL.  



<!--
The following set-up steps originated from the three videos here: https://httpsiseasy.com
Video 2: Under the same tab
https://www.youtube.com/watch?time_continue=1&v=mVzdEl5G0iM
-->

### Go to "SSL/TLS > Overview"  

- Select "Full" (Recommended by Cloudflare)  
- SSL/TLS Recommender - Receive an email regarding whether your website can use a more secure SSL/TLS mode.  

### Go to "SSL/TLS > Edge Certificates"  

- Always Use HTTPS - On  
- Click "Enable HSTS" - Turn on all 4 and set the Max Age Header to 12 months. (6 months is too short for hstspreload.org)  
- Minimum TLS Version: Minimum TLS 1.2 - because your server might be using 1.2<!--(but use TLS 1.3)-->  
- Leave the default of "TLS 1.3" as "On"  
- Keep on "Automatic HTTPS Rewrites" (ON by default) - Allows Cloudflare to automatically change all links in the HTML to https when appropriate, including links to external sites.  
- Certificate Transparency Monitoring - ON. Receive an email when a Certificate Authority issues a certificate for your domain.  

### hstspreload.org

Go to [hstspreload.org](https://hstspreload.org) and click two checkboxes here so browsers always preload as https.  

Optional: Test with [ssllabs.com](https://www.ssllabs.com/ssltest/analyze.html?hideResults=on&latest) (after 24 hours)  

### You won't be able to add a true wildcard redirect under "Page Rule"

Redirects only works for subdomains that are entered in the Cloudflare DNS list.  

Choose "Forwarding URL"  

\*.yourdomain.com/\*  
https://yourdomain.com/#go=$2  


## Host your Github repos using Cloudflare

<!--
No longer seeing this route, double-check then delete thiL
Add a custom domain in cloudflare Pages by clicking "Create a project" at "Account Home > Pages"

Workers & Pages > Pages tab > Connect to Git
-->

Connect to your repo, which can be a private repo.
(You'll have to install the Cloudflare Pages app into GitHub while logged into the GitHub account where the fork resides.)

**Workers Routes > Manage Workers**

1. Add your subdomain (subdomain.yoursite.com) as a custom domain in Cloudflare Pages by clicking: 

    Workers & Pages > Overview > Create application > Pages tab > Connect to Git


2. Make note of the generated subdomain where the project will be deployed.  It will be [generated subdomain].pages.dev

3. Under Environmental variables, add CFP_PASSWORD [your password]

4. Lastly, add a subdomain under Custom domains within Cloudflare pages.
This will automatically create a CNAME record pointed at [generated subdomain].pages.dev after a few minutes.

Optional: Include the "functions" folder from [charca's repo](https://dev.to/charca/password-protection-for-cloudflare-pages-8ma) to create the secure login single-password site.

Use [submodules](../submodules) to place multiple repos in your parent repo.

### Check for errors pulling submodules

**Workers Routes > Manage Workers**


## CloudFlare Firewall
Create a firewall rule to block IP's that attempt to hack a website. Websites check for hack attempts and send an email to admins with information such as the url, IP address, and url history. Since CloudFlare uses a proxy address, the IP address reported by the email is the CloudFlare proxy IP. CloudFlare includes the following header values which can be used to display non-proxied addresses: cf-connecting-ip, cf-connecting-ipv6, and x-forwarded-for. The code that checks for hack attempts checks for these header values and includes them in the email. Use the non-proxied IP addresses to create a firewall rule to block those IPs in CloudFlare. These same non-proxied IPs should be added to the Windows Firewall.

1. Perform a [Whois lookup](https://www.whois.com/whois/) for the IP address that you want to block to see what country the IP address is assigned to. In most cases, the IP address will be outside of the U.S. If it is inside the U.S., use your best judgement to determine whether to block the IP address or not, such as the amount of hack attempts from the IP address. Ensure that the IP address is not associated with CloudFlare (in case you accidently copied the CloudFlare proxied address from the hack attempt email notification). 
1. Login to the CloudFlare dashboard.
1. Create an IP block list first (See the Manage IP Lists section below).
1. Select the account and website. A firewall rule will need to be added to each website running on CloudFlare
1. Select Security > WAF.
1. Select Create rule.
1. Enter the following values:
    1. Rule Name: Blocked IP Addresses
    1. Field: IP Source Address
    1. Operator: is in list
    1. Value: blocked\_ip\_list (Should be selected by default if already created or create via the Manage Lists link - See Manage IP Lists section below)
    1. Choose action: Block
    1. Click Deploy to save and activate the firewall rule.

References:
[HTTP request headers](https://developers.cloudflare.com/fundamentals/get-started/reference/http-request-headers/#cf-connecting-ip)
[Create, edit, and delete firewall rules](https://developers.cloudflare.com/firewall/cf-dashboard/create-edit-delete-rules/)

### Manage IP Lists
The free version of CloudFlare allows 1 list to be created per account. This list can then be used in each firewall rule to block IP addresses trying to hack the websites. This allows you to enter an IP address or IP address range in one place rather than entering it manually for each website. A list will need to be created and maintained for each account in CloudFlare where you want to block IP addresses.

Create or access the list when creating the firewall rule (see above) or use the following steps to add an IP address to an existing list:
1. Log in to your Cloudflare account and select your account.
1. Go to Account Home > Manage Account > Configurations, and then select Lists.
1. To Create a new list:
    1. Enter a name for your list, observing the list name guidelines, such as blocked\_ip\_list.
    1. (Optional) Enter a description for the list, with a maximum length of 500 characters.
    1. For Content type, select the type of list you are creating: IP (the default)
    1. Click Create to save the list.
1. To Add or Remove IPs to an existing list:
    1. Click on the Edit link for the list.
    1. Click the Add Items button or select one or more IPs to delete and select the Remove button.
    1. Enter a single IP address or an address range (in CIDR format - see below) and an optional description. Addresses can also be added [using a CSV file](https://developers.cloudflare.com/fundamentals/global-configurations/lists/create-dashboard/#add-items-using-a-csv-file). Refer to the [IP CSV format](https://developers.cloudflare.com/fundamentals/global-configurations/lists/ip-lists/) for more information for IPv4 and IPV6 addresses. A CSV file may be easier to use rather than entering IP addresses manually and a single csv file can be used to update each account's blocked IP list easily. If you change or delete one or more entries in the CSV file, it may be easier to select all of the existing IPs in the list on CloudFlare, delete them, and then re-upload the file. The upload CSV process will only add entries that are not already in the list, it won't delete existing entries.
    1. Enter additional IP addresses as needed.
    1. When all the IP addresses have been entered, click the Add to list button to save the IP addresses to the list.

#### Specifying an IP address range using CIDR format
Typically, when entering an ip address range, the range should be something like: 111.222.333.0 - 111.222.333.255. Depending on the hack attempt you may need to widen the range to something like 111.222.333.0 - 111.222.336.255. Cloudflare requires IP address ranges to be entered in CIDR format. If you are not familiar with the CIDR format, search the internet for "ip address range in cidr notation". As an example, the [IPv4 Address to CIDR Notation](https://www.ipaddressguide.com/cidr) page can be used to convert from IP addresses to CIDR format and vice-versa.

If the address to block is an IPv6 address, you will need to mask the host bits which are the last 64 bits of the address. 
For example, instead of 2001:db8:6a0b:1a01:d423:43b9:13c5:2e8f, enter one of the following:

2001:db8:6a0b:1a01:0000:0000:0000:0000/64
2001:db8:6a0b:1a01::/64 (using the double colon notation)

Reference:
[Create a list in the dashboard](https://developers.cloudflare.com/fundamentals/global-configurations/lists/create-dashboard/)

<!--
## For Cloudflare Custom Purge

We use Cloudflare's free Content Delivery Network (CDN) to reduce traffic hitting GitHub.

After deploying, the following can be pasted into Cloudflare to do a custom purge.

https://model.earth/localsite/info/template-charts.html
https://model.earth/localsite/js/localsite.js
https://model.earth/localsite/js/map-embed.js
https://model.earth/localsite/js/map-filters.js
https://model.earth/localsite/js/naics.js
https://model.earth/localsite/js/navigation.js
https://model.earth/localsite/map/index.html
https://model.earth/localsite/css/search-filters.css
https://model.earth/localsite/css/map-display.css
https://model.earth/localsite/css/base.css
https://model.earth/localsite/css/map.css
https://model.earth/localsite/css/naics.css
https://model.earth/apps/
-->


