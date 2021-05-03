# Pickel rick Try hack me
# irony || May, 3 2021


ip = 10.10.15.32
new ip = 10.10.232.107


> enumeration 
1. nmap scan
```
sudo nmap -sC -sV --script=vuln 10.10.15.32

```
i saved the scan in nmap_initial.txt
well i konw this is a website so i saw the source code of it 

```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Rick is sup4r cool</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="assets/bootstrap.min.css">
  <script src="assets/jquery.min.js"></script>
  <script src="assets/bootstrap.min.js"></script>
  <style>
  .jumbotron {
    background-image: url("assets/rickandmorty.jpeg");
    background-size: cover;
    height: 340px;
  }
  </style>
</head>
<body>

  <div class="container">
    <div class="jumbotron"></div>
    <h1>Help Morty!</h1></br>
    <p>Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!</p></br>
    <p>I need you to <b>*BURRRP*</b>....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is,
    I have no idea what the <b>*BURRRRRRRRP*</b>, password was! Help Morty, Help!</p></br>
  </div>

  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->

</body>
</html>

```

i found that there is a login panel opened it
username: R1ckRul3s
password: Wubbalubbadubdub

so portal.php basically a terminal so i listed all the files and the opened them in bowerser bcoz cat as disabled

1. What is the first ingredient Rick needs? 
 	in http://10.10.15.32/Sup3rS3cretPickl3Ingred.txt
 	> mr. meeseek hair
then we have a clue.txt which says "Look around the file system for the other ingredient."

after view-source:http://10.10.15.32/portal.php i found this 
'Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0=='
but i don't know what is this

then just tried to use the shell to get inside the rick home dir and found the second ingridient
cat was not working but less can be used and got it

```
less "/home/rick/second ingredients"

```
2. Whats the second ingredient Rick needs?
	> 1 jerry tear

for 3rd ingredient i looked around in the root dir to find the flag but didn't get tried to use "find" command but not able to do so then i saw a walkthrogh can saw that
in /root we have the 3rd flag 

```
sudo ls /root
sudo less /root/3rd.txt
```

3. Whats the final ingredient Rick needs?
	>fleeb juice






# Post Exploitation


i got a rsa key
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCMLOT6NhiqH5Rp36qJt4jZwfvb/H/+YLRTrx5mS9dSyxumP8+chjxkSNOrdgNtZ6XoaDDDikslQvKMCqoJqHqp4jh9xTQTj29tagUaZmR0gUwatEJPG0SfqNvNExgsTtu2DW3SxCQYwrMtu9S4myr+4x+rwQ739SrPLMdBmughB13uC/3DCsE4aRvWL7p+McehGGkqvyAfhux/9SNgnIKayozWMPhADhpYlAomGnTtd8Cn+O1IlZmvqz5kJDYmnlKppKW2mgtAVeejNXGC7TQRkH6athI5Wzek9PXiFVu6IZsJePo+y8+n2zhOXM2mHx01QyvK2WZuQCvLpWKW92eF amiOpenVPN
```
------------------------------------------------END-------------------------------------------------------------------------------