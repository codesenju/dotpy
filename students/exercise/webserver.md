### Go to http://192.168.1.11:8080 login with student user

username: student

password: student

[login_form](../../images/login_form.png)

- Once logged in on the left pane menu go to Compute -> Instances, and click on **Add Instance**

### Setup Webserver
```bash
read -p "Enter your name: " name && \
sudo apt update && \
sudo apt install apache2 -y && \
sudo bash -c 'echo "<html><body style=\"display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; background-color: #e6f2ff;\"><div style=\"text-align: center; background-color: white; padding: 2rem; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);\"><h1>Congratulations '"$name"'<br> You'\''ve successfully launched your first VM on the cloud!</h1></div></body></html>" > /var/www/html/index.html' && \
sudo systemctl start apache2 && \
sudo systemctl enable apache2 && \
echo -e 'Setup Complete!\nNext step is to test by running the following command: "curl localhost"'
```
### Test
```bash
curl localhost
```
### Cleanup
```bash
sudo systemctl stop apache2 && sudo systemctl disable apache2 && sudo apt purge apache2* -y && sudo apt autoremove -y && sudo rm -rf /var/www/html/* && echo "Apache and all web content have been removed."
```