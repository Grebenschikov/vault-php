.. Vault PHP Client documentation master file, created by
   sphinx-quickstart on Thu Aug 10 14:35:27 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Vault PHP Client's documentation!
============================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

This is a PHP client for Vault - a tool for managing secrets.

Quick start
-----------

.. code-block:: php

    <?php

    use Vault\AuthenticationStrategies\AppRoleAuthenticationStrategy;
    use Vault\AuthenticationStrategies\UserPassAuthenticationStrategy;
    use Vault\AuthenticationStrategies\TokenAuthenticationStrategy;
    use Vault\Client;
    use Laminas\Diactoros\RequestFactory;
    use Laminas\Diactoros\StreamFactory;
    use Laminas\Diactoros\Uri;

    // Creating the client
    $client = new Client(
        new Uri('http://127.0.0.1:8200'),
        new \AlexTartan\GuzzlePsr18Adapter\Client(),
        new RequestFactory(),
        new StreamFactory()
    ); // Using alextartan/guzzle-psr18-adapter and laminas/laminas-diactoros

    // Authenticating using userpass auth backend.

    $authenticated = $client
        ->setAuthenticationStrategy(new UserPassAuthenticationStrategy('test', 'test'))
        ->authenticate();

    // Authenticating using approle auth backend.

    $authenticated = $client
        ->setAuthenticationStrategy(new AppRoleAuthenticationStrategy(
            'd4131206-384f-75fa-11d6-55d1d63c07c0',
            'cac86a12-c566-3932-09f3-5823ccdfa606'
        ))
        ->authenticate();

    // Authenticating using token auth backend.
    $authenticated = $client
        ->setAuthenticationStrategy(new TokenAuthenticationStrategy('463763ae-0c3b-ff77-e137-af668941465c'))
        ->authenticate();

Fetching a secret
-----------------

.. code-block:: php

    <?php

    use Vault\AuthenticationStrategies\TokenAuthenticationStrategy;
    use Vault\Client;
    use Laminas\Diactoros\RequestFactory;
    use Laminas\Diactoros\StreamFactory;
    use Laminas\Diactoros\Uri;

    // Creating the client
    $client = new Client(
        new Uri('http://127.0.0.1:8200'),
        new \AlexTartan\GuzzlePsr18Adapter\Client(),
        new RequestFactory(),
        new StreamFactory()
    ); // Using alextartan/guzzle-psr18-adapter and laminas/laminas-diactoros

    // Authenticating using token auth backend.
    // Request exception could appear here.
    $authenticated = $client
        ->setAuthenticationStrategy(new TokenAuthenticationStrategy('463763ae-0c3b-ff77-e137-af668941465c'))
        ->authenticate();

    if (!$authenticated) {
        // Throw an exception or handle authentication failure.
    }

    // Request exception could appear here.
    /** @var \Vault\ResponseModels\Response $response */
    $response = $client->read('/secret/database');

    $data = $response->getData(); // Raw array with secret's content.

    // ...

Indices and tables
==================

* :ref:`search`
