## Whitelist CSP Examples ##
Date: 2015-10-28<br>
Last Update: 2015-12-12<br>
Cosmetic Update: 2018-09-18

[`The Whitelist System`](the-whitelist-system.md) -> Whitelist `CSP` Examples

**THERE ARE MORE EXAMPLES COMING. DESCRIPTIONS WILL BE ADDED.**

THE `CSP` SHOULD BE ON EVERY HTML PAGE, WITH FEW EXCEPTIONS.

```
<!-- Good default declaration:
    * gap: is required only on iOS (when using UIWebView) and is needed for JS->native communication
    * https://ssl.gstatic.com is required only on Android and is needed for TalkBack to function properly
    * Disables use of eval() and inline scripts in order to mitigate risk of XSS vulnerabilities. To change this:
        * Enable inline JS: add 'unsafe-inline' to default-src
        * Enable eval(): add 'unsafe-eval' to default-src
-->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self' data: gap: https://ssl.gstatic.com; 
               style-src 'self' 'unsafe-inline'; 
               media-src *">

<!-- Enable all requests, inline styles, inline javascript and eval() -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src *; 
               style-src 'self' 'unsafe-inline'; 
               script-src 'self' 'unsafe-inline' 'unsafe-eval'">

<!-- My Extension, if any different -->
<!-- Same as above, except accept from any source -->
<meta http-equiv="Content-Security-Policy"
      content="default-src *; 
               style-src * 'self' 'unsafe-inline' 'unsafe-eval'; 
               script-src * 'self' 'unsafe-inline' 'unsafe-eval';">
```

#### <a name=bydefault>Access By Default</a> ####

| directive   | usage | Also Applies to |
|-------------|-------|-----------------|
| base-uri    | unclear | - |
| child-src   | used, if it contains value, otherwise 'default-src' | - |
| connect-src | used, if it contains value, otherwise 'default-src' | - |
| default-src | this directive is the fallback for many directives. | The follow directives use this directive as a fallback.<br>*child-src, connect-src, font-src, img-src, media-src, object-src, script-src, style-src* |
| font-src    | used, if it contains value, otherwise 'default-src'  | - |
| form-action | used, if it contains value, otherwise "acts" as if *fatal error* and report violation  | - |
| frame-ancestors<sup>¥</sup> | <s>(used, if it contains value, otherwise value is treated as "*".)</s><sup>¥</sup> | <s>The follow HTML elements are affected by this directive.<br>*frame, iframe, object, embed or applet*</s><sup>¥</sup> |
| frame-src   | is deprecated  | - |
| img-src     | used, if it contains value, otherwise 'default-src'  | - |
| media-src   | used, if it contains value, otherwise 'default-src' | - |
| object-src  | used, if it contains value, otherwise 'default-src' | overlaps with <s>'frame-ancestors'</s><sup>¥</sup> and 'plugin-types'<br>Unclear as to outcome. |
| plugin-types | restricts plugins via MIME type | - |
| report-uri<sup>¥</sup>  | -  | - |
| sandbox<sup>¥</sup>     | -  | - |
| script-src | used, if it contains value, otherwise 'default-src' | 'unsafe-inline' and 'unsafe-eval' are required to over-ride the default setting, regardless of 'default-src' |
| style-src  | used, if it contains value, otherwise 'default-src' | 'unsafe-inline' and 'unsafe-eval' are required to over-ride the default setting, regardless of 'default-src' |

- ¥ = W3 [CSP 3.3. HTML meta Element ](http://www.w3.org/TR/CSP2/#delivery-html-meta-element) says,<br>*5. Remove all occurrences of report-uri, frame-ancestors, and sandbox directives from directive-set.*

#### <a name=saferinline>Safer Inline</a> ####

Safer inline Javascript and CSS 

- nonce - cryptographically randomly generated value used to sign inline items
- hash - cryptographically generated hash value used to sign inline items

*Need add examples.*

#### <a name=references>References</a> ####

- [W3.org CSP2](http://www.w3.org/TR/CSP2/)
- [Mozilla CSP Policy Directives](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives)
- [Google Chrome Content Security Policy (CSP)](https://developer.chrome.com/extensions/contentSecurityPolicy)
- [http://content-security-policy.com/](http://content-security-policy.com/)
- [CSP Playground](http://www.cspplayground.com/home)
- [CanIUse](http://caniuse.com/#search=csp)
- [Crosswalk Content security policy](https://crosswalk-project.org/documentation/manifest/content_security_policy.html)


#### <a name=usefularticles>Useful Articles</a> ####

- [An Introduction to Content Security Policy](http://www.html5rocks.com/en/tutorials/security/content-security-policy/) - Updated: May 8th, 2015
- [Using Content Security Policy to Make Apps More Secure](https://60devs.com/using-content-security-policy.html) - Oct 13, 2015 

#### <a name=usefulblogposts>Useful Blog Posts</a> ####

- [[CSP] On Reporting and Filtering](https://blogs.dropbox.com/tech/2015/09/on-csp-reporting-and-filtering/) - September 21, 2015
- [[CSP] Unsafe-inline and nonce deployment](https://blogs.dropbox.com/tech/2015/09/unsafe-inline-and-nonce-deployment/) -  September 22, 2015


#### HISTORY ####

- 2015-11-28 - fixed typo, was *source-src* should have been *script-src*
