<!DOCTYPE html>
<html lang='en'>

<head>
    <meta charset='UTF-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1.0, minimum-scale=1'>
    <title>Document</title>
</head>

<body>
    <div>
        <div>
            Webpage
        </div>
        <div>
            <div id='chatBtn' style='display: none;'>
                Online
            </div>
    </div>

        <script type='text/javascript' src='https://service.force.com/embeddedservice/menu/fab.min.js'></script>
        <script type='text/javascript'>
            var initESW = function (gslbBaseURL) {
                console.log(embedded_svc);
                //(Sets the domain for your deployment so that visitors can navigate subdomains during a chat session)
                embedded_svc.menu.settings.storageDomain = "vax.co.uk"; 
                
                // Dev mode switches between .js and .min.js
                //embedded_svc.menu.settings.devMode = true;

                embedded_svc.menu.chat.settings.addEventHandler("onChatEstablished", function(data) {
                    console.log("onChatEstablished event was fired. data was " + data.liveAgentSessionKey);
                    });

                    embedded_svc.menu.chat.settings.addEventHandler("onChatRequestSuccess", function(data) {
                        console.log("onChatRequestSuccess event was fired. data was " + JSON.stringify(data));
                        });
                
                embedded_svc.menu.chat.settings.addEventHandler('onSettingsCallCompleted', function (data) {
                    console.log('onSettingsCallCompleted event was fired. Are agents available? ' + data.isAgentAvailable);
                    if (data.isAgentAvailable) {

                        //var elem1 = document.getElementById('esw-fab');
                        //elem1.style.display = 'none';

                        var elem = document.getElementById('chatBtn');
                        elem.addEventListener("click", function() {
                            document.getElementById("esw-fab").click();
                          });
                        elem.style.display = 'block';
                    }

                });

                embedded_svc.menu.init(
                    'https://tti-fc--akdev.sandbox.my.salesforce.com',
                    'https://d.la2s-core1.sfdc-cehfhs.salesforceliveagent.com/chat',
                    gslbBaseURL,
                    '00DS8000000Uzvh',
                    'ContactVax',
                    {
                        pageName: 'VaxSnippetSettings'
                    }
                );
            };
            if (!window.embedded_svc || !window.embedded_svc.menu) {
                var s = document.createElement('script');
                s.setAttribute('src', 'https://tti-fc--akdev.sandbox.my.salesforce.com/embeddedservice/menu/fab.min.js');
                s.onload = function () {
                    initESW(null);
                };
                document.body.appendChild(s);
            } else {
                initESW('https://service.force.com');
            }

            /*window.addEventListener("message", receiveMessage, false);
            function receiveMessage(event) {
            var payload = event.data;
            
            if(payload && payload.type === "chasitor.sendMessage") {
                embedded_svc.postMessage("chasitor.sendMessage", payload.message);
            }
        };*/

window.addEventListener("message", (event) => {
var payload = event.data;
if(payload && payload.type === "chasitor.sendMessage") {
embedded_svc.postMessage("chasitor.sendMessage", payload.message);
}
});

        </script>
</body>
</html>
