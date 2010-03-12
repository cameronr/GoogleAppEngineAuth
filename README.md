# GoogleAppEngineAuth

Some Objective-C classes for implementing Google's ClientLogin API ([http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html][ClientLoginAPI]),
particularly for use with Google App Engine

## Usage

#### Dependencies

This code depends on the Google Toolbox for Mac:

[http://code.google.com/p/google-toolbox-for-mac/][GTM]

The only method used is gtm_stringByEscapingForURLArgument (which escapes '&', unlike stringByAddingPercentEscapesUsingEncoding).
If you don't want to use the Google Toolbox for Mac, you can implement your own escaping function:

[http://simonwoodside.com/weblog/2009/4/22/how\_to\_really\_url\_encode/][HowToEncodeURL]

#### GoogleClientLogin class

Here's some sample code showing how to use the GoogleClientLogin class:

setup:

    GoogleClientLogin *gcl = [[GoogleClientLogin alloc] initWithDelegate:self];

    [gcl authWithUsername:someUsername andPassword:somePassword forService:someService withSource:someSource];

    < time passes and one of the delegate methods will be called ... >

delegate methods:

    - (void)authSucceeded:(NSString *)authKey { }

    - (void)authFailed:(NSString *)error { }

    - (void)authCaptchaTestNeededFor:(NSString *)captchaToken withCaptchaURL:(NSURL *)captchaURL { }


#### GoogleAppEngineAuth class

In order to get a cookie compatible with Google App Engine, you should use the GooleAppEngineAuth class (which uses the 
GoogleClientLogin class):

setup:
    GoogleAppEngineAuth *gaea = [[GoogleAppEngineAuth alloc] initWithDelegate:self andAppURL:[NSURL URLWithString:YOUR_APP_URL]];
    
    [gaea authWithUsername:someUsername andPassword:somePassword  withSource:someSource];

    < time passes and one of the delegate methods will be called ... >

delegate methods:

    - (void)authSucceeded:(NSString *)authKey { }

    - (void)authFailed:(NSString *)error { }

    - (void)authCaptchaTestNeededFor:(NSString *)captchaToken withCaptchaURL:(NSURL *)captchaURL { }

#### Captchas

If a CAPTCHA is required, the authCaptchaTestNeededFor delegate method will be called. In order to auth successfully, you should display
the CAPTCHA image to the user and ask them to enter what they see. Then, you'll need to pass what they entered along with the captchaToken
back to either GoogleClientLogin or GoogleAppEngineAuth.


 [ClientLoginAPI]: http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html 
 [GTM]: http://code.google.com/p/google-toolbox-for-mac/ 
 [HowToEncodeURL]: http://simonwoodside.com/weblog/2009/4/22/how_to_really_url_encode/

