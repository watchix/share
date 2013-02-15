
Share
=====

Simple app written in PHP for sharing files. Logs file hits to MySQL and Prowl (iOS).
Utilizes nginx's X-Accel-Redirect to minimize the load. 


## Sample nginx configuration

	server {
	    listen		80;
	    server_name	share.dev;
	    root		/www/share;
		
		location /f/ {
			internal;
			alias /www/share/files/;
		}

	    location ~ \.php$ {
			fastcgi_pass	127.0.0.1:9000;
			fastcgi_index	index.php;
			fastcgi_param	SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			fastcgi_param	CONFIG_FILE config.php;		# CONFIG FILE PATH GOES HERE
			include         fastcgi_params;
		}
		
		location /css/ {}
		location / {
			rewrite ^ /index.php last;
		}
	}


## Sample /config.php

	<?php

	return array(
		'timezone' => 'Europe/Stockholm',
		'dir' => array('files' => 'files/'),
		'db' => array('user' => 'root', 'pass' => '', 'dbname' => 'share'),
		'users' => array('username' => 'password'),
		'prowl' => array('keys' => array('xxxyyyzzz'), 'app' => 'Share')
	);