# Origin Trials Guide for Web Developers

Origin trials allow developers to try out new features and give feedback on usability, practicality, and effectiveness to the web standards community. Your feedback is valuable input into the final decision about the feature design, or even whether we want to proceed with standardizing and enabling the feature by default. When a feature is available as an origin trial, you are able to register to have it enabled for all users on your origin for a fixed period of time. Note that when the trial finishes we will contact you with a request to provide this feedback.

Once your origin has opted into a trial of an experimental feature you can then build demos and prototypes that your friends and beta testing users can try for the duration of the trial without them needing to flip special flags in Chrome.

## How do I enable an experimental feature on my origin?

You can opt any page on your origin into the trial of an experimental feature by [requesting a token for your origin](https://developers.chrome.com/origintrials/). After signing up for a trial, we will generate a token for your origin.

There are two ways to provide this token on any pages in your origin:

- Add an `origin-trial` \<meta\> tag to the head of any page. For example this may look something like:

```
<meta http-equiv="origin-trial" content="**insert your token as provided in the developer console**">
```

- If you can configure your server, you can also provide the token on pages using an `Origin-Trial` HTTP header. The resulting response header should look something like:

```
Origin-Trial: **token as provided in the developer console**
```

**NOTE:** 

- You can provide multiple tokens for a given page. See [Can I provide multiple tokens on a page?](developer-guide.md#15-can-i-provide-multiple-tokens-on-a-page).
- You can provide tokens programmatically, via script. See [Can I provide tokens by running script?](developer-guide.md#16-can-i-provide-tokens-by-running-script).

If you have trouble configuring pages with your token, or need other help, please contact us at origin-trials-support@google.com.

## How can I experiment with the new feature locally?

Each feature that is available as an origin trial can alternatively be enabled on individual machines by flipping the corresponding flag in about:flags. The correct flag depends on the feature, and should be mentioned in the blog post about that specific feature.

You can get started experimenting with the new feature on `localhost` either by flipping the flag locally or requesting an origin trials token for `localhost`.

## What is the thinking behind origin trials?
An exploration of the motivations and reasoning behind origin trials is provided in [the explainer](explainer.md). The TL;DR is that we strongly value the feedback of real web developers (that means you!) during the process of designing and standardizing new features. We believe origin trials provide a good way of encouraging that feedback, while being extremely careful that the experiments aren’t used by sites in production-critical roles or as if they’re finalized features.

## What experimental features are currently available?
The [developer console](https://developers.chrome.com/origintrials/#/trials/active) lists all of the currently available features.

## FAQ

### 1. How can I find out about new experiments when they become available?
  - When you register for an origin trial token you will be automatically added to a mailing list. We'll use this list to send high level updates about the origin trials system, including announcing new features.
  - Additionally, we will be posting updates to [developers.google.com/web/updates/](http://developers.google.com/web/updates/) about each new feature that becomes available as an origin trial.
### 2. Will all of these experiments ship eventually?
  - These are only experiments and there is a good chance that some of them will never ship as standardized features on the web. These experimental features are essentially very similar to Chrome flags: an exciting glimpse into one possible future that you can play around with today, and provide feedback for.
### 3. What happens if a large site such as a Google service starts depending on an experimental feature?
  - Origin trials have a built-in safeguard that automatically disables an experimental feature globally if its usage exceeds 0.5% of all Chrome page loads. This is to keep usage limited to developers experimenting and below Chrome’s threshold whereby features used on less than 0.5% of all page loads (as measured by [Chrome Status](https://www.chromestatus.com/metrics/feature/popularity)) may be deprecated.
### 4. Isn’t this just vendor prefixing all over again?
  - This topic has been explored in depth in Alex Russell’s Medium post [Doing Science on the Web](https://medium.com/@slightlylate/doing-science-on-the-web-af26d9be2faa#.94pf1lwmp). A couple of key differences include:
    - These features automatically stop working before they become too broadly adopted.
    - Developers cannot simply copy-paste sample code using an experimental feature, as they must provide a unique trial token obtained via the experimental feature registration signup process (and accept that the feature is going to shortly stop working).
### 5. Does this change impact how we think about security or privacy on the web?
  - No, these experimental features have all been held to the same high privacy and security standards as any Chrome platform feature.
### 6. Is there any restriction on which websites can sign up to use experimental features?
  - Origin trials are available to any website served over HTTPS. Note that there is no policy against specific large sites opting into origin trials, but the system is designed to prevent large populations of the web depending on experimental features. To achieve that, origin trials have a built-in safeguard that automatically limit it globally if its usage exceeds 0.5% of all Chrome page loads. This means that experimental features aren’t suitable for use on large production sites such as the Google home page.
### 7. Is there any review process for signing up a website to access an experimental features?
  - No. We do not review domain content before generating a token.
### 8. Is there a way to only enable an origin trial for some of my users or only some pages on my site?
  - Yes, origin trials are enabled on a per-page basis.
### 9. Will these experiments work in Opera or other web browsers?
  - Not today, but if this model proves to work well then it’s possible that other web browsers may build their own origin trials system. The experimental feature implementations likely won’t be compatible between browsers due to the nature of them being experimental.
### 10. Can I request a token for an origin that I don't own?
  - Yes, you can technically request a token for an origin that you don't own. However, generating the token won't cause the feature to be enabled on that origin - unless it is served in the pages on that origin (either in the \<head\> or as an HTTP header). Also note that these features have held up to the same high security and privacy standards as any other feature in Chrome.
### 11. Why do tokens expire before the trial ends?
  - Currently, we issue tokens that expire in 6 weeks. Before a token expires, we'll send you an email to invite you to renew and continue participating in the trial. We issue tokens that expire for a number of reasons, most importantly:
    - To prevent experimental features from becoming "burned in" to the web platform. With shorter-lived tokens, we can ensure that no site can use a feature for more than a month or two, without checking in with us.
    - To provide an opportunity to collect feedback on features from web developers. We can ask for feedback on each token renewal.
### 12. How do I renew a token that is about to expire/has expired?
  - You should receive a reminder email to renew the token before it expires. That email includes a link to the registration page in the [developer console](https://developers.chrome.com/origintrials/#/trials/active). You can also go directly to the page in the console, by finding the trial in the list on [My Registrations](https://developers.chrome.com/origintrials/#/trials/my).
  - On the registration page, you may need to provide feedback before you can renew. Use the Feedback button to complete a survey to provide feedback.
  - On the registration page, the Renew button will be available when feedback has been provided. Use the Renew button to generate a new token.
  - Feedback is required to renew tokens, approximately every 6 weeks. If you have multiple origins registered for a trial, we'll only ask you for once per period, rather than for every origin.
### 13. I have multiple testing/staging domains, or subdomains that are programmatically generated. Do I need to request a token for every subdomain?
  - No, we can issue a single token that will match multiple subdomains. These tokens will behave similarly to wildcard matching (like specifying "\*.\<some domain\>"). For example, you can request a token for "example.com", and it will enable the feature on all origins whose suffix matches "example.com", including:
    - a.example.com
    - b.example.com
    - a.b.example.com
    - example.com
  - To ensure that an experimental feature is not enabled too broadly, there are some additional checks on requests for subdomain tokens. Specifically, tokens will not be issued for origins found in the [Public Suffix List](https://publicsuffix.org/).
  - Subdomains do not apply to IP addresses. Tokens issued for IP addresses will only allow exact matching on origin, as before.
  - You can request a subdomain token by filling out the appropriate field on the trial signup form.
### 14. Are there different types of trials?
  - Yes, we've starting using the origin trial infrastructure and console to allow developers to temporarily control Chrome's behavior in different ways. For example, to register for a [temporary extension to Web Components V0 deprecation](https://developers.chrome.com/origintrials/#/view_trial/2431943798780067841). We're also planning to have trials to allow developers to opt-in or opt-out of some Chrome interventions (especially those based on heuristics). Some aspects of these trials may be different, such as the expiry period for tokens. However, all types of trials will be limited in duration - this is not meant as a new mechanism for permanent configuration.
### 15. Can I provide multiple tokens on a page?
  - Yes, you can provide multiple tokens for a given page. Only one valid token is required to enable a trial, any other invalid tokens or non-matching tokens are ignored. You may want to provide multiple tokens if the same page is served to different origins (e.g. example.com, example.ca, etc.). You can add multiple tokens in different ways:
    - Add multiple \<meta\> tags to the page, where each tag contains a single token.
    - Include multiple `Origin-Trial` response headers, where each header contains a single token.
    - Include a single `Origin-Trial` response header, with comma-separated tokens.
### 16. Can I provide tokens by running script?
  - Yes, you can provide tokens programmatically, from a script running on a given page. A token 
  can be provided by creating and injecting a \<meta\> tag into the head of the page, in the same
  format as would be provided in HTML markup. For example, using a function like:

    ```
      function addTrialToken(tokenContents) {
        const tokenElement = document.createElement('meta');
        tokenElement.httpEquiv = 'origin-trial';
        tokenElement.content = tokenContents;
        document.head.appendChild(tokenElement);
      }
    ```
  - Limitations for tokens injected via script include:
    - The token must match the origin of the page containing the script (i.e. the *first-party*
    origin). For external scripts (e.g. included as \<script src="some origin"\>), the token
    validation ignores the origin of the script. This does not apply to
    [*third-party* tokens](developer-guide.md#18-how-can-i-enable-an-experimental-feature-as-embedded-content-on-different-domains).
    - The token can usually be injected later in page lifecycle (e.g. after loading), but must be
    injected before any attempt to use the experimental feature, including feature detection. For
    experimental features that are exposed to Javascript, once the token is injected, you'll be
    able to test for API presence as expected.
    - There may be expermental features that are not compatible with token injection via script.
    For example, features that affect the initial loading of page or require configuration via HTTP
    response headers.
### 17. Are experimental features enabled in iframes?
  - Not by default - iframes in a page **do not** inherit the experimental features that are
  enabled from their containing page. When the page provides a token to enable an experimental
  feature, that only applies to the top-level document. Each iframe is considered a separate
  document, and must independently provide a valid token to enable an experimental feature. As
  well, each iframe must provide a token that matches its own origin, not the origin of the
  containing page. Finally, this applies to all nested iframes.
### 18. How can I enable an experimental feature as embedded content on different domains?
  - You can enable an experimental feature as embedded content, as long as you have third-party
  script running in the containing page.
  - To enable as embedded content, there are two options to request the appropriate tokens(s):
    1. Register for a token for each target origin.
        - Available for all trials.
        - May be feasible if you have a known, short list of the domains that embed your content.
        - Requires you to provide the correct token from your script, which matches the containing
        page.
    2. Register once for a *third-party* token for your own origin.
        - Currently, available on a limited basis for some trials.
        - Requests are subject to verification to prevent misuse.
        - Allows you to use a single token on any embedding page, regardless of its origin.
        - Requires you to provide the token from a script served from your origin. Inline scripts
        are not supported.
  - Your third-party script must provide the appropriate token as described in
  [Can I provide tokens by running script?](developer-guide.md#16-can-i-provide-tokens-by-running-script).