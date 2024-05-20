## Overview

Overview
ChromeExtension_URLinterceptor is a Chrome extension designed to block specific URLs from being accessed in the browser. Users can dynamically add URLs to a block list through the extension interface.

##
## Features

Features
- **Block URLs**: Prevent access to specified URLs.
- **Dynamic URL Addition**: Add new URLs to be blocked without needing to restart the browser.

##
## Installation Instructions

Installation Instructions
To install and set up the ChromeExtension_URLinterceptor, follow these steps:

1. **Clone the Repository**: Clone the GitHub repository to your local machine.
    ```bash
    git clone https://github.com/nitishprabhu26/ChromeExtension_URLinterceptor.git
    ```

2. **Open Chrome Extensions Page**:
    - Open Chrome and navigate to `chrome://extensions/` or click on the three-dot menu at the top right, then go to `More tools` > `Extensions`.

3. **Enable Developer Mode**:
    - In the top right corner of the Extensions page, toggle the slider to "Developer mode".

4. **Load the Extension**:
    - Click on the "Load unpacked" button.
    - Select the directory where you cloned the repository.

5. **Verify Installation**:
    - The extension should now appear in your list of installed extensions.

##
## Usage Examples

Usage Examples

1. **Blocking Predefined URLs**:
    - By default, the extension blocks `https://twitter.com` and `https://www.facebook.com`.

2. **Adding New URLs to Block List**:
    - Click on the extension icon in the Chrome toolbar.
    - Enter the URL you want to block in the input field.
    - Press the "Enter" key to add the URL to the block list.

##
## Code Summary

Code Summary
### content-script.js
Located at: `C:\Users\\Documents\GitHub\ChromeExtension_URLinterceptor\script\content-script.js`

This script listens for the "Enter" keypress event on the input field. When the "Enter" key is pressed, it sends a message containing the new URL to be blocked to the background script.
```javascript
document.getElementById("new-url").addEventListener("keypress",
    function(e){
        if(e.key === "Enter"){
            // broadcasts a message to any listener that we have instantiated in our chrome extension
            chrome.runtime.sendMessage({addUrl: e.target.value})
        }
    }
);
```

### script.js
Located at: `C:\Users\\Documents\GitHub\ChromeExtension_URLinterceptor\script\script.js`

This script uses the Chrome WebRequest API to block specified URLs. Initially, it blocks `https://twitter.com` and `https://www.facebook.com`. It also listens for messages from `content-script.js` to add new URLs to the block list.
```javascript
// using the chrome webRequest API, using the method onBeforeRequest
// for "all url's" and the type is "BLOCKING"
// chrome.webRequest.onBeforeRequest.addListener(
//     function(details) {
//         return {cancel: true}
//     }, { urls: ["<all_urls>"]}, ["blocking"]
// )

let urlsToBlock = ["https://twitter.com", "https://www.facebook.com"];

chrome.runtime.onMessage.addListener(
    function(request){
        if(request.addUrl){
            urlsToBlock.push(request.addUrl);
        }
    }
);
```

##
## License

License
This project is licensed under the MIT License. You are free to use, modify, and distribute this software in any project, subject to the conditions of the license.