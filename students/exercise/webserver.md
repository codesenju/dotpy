# Prerequisites

## Download and install tailscale client

- https://tailscale.com/download

### Setup tailscale client
-  Replace KEY with the key you got from your instructor.
```powershell
tailscale up --login-server=https://headscale.jazziro.com --authkey=KEY --accept-dns=false --accept-routes
```

# Launching VM

### Go to <http://192.168.1.11:8080> login with student user

username: student<br>
password: student

![login_form](../../images/login_form.png)

- Once logged in, from the left navigation menu go to ***Compute -> Instances***, and click on **Add Instance**

![add_instance](../../images/add_instance.png)

## Add the following configurations for your vm

#### 1. Select deployment infrastructure (Leave as is)

- zone1

#### 2. Template/ISO

- Ubuntu Server 24.04 LTS (Noble Numbat)

#### 3. Compute offering

- Small Instance

#### 4. Data disk

- No thanks

#### 5. Networks (Leave as is)

- mynet

#### 6. SSH key pairs (Skip)

#### 7. Advanced mode (Skip)

#### 8. Details

- Please fill in your name in the **Name** field.
- Please select **Standard (US) keyboard** from the keyboard language drop down list.

![details](../../images/step_8_details.png)

### Before proceeding to the next steps make sure your vm settings match with the following:

![vm_settings](../../images/vm_settings.png)

### Click **Launch Instance**

![launch_instance](../../images/launch_instance.png)

### Once your instance is running open the *view console*

![vm_starting](../../images/vm_starting.png)
![vm_running](../../images/vm_running.png)
![open_view_console](../../images/open_view_console.png)

### Login to your vm using the following credentials:

- username: cloud
- password: cloud

#### Run the bellow command to confirm your vm has internet acces:
```bash
ping google.com -c 2
```
You should result with `0% packet loss`:
![ping_google](../../images/ping_google.png)
If you are getting the same results as above, you're good move on to the next steps!
# Setup Networking

### From the left navigation menu, go to Network -> Guest Networks

### Open mynet, select the *Public IP addresses* panel, and click on *Acquire new IP*

![new_ip](../../images/new_ip.png)

### Pick the next available IP and write down the ip number, this will be used in the next steps

![ip_list](../../images/new_ip_list.png)

### Click the ip you selected in the previous step

![click_ip](../../images/click_ip.png)

## Configure your *Firewall* and *Port forwarding* to match the followng

## Port forwarding

- Add port 22 on all fields and click on the Add Instance button *Add*.

![add_instance](../../images/pf_add_instance.png)

- Make sure to select the instance you created earlier that has your name.

![select_instance](../../images/pf_select_instance.png)

### Repeat the two previous steps but for port 80

![port_forwarding_final](../../images/port_forwarding_final.png)

## Firewall

- protocol = TCP
- start port = 22
- end port = 22
- click add

![firewall_22](../../images/firewall_22.png)

...and again for port 80

![firewall_80](../../images/firewall_80.png)

## SSH

Open windows powershell and ssh into your vm using `your own public ip`. Enter the following command:

!! DO NOT FORGET TO REPLACE 192.168.1.23 WITH YOUR OWN PUBLIC IP !!
```powershell
ssh cloud@192.168.1.23
```
If asked `Are you sure you want to continue connecting`, type *yes* and press *Enter*

![ssh](../../images/ssh.png)

# Setup the Webserver
Copy and paste bellow in your terminal:

*You will be asked to enter your name, please do so and press enter*.
```bash
read -p "Enter your name: " name && \
sudo apt update && \
sudo apt install apache2 -y && \
sudo bash -c 'echo "<html><body style=\"display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; background-color: #e6f2ff;\"><div style=\"text-align: center; background-color: white; padding: 2rem; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);\"><h1>Congratulations '"$name"'<br> You'\''ve successfully launched your first VM on the cloud!</h1></div></body></html>" > /var/www/html/index.html' && \
sudo systemctl start apache2 && \
sudo systemctl enable apache2 && \
echo -e 'Setup Complete!\nNext step is to test by running the following command: "curl localhost"'
```

# Test

```bash
curl localhost
```
Open your browser and enter `your public ip`.

![webserver](../../images/webserver.png)

Final steps take proof and send to your instructor:
- take a screenshot of the browser

### Cleanup

- Make sure to clean up after yourself:
  - Remove your vm and delete the public ip.