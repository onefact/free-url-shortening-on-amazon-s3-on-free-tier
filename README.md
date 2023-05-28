## 🐂 + 💩 [Demo](https://puns.onefact.org/) (redirection rules below to let you create your own shortlinks such as [puns.onefact.org/create](https://puns.onefact.org/create) :)

This is like Bitly but free! 

As a small nonprofit, we can't afford custom URL shortening with services like Bitly (https://bit.ly, $348/year for custom domain support).

The alternative we found is to simply create redirect rules in Amazon S3 and edit a single json file.

# Steps to use this on your custom domain
We recommend following these steps by cloning this repository locally, using Visual Studio Code: https://learn.microsoft.com/en-us/azure/developer/javascript/how-to/with-visual-studio-code/clone-github-repository

(Visual Studio code lets you write new redirection rules even more easily using Copilot, which is *free* in the GitHub Education pack (free with an email address ending in `.edu`), using the following steps: https://education.github.com/pack.)

Steps:
* [one time] issue an SSL certificate for the domain (e.g. https://onefact.org) in Cloudfront
* [as-needed] to create a subdomain on which to generate shortlinks (e.g. [photos.onefact.org](https://photos.onefact.org)), [give.onefact.org](https://give.onefact.org)))
  * create an Amazon S3 bucket [direct link to create a bucket](https://s3.console.aws.amazon.com/s3/bucket/create?region=us-east-1) with the name subdomain.customdomain.com (e.g. photos.onefact.org): 
 
<img width="901" alt="image" src="https://user-images.githubusercontent.com/5317244/236970040-f60db2dc-f0ef-4113-9d81-87d321d9f058.png">

  * enable static website hosting on the bucket - click on the bucket name in the list: https://s3.console.aws.amazon.com/s3/buckets -- click on "edit" here:

<img width="1357" alt="image" src="https://user-images.githubusercontent.com/5317244/236970680-267c97a7-6f84-42c6-8855-31b9743cf733.png">
  * click on the "Enable" radio button here in the "Edit static webhosting": https://s3.console.aws.amazon.com/s3/bucket/puns.onefact.org/property/website/edit?region=us-east-1

<img width="1178" alt="image" src="https://user-images.githubusercontent.com/5317244/236970761-94e49d41-5ccf-48b4-ba14-dbb80b3da328.png">

  * in the bucket page, for example at https://s3.console.aws.amazon.com/s3/buckets/puns.onefact.org?region=us-east-1&tab=objects, click on "Properties"

<img width="1452" alt="image" src="https://user-images.githubusercontent.com/5317244/236970508-aff91a54-60f6-4eed-8de9-7310c3ef2109.png">

* [as-needed] to edit, add, delete individual shortlinks, click on "Edit properties" at the bottom of the page for the bucket management screen: https://s3.console.aws.amazon.com/s3/buckets/photos.onefact.org?region=us-east-1&tab=properties

<img width="1350" alt="image" src="https://user-images.githubusercontent.com/5317244/236968502-92740d33-97d4-4533-9643-27904a038b59.png">

  * edit the property that is at the bottom of the property, called "Redirection rules – optional" (https://s3.console.aws.amazon.com/s3/bucket/photos.onefact.org/property/website/edit?region=us-east-1)
  * copy and paste one of the below sets of JSON redirects -- **this is where the magic happens and you get to make "subdomain.customdomain.com/myshorterlink" redirect to a long link!** on this page: https://s3.console.aws.amazon.com/s3/bucket/puns.onefact.org/property/website/edit?region=us-east-1 

<img width="799" alt="image" src="https://user-images.githubusercontent.com/5317244/236970935-332c06c2-bdb0-4c1d-8a7d-255932ac3d28.png">

 * in the bucket permissions, add the following to enable public access:
```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "PublicReadForGetBucketObjects",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::give.onefact.org/*"
        }
    ]
}
```

<img width="1252" alt="image" src="https://github.com/onefact/free-url-shortening-on-amazon-s3-on-free-tier/assets/5317244/27b63e9a-c453-44c9-bde5-dba463fab29d">

## Steps to create a cloudfront distribution and redirect the subdomain DNS

1. Go to the Cloudfront management console: https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions
2. Click create distribution: 
3. Enter the origin domain, such as `puns.onefact.org.s3-website.us-east-1.amazonaws.com`
 (a) Select "Redirect HTTP to HTTPS" under `Viewer` options:
 <img width="257" alt="image" src="https://user-images.githubusercontent.com/5317244/236976829-5e45bc8e-7b62-4a9a-add4-55245d6d560f.png">

 (b) Select the custom SSL certificate associated with the toplevel domain, such as onefact.org:
 
<img width="771" alt="image" src="https://user-images.githubusercontent.com/5317244/236976629-6c95f845-36a0-46ed-ba5a-592adda4e3c4.png">
  (c) Select "User and agent referrer headers" under `Cache key and origin requests
`:

<img width="787" alt="image" src="https://user-images.githubusercontent.com/5317244/236976726-a93e6d0f-54af-4ab1-ae7a-37cbc009b199.png">
 (d) Enter the custom domain name under "CNAME", such as `photos.onefact.org`:
 
 <img width="1123" alt="image" src="https://user-images.githubusercontent.com/5317244/236983754-26d1f505-5d10-4350-981b-94bff8dad2a3.png">


## Steps to create DNS records for the new subdomain's cloudfront distribution
1. Go to a DNS provider such as dnsimple.com under your account identifier: https://dnsimple.com/a/[$YOUR_ACCOUNT_NUMBER]/domains/onefact.org/records
2. Add a CNAME record pointing to the Cloudfront distribution you created (e.g. https://yourcloudfrontsubdomain.cloudfront.net)
3. Back in Cloudfront add the CNAME to the distribution properties under "Alternate domain name":

<img width="793" alt="image" src="https://user-images.githubusercontent.com/5317244/236981856-4224f54f-7b0a-4230-b82e-e6249247d65c.png">

# give.onefact.org
Shortlinks for fundraising:

Cash app: give.onefact.org/cash
Stripe: give.onefact.org/stripe
Paypal: give.onefact.org/paypal
Venmo: give.onefact.org/venmo
Ether: give.onefact.org/ether

Redirection rules:
```
[
    {
        "Condition": {
            "KeyPrefixEquals": "paypal"
        },
        "Redirect": {
            "HostName": "www.paypal.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "donate/?hosted_button_id=NEJHHURVND8HG&"
        }
    },
    {
        "Condition": {
            "KeyPrefixEquals": "stripe"
        },
        "Redirect": {
            "HostName": "checkout.stripe.com",
            "Protocol": "https",
            "ReplaceKeyWith": "c/pay/cs_live_a1pYUzqiE0JdVTYyTRLqIXBp6g7LEBta1va19ewnWhX0iH5gnWgH4pSGuh#fidkdWxOYHwnPyd1blppbHNgWjA0STVSQVBBdkhMbm1PbFA9Szxud0pqNk9XVkJUNHVxdkh3N0pRRHVpSDA2ZGdXZFZWcXJGNk1sVE1xaEhgXDNhZnVCclF2fF9WZkZmUl00SEldRD03a0ZGNTVfVmF0REx8QicpJ3VpbGtuQH11anZgYUxhJz8nMnZMM3QzPUlhZGlQNjc9YVRUJykndXdgaWpkYUNqa3EnPydMa2Zqa3ZqaWRxZCd4JSUl"
        }
    },
    {
        "Condition": {
            "KeyPrefixEquals": "cash"
        },
        "Redirect": {
            "HostName": "cash.app",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "$onefact/"
        }
    },
    {
        "Condition": {
            "KeyPrefixEquals": "venmo"
        },
        "Redirect": {
            "HostName": "account.venmo.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "u/onefact/"
        }
    },
    {
        "Condition": {
            "KeyPrefixEquals": "ether"
        },
        "Redirect": {
            "HostName": "zapper.xyz",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "account/onefact.eth?modal=token-transfer"
        }
    }
]
```

# photos.onefact.org

## [photos.onefact.org/2023-five-boro-bike-tour](https://photos.onefact.org/2023-five-boro-bike-tour)

```
[
    {
        "Condition": {
            "KeyPrefixEquals": "2023-five-boro-bike-tour"
        },
        "Redirect": {
            "HostName": "amazon.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "photos/shared/sG-J8h0wSNiuAS7K3dQtpw.j9wLKmFmvA2IGghQ2AysqR?sort=sortOldestToNewest"
        }
    }
]
```

# [puns.onefact.org](https://puns.onefact.org)

## [puns.onefact.org/create](https://puns.onefact.org/create)

[Amazon AWS S3 Management Console direct link](https://s3.console.aws.amazon.com/s3/bucket/puns.onefact.org/property/website/edit?region=us-east-1) 

(<img width="794" alt="image" src="https://user-images.githubusercontent.com/5317244/236976094-8bd525db-204b-45a2-b7ed-3bb2704fd547.png">

Redirection rule in JSON:

```
[
    {
        "Condition": {
            "KeyPrefixEquals": "create"
        },
        "Redirect": {
            "HostName": "github.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "onefact/puns.onefact.org/tree/main#prompt-to-help-generate-more-puns-and-add-to-this-list-using-github-pull-requests-"
        }
    },
    {
        "Condition": {
            "KeyPrefixEquals": "index.html"
        },
        "Redirect": {
            "HostName": "github.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "onefact/puns.onefact.org/#punsonefactorg"
        }
    }
]
```

## [calendar.onefact.org](https://calendar.onefact.org)

Redirection rules in JSON for the Amazon S3 bucket `s3://calendar.onefact.org`:
```
[
    {
        "Condition": {
            "KeyPrefixEquals": "index.html"
        },
        "Redirect": {
            "HostName": "calendar.google.com",
            "Protocol": "https",
            "ReplaceKeyPrefixWith": "/calendar/embed?src=c_f61914946533f9d449f564fe22c898eb8bc91662b1a3298a305de45b0c9f1012%40group.calendar.google.com"
        }
    }
]
```
