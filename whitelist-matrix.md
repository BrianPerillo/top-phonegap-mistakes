## Whitelist Worksheet (Matrix) ##
Date: 2015-10-26<br>
Last Update: 2015-12-12

[`The Whitelist System`](the-whitelist-system.md) -> `Whitelist Worksheet (Matrix)`

By this point you should know there are [three (3) platforms](https://github.com/jessemonroy650/top-phonegap-mistakes/blob/master/new-to-Phonegap.md#001) and [four (4) security systems](the-whitelist-system.md).

If you have never heard of a [decision table](https://en.wikipedia.org/wiki/Decision_table), today you'll learn of the value of them. This easy enough. Start at the top (1.) and continue till to the bottom (9.). If you drop out anywhere along the way. You are done.  

However, the tables in 1., 7., 9., and 10. are short cuts (or aides) to help you get to the right place. You will notice, the tables do not loop back or interconnect. This was intentionally done to help you work in a linear fashion.

This is, in fact, a simple expert system &ndash; with you as the feedback. Meaning, if you discover an error, let me know. I or someone else will adjust this set of tables.


1. [Is my version of Cordova/Phonegap included](#version)?
2. [Which `whitelist` guide should I use](#guide)?
3. [Which plugin, `whitelist` or `legacy-whitelist`](#plugin)?<br>**The Cordova `whitelist`**
4. [Do I have to apply `allow-navigation`](#navigation)?
5. [Do I have to apply `allow-intent`](#intent)?
6. [Do I have to apply `access origin`](#access)?
7. [How does this affect `inappbrowser`](#inappbrowser)?
8. [How do I apply those `config.xml` elements](#config.xml)?<br>**W3's CSP**
9. [CSP (Content Security Policy)](#csp)?<br>**Apple's ATS**
10. [ATS](#ats)?

### 1. <a name=version>Is my version of Cordova/Phonegap included</a> ###

```
cordova -v
```

It is recommend that you move to *Cordova Tools* V4.x as a minimum. Older versions are difficult to support and many plugins do not work correctly.

| Your Version of<br>*Cordova/Phonegap Tools* <sup>¢</sup> | `whitelist`<br>support | notes |
|----------------------------------|---------|-------|
| 2.x | Not support | deprecated |
| 3.x | Available   | white-list, plugin |
| 4.x | Required    | white-list, plugin |
| 5.x<sup>¥</sup> | Required    | white-list, plugin, CSP |
| Any above AND iOS9<sup>£</sup> | Required | Apple's ATS<sup>§</sup> |

- ¢ = Cordova and Phonegap versions do NOT align, but are close. This version is not the \*pinned* version either. If you do not know the difference, [learn](http://devgirl.org/2014/11/07/cordovaphonegap-version-confusion/) &mdash; [official release policy](https://github.com/apache/cordova-coho/blob/master/docs/versioning-and-release-strategy.md).
- ¥ = This include cli-5.1.0 and cli.5.2.0
- £ = As of 2015-11-04, iOS9 is officially supported by Cordova ([SEE](https://cordova.apache.org/announcements/2015/11/02/cordova-ios-3.9.2.html)) ; *Phonegap Build* is still waiting
- § = ATS requires &ndash; if you are using Apple's SSL, then the server you connect to must [run TLSv1.2](http://ste.vn/2015/06/10/configuring-app-transport-security-ios-9-osx-10-11/)
- \*\* = [Beginning May 9, 2016](https://github.com/jessemonroy650/top-phonegap-mistakes/blob/master/android-block-pre-4.1.1.md), Google Play will block publishing of any new apps or updates that use pre-4.1.1 versions of Apache Cordova.

### 2. <a name=guide>Which guide</a> ###

For all intensive purpose, there are three (3) whitelist guides. They all have mistakes, lack current information, and suffer from neglect. However, there are important pointers for each platform. So, if you are working on *Windows, Blackberry, Tizen*, or one of the other platform, do read the appropriate guide.

However, the best and most accurate information seems to come from from the documentation for the [`cordova-plugin-whitelist`](https://www.npmjs.com/package/cordova-plugin-whitelist) plugin. It is at the bottom of this list.

1. [Cordova Whitelist Guide](http://cordova.apache.org/docs/en/5.1.1/guide/appdev/whitelist/index.html) - Cordova CLI and SDK
2. [Phonegap Whitelist Guide](http://docs.phonegap.com/en/edge/guide_appdev_whitelist_index.md.html#Whitelist%20Guide) - Phonegap CLI and SDK
3. [Phonegap Build Whitelist Guide](http://docs.build.phonegap.com/en_US/configuring_access_elements.md.html#Access%20Elements) - Phonegap Build only
4. [cordova-plugin-whitelist](https://www.npmjs.com/package/cordova-plugin-whitelist) - npm


### 3. <a name=plugin>Which plugin, `whitelist` or `legacy-whitelist`</a> ###

If you are not aware of it, we have move to an [NPM system](https://cordova.apache.org/announcements/2015/04/21/plugins-release-and-move-to-npm.html)

- [`cordova-plugin-whitelist`](https://www.npmjs.com/package/cordova-plugin-whitelist)

> This plugin implements a whitelist policy for navigating the application webview on *Cordova Tools 4.0* (or higher).

If you have not implemented the `whitelist` system yet, then you want to start here. This plugin has more parameters and has better granularity that the legacy plugin.

- [`cordova-plugin-legacy-whitelist`](https://www.npmjs.com/package/cordova-plugin-legacy-whitelist)

> This plugin implements the *Cordova Tools 3.6* Whitelist policy for *Cordova Tools 4.0*. 

If you implemented the whitelist system for Android before `15 Apr 2015`, then you can still use the legacy system. As of that date, the new `whitelist` plugin is available and it runs with a different set of rules. While the `cordova-plugin-legacy-whitelist` is currently supported, you are \*strongly\* encouraged to move to the `whitelist` plugin.

##The Cordova `whitelist`##

The Cordova `whitelist` has three (3) parts; `allow-navigation`, `allow-intent`, and `access origin`.

- [Cordova whitelist Examples]( whitelist-examples.md)

### 4. <a name=navigation>allow-navigation</a> ###

Controls which URLs the \*WebView\* itself can be navigated to. Applies to top-level navigations only. 

By default, navigations only to `file://` URLs are allowed. To allow other URLs, you must add `<allow-navigation>` tags to your `config.xml`.

*Quirks:* On Android, it also applies to iframes for non-http(s) schemes. This includes, but is not limited to: `data:`.

**DANGEROUS-SETTING:** `<allow-navigation href="*" />`

### 5. <a name=intent>allow-intent</a> ###

Controls which URLs the app is allowed to ask the \*system\* to open. By default, no external URLs are allowed.

This whitelist does not apply to plugins, only hyperlinks and calls to `window.open()`.

*Quirks:* On Android, this equates to sending an intent of type BROWSEABLE. This includes, but is not limited to: `tel:`, `sms:`, `mailto:`, `geo:`, `market:`, `itms:`, `itms-apps:`.

**DANGEROUS-SETTING:** `<allow-intent href="*" />`

### 6. <a name=access>access origin</a> ###

Controls which \*network\* requests (images, XHRs, etc) are allowed to be made (via cordova native hooks).

Without any `<access>` tags, only requests to `file://` URLs are allowed. However, the default Cordova application includes `<access origin="*">` by default.

*Quirk:* Even without this, Android also allows requests to `https://ssl.gstatic.com/accessibility/javascript/android/` by default, since this is required for TalkBack to function properly.

**DANGEROUS-SETTING:** `<access origin="*" />`

### 7. <a name=inappbrowser>inappbrowser</a> ###

This is one point where the documentation conflicts with it's self. If you do a search for 'whitelist' on the [documentation](https://www.npmjs.com/package/cordova-plugin-inappbrowser) you will see conflicting lines. The inconsistencey in the writing style points to multiple authors, poor design, and neglect. At a minimum, expect a rewrite of the documentation.

*In the middle of the  3rd paragraph is*

> The InAppBrowser is not subject to the whitelist, nor is opening links in the system browser.

*In `cordova.InAppBrowser.open()`, under 'target' is*

> _self: Opens in the Cordova WebView, if the URL is in the white list, otherwise it opens in the InAppBrowser.

### 8. <a name=config.xml>How do I apply those `config.xml` elements</a> ###

<b>FOREWARNED.</b> The documentation suggests that the CSP be used instead of `access origin`. I disagree. CSP is confusing and unclear. However, `access origin` is weak and ripe for more security warnings. There is inconsistency in both methods. *As a developer, you should make an objective decision on this.* <b>FOREWARNED.</b>

| Which XML element  |  Controls  |  Quirks  |
|--------------------|------------|----------|
| `allow-navigation` | [WebView](webview.md)<sup>¿</sup> | *Android* applies this to iframes (non-http(s))
| `allow-intent`     | URI or URL request to the system `window.open()`<sup>¿</sup> | 1. *Android* equivalent to BROWSEABLE<br>2. <b>does not apply to plugins</b>
| `access origin`    | Controls network requests (images, XHRs, etc) via Cordova Hooks | *Android* makes allowance for [Talkback](http://www.androidcentral.com/what-google-talk-back) <sup>µ</sup><br>the default Cordova application includes `<access origin="*">`

- ¿ = It unclear from the documentation how this interacts with `inappbrowser`.
- µ = The documentation alludes that Android has this built-in, or hard-coded.

- See [access by default](whitelist-examples.md#bydefault)


#### [Cordova whitelist Examples]( whitelist-examples.md) ####


## W3's CSP ##
### 9. <a name=csp>CSP (Content Security Policy)</a> ###

- [CSP Examples]( whitelist-csp-examples.md)

**Opinion**
> CSP has to be the most *heinous* part of the `whitelist` system. It has sixteen (16) directives and they have overlapping logic. Note, some developer have implemented this in a morning. *You are forewarned.*

The CSP is configured per web page using HTTP headers (or HTML meta equivalent). Whenever the browser loads an HTML document, the response headers of the HTTP  (or HTML meta equivalent) request that the delivered document use the *content security policy* for all content that originates from this HTML document.

The CSP is configured via a single header (`Content-Security-Policy` or `X-Content-Security-Policy`). The value of this header is a string that effectively sets directives separated by a semicolon. The directives define a list of sources that are safe to communicate with. There is a directive available for each of the resource types such as images, xhr, styles, scripts.

Several websites were culled for information to give this specificaiton clarity.

- [W3.org CSP2](http://www.w3.org/TR/CSP2/)
- [Mozilla CSP Policy Directives](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives)
- [http://content-security-policy.com/](http://content-security-policy.com/)


**Directives**

Each of the directives listed can define one or more sources that are safe to consume. 
You can combine sources, which can include 'CSP Keywords', 'CSP Data(words)', and 'Host (regular) Expressions'. Note, per the specification, not all directives apply. See [Whitelist CSP Examples](whitelist-csp-examples.md) -> [Access By Default](whitelist-csp-examples.md#bydefault) for details on this.

| CSP Directives | W3 Section | Governs
|----------------|------------|--------
| base-uri    | 7.1  | restricts the URLs that can be used to specify the document base URL
| child-src   | 7.2  | the nested browsing contexts and the execution of Workers
| connect-src | 7.3  | network activities such as XMLHttpRequest::send
| default-src | 7.4  | sets a default source list for a number of directives
| font-src    | 7.5  | fonts loading
| form-action | 7.6  | form submissions
| frame-ancestors | 7.7  | **Not Available** ~~embedding of iframes, objects, applets etc~~
| frame-src   | 7.8  | *is deprecated*
| img-src     | 7.9  | loading of images
| media-src   | 7.10 | video and audio sources
| plugin-types | 1.12 | (these are MIME-type plugins, not Cordova)
| report-uri  | 7.13 | **Not Available** ~~a URL to which the user agent sends reports about policy violation~~
| sandbox     | 7.14 | **Not Available** ~~specifies an HTML sandbox policy that the user agent applies to the protected resource~~
| object-src  | 7.11 | objects (flash, audio, etc.)
| script-src  | 7.15 | scripts execution
| style-src   | 7.16 | CSS sources

- SEE [W3.org directives](http://www.w3.org/TR/CSP2/#directives) for more details

**CSP Keywords** 

- \*The single quotes are required.*

| CSP Keywords | Meaning
|---------|---------
| 'none' | Refers to the empty set; that is, no URLs match.
| 'self' | Refers to the origin from which the protected document is being served, including the same URL scheme and port number.
| 'unsafe-inline' | Allows the use of inline resources, such as inline `<script>` elements, javascript: URLs, inline event handlers, and inline `<style>` elements.<br>**This is what makes this ridiculous for mobile Apps**<br>  [Early event handlers](http://www.quirksmode.org/js/events_early.html) -- [Working with Inline Event Handlers](http://www.htmlgoodies.com/html5/javascript/working-with-inline-event-handlers.html)
| 'unsafe-eval' | Allows the use of eval() and similar methods for creating code from strings. <br>**What does "similar methods" mean?** No one says.

- SEE [W3.org source list syntax](http://www.w3.org/TR/CSP2/#source-list-syntax) for some details. It is no clear definition or descriptions for these keywords.
- SEE [Mozilla Keywords](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives#Keywords) for some definitions.

**CSP Data** 

| CSP Data(words) | Meaning 
|------|---------|
| data: | Allows `data:` URIs to be used as a content source. This is insecure; an attacker can also inject arbitrary `data:` URIs. Use this sparingly and definitely not for scripts.
| mediastream: | Allows `mediastream:` URIs to be used as a content source.
| blob: | Allows `blob:` URIs to be used as a content source. <br>**This is another ridiculous thing for mobile Apps**
| filesystem: | Allows `filesystem:` URIs to be used as a content source.<br>**This is another ridiculous thing for mobile Apps**
| gap: | *This dataword only applies to Cordova/Phonegap.* At this time, it is required only on iOS (when using UIWebView) and is needed for JS->native communication. **It is badly documented**

- SEARCH [W3.org CSP2](http://www.w3.org/TR/CSP2/) for some usage rules
- SEE [Mozilla Data](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives#Data) for some description on these words.
- SEE [cordova-plugin-whitelist](https://www.npmjs.com/package/cordova-plugin-whitelist); search for `gap:`. This is the extent of the documentation.

**CSP Host (regular) Expressions**

| name     |   usage  | Example |
|----------|----------|---------|
| need add | examples | - |

There are four (4) documents worth reading on this subject.

- [Content Security Policy Reference](http://content-security-policy.com/) Website
- [CSP Policy Directives](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives#Keywords) - Mozilla
- [Using Content Security Policy to Make Apps More Secure](https://60devs.com/using-content-security-policy.html) - #60devs
- [CanIUse](http://caniuse.com/#search=csp)

## Apple's ATS##
### 10. <a name=ats>ATS</a> ###

As part of Apples move to iOS 9, Apple is mandating the use of [Apple Transport System](https://web.archive.org/web/20150905111538/https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/) (ATS). This basically means that all web communications should use `https:` rather than `http:`. At the moment, it is easy to create exceptions to the ATS policy. Apple is likely to tighten up these exceptions over time, though it is difficult to see how they can make it 100% mandatory. The issue being that some server sites simply will not work over `https:` and may never convert.

Note, there are known issues with some sites using `https:` *and* Apple's ATS. It is not as simple as adding an 's' to `http:`. Apple mandates [TLS1.2](https://en.wikipedia.org/wiki/Transport_Layer_Security) (or better) when using iOS9 and `https:`. On some sites, TLS1.2 will not work, though they will work with `https:`. Google Ads and Amazons Web Services are known to have problems. Luckily, it is easy to create an exception.

- Google Ads [Handling App Transport Security in iOS 9 ](http://googleadsdeveloper.blogspot.com/2015/08/handling-app-transport-security-in-ios-9.html)
- AWS [Preparing Your Apps for iOS 9](https://mobile.awsblog.com/post/Tx2QM69ZE6BGTYX/Preparing-Your-Apps-for-iOS-9)
- [ATS Examples]( whitelist-ats-examples.md)
- You can also by pass this subject by using an alternate SSL driver, more on that later.

**Turns off all security.** [Google](http://googleadsdeveloper.blogspot.com/2015/08/handling-app-transport-security-in-ios-9.html) and [Ionic](http://blog.ionic.io/preparing-for-ios-9/) provide these answers.<br>*CAUTION:* Your app maybe rejected, unless you have a good reason for using this. [SEE](whitelist-ats-examples.md#appRejected)
```
<key>NSAppTransportSecurity</key>
    <dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    </dict>
```

**Solution for amazonaws.com**
```
<key>NSAppTransportSecurity</key>
<dict>
      <key>NSExceptionDomains</key>
      <dict>
            <key>amazonaws.com</key>
            <dict>
                  <key>NSThirdPartyExceptionMinimumTLSVersion</key>
                  <string>TLSv1.0</string>
                  <key>NSThirdPartyExceptionRequiresForwardSecrecy</key>
                  <false/>
                  <key>NSIncludesSubdomains</key>
                  <true/>
            </dict>
            <key>amazonaws.com.cn</key>
            <dict>
                  <key>NSThirdPartyExceptionMinimumTLSVersion</key>
                  <string>TLSv1.0</string>
                  <key>NSThirdPartyExceptionRequiresForwardSecrecy</key>
                  <false/>
                  <key>NSIncludesSubdomains</key>
                  <true/>
            </dict>
      </dict>
</dict>
```



### 11. <a name=notworking>Still not working</a> ###


- You can search through all ["core" plugin bugs](plugins-core-bugs.md)
- You can check all the [Whitelist bugs](https://issues.apache.org/jira/browse/CB-9133?jql=project%20%3D%20CB%20AND%20issuetype%20%3D%20Bug%20AND%20status%20%3D%20Open%20AND%20resolution%20%3D%20Unresolved%20AND%20component%20in%20%28%22Plugin%20TestFramework%22%2C%20%22Plugin%20Whitelist%22%29%20ORDER%20BY%20priority%20DESC%2C%20summary%20ASC%2C%20updatedDate%20DESC)


----

#### Unapplied Notes ####

When I say the information about the `whitelist` system was buried, I mean these three (3) blog posts

Plugins Release and Moving plugins to npm: April 21, 2015
SEE: New Whitelist Plugins
https://cordova.apache.org/announcements/2015/04/21/plugins-release-and-move-to-npm.html

Apache Cordova Android 4.0.0: 15 Apr 2015
SEE: Major Changes 
http://cordova.apache.org/announcements/2015/04/15/cordova-android-4.0.0.html
- CSP is now supported
- Network requests are blocked by default without the plugin
- latest default app created by cordova-cli will include this plugin by default

SEE: org.apache.cordova.inappbrowser@0.5.4

SEE: Major Changes
http://cordova.apache.org/announcements/2015/04/15/cordova-android-4.0.0.html

What's New in iOS
https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html

Differences between test and production release for Android - A network puzzler
https://groups.google.com/d/msg/phonegap/ALL8SsuFCiQ/4m_WN6NcAwAJ
> The problem was caused by an intermediate certificate not being installed correctly on a CDN server. The main wildcard certificate installed on the CDN server was correct and that (...) The fact that the CDN server said everything was OK and IOS was very happy only lulled us into a false sense of security. 
> (Another developer remarks) *In short: certs are painful and capable of causing concussions and large dents in desks.*

