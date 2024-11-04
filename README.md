<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<link rel="stylesheet" href="style.css">
<title>Latihan Pengjarkom Lanjut</title>
</head>

<body>
<h2>Data Kelompok </h2>
<?php
 	include "koneksi.php";
	if($_GET["p"] == "useradd"){
		include "useradd.php";
	}else if($_GET["p"] == "useredit"){
		include "useredit.php";
	}else if($_GET["p"] == "userdel"){
		include "userdel.php";
	}else{
		include "user.php";
	}
?>
</body>
</html>

<?php
	$host = "localhost";	// berisi ip address dari server
	$user = "root";			// username masuk MySQL dari web hosting
	$pass = "";				// password masuk MySQL dari web hosting
	$db   = "latwebprog";		// nama database yang diakses
	
	$kon = mysqli_connect($host, $user, $pass, $db);
	
	// Cuma buat ngetes doank
	/*
	if($kon){
		echo "Terkoneksi dengan MySQL Server<br>";
		echo "Database $db bisa diakses";
	}else{
		echo "Maaf, Gagal Koneksi";
	}
	*/
?>

<?php
	echo "<a href='?p=useradd'>Tambah Data</a>";
	echo "<table width='100%' border='1' cellspacing='5' cellpadding='5'>";
	echo "<tr>
			<th>NO</th>
			<th>FOTO</th>
			<th>USER</th>
			<th>DATA PERSONAL</th>
			<th>KONTAK</th>
			<th>AKSI</th>
		  </tr>";
	$sqlm = mysqli_query($kon, "select *from user order by tgldaftar desc");
	$no =1;
	while($rm = mysqli_fetch_array($sqlm)){
		echo "<tr>
			<td>$no</td>
			<td><img src='foto/$rm[foto]' width ='100px'></td>
			<td>
				Id. User :<b>$rm[iduser]</b> <br>
				Username :<b>$rm[username]</b> <br>
				Nama :<b>$rm[nama]</b> <br>
				Password :<b>$rm[password]</b> <br>
				Data didaftar pada : <b>$rm[tgldaftar] WIB</b>
			</td>
			<td>
				Tempat / Tanggal Lahir : <br>
				<b>$rm[tmplahir] / $rm[tgllahir]</b> <br>
				Jenis Kelamin : <b>$rm[jk]</b>
			</td>
			<td>
				Email : <b>$rm[email]</b> <br>
			</td>
			<td><button class='btn2'><i class='fa-solid fa-pen-to-square'></i><a href='?p=useredit&iduser=$rm[iduser]'>Ubah </a></button>|
		<button><i class='fa-solid fa-trash-can'></i><a href='?p=userdel&iduser=$rm[iduser]'>Hapus</a></button></td>
		  </tr>";
		  $no++;
	}
	echo "</table>";
?>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<link rel="stylesheet" href="style.css">
<title>Untitled Document</title>
</head>

<body>
<form id="form1" name="form1" method="post" action="" enctype="multipart/form-data">
    <table width="100%" border="2">
      <tr>
        <td width="45%">Id User</td>
        <td width="55%"><input name="iduser" type="text" id="iduser" /></td>
      </tr>
      <tr>
        <td>Username</td>
        <td><input name="username" type="text" id="username" /></td>
      </tr>
      <tr>
        <td>Password</td>
        <td><input name="password" type="password" id="password" /></td>
      </tr>
	  <tr>
        <td>Email</td>
        <td><input name="email" type="text" id="email" /></td>
      </tr>
	  <tr>
        <td>Nama</td>
        <td><input name="nama" type="text" id="nama" /></td>
      </tr>
      <tr>
        <td>Tempat / Tanggal Lahir</td>
        <td><input name="tmplahir" type="text" id="tmplahir" />
/
  <input name="tgllahir" type="date" id="tgllahir" /></td>
      </tr>
      <tr>
        <td>Jenis Kelamin</td>
        <td><input name="jk" type="radio" value="L" />
Laki-Laki
  <input name="jk" type="radio" value="P" />
Perempuan </td>
      </tr>
        <td>Foto</td>
        <td><input name="foto" type="file" id="foto" /></td>
      </tr>
      <tr>
        <td>&nbsp;</td>
        <td><input name="simpan" type="submit" id="simpan" value="Simpan Data User" /></td>
      </tr>
    </table>
  <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
  <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
</form>

<?php
if($_POST["simpan"]){
	include "koneksi.php";
	$nmfoto  = $_FILES["foto"]["name"];
	$lokfoto = $_FILES["foto"]["tmp_name"];
	if(!empty($lokfoto)){
		move_uploaded_file($lokfoto, "foto/$nmfoto");
	}
	
	$sqlm = mysqli_query($kon, "insert into user (iduser, username, password, email, nama, tmplahir, tgllahir, jk, foto, tgldaftar) values ('$_POST[iduser]', '$_POST[username]', '$_POST[password]', '$_POST[email]', '$_POST[nama]', '$_POST[tmplahir]', '$_POST[tgllahir]', '$_POST[jk]', '$nmfoto', NOW())");
	
	if($sqlm){
		echo "Data User berhasil disimpan";
	}else{
		echo "Gagal menyimpan";
	}
	echo "<META HTTP-EQUIV='Refresh' Content='2; URL=?p=user'>";
}
?>

</body>
</html>

<?php
	include "koneksi.php";
	$sqlm = mysqli_query($kon, "delete from user where iduser='$_GET[iduser]'");
	
	if($sqlm){
		echo "Data User berhasil dihapus";
	}else{
		echo "Gagal menyimpan";
	}
	echo "<META HTTP-EQUIV='Refresh' Content='2; URL=?p=user'>";
?>

<?php
	$sqlm = mysqli_query($kon, "select * from user where iduser=$_GET[iduser]");
	$rm = mysqli_fetch_array($sqlm);
?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<link rel="stylesheet" href="style.css">
<title>Untitled Document</title>
</head>

<body>
<div class="container">
<h2 class="judul">Merubah Data User Baru</h2>

<form id="form1" name="form1" method="post" action="" enctype="multipart/form-data">
    <input name="iduser" type="hidden" id="iduser" value="<?php echo "$rm[iduser]"; ?>" />
  Id User
<input name="iduser" type="text" id="iduser" value="<?php echo "$rm[iduser]"; ?>" disabled="disabled"/>
    <p>Username
      <input name="username" type="text" id="username" value="<?php echo "$rm[username]"; ?>" />
  </p>
  <p>Password
    <label>
    <input type="password" name="password" id="password" value="<?php echo "$rm[password]"; ?>" />
    </label>
  </p>
  <p>Email
    <input name="email" type="text" id="email" value="<?php echo "$rm[email]"; ?>" />
</p>
  <p>Nama
    <label>
    <input type="text" name="nama" id="nama" value="<?php echo "$rm[nama]"; ?>" />
    </label>
  </p>
  <p>Tempat / Tanggal Lahir 
      <input name="tmplahir" type="text" id="tmplahir" value="<?php echo "$rm[tmplahir]"; ?>" />
    / 
    <input name="tgllahir" type="date" id="tgllahir" value="<?php echo "$rm[tgllahir]"; ?>" />
    </p>
  <p>Jenis Kelamin 
    <?php
		if ($rm["jk"] == "L"){
			$l = " checked"; $p="";
		}else if($rm["jk"] == "P"){
			$l=""; $p = " checked";
		}
	?>
  <input name="jk" type="radio" value="L" <?php echo "$l"; ?>/>
      Laki-Laki
    	<input name="jk" type="radio" value="P" <?php echo "$p"; ?>/>
      Perempuan<br>
    <?php
	echo "<img src='foto/$rm[foto]' width='200px'>";
	?>
  </p>
  <p>Foto 
      <input name="foto" type="file" id="foto" />
  </p>
    <p>
      <input name="simpan" type="submit" id="simpan" value="Simpan Data User" />
    </p>
</form>

<?php
if($_POST["simpan"]){
	include "koneksi.php";
	$nmfoto  = $_FILES["foto"]["name"];
	$lokfoto = $_FILES["foto"]["tmp_name"];
	if(!empty($lokfoto)){
		move_uploaded_file($lokfoto, "foto/$nmfoto");
		$foto =  ", foto='$nmfoto'";
	}else{
		$foto = "";
	}
	
	$sqlm = mysqli_query($kon, "update user set username='$_POST[username]', password='$_POST[password]', email='$_POST[email]', nama='$_POST[nama]', tmplahir='$_POST[tmplahir]', tgllahir='$_POST[tgllahir]', jk='$_POST[jk]' $foto where iduser='$_POST[iduser]'");
	
	if($sqlm){
		echo "Data User berhasil disimpan";
	}else{
		echo "Gagal menyimpan";
	}
	echo "<META HTTP-EQUIV='Refresh' Content='2; URL=?p=user'>";
}
?>

</body>
</html>
![lara](https://github.com/user-attachments/assets/67449a67-f188-4cb9-b584-66456aebda01)

![sasa](https://github.com/user-attachments/assets/9a00fd50-6177-4fa0-b54f-c49883869140)

![WhatsApp Image 2024-02-12 at 01 12 53](https://github.com/user-attachments/assets/b6de2841-7230-42e7-8d45-40b4a11620f3)




