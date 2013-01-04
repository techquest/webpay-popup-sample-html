Overview
========
The trick to opening WebPay in a popup is to first redirect to a page in your website that is hosted in a popup. From there the user can either submit the web page or you can use script to submit the page automatically. The popup must support iframe to be able to navigate to WebPay from your site.
There are many popup plugins out there but we recommend using Colorbox, a jQuery based popup plugin. Check here ((http://www.jacklmoore.com/colorbox) for colorbox download and sample usages.
Note: In popup, WebPay integrations remain the same as in the previous discussions.

Implementation
==============
To start with add reference to jQuery and colorbox css and scripts as shown below. The hyperlink for redirecting to the popup page should have a class named iframe. Other css classes can be added to the hyperlink to style it appropriately. There can be more than one hyperlink if you want to pay for more than one item on the web page but they must all have the iframe class name.

To hook up the hyperlinks to colorbox run ‘$(".iframe").colorbox({ iframe: true});’ when the web page has finished loading. The best configuration settings of colorbox that we have tested with are as shown below. You can check colorbox page (http://www.jacklmoore.com/colorbox) for more configuration settings. 

```html
<html>
    <head>
        <link href="~/Content/colorbox.css" rel="stylesheet" type="text/css" />
    </head>
    <body>
        <br />
        <h2>Click to Pay</h2>
        <a class='iframe otherclassnames' href="http://abc.com/webpaypopup ">Pay</a>
    </body>
    <script src="~/Scripts/jquery-1.7.1.min.js" type="text/javascript"></script>
    <script src="~/Scripts/jquery.colorbox-min.js" type="text/javascript"></script>
    <script type="text/javascript" language="javascript">
        $(document).ready(function () {
$(".iframe").colorbox({ iframe: true, fastIframe: false, escKey: false,         overlayClose: false, innerWidth: 503, innerHeight: 600, returnFocus: false, scrolling: false });
        })
</script>
</html>
```

Implementation of the popup page is like any of the earlier Implemented ones. A sample is shown below. 
Note: the form tag is given a name to allow handling automatic WebPay posting with javascript.

```html
<html>
<head>
</head>
<body>
    <form name="webpayform" method="post" action="https://testwebpay.interswitchng.com/test_paydirect/pay/">
    <input name="product_id" type="hidden" value="174" />
    <input name="pay_item_id" type="hidden" value="23" />
    <input name="amount" type="hidden" value="1000000" />
    <input name="currency" type="hidden" value="566" />
    <input name="site_redirect_url" type="hidden" value="http://abc.com/redirect" />
    <input name="txn_ref" type="hidden" value="7645536" />
    <input name="hash" type="hidden" value="CE69DB9638BB5F26FFF0B80F1F53640E3F52C5129D06CEBF035C0125D1BCE4173AD4D597FEC8179466DA295E67F43DE41F43E4913E9AD6236B449F4ED05CCCE3" />
        <!--In case you want the user to initiate the post to webpay in the popup-->
        <!--<input type="submit" value="Click to Pay" />-->
    </form>
    <script type="text/javascript">
//Comment out or remove this line if you do not want automatic posting of the //page content to webpay
        document.webpayform.submit();
    </script>
</body>
</html>
```

Handling Full Page Redirect
===========================
Some merchants want to be able to show a full receipt or redirect page outside the popup. In that case, it is advised that you allow WebPay to first redirect to a dummy page from where you can redirect outside the iframe to a full receipt or redirect page using javascript. A sample dummy page is shown below with the redirect script.

Note:  It is advised that you put the script in the head tag of the dummy page or immediately after the opening body tag to make the redirect happen as quickly as possible.

```html
<html>
<head>
    <script type="text/javascript">
        if (window.self !== window.top) {
window.top.location.href = 'http://abc.com/fullresponse’+ window.location.search;
        }
    </script>
</head>
<body>
    <h2>Dummy redirect page</h2>
</body>
</html>
```
