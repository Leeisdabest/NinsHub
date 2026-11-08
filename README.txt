Roblox Key Website
==================

Your website:

    https://roblox-key-system-hr3h.onrender.com

Key page:

    https://roblox-key-system-hr3h.onrender.com/generate-key

How it works
============

1. Person opens /generate-key.
2. Website shows Checkpoint 1.
3. Clicking that opens Linkvertise checkpoint 1 in the same tab and creates a hidden browser session.
4. If checkpoint 1 returns to /checkpoint-one with the correct pass, the site unlocks checkpoint 2.
5. If checkpoint 2 returns to /checkpoint-two-finish with the correct pass, the key is generated.
6. If someone backs out to /generate-key, no key is generated.
7. The key page shows a check mark, the key, a copy button, and a countdown timer.
8. Roblox checks the key with /check-key.

Upload these files to GitHub
============================

    server.js
    package.json
    render.yaml
    roblox-loader.lua
    README.txt

Owner access
============

The owner panel is intentionally not linked from the public Premium page. Open /owner directly to sign in. The password is checked on the server and is never placed in a URL or sent to browser JavaScript. Successful sign-in creates an HttpOnly, Secure, SameSite=Strict session cookie that expires after 8 hours.

OWNER_PASSWORD must be configured as a server-side secret in the hosting provider. Do not put a real owner password in a public repository or in client-side HTML/JavaScript. If you refuse to use server-side secret configuration, there is no safe way to keep an owner password private in a source file that is deployed publicly.

Security cleanup
================

The previously uploaded source contained hardcoded service credentials and signing secrets. Treat every credential in the old source as exposed and rotate/revoke it with its provider before redeploying. Keep replacement credentials in server-side secret settings, not in GitHub or the browser. A signing-secret change invalidates signatures on existing keys unless you migrate/re-sign the key store.

Lifetime key behavior
=====================

Owner-generated lifetime Premium keys are reusable across Roblox accounts and devices and are not subject to the 24-hour expiration check. Standard free keys keep their existing expiry and binding behavior.

Then upload the updated files to GitHub and redeploy this Render service:

    roblox-key-system-hr3h

Lifetime key storage
====================

Owner-generated Premium keys are stored in the same keys.json store as the existing keys. They do not expire, can be checked repeatedly, and are not bound to the first Roblox account/device that uses them.

Important: Render's free web-service filesystem is ephemeral. A file stored only in the service directory can be lost after a redeploy or instance replacement. If you need lifetime keys to survive those events, attach persistent storage to the service. Without persistent storage or an external database, the project cannot guarantee keys survive redeploys/restarts.

    STORE_PATH = /var/data/keys.json

Mount the disk at /var/data. Persistent disks may require a paid Render service. Do not change your plan unless you are comfortable with the cost. Until persistent storage is configured, back up keys.json before redeploying.

The Linkvertise links, unlock pass, strict anti-bypass mode, and anti-bypass verification token are already inside server.js.

Your Linkvertise links must have Use anti-bypassing enabled. This makes the return URL include a short-lived token such as RS_EvaluationDT. The server now checks that token instead of trusting the page URL alone. Without that token, the site blocks the checkpoint return.

Then redeploy.

Linkvertise setup
=================

For your first Linkvertise link, set the final destination/target URL to:

    https://roblox-key-system-hr3h.onrender.com/checkpoint-one?pass=3b913615466d0554a0ac12eb50fde9be4d35685300eef38ecf993a6ce7e45f12

For your second Linkvertise link, set the final destination/target URL to:

    https://roblox-key-system-hr3h.onrender.com/checkpoint-two-finish?pass=3b913615466d0554a0ac12eb50fde9be4d35685300eef38ecf993a6ce7e45f12

If Linkvertise sends you back to /generate-key, the Linkvertise destination is wrong.
Edit the Linkvertise destinations:

    First link -> /checkpoint-one
    Second link -> /checkpoint-two-finish

Roblox loader setup
===================

roblox-loader.lua is already set to:

    https://roblox-key-system-hr3h.onrender.com

Use it like this:

    getgenv().ScriptKey = "PASTE_KEY_HERE"
    loadstring(game:HttpGet("YOUR_RAW_LOADER_LINK_HERE"))()

Owner and Premium keys
=====================

1. Open /premium.
2. Use the "I'm the owner" section and enter the password configured as OWNER_PASSWORD in Render.
3. Generate a lifetime Premium key from the owner page.
4. The same lifetime key can be validated repeatedly and used on multiple Roblox accounts/devices. It does not expire.
5. Regular 24-hour keys keep their existing first-account/device binding and expiry behavior.

Security note
=============

The previously uploaded server source contained hardcoded service credentials and signing/verification secrets. Treat those as exposed and rotate them before continuing public use. Do not put the owner password or replacement secrets in source files or GitHub; use Render environment variables.

Important
=========

The public key page does not show the secret return URL.
The return page requires the secret pass from Linkvertise.
Backing out of Linkvertise to the key page will not generate a key.
The return page now also requires the hidden browser session from /go and blocks returns that happen too fast.
Keys are signed on the server, so edited/fake key records are rejected.
The Roblox loader can remember one valid Roblox account/device until the 24 hour key expires.
If REQUIRE_ANTIBYPASS_TOKEN is true, checkpoint returns must include the short-lived anti-bypass token before the site will unlock or generate a key.
STRICT_ANTIBYPASS_TOKEN should be true when you want bypass-city style shortcuts blocked. If a real user gets blocked, Linkvertise is probably not appending the anti-bypass token to that link yet.

The strongest protection is Linkvertise server-side API verification. This setup is made to be reliable while still blocking the easy shortcut URL problem.
