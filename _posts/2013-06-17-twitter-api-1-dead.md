---
layout: post
title:  "Twitter API 1.0 now 100% dead and my website too"
date:   2013-06-17 21:33:00
categories: twitter
---

Twitter *gracefully* dropped dead support of API 1.0, your calls are now being
served a `410 GONE` http answer with the folloing explaination :

    {"errors":
      [
        {
          "message": "The Twitter REST API v1 is no longer active.
            Please migrate to API v1.1. https://dev.twitter.com/docs/api/1.1/overview.",
          "code": 68
        }
      ]
    }

The worst thing is JSONP support is also completely dropped and CORS
(cross origin resource sharing) requests aren't allowed either
(all API calls muse be fully authentified via oAuth).

We were doing client-side request to twitter search API on some websites and that is now a thing
of the past. Must use a server-side proxy (thanks Heroku for the free dynos!).

I should update j15e.com to show my timeline using API 1.1, but as I am not very interested in
updating old [PHP code][j15-github] running on recently acquired [AppFog][appfog],
I think I'll just wait until I find some time to just build a new nicer page.

[j15-github]: https://github.com/j15e/j15e.com
[appfog]: https://www.appfog.com/savvis/