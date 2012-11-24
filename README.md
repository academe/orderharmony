orderharmony
============

Library for OrderHarmony. This library is under development, and is subject to radical changes
Still just fumbling in the dark ;-)

Typical Usage
-------------

This is for API version 1

    require 'vendor/autoload.php';
    
    $OrderHarmony = new academe\OrderHarmony\OrderHarmony();
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


