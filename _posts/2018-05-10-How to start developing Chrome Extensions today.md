---
published: false
---
Hey! You are interesting in developing chrome extensions if you are here... Yes, my name's Sherlock.
OK, here you will find out about only basic principles of developing extensions + tips how to dive deeper when you will be ready.
Let's start from reqired skill to make you sure that you are able to understand this article.
## Requirements
- HTML
- JavaScript
That's all =)
## Remark 
Below are explanations to make process of extension development clear and easy.
But you can do a different way (the way that I passes, BTW):
- visit [GitHub repo](https://github.com/orbitbot/chrome-extensions-examples) with Chrome extensions examples 
- And learn about what do you need to know using this [official docmentation](https://developer.chrome.com/extensions/devguide)
## Simpliefied structure of an extension
- manifest.json (this is all about your extension)
- popup.html (it uses to dispay popup window)
- background.js (a script that running in a background)
- content.js (a script that injects to users' web experience)
- options.html
_Only manifest.json is required_ 
## manifest file example
{
    "name": "Sample Extension",
    "version": "0.1",
    "author": "Vadagon",
    "manifest_version": 2,
    "description": "Description for web store",
    "permissions": [
        "tabs"
    ],
    "browser_action": {
        "default_icon": {
            "16": "images/16.png",
            "19": "images/19.png",
            "20": "images/20.png",
            "32": "images/32.png",
            "38": "images/38.png",
            "40": "images/40.png",
            "128": "images/128.png"
        },
        "default_title": "Title that shows while mouse over icon",
        "default_popup": "popup.html"
    },
    "content_scripts": [
    	{
          "matches": ["https://*/*", "http://*/*", "<all_urls>"],
          "js": ["content.js"]
    	}
  	],
    "background": {
        "scripts": ["background.js"]
    },
    "icons": {
        "16": "images/16.png",
        "32": "images/32.png",
        "48": "images/48.png",
        "64": "images/64.png",
        "128": "images/128.png"
    },
    "web_accessible_resources": [
        "*"
    ],
    "options_page": "options.html",
    "offline_enabled": true,
    "content_security_policy": "script-src 'self'; object-src 'self'",
    "minimum_chrome_version": "50"
}
If you don't need content scripts, for example, just delete _contentscripts_ key and all it's content.
## How to communicate between scripts
Communication between extensions and their content scripts works by using message passing. Either side can listen for messages sent from the other end, and respond on the same channel. A message can contain any valid JSON object (null, boolean, number, string, array, or object). There is a simple API for one-time requests
### Simple one-time requests
If you only need to send a single message to another part of your extension (and optionally get a response back), you should use the simplified runtime.sendMessage or tabs.sendMessage . This lets you send a one-time JSON-serializable message from a content script to extension , or vice versa, respectively . An optional callback parameter allows you handle the response from the other side, if there is one.

Sending a request from a content script looks like this:

  chrome.runtime.sendMessage({greeting: "hello"}, function(response) {
    console.log(response.farewell);
  });

Sending a request from the extension to a content script looks very similar, except that you need to specify which tab to send it to. This example demonstrates sending a message to the content script in the selected tab.

  chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {
    chrome.tabs.sendMessage(tabs[0].id, {greeting: "hello"}, function(response) {
      console.log(response.farewell);
    });
  });

On the receiving end, you need to set up an runtime.onMessage event listener to handle the message. This looks the same from a content script or extension page.

  chrome.runtime.onMessage.addListener(
    function(request, sender, sendResponse) {
      console.log(sender.tab ?
                  "from a content script:" + sender.tab.url :
                  "from the extension");
      if (request.greeting == "hello")
        sendResponse({farewell: "goodbye"});
    }); 
  
In the above example, sendResponse was called synchronously. If you want to asynchronously use sendResponse, add return true; to the onMessage event handler.
 

