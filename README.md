As a small nonprofit, we can't afford custom URL shortening with services like Bitly (https://bit.ly, $348/year for custom domain support).

The alternative we found is to simply create redirect rules in Amazon S3 and edit a single json file.

# Steps to use this on your custom domain
We recommend following these steps by cloning this repository locally, using Visual Studio Code: https://learn.microsoft.com/en-us/azure/developer/javascript/how-to/with-visual-studio-code/clone-github-repository

(Visual Studio code lets you write new redirection rules even more easily using Copilot, which is *free* in the GitHub Education pack (free with an email address ending in `.edu`), using the following steps.)

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




# give.onefact.org
Shortlinks for fundraising:

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
    }
]
```
