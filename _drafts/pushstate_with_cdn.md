title: How to Use pushState with CDNs

# Problem

HTML5 pushState is used to make app URLs look better (no #!) and is 
sometimes required for redirects to work. Like redirecting back to the app URL
after OAuth.

Let's say app just has index.html and is served from a CDN.

CDN (S3, CloudFront, CloudFlare, etc.) does not know that an app 
URL like `your.app/user/me/activity` should just serve `your.app/index.html`.

Most CDNs support redirect rules, but not rewrite rules.


# Solutions

* http://stackoverflow.com/questions/16267339/s3-static-website-hosting-route-all-paths-to-index-html
* Elastic Load Balancer
* server that does rewrite rules as backing for CDN


