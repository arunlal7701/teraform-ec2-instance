# teraform-ec2-instance
This code is used to create an ec2 instance using Teraform


#### ec2 installation ################


resource "aws_instance" "webserver" {
  ami  =  "ami-09d95fab7fff3776c"
  instance_type = "t2.micro"
  key_name = "Your key name"
  availability_zone = "us-east-1a"
  vpc_security_group_ids = [ "sg-02b6ebf901f612702" ]
  tags = {
    Name = "Blog"
  }


### httpd & php installation and file transfer #################



provisioner "file" {
    source      = "healthcheck.html"
    destination = "/var/www/html/healthcheck.html"
  }


provisioner "file" {
    source      = "index.php"
    destination = "/var/www/html/index.php"
  }



provisioner "remote-exec" {
    inline = [
      "sudo yum install httpd -y",
      "sudo service httpd start",
      "sudo yum install php -y",
      "sudo chkconfig httpd on",
      "sudo chmod 644 /var/www/html/index.php",
    ]
  }

}
