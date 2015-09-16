# login-for-PHP-through-Android
<?php
   
		if((!$_POST['username'] == "") && (!$_POST['pass'] == ""))
		{
                                                                    //inkludera config class och skapa en connection objekt.
			require_once ("config.php");
                                                      //inkludera databas anslutning och skapa objekt för connection class
			$db = new Connection();
		
			$user = $_POST['username'];
			$password = $_POST['pass'];
                                                      //använda username för att ta fram password.
	
			$check = $db->GetRows("SELECT * FROM your WHERE UserName = '" .$user. "'");

		if (mysql_num_rows($check) != 0)
		{
			while($info = mysql_fetch_array( $check ))
			{
				$salt="yourmessage";
				$pass = stripslashes($_POST['pass']);
				$pass = htmlspecialchars($pass);         
				$password = $info['password'];
				$pass =  crypt($pass, $salt); 
				$fornamn = $info['Fornamn'];
				$efternamn = $info['EfterName'];
				$citycode = $info['Citycode'];
			}
									
            
			if ($pass != $password)
			{		
				$response["success"] = 0;
				$response["message"] = "Invalid Credentials!";
				die(json_encode($response));
			}else{
					 $response["success"] = 1;
					 $response["fornamn"] = $fornamn;
					 $response["efternamn"] = $efternamn;
					 $response["Citycode"] = $citycode;
					 $response["message"] = "Login successful!";
					die(json_encode($response));
				}
                                                       //användare få fel meddelande om man skriva fel användarnamn.
		}else{
      		  $response["success"] = 0;
        	  $response["message"] = "Invalid Credentials!";
        	  die(json_encode($response));
		}
      
      }else{
      	      $error = 'Användaren existerar inte.<a href=http://yourweb.com/index.html>Kontakta administratören för att få en användare</a>';
              include 'error.html.php';
      }

?>

