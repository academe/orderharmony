<?php

namespace academe\OrderHarmony;

class OrderHarmony
{
    // The details of the account.
    public $account_name = '';
    public $api_token = '';
    public $api_shared_secret = '';

    // The format of data to receive (not sending - TBC).
    public $api_format = 'json';

    // The API version.
    public $api_version = '1';

    // API target details.
    public $api_protocol = 'https';
    public $api_domain = 'orderharmony.com';
    public $api_base_path = '/api';
    public $api_get_sep = '&';

    // API target templates
    public $api_server_url_template = '{api_protocol}://{account_name}.{api_domain}';
    public $api_base_uri_template = '{api_base_path}/{api_version}';
    public $api_base_url_template = '{api_server_template}{api_base_uri_template}'; // Deprecated: not needed.

    // The constructed base URL.
    public $api_server_url = '';
    public $api_base_uri = '';
    public $api_base_url = ''; // Deprecated: not needed.

    // The rest controller.
    // We will be using resty/resty for now, but I have no idea how generic that is,
    // and so how easily it can be swapped out for something else.
    public $rest = NULL;

    // Default headers and options for the REST controller.
    public $restHeaders = NULL;
    public $restOptions = NULL;

    // Setter/injecter for the rest controller.
    // Perhaps we need to be able to default to resty/resty here, assuming it is set as
    // a dependancy.
    public function setRest($restController)
    {
        $this->rest = $restController;
    }

    // The server part of the API URL.
    public function getServerUrl($uri = '')
    {
        // Construct the base URL from its constituent parts, only if necessary.
        if (empty($this->api_server_url)) {
            $this->api_server_url = $this->expandtemplate($this->api_server_url_template);
        }

        return $this->api_server_url . $uri;
    }

    // Construct the base URL from the template.
    // We probably won't need this.
    public function getBaseUrl($path = '')
    {
        // Construct the base URL from its constituent parts, only if necessary.
        if (empty($this->api_base_url)) {
            $this->api_base_url = $this->expandtemplate($this->api_base_url_template);
        }

        return $this->api_base_url . $path;
    }

    // Construct the base URI only from the template.
    // This is the URI before the method-specific parts are added.
    // Deprecated: not needed.
    public function getBaseUri($path = '')
    {
        // Construct the base URL from its constituent parts, only if necessary.
        if (empty($this->api_base_uri) || $force_rebuild) {
            $this->api_base_uri = $this->expandtemplate($this->api_base_uri_template);
        }

        return $this->api_base_uri . $path;
    }

    // Expand {field_name} fields by substituting each occurance with the value of
    // $this->field_name
    public function expandTemplate($template)
    {
        while(preg_match_all('/{([a-z][a-z0-9_]+)}/i', $template, $matches)) {
            // Loop for each matched field.
            foreach($matches[1] as $match) {
                if (isset($this->$match)) {
                    $template = str_replace('{'.$match.'}', $this->$match, $template);
                } else {
                    $template = str_replace('{'.$match.'}', '', $template);
                }
            }
        }

        return $template;
    }

    // Calculate authentication details to a method URI.
    // Supplied is the full URI for the method being called.
    // Returned is the authentication query parameters as name->value pairs.
    public function uriAuthQuery($uri)
    {
        // Get a serial number.
        // This protects from man-in-the-middle attacks.
        // TODO: need a finer-grained unit of time, so we don't have clashes, 
        // or perhaps some additional component to the timestamp.
        $serial = time();

        $uri .= ((strpos($uri, '?') === false) ? '?' : $this->api_get_sep);

        $uri .= 'token=' . $this->api_token;
        $uri .= $this->api_get_sep . 'serial=' . $serial;
        $uri .= $this->api_get_sep . 'secret=' . $this->api_shared_secret;

        return array(
            'token' => $this->api_token,
            'serial' => $serial,
            'signature' => sha1($uri),
        );
    }

    // Set the account details to access the API.
    // Three items of information need to be known.
    public function setAccount($accountName, $token, $shared_secret)
    {
        $this->account_name = $accountName;
        $this->api_token = $token;
        $this->api_shared_secret = $shared_secret;
    }

    // Call the "products" APi to get all products.
    public function getProducts()
    {
        $uri = $this->getBaseUri('/products');
        $AuthQuery = $this->uriAuthQuery($uri);
        return $this->rest->get($this->GetServerUrl($uri), $AuthQuery, $this->restHeaders, $this->restOptions);
    }

    // Get a single product.
    public function getProduct($productId)
    {
        $uri = $this->getBaseUri('/products/' . $productId);
        $AuthQuery = $this->uriAuthQuery($uri);
        return $this->rest->get($this->GetServerUrl($uri), $AuthQuery, $this->restHeaders, $this->restOptions);
    }

    // Create an order.
    public function createOrder(array $orderData)
    {
        $strOrderData = json_encode($orderData);

        $uri = $this->getBaseUri('/orders');
        $AuthQuery = $this->uriAuthQuery($uri);
        // It's not documented, but the payload is added to the authorisation parameters.
        // This is, of course, a POST, so "query" is a bit of a misnomer here.
        $AuthQuery = array_merge($AuthQuery, array('order' => $strOrderData));
        return $this->rest->post($this->GetServerUrl($uri), $AuthQuery, $this->restHeaders, $this->restOptions);
    }
}
