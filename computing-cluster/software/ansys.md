## Licensing

With the new licensing you will need to login to your ansys account on the cluster. I have found the easiest way to do this is with an access token. Do step 1 from this link: https://ansyshelp.ansys.com/public/account/secured?returnurl=/Views/Secured/CSP/v000/en/gateway_ag/ag/configuring_shared_web_licensing.html

You can either download the json token or copy the string. don't worry when after you click create and save that the create box doesn't go away.

Use the following command on the cluster:

/apps/ansys/2024R1/v241/licensingclient/linx64/LicensingSettings account login --input "path/to/token.json"

-- OR --

/apps/ansys/2024R1/v241/licensingclient/linx64/LicensingSettings account login --token "TOKEN STRING"


### Current Version of Ansys: 2025R1

Lauching Ansys Workbench: `/apps/ansys/2025R1/v251/Framework/bin/Linux64/runwb2`
