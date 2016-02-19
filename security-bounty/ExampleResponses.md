# Example Responses

## Already Tracked

> Hello,
> 
> Thank you for your report. This is a duplicate of a previously reported issue (tracked internally as #cjLCQ8Jg).
> 
> While re-use of session cookies is possible, it's an understood effect of the bearer-token style of cookie authentication we employ. Since these cookies are only delivered over SSL, copying the cookie prior to sign-out would require the client browser to already be compromised, so would not expose anything that wasn't already exposed.
> 
> Please let me know if you believe this to be incorrect. Thanks,
> 
> Joey Aghion
> 
> Artsy

## Nonissue

> Hi Mayur,
> 
> Thanks for reporting this issue and sorry for the delay replying.
> 
> I've investigated and found that the profiles you mention are associated with accounts with different email addresses. As such, I can't verify your claim that multiple accounts can share an email.
> 
> As always, please let me know if you think this assessment is incorrect. Thanks for your attention to Artsy's security.
> 
> Joey Aghion
> 
> Artsy

## Eligible for Bounty

> Hello Shailesh,
> 
> Thanks for your report. Open redirects are a legitimate issue. This particular one is tracked internally as #pqknAkIK. We address security issues according to their severity, and while we don't have immediate plans to correct this one, it will be eligible for a bounty should we deploy a fix.
> 
> The code for developers.artsy.net is open-source at https://github.com/artsy/doppler in case you want to beat us to it.
> 
> Thanks,
> 
> Joey

## Bounty Nag

_Quoted from [https://www.artsy.net/security](https://www.artsy.net/security)_

> We are a small and very busy Engineering Team, and we receive a lot of email. Please do not send us multiple or repetitious email asking the same questions about submitted reports or the status of potential bounty payments. This will not accelerate the process, and may actually result in a slower response due to the extra burden on our inbox.
> 
> We appreciate your patience.


## Confirm Fix to Receive Payment

> This is fixed (in https://github.com/artsy/doppler/pull/90). Please confirm?
> 
> Since this security bug report is eligible for a 25$ bounty. Please send me:
> 
> * Your full name and postal address.
> * A Paypal account e-mail address where we can send the payment.
> * A name + link that you would like to use for being listed on https://www.artsy.net/security.
> 
> Thanks,
> 
> dB.

## Bounty Complainer

_To-Do_

