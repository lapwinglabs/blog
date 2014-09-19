```
title: Bluu: TestFlight vs. Crashlytics vs. HockeyApp
tags: TestFlight, Crashlytics, Hockey App, beta testing, crash reporting, Bluu
date: August 3, 2014
slug: bluu-testflight-crashlytics-hockeyapp
excerpt: We're happy to be using TestFlight for seamless beta distribution services and Crashlytics for its awesome crash reporting.
author: Andy Pai
```
### Bluu: TestFlight vs. Crashlytics vs. HockeyApp
At the end of the weekend, we wanted to get Bluu into some hands to start getting feedback. Deployment was surprisingly far more complicated than we had envisioned.

We had to reach out to our friends and family to get their UDIDs, Apple's unique identifier for iPhones. As expected, none of them had ever heard of a UDID. Some started worrying we were trying to send them some type of phone virus. After some explaining, we had to get them to either:

1. Download an app that gave them their UDID or 
2. Plug in their iPhones into their computer to get it for us. 

But the pain didn't stop there. Once we received their UDID's, we had to create a build including the collected UDID's and send it to them for installation via e-mail. Once they received the package, they had to reconnect their phone to iTunes and install the app. Even after these steps, sometimes the app wouldn't install! We needed a better beta testing experience, or we risked losing all our friends.

The three services we came across that promised to improve our workflow were TestFlight, Crashlytics and HockeyApp. HockeyApp charges $10 / month even for their cheapest plan, so they were out.

After some research on TestFlight and Crashlytics, our first impressions were:

TestFlight

- Free
- Collects UDID's by downloading an app on the user's phone. They use the same app to deliver the app for download. No chords necessary. Hallelujah!
- Recently acquired by Apple, so should work seamlessly for iOS stuff. Unfortunately, this also means that they won't support Android anymore
SDK integration provides crash reporting and user data

Crashlytics

- Free? Seriously? Even the Enterprise services? Is this a scam?... Oh, they got acquired by Twitter. Hopefully Twitter's deep pockets can probably afford to keep it around and free, at least for a while :)
- Offers beta distribution for iOS and Android
- Crash reporting seems more visual and intuitive than TestFlight

After trying out Crashlytics and TestFlight, we decided to use both. TestFlight no longer allows downloads of their SDK, so you don't get session tracking, crash logging, and in-app updates. While Crashlytics offers a complete solution including Beta distribution, TestFlight's beta distribution is far more seamless and less buggy. Having to use Crashlytics for crash reporting isn't all a loss though since it's reporting format is superior to TestFlight's format. Crashlytics presents all data with amazing visuals making it very easy to interpret and act accordingly.

So, we're happy to be using TestFlight for seamless beta distribution services and Crashlytics for its awesome crash reporting.
