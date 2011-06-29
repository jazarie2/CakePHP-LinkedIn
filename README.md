# Installation

1. Download / clone the following plugins: 
* **LinkedIn** to _plugins/linkedin_
* [HttpSocketOauth plugin](https://github.com/ProLoser/http_socket_oauth) (ProLoser fork) to _plugins/http_socket_oauth_
* [Apis plugin](https://github.com/ProLoser/CakePHP-Api-Datasources) to _plugins/apis_

2. Setup your `database.php`

```
var $linkedin = array(
	'datasource' => 'Apis.Apis',
	'driver' => 'Linkedin.Linkedin',
	'login' => '<linkedin api key>',
	'password' => '<linkedin api secret>',
);
```

3. Install the Apis-OAuth Component for authentication

```
MyController extends AppController {
	var $components = array(
		'Apis.Oauth' => 'linkedin',
	);
	
	function connect() {
		$this->Oauth->connect();
	}
	
	function linkedin_callback() {
		$this->Oauth->callback();
	}
}
```

4. Use the datasource normally (check [wiki](https://github.com/ProLoser/CakePHP-LinkedIn/wiki) for available commands & usage)
```
Class MyModel extends AppModel {
	var $useDbConfig = 'linkedin';

	function readProfile() {
		$data = $this->find('all', array(
			'path' => 'people/~',
			'fields' => array(
				'first-name', 'last-name', 'summary', 'specialties', 'associations', 'honors', 'interests', 'twitter-accounts', 
				'positions' => array('title', 'summary', 'start-date', 'end-date', 'is-current', 'company'), 
				'educations', 
				'certifications',
				'skills' => array('id', 'skill', 'proficiency', 'years'), 
				'recommendations-received',
			),
		));
	}
}
```