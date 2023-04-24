Two-tier architecture in AWS
-

![Diagram.png](files%2FDiagram.png)

First launch an AWS instance:
1. Choose proper name.
2. Choose appropriate system :`Canonical, Ubuntu, 18.04 LTS, amd64 bionic image build on 2022-06-10`
3. Key pair : `tech221`.
4. Select existing security group:
- make sure proper ports are set.
5. Click `Launch instance`.

![launch_app_instance.png](files%2Flaunch_app_instance.png)

6. We need to make sure that our security group also has `port 3000` available.

![inbound_app.png](files%2Finbound_app.png)

- Select your instance and go into your Security Groups
- Click on "Edit Inbound Rules"
- Add a new rule:
- Custom TCP
- Set port to 3000
- Source "IPv4 from anywhere (0.0.0.0)"
- Save the rule

7. Now we need two terminals.
8. One to SSH into our new instance & second to copy our `app` folder using `Data Migration` (make sure that both terminals are in .ssh file):
- `ssh -i "tech221.pem" ubuntu@ec2-52-50-230-119.eu-west-1.compute.amazonaws.com`.
- `scp -i "tech221.pem" -r /c/Users/Mateusz/Documents/tech221/virtualisation/app ubuntu@ec2-52-50-230-119.eu-west-1.compute.amazonaws.com:/home/ubuntu`
 
- `scp` – secure copy from our local host to secure instance that is running on our cloud, pick the folder, secure it, copy to instance running.

`scp -i example.pem -r app-folder example@example.com:~/`

In this command, the -i option specifies the private key file to use for authentication, the -r option specifies that the app folder should be copied recursively (i.e., including all subdirectories and files), and the example@example.com:~/ specifies the remote host and destination directory for the copied folder.

![copy_app_instance.png](files%2Fcopy_app_instance.png)

- make sure to add `:/home/ubuntu` at the end.
9. Now in our VM install required dependencies:

![app_prov_file.png](files%2Fapp_prov_file.png)

- `cd app`
- `sudp npm install` - to install the app.
- `node app.js` - to run the app.

![app_works.png](files%2Fapp_works.png)

Setting Reverse Proxy:
-
1. Make sure that Nginx is installed: `sudo apt-get install nginx` or `sudo systemctl status nginx`.
2. Now we need to navigate to Nginx configuration file : `cd /etc/nginx/sites-available/`
3. Create reverse proxy conf file in the folder: `sudo nano nodeapp.conf`.

```server {
   listen 80;
   server_name <server name>;

   location / {
       proxy_pass http://<server name>:3000;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
   }
}
```

`<server name>` - your EC2 IP address

4. Enable the configuration by creating a symbolic link to enable a new config file: `sudo ln -s /etc/nginx/sites-available/nodeapp.conf /etc/nginx/sites-enabled/nodeapp.conf`.
5. Check configuration for errors: `sudo nginx -t`.
6. Reload nginx - `sudo systemctl reload nginx`.
7. Enable nginx - `sudo systemctl enable nginx`.
8. Go back to main folder using `cd` & navigate to "app": `cd app`.
9. Launch app by using `node app.js`.

![rev_works.png](files%2Frev_works.png)

Creating new EC2 Instance for Database.
-

1. Launch instance.
2. Choose proper system: `Canonical, Ubuntu, 18.04 LTS, amd64 bionic image build on 2022-05-26`. (20.04 doesn't work).
3. Key pair login : `tech221`.
4. Security group:

![db_instance.png](files%2Fdb_instance.png)

- Add SSH connection with "MyIP" rule
- Add "Custom TCP" connection with port "27017" for MondoDP and "IP from anywhere (0.0.0.0)".

5. Launch instance.
6. Connect to the Instance using SSH connection and GitBash terminal.
7. Run this commands.

```
sudo apt update -y 

sudo apt upgrade -y

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927

echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

sudo apt update -y 

sudo apt upgrade -y

sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

sudo systemctl start mongod

```

Connecting MongoDB VM & App VM.
-

On DB instance:

1. First in our DB VM: `sudo nano /etc/mongod.conf` - open mongod configuration file.
2. Change bindip to 0.0.0.0.
3. `Ctrl + s` & `Ctrl + x` to save and exit.
4. Restart mongod - `sudo systemctl restart mongod`.
5. `sudo systemctl enable mongod` - enable autostart of mongo.
6. `sudo systemctl status mongod` - ensure that mongo is running.

On App instance:

1. Use `cd` to go back to main directory.
2. `sudo nano .bashrc` - open .bashrc file to create your environment variable there.
3. Scroll to the bottom and write `export DB_HOST=mongodb://<server>:27017/posts` where:

- `export DB_HOST` - creating an environment variable called DB_HOST
- `mongodb://<server>:27017/posts` - telling machine to connect to mongodb database at the specific IP address that we have assigned in our DB Instance and to the page called posts.

4. Type `source .bashrc` - restarts .bashrc file to apply changes.
5. `cd app` to navigate to app folder.
6. `node seeds/seed.js` - seed data to database

![seeded.png](files%2Fseeded.png)

7. Run the app by using `node app.js`. (If the port is already assigned use: `sudo lsof -i :3000` / or `lsof -i tcp:3000`, find PID and then use: kill -9 <PID>)
8. Pass your App Instance IP into browser, add `/posts` and check if everything works.

![last_work.png](files%2Flast_work.png)





