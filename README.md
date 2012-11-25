orderharmony
============

Library for OrderHarmony. This library is under development, and is subject to radical changes
Still just fumbling in the dark ;-)

Typical Usage
-------------

This is for API version 1

    require 'vendor/autoload.php';
    
    $OrderHarmony = new Academe\OrderHarmony\OrderHarmony();
    $OrderHarmony->setAccount('account_name', 'token_code', 'shared_secret'); 
    
    $rest = new Resty();
    $OrderHarmony->setRest($rest); 
    
    $products = $OrderHarmony->getProducts();
    $product = $OrderHarmony->getProduct(764);
    $result = $OrderHarmony->createOrder($arrayOrderData);
    $orders = $OrderHarmony->getOrders('123');
    $result = $OrderHarmony->createProducts($arrayProductsData);

Composer
--------

To include the development version from guthub:

    {
        "repositories": [
            {
                "type": "git",
                "url": "https://github.com/academe/orderharmony"
            }
        ],
        "require": {
            "academe/orderharmony": "dev-master"
        }
    }

Notable TODOs
-------------

1. The API is versioned, and a new version could change anything about an API 
   from the data that is served and received, to the URL for a method, to the 
   list of available methods. We need to think about how this is going to work.
2. Tests. Tests. Tests.
3. Error and exceptino handling throughout. Handling errors returned by the remote 
   service as well as lower-level connection and authentication errors.
4. Parsing of return and sent data into appropriate objects: customer, product, 
   variation, address, order, payment, shipment, etc. There is little API documentation 
   on this so it is mostly guesswork at this time.


