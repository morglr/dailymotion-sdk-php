Dailymotion PHP SDK
===================

This repository contains the official open source PHP SDK that allows you to access the Dailymotion
API from your website.

Usage
-----

### Authentication

The Dailymotion API requires OAuth 2.0 authentication in order to be used. This library implements
three granting methods of OAuth 2.0 for different kind of usages.

#### Token Grant Type

This mode is the mode you should use in most cases. In this mode you redirect the user to an
authorization page on Dailymotion, and you script is called back once the end-user authorized your API
key to access the Dailymotion service on its behalf.

Here is a usage example:

    <?php

    $api = new Dailymotion();
    $api->setGrantType(Dailymotion::GRANT_TYPE_TOKEN, $apiKey, $apiSecret)

    try
    {
        $result = $api->call($method, $arguments);
    }
    catch (DailymotionAuthRequiredException $e)
    {
        // Redirect the user to the Dailymotion authorization page
        header('Location: ' . $api->getAuthorizationUrl());
        return;
    }
    catch (DailymotionAuthRefusedException $e)
    {
        // Handle case when user refused to authorize
        // <YOUR CODE>
    }

#### Password Grant Type

If you PHP application isn't a web application and cannot redirect the user to the Dailymotion
authorization page, the password grant type can be used. With this grant type you have the
responsibility to ask the user for its credentials. Make sure you API secret remains secret though.

    <?php

    $api = new Dailymotion();

    if (!isset($_POST['username']) || !isset($_POST['password']))
    {
        // Ask end-user's credentials
        // <YOUR CODE>
    }
    else
    {
        $api->setGrantType(Dailymotion::GRANT_TYPE_PASSWORD, $apiKey, $apiSecret, null,
                           array('username' => $_POST['username'], 'password' => $_POST['password']));

        $result = $api->call($method, $arguments);
    }


#### None Grant Type

If you don't need to access the Dailymotion API on behalf of a user because, for instance, you plan to
only access public data, you can use the NONE grant type. With this grant type, you will only have
access to public data or private date of the user owning the API key.

    <?php

    $api = new Dailymotion();
    $api->setGrantType(Dailymotion::GRANT_TYPE_NONE, $apiKey, $apiSecret);
    $result = $api->call($method, $arguments);

### File Upload

Certain methods like `video.upload` requires a URL to a file. To create those URLs, the `uploadFile:delegate:` method have to be used like this:

    $url = $api->uploadFile($filePath);

You can then use this URL as an argument to methods requiring such parameter. For instance to create a video:

    $result = $api->call('video.create', array('url' => $url));

Feedback
--------

We are relying on the [GitHub issues tracker][issues] linked from above for feedback. File bugs or
other issues [here][issues].

[issues]: http://github.com/dailymotion/dailymotion-sdk-php/issues