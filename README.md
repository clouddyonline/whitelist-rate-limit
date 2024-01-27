Since our application is cloud-based, it downloads and processes user playlists on our server side. We often see **429** errors in the playlist processing log, which indicates that the provider’s server is limiting the number of requests from our server. To fix this, just add the IP address **212.95.49.213** of our server to the whitelist. For example, in the Xtream Codes panel it can be done like this:

in the **nginx.conf** file, find the line:

    limit_req_zone $binary_remote_addr zone=one:30m rate=20r/s;

and replace it with the following lines:

    geo $whitelist {
    	default 0;
    
    	# ClouDDy main server IP address
    	212.95.49.213 1;
    }
    
    map $whitelist $limit_remote_addr {
    	0 $binary_remote_addr;
    	1 "";
    }
    
    limit_req_zone $limit_remote_addr zone=one:30m rate=20r/s;

