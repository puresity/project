# project
 
- logcall.php
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>logcall.php</title>
	
	<link rel="stylesheet" type="text/css" href="style.css">
	<script type="text/javascript">
function validateForm()
{
	var x=document.forms["frmLogCall"]["callerName"].value;
	if (x==null || x=="")
  	{
  		alert("Caller Name is required.");
  		return false;
  	}
  	// may add code for validating other inputs
}
</script>
</head>

<body >
	
<?php // import nav.php
require_once 'nav.php';
?>
<?php	// import db.php
require_once 'db.php';

	
// Create connection
$conn = new mysqli(DB_SERVER,DB_USER, DB_PASSWORD, DB_DATABASE);
// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
} 

$sql = "SELECT * FROM incidenttype";
	
$result = $conn->query($sql);
  
if ($result->num_rows > 0) {
  while ($row = $result->fetch_assoc()) {
	/* create an associative array of $incidentType {incident_type_id, incident_type_desc] */
	$incidenttype[$row['incidentTypeId']] = $row['incidentTypeDesc'];
  }
}

$conn->close();
?>
	
	
	
<form name="frmLogCall" method="post" onSubmit="return validateForm()" action="dispatch.php">

	<br><br>
	
<fieldset>
	<legend>Log call</legend>
	
<p>
    <label for="username">Caller's Name: </label>
    <input type="text" id="username" name="username" value="Peter Leow">
</p>
	
<p>	
	<label for="phone">Contact No: </label>
  <input type="tel" id="phone" name="phone" placeholder="81234567" pattern="[0-9]{8}" required>
</p>
	
 <p>
	 <label for="location">Location: </label>
	 <input type="text" id="location" name="location" required>
</p>
		
<p>
	<tr>
    <td>Incident Type :</td>
   <td>
	
<select name="incidentTypeId" id="incidentTypeId"> 
	<?php	
        foreach( $incidenttype as $key => $value){
	?>
	
   <option value="<?php echo $key ?>" >
	   <?php echo $value ?>
	</option>
	
	<?php 
		}
	?>

  </select>
	 </td>
</tr>

</p>
	
<p>	
	 <label for="Descripton">Description :</label>
  <textarea id="Description" name="Description" rows="5" cols="50" required></textarea>
</p>
	
	 <p>
	 <input type="reset" name="btnCancel" id="btnCancel" value="Reset"> 
    <input type="submit" name="btnProcessCall" id="btnProcessCall" value="Process Call">
  </p>
	

	</form>
</fieldset>
	
</body>
</html>
