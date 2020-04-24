# Dynamic Variable

## Setting dynamic variable via external script.

Postman can only trigger HTTP request, So there is no direct way we can execute script such as **.sh script**. 
But you can run a local server (here node js server) and write a node js script to execute a **shell script** or other executable file
Now let do that In step by step

### Step 1: Define Your Task
I hav a php script that provide me the token. This token will be later use in api request. This script looks some thing 
like this whichis saved as **TokenGenerato.php** 

```php
<?php
require "vendor/autoload.php";

class TokenGeneration {

	public  $appID= "50A454F3-EE11-4A4C-B708-965BC7640C08";
	public $token = NULL; 

	public function __construct(){
		//echo "\n URL: ".$this->url;
		$this->token = $this->generateAppToken();
	}
  
	function generateAppToken(){
		$message =   $this->appID."~REQUEST".time();
		//echo "\n message: ".$message;

		$cryptor = new \RNCryptor\RNCryptor\Encryptor;
		$base64Encrypted = $cryptor->encrypt($message, $this->appID);
		//echo "\n token: ". $base64Encrypted;
		return $base64Encrypted;
	}
}
?>
```

### Step 2: Get your token
We will write **token.php** program to get token from  **TokenGenerato.php** 

```php
<?php
  require "TokenGeneration.php";
  $tokengen = new TokenGeneration();
  echo $tokengen->token;
?>
```

we can run this php script via 
> php token.php

Output:
> AwFmHNkA1HdM1VHFadu89nE3xZuKO3pLQ7cHOrCj2x2WZoSt                                                                                                                                                                    

### Step 3: Create Node js script.
You can use **chile_process module** to execute shell command in node js. [Tutorial on child_process](https://stackabuse.com/executing-shell-commands-with-node-js/)

Create node js script in **token.js** with following code

```javascript
const http = require('http'), { exec } = require('child_process');

http.createServer((req, res) => {
  // Give the path of your bat or exe file
  exec('php /Volumes/Product/Research/uatci/token.php', (err, stdout, stderr) => {
    console.log({ err, stdout, stderr });
    
    if (err) {
      return res.writeHead(500).end(JSON.stringify(err));
    }
    
    // Output of the script in stdout
    return res.writeHead(200).end(stdout);
  });
}).listen(8000);
console.log('Server running at http://127.0.0.1:8000/');
```

Next start the node server with following command
> node token.js


### Step 4: Pre-Request Script for Postman.
Firt create the new api request in post man, then select Pre-request Script and write following code to send http request before the actual api call happens and save the response in local variable.

```javascript
pm.variables.set("password", "AwErFEinDqiOvXFx2wVgHvt+Rpo0jdoTH0D0QldS");

console.log("password: ", pm.variables.get("password"));

pm.sendRequest('http://127.0.0.1:8000', (err, response) => {
    // This will have the output of your batch file
    console.log(response.text());
    pm.variables.set("token", response.text());
})

```

Finally, prepare the json body like this
> {"UserName": "iapple@javra.com", "Password":"{{password}}"}
and header 
> token = {{token}}

## Creating custom Shortcut

### Create shell script to copy token into clipboard.
Mac provide some execlusive command such as **pbcopy** and **pbpaste** to copy the standard input into clipboard. Now create 
**token.sh** script with following code

```sh 
php /Volumes/Product/Research/uatci/token.php | pbcopy
```
make the script executable.
> chmod +x token.sh

[Refer here for other platform](https://www.ostechnix.com/how-to-use-pbcopy-and-pbpaste-commands-on-linux/) for similar command like **pbcopy**

### Make the Shell script accessible globally

Create a symbolic link to token.sh in **/usr/local/bin/**  so that you can directly access this script from anywhere.

> ln -s /your-directory-path/token.sh /usr/local/bin/token

After this we can run following command to get token from out php script
> token

output: 
> AwFmHNkA1HdM1VHFadu89nE3xZuKO3pLQ7cHOrCj2x2WZoSt 






