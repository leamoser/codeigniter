# codeigniter
## Login.php
```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Login extends CI_Controller {

	public function __construct()
	{
    parent::__construct();
    $this->load->model('login_model');
    $this->load->helper('url_helper');
  }

	public function index()
	{
		$data = array();
		//$data['allepersonen'] = $this->login_model->get_person();
		//$data['eineperson'] = $this->login_model->get_person(7);
		$sent = $this->input->post('login_submit');
		if(isset($sent)){

			//Daten aus der Eingabe holen
			$email = $this->input->post('email');
			$password = $this->input->post('password');
			//Überprüfen ob beide ausgefüllt sind (mit validate())
			$emailval = $this->login_model->validate($email);
			$passwordval = $this->login_model->validate($password);
			//Wenn Validierung erfolgreich, dann mach etwas
			if($emailval == TRUE && $passwordval == TRUE){

				//Datenbankabfrage machen
				$person = $this->login_model->check_login_data($email, $password);
				//Wenn Kombi vorhanden dann gib eine Positive Meldung zurück
				if($person != NULL){

					echo "Contrats, du bist eingeloggt";

				}else{

					echo "Deine Daten stimmen nicht";

				}

			}else{

				echo "Du hast nicht alles ausgefüllt :( <br>";

			}

		}
		//View immer zuunterst laden
		$this->load->view('login_view', $data);
	}
}
?>
```
## Login_model.php
```PHP
<?php
class Login_model extends CI_Model {

  public function __construct()
  {
    $this->load->database();
  }
  public function get_person($id = FALSE)
  {
    if ($id === FALSE)
    {
      $query = $this->db->get('Person');
      return $query->result_array();
    }
    $query = $this->db->get_where('Person', array('id' => $id));
    return $query->row_array();
  }

  public function check_login_data($email, $password)
  {
    $wherequery = array(
      'email' =>  $email,
      'password' => $password
    );
    $query = $this->db->get_where('Person', $wherequery);
    return $query->row_array();
  }

  public function validate($thingtovalidate){
    if(!empty($thingtovalidate)){
      return TRUE;
    }else{
      return FALSE;
    }
  }

}
```
## login_view.php
```HTML
<?php
defined('BASEPATH') OR exit('No direct script access allowed');
?>
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Übung PMM-Tool | Login</title>
</head>
<body>
  <!-- Navigation -->
  <div>
  <section>
      <h1>Anmelden</h1>
      <p>Bitte melde dich an, um neue Inserate zu erstellen oder deine bestehenden Inserate zu löschen.</p>

      <form action="<?php $_SERVER['REQUEST_URI'] ?>" method="post">
        <div>
          <label for="id_email">E-Mail: </label>
          <input type="email" name="email" id="id_email" value="wolfgang.bock@fhgr.ch">
        </div>
        <div>
          <label for="id_password">Passwort: </label>
          <input type="password" name="password" id="id_password" value="12345">
        </div>
        <button type="submit" name="login_submit" value="einloggen">Anmelden</button>
      </form>

      <?php if(isset($msg)){ ?>
        <p><?php echo $msg ?></p>
      <?php } ?>
    </section>
  </div>
</body>
</html>
```
