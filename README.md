# RealTimeNotifications-Laravel
Here you will find some basic knowledge of how to send push notifications from laravel code to mobile application device .

### 1) Sending notifications to [specific devices(https://firebase-php.readthedocs.io/en/stable/cloud-messaging.html#send-messages-to-specific-devices)]! 
_________________________________________________________________________________________________
- Usually push notifications has become an essential part of every mobile application.
- Before we start , look at this [ package ](https://firebase-php.readthedocs.io/en/stable/index.html) ,we will use it in our implementation.

- You may notice that the package uses firebase platform!  ... yes, it is true . firebase is the most easier platform you can use for notifications , so to integrate the firebase credentials into your laravel application , watch this simple [ video ](https://www.youtube.com/watch?v=kCq3tQJi88s&ab_channel=FundaOfWebIT) .
- After firebase integration, let's suppose that you are responsible for the backend implementation part of mobile application project and you need to send a notification for the new system users for just greeting them .
- After package installation , you can start send notications now !

In your terminal , make a controller using the command:

```php artisan make:controller NotificationController```


```
<?php

namespace App\Http\Controllers\notify;

use Kreait\Firebase\Messaging\CloudMessage;


class NotificationController
{
    private Messaging $messaging;

    public function __construct(Messaging $messaging)
    {
        $this->messaging = $messaging;
    }

    public function notify(Request $request)
    {
          $message = CloudMessage::fromArray([
              'token' => $request->deviceToken,
              'notification' => ['Welcome for our new users !!'],
              'data' => [/* any data you need to send with the notifications*/],// optional
          ]);
          
          $messaging->send($message);
      }


}

``` 
- you may wonder what is the device token sent in the request , The device token or the device registeration token is a token the mobile developer should sent to you as an identification token for each device
- This token is generated by firebase using specific package , don't worry about it , just recieve it in your request .
 _________________________________________________________________________________________________
  
### 2) Sending notifications to collection of users or ([topics](https://firebase-php.readthedocs.io/en/stable/cloud-messaging.html#send-messages-to-topics))! 
_________________________________________________________________________________________________


- Let's suppose there are three different types of users in your application , customers,sellers and managers ... each of them is called a topic or a role as known in laravel
- as an admin of dashboard , I may need to send a notification to all sellers at one time .. let's try it.

 
In your terminal , make a controller using the command:

```php artisan make:controller TopicController```


```
<?php

namespace App\Http\Controllers\notify;

use Kreait\Firebase\Messaging\CloudMessage;


class TopicController
{
    private Messaging $messaging;

    public function __construct(Messaging $messaging)
    {
        $this->messaging = $messaging;
    }

    public function notify(Request $request)
    {
          $topic = 'a-topic';
            $message = CloudMessage::fromArray([
                'topic' => $topic,
                'notification' => [/* Notification data as array */], // optional
                'data' => [/* data array */], // optional
            ]);
            $messaging->send($message);
      }


}

```

And what else ??!!
yes, that's so easy !
- The mobile application developer should listen for the topic based on the role of each user .. . .. . for example the seller should listen for a-topic , the customer should listen for 'b-topic' and so on ...
