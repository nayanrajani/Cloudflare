Microsoft Windows [Version 10.0.19044.1415]
(c) Microsoft Corporation. All rights reserved.

C:\Users\<%UserProfile%>\Development>cd C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel login
A browser window should have opened at the following URL:

https://dash.cloudflare.com/argotunnel?callback=https%3A%2F%2Flogin.cloudflareaccess.org%2FHVtwHkhrFmWUE1usi2GetZEoSgwhDLrLHXouwIySzgM%3D

If the browser failed to open, please visit the URL above directly in your browser.
2022-02-07T07:08:04Z INF Waiting for login...
You have successfully logged in.
If you wish to copy your credentials to a server, they have been saved to:
C:\Users\<%UserProfile%>\.cloudflared\cert.pem

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared update
2022-02-07T07:08:51Z INF cloudflared is up to date version=

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel list
You can obtain more detailed information for each tunnel with `cloudflared tunnel info <name/uuid>`
ID                                   NAME              CREATED              CONNECTIONS
<UUID> chromewebb        2022-02-07T05:17:24Z 2xBOM, 2xSIN 
<UUID> greencoinlocalWEB 2022-02-07T05:45:50Z

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel list
C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudfl
Tunnel credentials written to C:\Users\<%UserProfile%>\.cloudflared\<UUID>.ased on where your origin certificate was found. Keep this file secret. To revoke these credentials, d

Created tunnel GreencoinLocal with id <UUID>

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudfl
You can obtain more detailed information for each tunnel with `cloudflared tunnel info <name/uuid>`
ID                                   NAME           CREATED              CONNECTIONS
<UUID> GreencoinLocal 2022-02-07T07:11:34Z
<UUID> chromewebb     2022-02-07T05:17:24Z 2xBOM, 2xSIN

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudfl


C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel route dns GreencoinLocal private.greencoin.cf
2022-02-07T07:27:41Z INF Added CNAME private.greencoin.cf which will route to this tunnel tunnelID=<UUID>

C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel list
You can obtain more detailed information for each tunnel with `cloudflared tunnel info <name/uuid>`
ID                                   NAME           CREATED              CONNECTIONS  
<UUID> GreencoinLocal 2022-02-07T07:11:34Z
<UUID> chromewebb     2022-02-07T05:17:24Z 2xBOM, 2xSIN 


C:\Users\<%UserProfile%>\Development\Cloudflare Part\Cloudflare\Zero Trust\CIS\Cloudflared_Tunnel>cloudflared tunnel --config C:\Users\<%UserProfile%>\.cloudflared\config.yml --origin-ca-pool C:\Users\<%UserProfile%>\.cloudflared\cert.pem run GreencoinLocal
2022-02-07T07:45:44Z INF Starting tunnel tunnelID=<UUID>
2022-02-07T07:45:44Z INF Version 2022.2.0
2022-02-07T07:45:44Z INF GOOS: windows, GOVersion: go1.17.5, GoArch: amd64
2022-02-07T07:45:44Z INF Settings: map[config:C:\Users\<%UserProfile%>\.cloudflared\config.yml cred-file:C:/Users/<%UserProfile%>/.cloudflared/<UUID>.json credentials-file:C:/Users/<%UserProfile%>/.cloudflared/<UUID>.json origin-ca-pool:C:\Users\<%UserProfile%>\.cloudflared\cert.pem]
2022-02-07T07:45:44Z INF cloudflared will not automatically update on Windows systems.
2022-02-07T07:45:44Z INF Generated Connector ID: 76d6f38d-7eb6-4b90-8cc0-beea1cb5d113
2022-02-07T07:45:44Z INF Initial protocol http2
2022-02-07T07:45:44Z INF Starting metrics server on 127.0.0.1:55186/metrics
2022-02-07T07:45:45Z INF cloudflared does not support loading the system root certificate pool on Windows. Please use --origin-ca-pool <PATH> to specify the path to the certificate pool
2022-02-07T07:45:45Z INF Connection d35637ab-25ee-4893-b3cb-3c95f92ae5f6 registered connIndex=0 location=SIN
2022-02-07T07:45:46Z INF Connection e05c8351-4b77-4179-ab1d-82e92cde035d registered connIndex=1 location=BOM
2022-02-07T07:45:47Z INF Connection 914da55d-e203-415f-b0c8-c61e84de2d1d registered connIndex=2 location=SIN
2022-02-07T07:45:48Z INF Connection 4a5937d8-9ff9-47a7-be17-95cffa1da103 registered connIndex=3 location=BOM


- cloudflared update
- cloudflared tunnel login
- cloudflared tunnel create <NAME>
    - Create ingress rule in the config file of .cloudflared in your <%UserProfile%>

        tunnel: <Tunnel_UUID>
        credentials-file: C:/Users/<%UserProfile%>/.cloudflared/<Tunnel_UUID>.json

        ingress: 
        - hostname: private.greencoin.cf
        service: http://localhost:8887
        - service: http_status:404

- cloudflared tunnel list
- cloudflared tunnel route dns GreencoinLocal private.greencoin.cf

- cloudflared tunnel --config C:\Users\nayan.rajani\.cloudflared\config.yml --origin-ca-pool C:\Users\nayan.rajani\.cloudflared\cert.pem run GreencoinLocal
#Tunnel as a service
OR 

- cloudflared tunnel run <Tunnel_Name>



