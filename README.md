# Jenkins MySQL GitHub
> This repo comprises a GitHub repo with a sql-script that triggers Jenkins every time changes are pushed to GitHub.

## Installation
A VPS is rolled out in a cloud.
Then MySQL and Jenkins get installed.
### MySQL
```bash
sudo apt update
wget https://dev.mysql.com/get/mysql-apt-config_0.8.17-1_all.deb -P /tmp
sudo apt install gnupg
sudo dpkg -i /tmp/mysql-apt-config_0.8.17-1_all.deb
sudo apt update
sudo apt install mysql-server
systemctl status mysql
systemctl is-enabled mysql
# checking that rfam db works
mysql --user rfamro --host mysql-rfam-public.ebi.ac.uk --port 4497 --database Rfam -e "SELECT fr.rfam_acc, fr.rfamseq_acc, fr.seq_start, fr.seq_end, f.type FROM full_region fr, rfamseq rf, taxonomy tx, family f WHERE rf.ncbi_id = tx.ncbi_id AND f.rfam_acc = fr.rfam_acc AND fr.rfamseq_acc = rf.rfamseq_acc AND tx.tax_string LIKE '%Mammalia%' AND f.type LIKE '%snoRNA%' AND is_significant = 1 limit 10;"
```
### Jenkins
```bash
sudo apt install default-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
sudo systemctl is-enabled jenkins
ss -stlpn
sudo cat /var/lib/jenkins/secrets/initialAdminPassword # copy this password to http://178.154.253.26:8080/
```
## Usage
### Jenkins
Run in your web browser:  
http://178.154.253.26:8080/  

### GitHub repo
https://github.com/makuznet/12_5-mysql-jenkins

### Integrating GitHub with Jenkins
Follow this tutorial:  
https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project

Use this command in the `build > run shell command` field when creating a pipeline:
```bash
([ -d /tmp/rfam ]) && (sudo rm -rf /tmp/rfam); git clone https://github.com/makuznet/12_5-mysql-jenkins.git /tmp/rfam; sudo /tmp/rfam/rfam.sh
```
Then change limit parameter value in the very end of command of rfam.sh, commit and push to GitHub.  
This will trigger Jenkins to build :)  

## Extra
### Ansible ping
```bash
ansible -i inventory.yml -m ping all
```

## Acknowledgments
This repo was inspired by [skillfactory.ru](https://skillfactory.ru/devops#syllabus) team

## See Also
- [How To Install the Latest MySQL on Debian 10](https://www.digitalocean.com/community/tutorials/how-to-install-the-latest-mysql-on-debian-10)  
- [Rfam MySQL Database access](https://docs.rfam.org/en/latest/database.html)  
- [How to Integrate Your GitHub Repository to Your Jenkins Project](https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project)   
- [Run SQL Query Using Bash Script and Command Line](https://www.nitendratech.com/database/run-sql-query-using-bash-script/) 
- [How to Install Jenkins on Debian 10 Linux](https://linuxize.com/post/how-to-install-jenkins-on-debian-10/)  


## License
Follow all involved parties licenses terms and conditions.