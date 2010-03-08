# GoogleAppEngineAuth

Some Objective-C classes for implementing Google's ClientLogin API (http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html),
particularly for use with Google App Engine

## Usage

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

